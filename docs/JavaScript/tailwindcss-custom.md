# Tailwind CSS 自定义

## tailwind.config

1. 原始Breakpoint配置如下：

|Breakpoint prefix|Minimum width|CSS|
|--|--|--|
|sm|640px|@media (min-width: 640px) { ... }|
|md|768px|@media (min-width: 768px) { ... }|
|lg|1024px|@media (min-width: 1024px) { ... }|
|xl|1280px|@media (min-width: 1280px) { ... }|
|2xl|1536px|@media (min-width: 1536px) { ... }|

2. 自定义修改：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
      tailwind.config = {
        theme: {
          screens: {
            sm: '550px',
            md: '800px',
            lg: '1200px',
            xl: '1440px',
          },
          fontFamily: {
            sans: ['Graphik', 'sans-serif'],
            serif: ['Merriweather', 'serif'],
          },
          extend: {
            colors: {
              primary: '#ff5733',
              secondary: '#fffc33',
            },
            spacing: {
              6: '2.5rem',
              128: '32rem',
            },
            borderRadius: {
              '4xl': '2rem',
            },
          },
        },
      }
    </script>
    <title>Customization</title>
  </head>
  <body
    class="bg-black sm:bg-green-800 md:bg-blue-800 lg:bg-yellow-800 xl:bg-purple-800 2xl:bg-orange-800"
  >
    <div
      class="border-secondary border p-6 mb-128 max-w-sm mx-auto rounded-4xl"
    >
      <h1 class="text-primary text-xl"></h1>
    </div>

    <script>
      function showBrowserWidth() {
        const width = window.innerWidth

        document.querySelector('h1').innerHTML = `Width: ${width}`
      }

      window.onload = showBrowserWidth
      window.onresize = showBrowserWidth
    </script>
  </body>
</html>
```

## Functions and directives

方法一：

1. 在`index.css`中定义样式：

```css
@import "tailwindcss";

@layer base {
    h1 {
        font-size: 2rem;
    }

    h2 {
        @apply text-xl;
    }
}

@layer components {
    .btn-blue {
        @apply bg-blue-500 py-2 px-4 rounded-xl font-bold hover:bg-blue-700;
    }
}
```

2. 然后在html中使用：

```html
<h1>Simple Tailwind Starter</h1>
<h2>Hello World</h2>
<button className="btn-blue">Click</button>
```

方法二：

1. 编写`tailwind.config.js`：

```js
module.exports = {
  content: [],
  theme: {
    extend: {
        spacing: {
            128: '32rem',
        }
    },
  },
  plugins: [],
}
```

2. 在`index.css`中导入配置：

```css
@import "tailwindcss";

@config "../tailwind.config.js";

.content-area {
    @apply bg-green-200;
    height: theme('spacing.128');
}
```

3. 然后在html中使用：

```html
<div className="content-area">
  <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Quidem enim repellendus molestiae dolore rerum labore itaque, recusandae nemo corporis ullam. Fugit quos inventore delectus laudantium molestias dignissimos modi quo exercitationem, porro nesciunt saepe maiores at quas numquam similique, doloremque vitae repudiandae, itaque quisquam sequi cum quod earum natus! Quae, ratione.</p>
</div>
```