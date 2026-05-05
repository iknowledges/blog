# shadcn/ui安装教程

1. 创建新项目：

```
npm create vite@latest
cd my-project
npm install
```

- Project name: my-project
- Select a framework: React
- Select a variant: JavaScript

2. 添加Tailwind CSS

```
npm install tailwindcss @tailwindcss/vite
```

替换src/index.css的内容为：

```css
@import "tailwindcss";
```

3. 创建jsconfig.json：

```json
{
  "files": [],
  "references": [],
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

4. 修改vite.config.js：

```javascript
import path from "path"
import tailwindcss from "@tailwindcss/vite"
import react from "@vitejs/plugin-react"
import { defineConfig } from "vite"

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(), tailwindcss()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
    },
  },
})
```

4. 运行CLI初始化命令

```
npx shadcn@latest init
```

- Select a component library: Radix
- Which preset would you like to use: Nova

5. 添加组件

```
npx shadcn@latest add button
```

修改src/App.jsx使用该组件：

```javascript
import { Button } from "@/components/ui/button"

function App() {
  return (
    <div className="flex min-h-svh flex-col items-center justify-center">
      <Button>Click me</Button>
    </div>
  )
}

export default App
```

6. 启动项目

```
npm run dev
```

#### 参考资料

- [shadcn installation](https://ui.shadcn.com/docs/installation/vite)