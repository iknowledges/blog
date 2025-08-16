# vim常用命令

## 基本命令

- i：在当前光标位置插入文本。
- x：删除当前光标所在位置的字符。
- :w：保存文件。
- :q：退出Vim编辑器。
- :q!：强制退出Vim编辑器，不保存文件。
- :wq：保存文件并退出Vim编辑器。

## 光标移动命令

- h：将光标向左移动一个字符。
- j：将光标向下移动一行。
- k：将光标向上移动一行。
- l：将光标向右移动一个字符。
- w：将光标移动到下一个单词的开头。
- e：将光标移动到当前单词的末尾。
- b：将光标移动到上一个单词的开头。
- 0：将光标移动到当前行的开头。
- $：将光标移动到当前行的末尾。
- G：将光标移动到文件的末尾。
- gg：将光标移动到文件的开头。
- /<pattern>：向下搜索<pattern>。

## 文本编辑命令

- dd：删除或剪切当前行。
- yy：复制当前行。
- p：粘贴已复制或删除的文本。
- u：撤销上一次操作。
- Ctrl-r：重做上一次操作。
- r：替换当前光标所在位置的字符。
- c：删除从当前光标位置到指定位置的文本并进入插入模式。
- v：进入可视模式，选择文本。
- :s/<old>/<new>/g：将当前行中的<old>替换为<new>。
- :%s/<old>/<new>/g：将整个文件中的<old>替换为<new>。

## 插入模式命令

- Esc：退出插入模式。
- Ctrl-h：删除光标左侧的字符。
- Ctrl-w：删除光标左侧的单词。
- Ctrl-u：删除当前行的所有文本。
- Ctrl-a：插入文本到行首。
- Ctrl-e：插入文本到行尾。
- Ctrl-t：插入一个制表符。

## 宏命令

宏是一种将多个操作序列记录并重复执行的方法。

- qa：开始录制宏并将其存储在寄存器a中。
- q：停止录制宏。
- @a：执行存储在寄存器a中的宏。
- @@：重复上一次执行的宏。

## 分屏命令

- :sp：水平分屏当前窗口。
- :vsp：垂直分屏当前窗口。
- Ctrl-w h：将光标移到左侧窗口。
- Ctrl-w j：将光标移到下方窗口。
- Ctrl-w k：将光标移到上方窗口。
- Ctrl-w l：将光标移到右侧窗口。
- Ctrl-w +：增加当前窗口的高度。
- Ctrl-w -：减小当前窗口的高度。

## 多文件编辑命令

- :e <filename>：打开指定的文件。
- :tabnew <filename>：在新选项卡中打开指定的文件。
- :tabnext：切换到下一个选项卡。
- :tabprev：切换到上一个选项卡。
- :tabclose：关闭当前选项卡。

## 其他命令

- :set number：显示行号。
- :set nonumber：隐藏行号。
- :set expandtab：使用空格代替制表符。
- :set tabstop=4：设置制表符宽度为4个字符。
- :set hlsearch：高亮显示搜索结果。
- :set nohlsearch：取消高亮显示搜索结果。
- :set background=dark：将背景设置为暗色。
- :set background=light：将背景设置为亮色。

#### 参考资源

- [vim怎样将指定行内容复制到另外一个文件](https://www.jianshu.com/p/1ddbcc6aee7a)
- [打开多个文件、同时显示多个文件、在文件之间切换](https://www.cnblogs.com/pengdonglin137/p/3525297.html)
- [vim常用搜索技巧](https://www.jianshu.com/p/bdc1f7e689b3)
- [史上最全Vim快捷键键位图](https://www.runoob.com/w3cnote/all-vim-cheatsheat.html)
