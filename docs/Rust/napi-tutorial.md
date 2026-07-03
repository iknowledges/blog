# napi-rs使用教程

## 创建Rust项目

1. 安装cli工具

```
npm install -g @napi-rs/cli
```

2. 创建my-rust-lib项目

```
napi new my-rust-lib
```

- Package name: my-rust-lib
- Minimum node-api version: napi9
- Choose target(s) your crate will be compiled to: x86_64-unknown-linux-gnu
- License for open-sourced project: MIT
- Enable type definition auto-generation: Yes
- Enable Github Actions CI: No

3. 编译项目

```
cd my-rust-lib
npm install
npm run build
```

## 创建Nextjs项目

1. 新建项目，这里最新版的next.js是16.2.10：

```
npx create-next-app@latest my-app
cd my-app
npm install
```

2. 由于新版本默认打包工具是turbopack，这里改用webpack，修改package.json：

```json
{
  "scripts": {
    "dev": "next dev --webpack",
    "build": "next build --webpack",
    "start": "next start",
    "lint": "eslint"
  },
}
```

3. 然后本地安装前面创建的Rust包，并安装加载`.node`文件需要的工具nextjs-node-loader：

```
npm install ../my-rust-lib
npm install nextjs-node-loader --save
```

4. 修改next.config.ts：

```ts
import type { NextConfig } from "next";

const nextConfig: NextConfig = {
  /* config options here */
  webpack: (config, { isServer }) => {
    if (isServer) {
      config.module.rules.push({
        test: /\.node$/,
        use: 'nextjs-node-loader',
      });
    }
    return config;
  },
};

export default nextConfig;
```

5. 最后在Server容器中引入my-rust-lib包：

- app/page.tsx

```tsx
import { plus100 } from "my-rust-lib";

export default function Home() {
  const result = plus100(100);
  console.log(result);

  return (
      <h1>Hello World</h1>
  );
}
```

6. 启动服务，就能在Console中看到打印内容：

```
npm run dev
```

# 参考资料

- [Turbopack](https://nextjs.org/docs/app/api-reference/turbopack)
- [NextJS and Rust: Creating a Custom Webpack Loader for Native Node Modules](https://www.amarjanica.com/nextjs-and-rust-creating-a-custom-webpack-loader-for-native-node-modules/)