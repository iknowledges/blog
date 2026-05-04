# Ollama安装教程

## 安装

参照[官方文档](https://docs.ollama.com/linux)

```
curl -fsSL https://ollama.com/install.sh | sh
```

## 卸载

```
# 删除ollama服务
sudo systemctl stop ollama
sudo systemctl disable ollama
sudo rm /etc/systemd/system/ollama.service
# 删除ollama的库
sudo rm -r $(which ollama | tr 'bin' 'lib')
# 删除ollama的命令
sudo rm $(which ollama)
# 删除ollama用户和组，以及下载的模型
sudo userdel ollama
sudo groupdel ollama
sudo rm -r /usr/share/ollama
```

## 常用命令

```
# 下载模型，27b适合16G显存，35b适合24G显存
ollama pull qwen3.6:27b
# 显示下载的模型
ollama list
# 运行模型，输入/bye退出
ollama run qwen3.6:27b
# 删除模型
ollama rm qwen3.6:27b
```
