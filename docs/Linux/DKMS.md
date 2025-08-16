# Ubuntu禁用Secure Boot

## 禁用操作步骤
1. 打开终端并执行：
```bash
sudo mokutil --disable-validation
```
2. 输入一个8-16位的临时密码并确认，如：12345678
3. 重启系统进入蓝色的MOK management界面
4. 选择Change Secure Boot state
5. 按提示输入临时密码对应的字符，并按Enter键确认
6. 选择Yes禁用Secure Boot

## 重新启用
```bash
sudo mokutil --enable-validation
```

## 参考链接
[DKMS](https://wiki.ubuntu.com/UEFI/SecureBoot/DKMS)