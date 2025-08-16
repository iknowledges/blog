# MySQL存储过程

## 存储过程的语法

```sql
-- 声明语句结束符为$$
DELIMITER $$
-- 删除存储过程
DROP PROCEDURE <过程名>
-- 定义变量
DECLARE 变量名 类型
-- 用户变量
@变量名
```

## 示例

- 用户表

```sql
create table `s_user` (
    `id` bigint not null auto_increment,
    `username` varchar(30) not null,
    `age` int not null,
    primary key(`id`)
);
```

- 处理单条结果

```sql
drop procedure if exists get_one_user;

delimiter $$
create procedure get_one_user(in user_id bigint, out user_name varchar(30), out user_age int)
begin 
  declare t_name varchar(30);
  declare t_age int;
  select username, age into t_name, t_age from s_user where id = user_id;
  set user_name = t_name;
  set user_age = t_age;
end $$
delimiter ;

call get_one_user(1, @name, @age);
select @name, @age;
```

- 处理多条结果

```sql
drop procedure if exists get_all_user;

delimiter $$
create procedure get_all_user()
begin
  declare t_name varchar(30);
  declare t_age int;

  -- 定义游标
  declare cur_user cursor for select name, age from s_user;
  -- 标记游标位置
  declare done int default false;
  -- 定义处理器
  declare continue handler for not found set done = true;
  
  open cur_user;
  read_user: loop
    fetch cur_user into t_name, t_age;

    if done then
       -- 结束循环
       leave read_user;
    end if;

    if t_age = 18 then
      select t_name;
    end if;
  end loop;
  close cur_user;
end $$
delimiter ;

call get_all_user();
```
