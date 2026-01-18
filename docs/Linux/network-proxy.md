# 命令行网络代理设置

## Linux

1. 新建脚本proxy.sh:

- http/https

```shell
proxy_on() {
    export http_proxy="http://127.0.0.1:1080"
    export https_proxy="http://127.0.0.1:1080"
    export no_proxy="localhost,127.0.0.1,::1,192.168.*.*,"
    echo -e "\033[32mProxy On\033[0m"
}

proxy_off() {
    unset http_proxy
    unset https_proxy
    unset no_proxy
    echo -e "\033[31mProxy Off\033[0m"
}
```

- socks5

```shell
proxy_on() {
    export http_proxy="socks5://127.0.0.1:1080"
    export https_proxy="socks5://127.0.0.1:1080"
    export no_proxy="localhost,127.0.0.1,::1,192.168.*.*,"
    echo -e "\033[32mProxy On\033[0m"
}

proxy_off() {
    unset http_proxy
    unset https_proxy
    unset no_proxy
    echo -e "\033[31mProxy Off\033[0m"
}
```

2. 开启或关闭脚本

```
source proxy.sh
# 开启代理
proxy_on
# 关闭代理
proxy_off
```

3. 测试网络时不要使用ping命令，因为使用ping时系统会先进行DNS解析。如果你的本地DNS被污染，即使有代理也无法获取正确的IP。

```
curl -I https://www.google.com
```

## Window的PowerShell

1. 新建proxy.ps1：

```powershell
function proxy_on {
    $env:HTTP_PROXY="http://127.0.0.1:1080"
    $env:HTTPS_PROXY="http://127.0.0.1:1080"
    $env:NO_PROXY="localhost,127.0.0.1,192.168.*,*.local"
    Write-Host "Proxy On" -ForegroundColor Green
}

function proxy_off {
    $env:HTTP_PROXY=$null
    $env:HTTPS_PROXY=$null
    $env:NO_PROXY=$null
    Write-Host "Proxy Off" -ForegroundColor Red
}
```

2. 开启或关闭脚本

```
. .\proxy.ps1
proxy_on
proxy_off
```

- 解决“此系统上禁止运行脚本”的错误，以管理员身份打开 PowerShell 并运行以下命令：

```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

3. 测试网络

```
curl.exe -I --connect-timeout 5 https://www.google.com
```

## Window的CMD

1. 创建proxy_on.bat和proxy_off.bat：

- proxy_on.bat

```bat
@echo off
set HTTP_PROXY=http://127.0.0.1:1080
set HTTPS_PROXY=http://127.0.0.1:1080
set NO_PROXY=localhost,127.0.0.1
echo "Proxy On"
```

- proxy_off.bat

```bat
@echo off
set HTTP_PROXY=
set HTTPS_PROXY=
set NO_PROXY=
echo "Proxy Off"
```

2. 开启或关闭脚本

```
proxy_on.bat
proxy_off.bat
```

#### 参考链接

- [命令行使用代理](https://fiix.one/docs/os/%E5%91%BD%E4%BB%A4%E8%A1%8C%E4%BD%BF%E7%94%A8%E4%BB%A3%E7%90%86/)