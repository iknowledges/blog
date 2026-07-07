# Prisma使用教程

## 安装

1. 进入已创建的next.js项目目录，输入下面命令进行安装并初始化：

```
pnpm install -D prisma @prisma/client @prisma/adapter-pg
pnpm dlx prisma init
```

2. 根据[官方文档](https://www.prisma.io/docs/prisma-orm/add-to-existing-project/postgresql#3-connect-your-database)，修改`.env`文件连接PostgreSQL数据库配置：

```
DATABASE_URL="postgresql://user:password@localhost:5432/mydb?schema=public"
```

## ORM

1. 在`schema.prisma`中添加如下model：

```
model UserEntity {
  id        String   @id @default(dbgenerated("gen_random_uuid()")) @db.Uuid
  email     String   @unique(map: "user_email_idx")
  password  String
  createdAt DateTime @default(now()) @db.Timestamp
}
```

2. 执行下面命令：

```
# 生成ts代码
pnpm dlx prisma generate
# 迁移数据库表
pnpm dlx prisma migrate dev --name init
# 启动可视化工具
pnpm dlx prisma studio
```

3. 在Server Component中查询数据：

```ts
import { PrismaClient } from "@/generated/prisma/client";
import { PrismaPg } from "@prisma/adapter-pg";

export async function getUsers() {
  const adapter = new PrismaPg({
    connectionString: process.env.DATABASE_URL,
  });
  const prisma = new PrismaClient({ adapter });
  const users = await prisma.userEntity.findMany();
  return users;
}
```
