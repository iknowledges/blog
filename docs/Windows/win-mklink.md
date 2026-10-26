# Windows设置软链接（WorkBuddy为例）

1. 完全退出WorkBuddy。
2. 将`.workbuddy`和`.workbuddy-key-fallback`两个文件夹剪切到D盘。
3. 以管理员权限打开CMD，创建目录链接：

```
mklink /J "C:\Users\abc\.workbuddy" "D:\Users\abc\.workbuddy"
mklink /J "C:\Users\abc\.workbuddy-key-fallback" "D:\Users\abc\.workbuddy-key-fallback"
```