# Docsify配置

## drawio

1. html文件引入js脚本

```html
<!-- drawio start -->
<script src="https://app.diagrams.net/js/viewer.min.js"></script>
<script src='https://fastly.jsdelivr.net/npm/docsify-drawio/drawio.js'></script>
<!-- drawio end -->
```

2. $docsify配置

```javascript
// drawio start
markdown: {
  renderer: {
    code: function (code, lang) {
      if (lang === 'drawio') {
        if (window.drawioConverter) {
          console.log('drawio 转化中')
          return window.drawioConverter(code)
        } else {
          return `<div class='drawio-code'>${code}</div>`
        }
      } else {
        return this.origin.code.apply(this, arguments);
      }
    }
  }
},
// drawio end
```

3. Markdown文件中写入test.drawio的相对路径

```
[filename](path_to/test.drawio ':include :type=code')
```

#### 参考资料

[docsify-drawio](https://github.com/KonghaYao/docsify-drawio)