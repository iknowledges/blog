# llama-cpp-rs教程

1. 下载模型：

```
hf download hf://unsloth/Qwen3.8-27B-GGUF/Qwen3.8-27B-UD-IQ1_S.gguf
```

2. 安装clang：

```
sudo apt install clang
```

3. 新建rust项目并编译llama-cpp-rs：

```
cargo add llama-cpp-2
cargo build -vv -j 8
```

4. 测试代码：

```rust
use std::{num::NonZeroU32, path::PathBuf};
use llama_cpp_2::{context::params::LlamaContextParams, llama_backend::LlamaBackend, llama_batch::LlamaBatch, model::{AddBos, LlamaChatMessage, LlamaModel, params::LlamaModelParams}, sampling::LlamaSampler};


fn main() -> Result<(), Box<dyn std::error::Error>> {
    let mut backend = LlamaBackend::init()?;
    backend.void_logs();

    let model_params = LlamaModelParams::default()
        .with_main_gpu(0);

    let model_path = PathBuf::from("/home/ubuntu/.cache/huggingface/hub/models--unsloth--Qwen3.8-27B-GGUF/blobs/3895b6eaa91e705c06ad1938d16c22e86f073c6a67df86260a1da79be3d1f887");

    let model = LlamaModel::load_from_file(&backend, &model_path, &model_params)?;

    let chat_template = model.chat_template(None)?;

    let prompt = "What is 2+3? Answer briefly.";

    let messages = [
        LlamaChatMessage::new("system".into(), "You are a mathematics assistant.".into())?,
        LlamaChatMessage::new("user".into(), prompt.into())?,
    ];

    let formatted = model.apply_chat_template(&chat_template, &messages, true)?;
    println!("Input: {}", formatted);

    let prompt_tokens = model.str_to_token(&formatted, AddBos::Always)?;

    let n_ctx = 8192;

    let ctx_params = LlamaContextParams::default()
        .with_n_ctx(NonZeroU32::new(n_ctx))
        .with_n_batch(n_ctx);

    let mut ctx = model.new_context(&backend, ctx_params)?;

    let max_tokens = 512;

    let mut batch = LlamaBatch::new(prompt_tokens.len() + max_tokens, 1);

    for (i, token) in prompt_tokens.iter().enumerate() {
        // llama_decode will output logits only for the last token of the prompt
        let is_last = i == prompt_tokens.len() - 1;
        batch.add(*token, i as i32, &[0], is_last)?;
    }

    ctx.decode(&mut batch)?;

    let mut sampler = LlamaSampler::chain_simple([
        LlamaSampler::temp(0.7),
        LlamaSampler::top_p(0.9, 1),
        LlamaSampler::dist(1234)
    ]);

    let mut output = String::new();
    let mut decoder = encoding_rs::UTF_8.new_decoder();

    for step in 0..max_tokens {
        let pos = (prompt_tokens.len() + step) as i32;
        let token = sampler.sample(&ctx, batch.n_tokens() - 1);

        if model.is_eog_token(token) {
            break;
        }

        let piece = model.token_to_piece(token, &mut decoder, true, None)?;
        output.push_str(&piece);

        batch.clear();
        batch.add(token, pos, &[0], true)?;

        ctx.decode(&mut batch)?;
    }

    println!("Output: {}", output);
    Ok(())
}
```

#### 参考资料

- [llama-cpp-rs](https://github.com/utilityai/llama-cpp-rs)