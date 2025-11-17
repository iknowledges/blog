# OpenAI使用教程

```python
from openai import OpenAI
import httpx

proxy_url = "socks5://127.0.0.1:1080"
client = OpenAI(api_key, http_client=httpx.Client(proxy=proxy_url))

response = client.responses.create(
    model="gpt-3.5-turbo",
    input="Write a short bedtime story about a unicorn."
)
print(response.output_text)
```