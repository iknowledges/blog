# BigQuery教程

## 安装gcloud

1. 下载安装包，并解压安装：

```
curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-linux-x86_64.tar.gz
tar -xf google-cloud-cli-linux-x86_64.tar.gz
./google-cloud-sdk/install.sh
```

2. 初始化并授权gcloud CLI：

```
gcloud init
```

3. 创建本地认证凭证，否则会出现错误提示`google.auth.exceptions.DefaultCredentialsError: Your default credentials were not found. To set up Application Default Credentials`：

```
gcloud auth application-default login --no-browser
```

## python调用BigQuery

1. 安装依赖：

```
pip install google-cloud-bigquery google-cloud-bigquery-storage
```

2. 编写测试代码：

```python
from google.cloud import bigquery
import polars as pl

client = bigquery.Client()
sql = "SELECT * FROM `project.dataset.table`"
query_job = client.query(sql)

rows = query_job.result()
df = rows.to_dataframe()
print(df.head())

df_pl = pl.from_arrow(query_job.to_arrow())
print(df_pl.tail())
```

#### 参考资料

- [Install the Google Cloud CLI](https://docs.cloud.google.com/sdk/docs/install-sdk)
- [Set up ADC for a local development environment](https://docs.cloud.google.com/docs/authentication/set-up-adc-local-dev-environment)