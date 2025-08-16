# JS自定义右键菜单

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Context Menu for Selected Text</title>
    <style>
        /* 自定义上下文菜单样式 */
        #contextMenu {
            display: none;
            position: absolute;
            background-color: #fff;
            border: 1px solid #ccc;
            padding: 10px;
            z-index: 9999;
        }

        .context-menu-item {
            cursor: pointer;
            padding: 5px;
        }

        .context-menu-item:hover {
            background-color: #f0f0f0;
        }
    </style>
</head>
<body>
    <p>Select some text, then right-click to see the context menu.</p>
    
    <!-- 自定义上下文菜单 -->
    <div id="contextMenu">
        <div class="context-menu-item" id="copyMenuItem">Copy</div>
        <div class="context-menu-item" id="cutMenuItem">Cut</div>
        <div class="context-menu-item" id="pasteMenuItem">Paste</div>
    </div>

    <script>
        // 获取需要操作的元素
        const contextMenu = document.getElementById("contextMenu");
        const copyMenuItem = document.getElementById("copyMenuItem");
        const cutMenuItem = document.getElementById("cutMenuItem");
        const pasteMenuItem = document.getElementById("pasteMenuItem");

        // 监听上下文菜单事件
        document.addEventListener("contextmenu", function(event) {
            event.preventDefault(); // 阻止默认的上下文菜单

            // 检查是否有选中的文本
            const selectedText = window.getSelection().toString();
            if (selectedText) {
                // 显示自定义上下文菜单
                showContextMenu(event.clientX, event.clientY);
            }
        });

        // 显示自定义上下文菜单
        function showContextMenu(x, y) {
            contextMenu.style.left = x + "px";
            contextMenu.style.top = y + "px";
            contextMenu.style.display = "block";
        }

        // 隐藏自定义上下文菜单
        function hideContextMenu() {
            contextMenu.style.display = "none";
        }

        // 监听上下文菜单项的点击事件
        copyMenuItem.addEventListener("click", function() {
            // 执行复制操作，您可以自定义操作
            hideContextMenu();
            alert("Text copied!");
        });

        cutMenuItem.addEventListener("click", function() {
            // 执行剪切操作，您可以自定义操作
            hideContextMenu();
            alert("Text cut!");
        });

        pasteMenuItem.addEventListener("click", function() {
            // 执行粘贴操作，您可以自定义操作
            hideContextMenu();
            alert("Text pasted!");
        });

        // 点击页面任何地方都会隐藏自定义上下文菜单
        document.addEventListener("click", hideContextMenu);
    </script>
</body>
</html>
```