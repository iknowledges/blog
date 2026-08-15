# TanStack Router教程

## 安装

1. 创建react项目：

```
pnpm create vite tanstack-router-demo --template react-ts
```

2. 添加TanStack Router依赖和插件：

```
pnpm add @tanstack/react-router
pnpm add -D @tanstack/router-plugin
```

3. 修改vite.config.ts：

```ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { tanstackRouter } from '@tanstack/router-plugin/vite'

// https://vite.dev/config/
export default defineConfig({
  plugins: [
    tanstackRouter({
      target: 'react',
      autoCodeSplitting: true
    }),
    react()
  ],
})
```

4. 创建如下路由文件，文件内容在项目启动后会自动生成：

- src/routes/__root.tsx
- src/routes/index.tsx

5. 修改src/App.tsx，注册路由：

```tsx
import { createRouter, RouterProvider } from '@tanstack/react-router';
import { routeTree } from './routeTree.gen';

const router = createRouter({ routeTree });

declare module "@tanstack/react-router" {
  interface Register {
    router: typeof router;
  }
}

function App() {
  return <RouterProvider router={router} />;
}

export default App
```

## Authentication

1. 创建`src/hooks/useAuth.tsx`：

```tsx
import { useState } from "react";

export const useAuth = () => {
    const [isAuth, setIsAuth] = useState<boolean>(() => {
        return localStorage.getItem("isAuthenticated") === "true";
    });

    const login = () => {
        localStorage.setItem("isAuthenticated", "true")
        setIsAuth(true)
    }
    const logout = () => {
        localStorage.removeItem("isAuthenticated")
        setIsAuth(false)
    }
    const isAuthenticated = () => isAuth;

    return { login, logout, isAuthenticated }
}

export type AuthContext = ReturnType<typeof useAuth>
```

2. 修改`src/routes/__root.tsx`，设置路由上下文：

```tsx
import { Link, Outlet, createRootRouteWithContext } from '@tanstack/react-router'
import type { AuthContext } from '../hooks/useAuth'

type MyRouterContext = {
    auth: AuthContext
}

export const Route = createRootRouteWithContext<MyRouterContext>()({
  component: () => <Outlet />,
})
```

3. 修改`src/App.tsx`中的路由配置：

```tsx
import { createRouter, RouterProvider } from '@tanstack/react-router';
import { routeTree } from './routeTree.gen';
import { useAuth } from './hooks/useAuth';

const router = createRouter({
  routeTree,
  context: {
    // auth will be passed down from App component
    auth: undefined!
  }
});

declare module "@tanstack/react-router" {
  interface Register {
    router: typeof router;
  }
}

function App() {
  const auth = useAuth()
  return <RouterProvider router={router} context={{ auth }} />;
}

export default App
```

4. 创建`src/routes/_authenticated.tsx`：

```tsx
import { createFileRoute, redirect } from "@tanstack/react-router";

export const Route = createFileRoute("/_authenticated")({
  beforeLoad: async ({ context }) => {
    const { isAuthenticated } = context.auth;
    if (!isAuthenticated()) {
      throw redirect({
        to: "/login"
      });
    }
  },
});
```

5. 创建`src/routes/_authenticated/dashboard.tsx`，目录名称要与上一步的文件名相同，所有`_authenticated`目录下的页面都需要登录才能访问：

```tsx
import { createFileRoute } from '@tanstack/react-router'

export const Route = createFileRoute('/_authenticated/dashboard')({
  component: RouteComponent,
})

function RouteComponent() {
  return <div>Hello Dashboard!</div>
}
```

6. 创建登录页`src/routes/login.tsx`：

```tsx
import { createFileRoute } from '@tanstack/react-router'
import { useAuth } from '../hooks/useAuth';

export const Route = createFileRoute('/login')({
  component: RouteComponent,
})

function RouteComponent() {
  const auth = useAuth();
  return (
    <>
      <h2>Login</h2>
      {auth.isAuthenticated() ? (
        <>
          <p>Hello user!</p>
          <button onClick={() => {
            auth.logout()
          }}>
            Logout
          </button>
        </>
      ) : (
        <button onClick={() => {
          auth.login()
        }}>
          Login
        </button>
      )}
    </>
  )
}
```

7. 最后，没登陆访问http://localhost:5173/dashboard就会跳转到/login，登录后就能正常访问。

#### 参考文档

- [How to Set Up Basic Authentication and Protected Routes](https://tanstack.com/router/latest/docs/how-to/setup-authentication)
- [TanStack Router Demo](https://github.com/Balastrong/tanstack-router-demo/tree/04-authenticated-routes)
