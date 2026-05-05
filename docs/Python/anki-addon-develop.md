# Anki开发教程

1. 安装依赖

```
pip install mypy aqt[qt6]
```

2. 创建如下目录结构：

```
my-addon/
├── runanki.py
└── src
    └── my-addon
        └── __init__.py
```

3. 编写启动文件`runanki.py`:

```python
import os
import aqt

if not os.environ.get("ANKI_IMPORT_ONLY"):
    aqt.run()
```

4. 编写插件逻辑`src/my-addon/__init__.py`:

```python
from aqt import mw, gui_hooks
from aqt.qt import QAction
from aqt.utils import showInfo


def update_selected_cards(browser):
    # 1. Get the IDs of currently selected cards
    card_ids = browser.selected_cards()
    
    if not card_ids:
        showInfo("No cards selected.")
        return
    
    for card_id in card_ids:
        # 2. Get the card and its associated note
        card = mw.col.get_card(card_id)
        note = card.note()
        note_type = card.note_type()
        
        # 3. Update a specific field (e.g., "Back")
        if note_type['type'] == 0:
            if "Front" not in note:
                showInfo("Cannot find Front field")
                break
            if "Back" not in note:
                showInfo("Cannot find Back field")
                break
            front = note["Front"]
            note["Back"] = front + "(Update Basic Card)"
        elif note_type['type'] == 1:
            if "Text" not in note:
                showInfo("Cannot find Text field")
                break
            note['Text'] += "(Update Cloze)"
            
        # 4. Save the note changes to the database
        mw.col.update_note(note)

    showInfo("Update cards finished!")

def setup_browser_action(browser):
    action = QAction("Update Cards", browser)
    action.triggered.connect(lambda: update_selected_cards(browser))
    # Add to the 'Edit' menu in the Browser
    browser.form.menuEdit.addAction(action)

# Register the hook to add the menu item
gui_hooks.browser_menus_did_init.append(setup_browser_action)
```

5. 启动插件

```
robocopy ".\src\my-addon\" "C:\Users\<你的用户名>\AppData\Roaming\Anki2\addons21\my-addon" /E
python .\runanki.py
```

此时打开【Browse】->【Edit】下就会出现【Update Cards】按钮

## 安装第三方的Python库

1. 打开【Help】->【About Anki】查看python版本

2. 安装依赖

```
pip install numpy -t .\src\my-addon\vendor
```

3. 在`__init__.py`文件中添加

```python
addon_path = os.path.dirname(__file__)
vendor_path = os.path.join(addon_path, "vendor")
if vendor_path not in sys.path:
    sys.path.append(vendor_path)
```

#### 参考资料

- [Writing Anki Add-ons](https://addon-docs.ankiweb.net/)
- [PyCharm setup for add-on debugging](https://forums.ankiweb.net/t/pycharm-setup-for-add-on-debugging/17733)
- [Bundling Python modules with add-ons](https://forums.ankiweb.net/t/wiki-bundling-python-modules-with-add-ons/63626)
