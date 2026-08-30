# LM Studio安装教程

1. 打开[Download LM Studio](https://lmstudio.ai/download)下载安装包，并运行安装。

2. 运行LM Studio后，打开设置，找到【General】->【App Info】->【App home directory】，如`C:\Users\abc\.lmstudio`

3. 以管理员身份运行CMD，执行下面命令：

```
# 备份.lmstudio文件夹
robocopy "C:\Users\abc\.lmstudio" "E:\backup\.lmstudio" /E
# 删除原文件
rmdir /s /q "C:\Users\abc\.lmstudio"
# 创建新的.lmstudio文件夹
mkdir "E:\Users\abc\.lmstudio"
# 创建符号链接
mklink /J "C:\Users\abc\.lmstudio" "E:\Users\abc\.lmstudio"
# 还原备份文件
robocopy "E:\backup\.lmstudio" "E:\Users\abc\.lmstudio" /E
# 删除备份文件
rmdir /s /q "E:\backup\.lmstudio"
```

4. 如果下载太慢，可以添加如下Windows系统环境变量：

- HF_ENDPOINT: https://hf-mirror.com