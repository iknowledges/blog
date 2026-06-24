# Tailwind CSS 教程

## 1. Colors

Default colors: white, black, red, green, blue, orange, yellow, purple, lime, emerald, teal, cyan, indigo, violet, fuchsia, pink, rose, sky, gray, slate, zinc, neutral, stone, amber

- [Text Colors](https://tailwindcss.com/docs/color)
- [Background Colors](https://tailwindcss.com/docs/background-color)
- [Decoration Colors](https://tailwindcss.com/docs/text-decoration-color)
- [Border Colors](https://tailwindcss.com/docs/border-color)
- [Outline Colors](https://tailwindcss.com/docs/outline-color)
- [Box Shadow Colors](https://tailwindcss.com/docs/box-shadow)
- [Accent Colors](https://tailwindcss.com/docs/accent-color)

### Text Colors

```html
<p class="text-green">Tailwind is awesome</p>
<p class="text-red-50">Tailwind is awesome</p>
```

### Background Colors

```html
<div class="bg-slate-600">
    <p class="text-white">Tailwind is awesome</p>
</div>
```

### Decoration Colors

```html
<p class="underline decoration-red-700">Tailwind is awesome</p>
```

### Border Colors

```html
<input class="border border-rose-500" />
<input class="border-2 border-yellow-500" />
```

- Divide Colors

```html
<div class="divide-y divide-blue-200">
    <div>Item 1</div>
    <div>Item 2</div>
    <div>Item 3</div>
</div>
```

### Outline Colors

```html
<button class="outline outline-blue-500">Subscribe</button>
```

### Box Shadow Colors (Opacity defaults to 100, but you cans set it)

```html
<button class="bg-cyan-500 shadow-lg shadow-cyan-500">Subscribe</button>
<button class="bg-indigo-500 shadow-lg shadow-indigo-500/50">Subscribe</button>
```

### Accent Colors

```html
<input type="checkbox" class="accent-purple-500" checked /> Option 1
```

### Arbitrary Colors

```html
<div class="bg-[#427fab] h-10">Hello</div>
<div class="bg-[rgb(255,0,0)] h-10">Hello</div>
<div class="text-[#427fab] h-10">Hello</div>
<div class="border border-[#427fab] h-10">Hello</div>
```

## 2. Container & Spacing

- [Margin / Space](https://tailwindcss.com/docs/margin)
- [Padding](https://tailwindcss.com/docs/padding)

### Space Between X/Y

```html
<!-- Space Between X -->
<div class="flex space-x-4">
    <div class="p-3 bg-red-100">01</div>
    <div class="p-3 bg-red-100">02</div>
    <div class="p-3 bg-red-100">03</div>
</div>

<!-- Space Between Y -->
<div class="flex flex-col space-y-4">
    <div class="p-3 bg-red-100">01</div>
    <div class="p-3 bg-red-100">02</div>
    <div class="p-3 bg-red-100">03</div>
</div>
```

### Arbitrary Spacing

```html
<div class="ml-[200px] bg-slate-700">ml-[200px]</div>
```

## 3. Typography

- [Font Family](https://tailwindcss.com/docs/font-family)
- [Font Size](https://tailwindcss.com/docs/font-size)
- [Font Weight](https://tailwindcss.com/docs/font-weight)
- [Letter Spacing](https://tailwindcss.com/docs/letter-spacing)
- [Text Alignment](https://tailwindcss.com/docs/text-align)
- [Text Decoration](https://tailwindcss.com/docs/text-decoration-thickness)
- [Text Decoration Line](https://tailwindcss.com/docs/text-decoration-line)
- [Text Decoration Style](https://tailwindcss.com/docs/text-decoration-style)
- [Text Decoration Offset](https://tailwindcss.com/docs/text-underline-offset)
- [Text Transform](https://tailwindcss.com/docs/text-transform)

### Font Family

```html
<p class="font-sans">Tailwind is awesome</p>
<p class="font-serif">Tailwind is awesome</p>
<p class="font-mono">Tailwind is awesome</p>
```

### Font Size

```html
<p class="text-sm">Tailwind is awesome</p>
```

### Font Weight

```html
<p class="font-light">Tailwind is awesome</p>
```

### Letter Spacing

```html
<p class="tracking-tight">Tailwind is awesome</p>
```

### Text Alignment

```html
<p class="text-center">Tailwind is awesome</p>
```

### Text Decoration

- Text Decoration

```html
<p class="underline decoration-4 decoration-blue-400">Tailwind is awesome</p>
<p class="line-through">Tailwind is awesome</p>
<p class="overline">Tailwind is awesome</p>
<p class="no-underline">Tailwind is awesome</p>
```

- Decoration Style

```html
<p class="underline decoration-solid">Tailwind is awesome</p>
<p class="underline decoration-double">Tailwind is awesome</p>
<p class="underline decoration-dotted">Tailwind is awesome</p>
<p class="underline decoration-dashed">Tailwind is awesome</p>
<p class="underline decoration-wavy">Tailwind is awesome</p>
```

- Decoration Offset

```html
<p class="underline underline-offset-1">Tailwind is awesome</p>
<p class="underline underline-offset-2">Tailwind is awesome</p>
<p class="underline underline-offset-4">Tailwind is awesome</p>
<p class="underline underline-offset-8">Tailwind is awesome</p>
```

### Text Transform

```html
<p class="normal-case">Tailwind is awesome</p>
<p class="uppercase">Tailwind is awesome</p>
<p class="lowercase">Tailwind is awesome</p>
<p class="capitalize">Tailwind is awesome</p>
```

## 4. Sizing

- [Width Sizes](https://tailwindcss.com/docs/width)
- [Min Width Sizes](https://tailwindcss.com/docs/min-width)
- [Max Width Sizes](https://tailwindcss.com/docs/max-width)
- [Height Sizes](https://tailwindcss.com/docs/height)
- [Min Height Sizes](https://tailwindcss.com/docs/min-height)
- [Max Height Sizes](https://tailwindcss.com/docs/max-height)

### Width Sizes

```html
<div class="bg-black text-white my-2 w-20">w-20</div>
```

- Percentages

```html
<div class="bg-green-500 text-white my-2 w-2/3">w-2/3</div>
```

- Width of the viewport

```html
<div class="bg-purple-500 text-white my-2 w-screen">w-screen</div>
```

- 100% of container

```html
<div class="bg-zinc-500 text-white my-2 w-full">w-full</div>
```

- min/max content

```html
<div class="bg-emerald-500 text-white my-2 w-min">w-min</div>
<div class="bg-emerald-500 text-white my-2 w-max">w-max</div>
```

- Arbitrary Width

```html
<div class="bg-red-500 text-white my-2 w-[300px]">300px</div>
```

### Max Width Sizes

```html
<div class="bg-gray-100 max-w-lg mx-auto">
  Lorem ipsum dolor, sit amet consectetur adipisicing elit. Laborum
  perferendis incidunt nihil ullam repellendus praesentium consectetur et
  sed distinctio, magni iusto quos repellat officiis cum dolore aliquid
  minus esse pariatur.
</div>
```

### Height Sizes

```html
<div class="h-screen bg-blue-300">Hello</div>
```

### Min Height Sizes

```html
<div class="h-48 bg-red-200">
    <div class="h-24 bg-red-600 min-h-full">Tailwind is awesome</div>
</div>
```

### Max Height Sizes

```html
<div class="h-24 bg-orange-200">
    <div class="h-48 bg-orange-500 max-h-full">Tailwind is awesome</div>
</div>
```

## 5. Layout & Position

- [Position](https://tailwindcss.com/docs/position)
- [top/right/bottom/left](https://tailwindcss.com/docs/top-right-bottom-left)
- [Display](https://tailwindcss.com/docs/display)
- [Z-Index](https://tailwindcss.com/docs/z-index)
- [Float](https://tailwindcss.com/docs/float)

### Position

```html
<div class="relative left-5 w-1/2 h-12 bg-red-200">
    <p>Relative parent</p>
    <div class="absolute bottom-0 right-0 bg-red-500">
        <p>Absolute child</p>
    </div>
</div>
```

### top/right/bottom/left

- Top left corner

```html
<div class="relative h-32 w-32 bg-yellow-100">
    <div class="absolute left-0 top-0 h-16 w-16 bg-yellow-300">01</div>
</div>
```

- Span top edge

```html
<div class="relative h-32 w-32 bg-yellow-100">
    <div class="absolute inset-x-0 top-0 h-16 bg-yellow-300">02</div>
</div>
```

- Span left edge

```html
<div class="relative h-32 w-32 bg-yellow-100">
    <div class="absolute inset-y-0 left-0 w-16 bg-yellow-300">03</div>
</div>
```

### Display

```html
<div>
    Lorem ipsum dolor sit amet, consectetur adipisicing elit. Voluptatibus
    <span class="inline">This is display inline and will wrap normally</span>
    sapiente ut rerum esse ullam provident, fugit
    <span class="inline-block">This is display inline-block and will not extend beyond it's parent</span>
    eos quam
    <span class="block">This is display block and will move to it's own line</span>
    reprehenderit est hic aut unde sequi, officia, ipsa amet doloribus ratione
    <span class="hidden">This is display none and will not display at all</span>
    ad.
</div>
```

### Z-Index

```html
<div class="relative h-36">
    <div class="absolute left-10 w-24 h-24 bg-blue-200 z-40">1</div>
    <div class="absolute left-20 w-24 h-24 bg-blue-300">2</div>
    <div class="absolute left-40 w-24 h-24 bg-blue-400 z-10">3</div>
    <div class="absolute left-60 w-24 h-24 bg-blue-500 z-20">4</div>
    <div class="absolute left-80 w-24 h-24 bg-blue-600">5</div>
</div>
```

### Float

```html
<div class="w-1/2">
    <img src="/img/logo.svg" class="h-48 w-48 border-2 float-right" alt="Tailwind Play" />
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Facere numquam voluptatem accusantium atque cupiditate nulla ratione saepe veniam, ex iure nisi mollitia sed rerum veritatis temporibus iusto! Molestiae, ratione doloribus?</p>
</div>
```

## 6. Backgrounds & Shadows

- [Background Size](https://tailwindcss.com/docs/background-size)
- [Background Repeat](https://tailwindcss.com/docs/background-repeat)
- [Background Position](https://tailwindcss.com/docs/background-position)
- [Background Attachment](https://tailwindcss.com/docs/background-attachment)
- [Background Image](https://tailwindcss.com/docs/background-image)
- [Mix Blend](https://tailwindcss.com/docs/mix-blend-mode)
- [Shadows](https://tailwindcss.com/docs/box-shadow)

### Background Classes

```html
<div class="h-64 w-72 bg-blue-500 bg-cover bg-no-repeat bg-center"
    style="background-image: url('/img/logo.svg')"
></div>
```

### Gradients

```html
<div class="h-24 bg-linear-to-r from-cyan-500 to-blue-500"></div>
<div class="h-24 bg-linear-to-l from-indigo-500 via-purple-500 to-pink-500"></div>
```

### Mix Blend

```html
<div class="flex justify-center -space-x-24">
  <div class="bg-blue-400 mix-blend-multiply">Lorem, ipsum dolor sit amet consectetur adipisicing elit. Mollitia minus deleniti iusto delectus alias natus quam dolor explicabo quas eius!</div>
  <div class="bg-pink-400 mix-blend-multiply">Lorem, ipsum dolor sit amet consectetur adipisicing elit. Mollitia minus deleniti iusto delectus alias natus quam dolor explicabo quas eius!</div>
</div>
```

### Shadows

```html
<div class="w-96 mt-6 ml-4 p-3 shadow-md">
    Lorem, ipsum dolor sit amet consectetur adipisicing elit. Mollitia minus
    deleniti iusto delectus alias natus quam dolor explicabo quas eius!
</div>
<div class="w-96 mt-6 ml-4 p-3 shadow-inner">
    Lorem, ipsum dolor sit amet consectetur adipisicing elit. Mollitia minus
    deleniti iusto delectus alias natus quam dolor explicabo quas eius!
</div>
```

- Shadow Colors

```html
<div class="w-96 mt-6 ml-4 p-3 shadow-xl shadow-blue-500/50">
    Lorem, ipsum dolor sit amet consectetur adipisicing elit. Mollitia minus
    deleniti iusto delectus alias natus quam dolor explicabo quas eius!
</div>
```

## 7. Borders

- [Border Widths](https://tailwindcss.com/docs/border-width)
- [Border Radius](https://tailwindcss.com/docs/border-radius)
- [Outline](https://tailwindcss.com/docs/outline-width)

### Border Widths

```html
<div class="w-96 m-3 p-5 border-2 border-blue-500">
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Id consequuntur
    reiciendis odio eaque vitae nostrum sunt tempora deleniti repellendus
    dolores.
</div>
```

### Border Radius

```html
<div class="w-96 m-3 p-5 bg-gray-300 rounded-lg">
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Id consequuntur
    reiciendis odio eaque vitae nostrum sunt tempora deleniti repellendus
    dolores.
</div>

<!-- Specific Sides -->
<div class="w-96 m-3 p-5 bg-gray-300 rounded-b-xl">
    Lorem ipsum dolor sit amet consectetur adipisicing elit. Id consequuntur
    reiciendis odio eaque vitae nostrum sunt tempora deleniti repellendus
    dolores.
</div>
```

### Outline

```html
<button class="outline outline-offset-2">Button 1</button>
```

## 8. Filters

- [Blur](https://tailwindcss.com/docs/filter-blur)
- [Brightness](https://tailwindcss.com/docs/filter-brightness)
- [Contrast](https://tailwindcss.com/docs/filter-contrast)
- [Grayscale](https://tailwindcss.com/docs/filter-grayscale)
- [Invert](https://tailwindcss.com/docs/filter-invert)
- [Sepia](https://tailwindcss.com/docs/filter-sepia)
- [Hue Rotate](https://tailwindcss.com/docs/filter-hue-rotate)
- [Saturate](https://tailwindcss.com/docs/filter-saturate)

### Blur

```html
<div class="blur-sm">
    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Itaque deserunt
    animi quas aliquam consectetur ut obcaecati voluptas repudiandae odit
    harum?
</div>
```

### Brightness

```html
<div class="brightness-50">
    <img class="w-48" src="/img/logo.svg" alt="" />
</div>
```

### Contrast

```html
<div class="contrast-50">
    <img class="w-48" src="/img/logo.svg" alt="" />
</div>
```

### Grayscale

```html
<div class="grayscale">
    <img class="w-48" src="/img/logo.svg" alt="" />
</div>
```

### Invert

```html
<div class="invert">
    <img class="w-48" src="/img/logo.svg" alt="" />
</div>
```

### Sepia

```html
<div class="sepia">
    <img class="w-48" src="/img/logo.svg" alt="" />
</div>
```

### Hue Rotate

```html
<div class="hue-rotate-90">
    <img class="w-48" src="/img/logo.svg" alt="" />
</div>
```

### Saturate

```html
<div class="saturate-50">
    <img class="w-48" src="/img/logo.svg" alt="" />
</div>
```

## 9. Interactivity

- [Scroll Behavior](https://tailwindcss.com/docs/scroll-behavior)
- [Pseudo Classes](https://tailwindcss.com/docs/hover-focus-and-other-states#pseudo-classes)
- [Styling based on parent state](https://tailwindcss.com/docs/hover-focus-and-other-states#styling-based-on-parent-state)
- [Appearance](https://tailwindcss.com/docs/appearance)
- [Cursor](https://tailwindcss.com/docs/cursor)
- [Resize](https://tailwindcss.com/docs/resize)
- [User Select](https://tailwindcss.com/docs/user-select)

### Scroll Behavior

```html
<html class="scroll-smooth">
  <body>
    <a href="#item">Scroll To Item</a>
    <!-- ... -->
    <div id="item">Item</div>
  </body>
</html>
```

### Hover State Styling

```html
<button type="button" class="m-3 rounded-lg bg-black px-5 py-3 text-white hover:bg-orange-500 hover:text-black">Submit</button>
```

### Focus State Styling

```html
<button type="button" class="m-3 rounded-lg bg-black px-5 py-3 text-white focus:bg-green-500 focus:text-black">Submit</button>
```

### Active State Styling

```html
<button type="button" class="m-3 rounded-lg bg-black px-5 py-3 text-white active:bg-yellow-500 active:text-black">Submit</button>
```

### Styling based on parent state

When you need to style an element based on the state of some parent element, mark the parent with the group class, and use `group-*` modifiers like `group-hover` to style the target element.

```html
<a href="#" class="group mx-auto block max-w-xs space-y-3 rounded-lg bg-white p-6 shadow-lg hover:bg-sky-500">
  <div class="flex items-center">
    <h3 class="group-hover:text-white">Card Title</h3>
  </div>
  <p class="group-hover:text-white">This is some card text</p>
</a>
```

### Pseudo Classes

```html
<ul>
  <li class="odd:bg-blue-200 even:bg-green-200">Item 1</li>
  <li class="odd:bg-blue-200 even:bg-green-200">Item 2</li>
  <li class="odd:bg-blue-200 even:bg-green-200">Item 3</li>
  <li class="odd:bg-blue-200 even:bg-green-200">Item 4</li>
  <li class="odd:bg-blue-200 even:bg-green-200">Item 5</li>
</ul>
```

### Appearance

Use appearance-none to reset any browser specific styling on an element. This utility is often used when creating custom form components.

```html
<select>
  <option>Yes</option>
  <option>No</option>
</select>

<select class="appearance-none">
  <option>Yes</option>
  <option>No</option>
</select>
```

### Cursor

```html
<button type="button" class="m-3 cursor-pointer rounded-lg bg-black px-5 py-3 text-white">Submit</button>

<button type="button" class="m-3 cursor-progress rounded-lg bg-black px-5 py-3 text-yellow-500">Loading...</button>

<button type="button" disabled class="m-3 cursor-not-allowed rounded-lg bg-black px-5 py-3 text-red-200">Confirm</button>
```

### Resize

```html
<textarea class="border border-black rounded resize"></textarea>
```

### User Select

```html
<div class="select-none">Lorem ipsum dolor sit amet.</div>
<div class="select-text">Lorem ipsum dolor sit amet.</div>
<div class="select-all">Lorem ipsum dolor sit amet.</div>
<div class="select-auto">Lorem ipsum dolor sit amet.</div>
```

## 10. Breakpoints & Media Queries

- [Breakpoint Classes](https://tailwindcss.com/docs/responsive-design#Overview)

```html
<div class="h-screen w-screen bg-black sm:bg-green-800 md:bg-blue-800 lg:bg-yellow-800 xl:bg-purple-800 2xl:bg-orange-800">
  <h1 class="text-white text-xl md:text-3xl xl:text-5xl">Tailwind is awesome</h1>
</div>
```

## 11. Columns

- [Columns](https://tailwindcss.com/docs/columns)
- [Break After](https://tailwindcss.com/docs/break-after)
- [Break Before](https://tailwindcss.com/docs/break-before)
- [Aspect Ratio](https://tailwindcss.com/docs/aspect-ratio)

### Break After

```html
<div class="columns-2 gap-8">
  <img class="w-full" src="/img/logo.svg" />
  <img class="w-full" src="/img/logo.svg" />
  <img class="w-full break-after-column" src="/img/logo.svg" />
  <img class="w-full" src="/img/logo.svg" />
</div>
```

### Break Before

```html
<div class="columns-3 gap-24">
  <img class="w-full" src="/img/logo.svg" />
  <img class="w-full break-before-column" src="/img/logo.svg" />
  <img class="w-full" src="/img/logo.svg" />
  <img class="w-full" src="/img/logo.svg" />
  <img class="w-full" src="/img/logo.svg" />
</div>
```

### Aspect Ratio

```html
<div class="columns-3xs">
  <!-- Video Aspect Ratio -->
  <img class="aspect-video w-full" src="/img/logo.svg" />
  <!-- Square Aspect Ratio -->
  <img class="aspect-square w-full" src="/img/logo.svg" />
  <img class="break w-full" src="/img/logo.svg" />
  <img class="w-full" src="/img/logo.svg" />
</div>
```

## 12. Flex

- [Justify Content](https://tailwindcss.com/docs/justify-content)
- [Align Items](https://tailwindcss.com/docs/align-items)
- [Flex Direction](https://tailwindcss.com/docs/flex-direction)
- [Flex Wrap](https://tailwindcss.com/docs/flex-wrap)
- [Flex Properties](https://tailwindcss.com/docs/flex)

### Flex and alignment

```html
<div class="flex h-72 w-150 flex-wrap items-center justify-around bg-gray-100">
  <div class="border border-blue-600 bg-blue-100 p-10">01</div>
  <div class="border border-blue-600 bg-blue-100 p-10">02</div>
  <div class="self-start border border-blue-600 bg-blue-100 p-10">03</div>
  <div class="self-end border border-blue-600 bg-blue-100 p-10">04</div>
</div>
```

### Flex Column, Gap and Order

```html
<div class="flex w-100 flex-col items-center justify-around gap-4 bg-gray-200">
  <div class="order-4 border border-blue-600 bg-blue-100 p-10">01</div>
  <div class="order-1 border border-blue-600 bg-blue-100 p-10">02</div>
  <div class="border border-blue-600 bg-blue-100 p-10">03</div>
  <div class="border border-blue-600 bg-blue-100 p-10">04</div>
</div>
```

### Grow and shrink

```html
<div class="flex w-100 bg-gray-300">
  <!-- flex-none: Prevent item from growing or shrinking -->
  <div class="w-64 flex-none border border-blue-600 bg-blue-100 p-10">01</div>
  <!-- flex-initial:  Allow item to shrink but not grow, taking into account its initial size  -->
  <div class="w-64 flex-initial border border-blue-600 bg-blue-100 p-10">02</div>
  <!-- flex-auto: Allow item to grow and shrink, taking into account its initial size -->
  <div class="w-64 flex-auto border border-blue-600 bg-blue-100 p-10">03</div>
  <!-- flex-1: Allow item to grow and shrink as needed, ignoring its initial size -->
  <div class="w-64 flex-1 border border-blue-600 bg-blue-100 p-10">04</div>
  <div class="border border-blue-600 bg-blue-100 p-10">05</div>
  <div class="border border-blue-600 bg-blue-100 p-10">06</div>
  <div class="border border-blue-600 bg-blue-100 p-10">07</div>
</div>
```

## 13. Grid

- [Grid Template Columns](https://tailwindcss.com/docs/grid-template-columns)
- [Grid Template Rows](https://tailwindcss.com/docs/grid-template-rows)

### Grid cols and rows

```html
<div class="grid w-100 grid-cols-3 grid-rows-4 gap-4 bg-gray-100">
  <div class="border border-blue-600 bg-blue-100 p-10">01</div>
  <div class="border border-blue-600 bg-blue-100 p-10">02</div>
  <div class="border border-blue-600 bg-blue-100 p-10">03</div>
  <div class="border border-blue-600 bg-blue-100 p-10">04</div>
  <div class="border border-blue-600 bg-blue-100 p-10">05</div>
  <div class="border border-blue-600 bg-blue-100 p-10">06</div>
  <div class="border border-blue-600 bg-blue-100 p-10">07</div>
  <div class="border border-blue-600 bg-blue-100 p-10">08</div>
  <div class="border border-blue-600 bg-blue-100 p-10">09</div>
</div>
```

### Col and row span

```html
<div class="grid w-100 grid-cols-3 gap-4 bg-gray-100">
  <div class="col-span-2 row-span-2 border border-blue-600 bg-blue-100 p-10">01</div>
  <div class="border border-blue-600 bg-blue-100 p-10">02</div>
  <div class="border border-blue-600 bg-blue-100 p-10">03</div>
  <div class="border border-blue-600 bg-blue-100 p-10">04</div>
  <div class="border border-blue-600 bg-blue-100 p-10">05</div>
  <div class="border border-blue-600 bg-blue-100 p-10">06</div>
  <div class="col-span-3 border border-blue-600 bg-blue-100 p-10">07</div>
  <div class="border border-blue-600 bg-blue-100 p-10">08</div>
  <div class="col-span-2 border border-blue-600 bg-blue-100 p-10">09</div>
</div>
```

## 14. Transform & Transition

### Transition

- [Transition Property](https://tailwindcss.com/docs/transition-property)
- [Duration](https://tailwindcss.com/docs/transition-duration)
- [Timing Function](https://tailwindcss.com/docs/transition-timing-function)
- [Delay](https://tailwindcss.com/docs/transition-delay)

```html
<button class="m-24 rounded bg-blue-500 px-8 py-2 text-white transition-colors duration-1000 hover:bg-blue-700">Click me</button>
```

### Transform

- [Scale](https://tailwindcss.com/docs/scale)
- [Rotate](https://tailwindcss.com/docs/rotate)
- [Translate](https://tailwindcss.com/docs/translate)
- [Skew](https://tailwindcss.com/docs/skew)
- [Transform Origin](https://tailwindcss.com/docs/transform-origin)

```html
<button class="m-24 rounded bg-blue-500 px-8 py-2 text-white transition delay-150 duration-1000 ease-in-out hover:-translate-y-1 hover:scale-110 hover:bg-indigo-500">Click me</button>

<img src="/img/logo.svg" alt="" class="transition hover:scale-75 hover:rotate-180 hover:skew-x-12 hover:transform" />
```

## 15. Animation

- [Animation](https://tailwindcss.com/docs/animation)

```html
<div class="flex min-h-screen items-center justify-center bg-slate-900">
  <svg class="w-48 animate-spin text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
    <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
    <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
  </svg>
</div>
```