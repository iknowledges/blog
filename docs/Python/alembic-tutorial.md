# Alembic教程

## 安装

```
pip install alembic
```

## 使用方法

1. 编写models.py

```python
from sqlalchemy import Integer, Text
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column


class Base(DeclarativeBase):
    pass

class Users(Base):
    __tablename__ = "users"

    id: Mapped[int] = mapped_column(Integer, primary_key=True)
    name: Mapped[str] = mapped_column(Text)
```

2. 初始化Alembic:

```
alembic init alembic
```

3. 修改`alembic.ini`：

```
sqlalchemy.url = postgresql+psycopg://user:password@127.0.0.1:5432/alembic_demo
```

4. 修改`alembic/env.py`，引入`Base`

```python
from models import Base

target_metadata = Base.metadata
```

5. 创建迁移脚本

```
alembic revision --autogenerate -m "create users table"
```

6. 执行迁移

```
alembic upgrade head
```

#### 参考资料

- [Alembic 入门：第一次用 SQLAlchemy 做数据库迁移](https://shio-chan-dev.github.io/jeanblog/zh/dev/python/alembic-intro-sqlalchemy-migrations/)