# OpenSSL教程

```
# 生成私钥
openssl genpkey -algorithm RSA -out private.key
# 生成证书签名请求(CSR)
openssl req -new -key private.key -out csr.pem
# 使用CSR和私钥生成自签名证书
openssl req -x509 -days 365 -key private.key -in csr.pem -out certificate.crt
```

#### 参考资料

- [如何使用 OpenSSL 生成自签名证书？](https://www.ssldragon.com/zh/how-to/openssl/create-self-signed-certificate-openssl/)