# TypeScript使用教程

1. 创建Node.js项目

```
mkdir my-project
cd my-project
npm init -y
```

2. 安装TypeScript

```
npm install --save-dev typescript
# 生成tsconfig.json
npx tsc --init
```

3. 修改tsconfig.json配置：

```json
{
  "compilerOptions": {
    // File Layout
    "rootDir": "./src",
    "outDir": "./dist",

    ...
  }
}
```

4. 安装TypeScript运行命令：

```
npm install tsx --save-dev
```

5. 修改package.json配置：

```json
{
  "scripts": {
    "dev": "tsx src/index.ts",
    "build": "tsc",
    "start": "node dist/index.js"
  },
  "type": "module",

  ...
}
```

6. 创建示例代码src/index.ts：

```ts
console.log("Hello world!");
```

7. 执行命令：

```
# 直接运行
node run dev
# 先编译
node run build
# 再运行
node run start
```

#### 参考资料

- [TypeScript in a Node.js Project](https://www.robinwieruch.de/typescript-node-js/)