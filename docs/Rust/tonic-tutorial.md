# Tonic使用教程

## 一、创建项目

1. 新建rust项目

```
cargo new calculator
```

2. 修改Cargo.toml，添加如下依赖

```
[dependencies]
tokio = { version = "1.49.0", features = ["full"] }
tonic = "0.14.5"
prost = "0.14.3"
tonic-prost = "0.14.5"

[build-dependencies]
tonic-prost-build = "0.14.5"
```

## 二、编写proto文件

1. 新建proto/calculator.proto文件，内容如下：

```
syntax = "proto3";

package calculator;

service Calculator {
    rpc Add(CalculationRequest) returns (CalculationResponse);
}

message CalculationRequest {
    int64 a = 1;
    int64 b = 2;
}

message CalculationResponse {
    int64 result = 1;
}
```

2. 在根目录创建build.rs，内容如下：

```rust
fn main() -> Result<(), Box<dyn std::error::Error>> {
    tonic_prost_build::compile_protos("proto/calculator.proto")?;
    Ok(())
}
```

## 三、编写服务端代码

1. 修改Cargo.toml：

```
[[bin]]
name = "server"
path = "src/server.rs"
```

2. 编写src/server.rs：

```rust
use proto::calculator_server::{Calculator, CalculatorServer};
use tonic::transport::Server;

mod proto {
    // calculator is the package name in proto file
    tonic::include_proto!("calculator");
}

#[derive(Debug, Default)]
struct CalculatorService {}

#[tonic::async_trait]
impl Calculator for CalculatorService {
    async fn add(
        &self,
        request: tonic::Request<proto::CalculationRequest>
    ) -> Result<tonic::Response<proto::CalculationResponse>, tonic::Status> {
        println!("Got a request: {:?}", request);
        let input = request.get_ref();
        let response = proto::CalculationResponse {
            result: input.a + input.b,
        };
        Ok(tonic::Response::new(response))
    }
}

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let addr = "0.0.0.0:50051".parse()?;
    let calc = CalculatorService::default();
    Server::builder()
        .add_service(CalculatorServer::new(calc))
        .serve(addr)
        .await?;
    Ok(())
}
```

3. 启动服务端：

```
cargo run --bin server
```

## 四、编写客户端代码

1. 修改Cargo.toml：

```
[[bin]]
name = "client"
path = "src/client.rs"
```

2. 编写src/client.rs：

```rust
use proto::calculator_client::CalculatorClient;

mod proto {
    tonic::include_proto!("calculator");
}

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let url = "http://127.0.0.1:50051";
    let mut client = CalculatorClient::connect(url).await?;

    let req = proto::CalculationRequest {
        a: 1,
        b: 2,
    };
    let request = tonic::Request::new(req);

    let response = client.add(request).await?;

    println!("Response: {:?}", response.get_ref().result);
    Ok(())
}
```

3. 启动客户端：

```
cargo run --bin client
```

#### 参考资料

- [Tonic makes gRPC in Rust stupidly simple](https://youtu.be/kerKXChDmsE)
