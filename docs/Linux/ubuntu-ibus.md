# Ubuntu安装中文输入法

在安装中文输入法之前，首先要添加中文语言支持。

1. Settings→Region&Language→Manage Install Languages→Install/Remove Languages→Chinese(simplified) 
2. 返回Region&Language→点击Language→选择汉语→重启系统
3. 在终端输入命令安装拼音输入法：sudo apt install ibus-libpinyin
4. 之后在Sttings→Keyboard→Input Sources→添加Chinese(Intelligent Pinyin)

最后桌面右上角会出现一个输入法选择器，点击选择拼音，就可以开始输入中文了。

#### 参考资料

[IBus](https://wiki.ubuntu.org.cn/IBus)