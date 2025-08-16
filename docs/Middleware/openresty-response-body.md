# OpenResty修改响应体

1. 修改nginx.conf

```
# 开启日志输出，方便调试
error_log  logs/error.log  debug;

http {

    # 导入zlib包，用于后面处理gzip
    init_by_lua_block {
        local zlib = require("zlib")
    }

    server {
        listen 8080;

        # 首页
        location = / {
            proxy_pass https://www.baidu.com;
            header_filter_by_lua_block {
                ngx.header.content_length = nil
            }
            body_filter_by_lua_file /usr/local/openresty/code/body_filter.lua;
            # 处理跨域
            add_header 'Access-Control-Allow-Origin' '*';
        }
    }
}
```

2. 编写body_filter.lua

```lua
local chunk, eof = ngx.arg[1], ngx.arg[2]
local buffered = ngx.ctx.buffered
if not buffered then
    buffered = {}
    ngx.ctx.buffered = buffered
end
if chunk ~= "" then
    buffered[#buffered + 1] = chunk
    ngx.arg[1] = nil
end
if eof then
    local whole = table.concat(buffered)
    ngx.ctx.buffered = nil
    -- 解压gzip
    local uncompress_whole = zlib.inflate()(whole)
    print(uncompress_whole)
    -- 修改响应体内容
    uncompress_whole = string.gsub(uncompress_whole, "百度一下，你就知道",  "我不知道")
     -- 压缩成gzip
    local level = 5
    local windowSize = 15+16
    whole = zlib.deflate(level, windowSize)(uncompress_whole, "finish")
    ngx.arg[1] = whole
end
```

3. 动态生成反向代理URL

```
location ~ /videoplayback {
    resolver 8.8.8.8;
    set $proxy_root ".video.com";
    if ($request_uri ~* "/(.*)/videoplayback") {
        set $path_remainder $1;
    }
    proxy_pass https://$path_remainder$proxy_root;
}
```

这里将http://hello/videoplayback代理到https://hello.video.com/videoplayback

#### 参考资源

- [Lua + OpenResty修改response body](https://jkzhao.github.io/2018/05/03/Lua-OpenResty%E4%BF%AE%E6%94%B9response-body/)
- [Lua实现返回内容gzip解压/压缩](https://ops.m114.org/post/lua-gzip/)
- [浅谈OpenResty中的body_filter_by_lua*](https://zhuanlan.zhihu.com/p/67904411)
- [带变量的 Nginx proxy_pass - 完整路径](https://www.coder.work/article/6259946)
