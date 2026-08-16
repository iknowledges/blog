# React Router教程

## 安装

1. 创建react项目：

```
pnpm create vite react-router-demo --template react-ts
```

2. 添加React Router依赖。关于react-router和react-router-dom的区别是：react-router是react-router-dom和react-router-native的核心库，其中react-router-dom专注于Web应用，而react-router-native专注于本地应用。所以react-router-dom更兼容老版本，而新版本官方更推荐使用react-router：

```
pnpm add react-router-dom
```

3. 修改`src/main.tsx`：

```tsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import App from './App.tsx'
import { BrowserRouter } from 'react-router-dom'

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </StrictMode>,
)
```

4. 新建Home和About两个组件，然后修改`src/App.tsx`如下：

```tsx
import { Link, Route, Routes } from "react-router-dom"
import Home from "./components/Home"
import About from "./components/About"

function App() {
  return (
    <>
      <nav>
        <Link to='/'>Home</Link>
        <br />
        <Link to='/about'>About</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </>
  )
}

export default App
```

#### 参考文档

- [Declarative Mode](https://reactrouter.com/start/declarative/installation)
- [React Router vs. React Router DOM: Understanding the Differences](https://www.syncfusion.com/blogs/post/react-router-vs-react-router-dom)
