# Vite中配置CORS

修改`vite.config.js`配置文件：

```js
import { defineConfig } from 'vite'

export default defineConfig({
  server: {
    proxy: {
      // Short hand: forwards localhost:5173/api/users -> http://localhost:8080/api/users
      '/api': 'http://localhost:8080',

      // Detailed config: forwards localhost:5173/v1/users -> http://localhost:8080/users
      '/v1': {
        target: 'http://localhost:8080',
        changeOrigin: true,                // Changes the origin header to match target URL
        rewrite: (path) => path.replace(/^\/v1/, '') // Removes '/v1' from path before sending
      }
    }
  }
})
```