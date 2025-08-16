# Vim配置

## 设置Tab为4个空格

修改`/etc/vimrc`：

```
set ts=4
set softtabstop=4
set shiftwidth=4
set expandtab
set autoindent
```

## 利用Tab键进行补全

修改`/etc/vimrc`：

```
function! CleverTab()
     if strpart( getline('.'), 0, col('.')-1 ) =~ '^\s*$'
         return "\<Tab>"
     else
         return "\<C-N>"
     endif
endfunction
inoremap <Tab> <C-R>=CleverTab()<CR>
```

#### 参考资料

- [vi/vim 设置tab为4个空格](https://blog.csdn.net/leo09999/article/details/100710325)
- [Linux中 vim 实现代码补全](https://blog.csdn.net/m0_51961114/article/details/122985695)
