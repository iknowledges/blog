# Mybatis的XML映射器

## sql

1. 这个元素用来定义可重用的SQL代码片段，以便在include元素中使用。比如：

```xml
<sql id="userColumns"> ${alias}.id,${alias}.username,${alias}.password </sql>

<select id="selectUsers" resultType="map">
  select
    <include refid="userColumns"><property name="alias" value="t1"/></include>,
    <include refid="userColumns"><property name="alias" value="t2"/></include>
  from some_table t1 cross join some_table t2
</select>
```

2. 也可以在include元素的refid属性或内部语句中使用属性值，例如：

```xml
<sql id="sometable">
  ${prefix}Table
</sql>

<sql id="someinclude">
  from
    <include refid="${include_target}"/>
</sql>

<select id="select" resultType="map">
  select
    field1, field2, field3
  <include refid="someinclude">
    <property name="prefix" value="Some"/>
    <property name="include_target" value="sometable"/>
  </include>
</select>
```

## 字符串替换

- `#{}`：MyBatis会创建`PreparedStatement`参数占位符，并通过占位符安全地设置参数（就像使用?一样）。
- `${}`：直接在SQL语句中直接插入一个不转义的字符串。

```java
// ${column}会被直接替换，而#{value}会使用?预处理。
@Select("select * from user where ${column} = #{value}")
User findByColumn(@Param("column") String column, @Param("value") String value);
```

## 结果映射（resultMap）

### id & result

```xml
<id property="id" column="post_id"/>
<result property="subject" column="post_subject"/>
```
id和result元素都是将一个列的值映射到一个简单数据类型的属性。区别的是id元素对应的属性会被标记为对象的标识符，在比较对象实例时使用。这样可以提高整体的性能，尤其是进行缓存和嵌套结果映射的时候。

| 属性 | 描述 |
| --- | --- |
| property | 映射到列结果的字段或属性。 |
| column | 数据库中的列名，或者列的别名。 |
| javaType | 一个Java类的全限定名，或者类型别名。 |
| jdbcType | JDBC类型名 |
| typeHandler | 一个类型处理器实现类的全限定名，或者类型别名。 |

### association

association元素用于处理“has-one”类型的关系。

#### association的嵌套Select查询

| 属性 | 描述 |
| --- | --- |
| column | 数据库中的列名，或者是列的别名。注意：在使用复合主键的时候，可以使用`column="{prop1=col1,prop2=col2}"`这样的语法来指定多个列名。prop1和prop2被设置为对应嵌套Select语句的参数。 |
| select | 用于加载映射语句的ID，它会从column属性指定的列检索数据，作为参数传递给目标select语句。 |
| fetchType | 可选的。有效值为lazy和eager。 |

示例：
```xml
<resultMap id="blogResult" type="Blog">
  <association property="author" column="author_id" javaType="Author" select="selectAuthor"/>
</resultMap>

<select id="selectBlog" resultMap="blogResult">
  SELECT * FROM BLOG WHERE ID = #{id}
</select>

<select id="selectAuthor" resultType="Author">
  SELECT * FROM AUTHOR WHERE ID = #{id}
</select>
```

#### association的嵌套结果映射

| 属性 | 描述 |
| --- | --- |
| resultMap | 结果映射的ID。 |
| columnPrefix | 当连接多个表时，指定列名前缀将带有这些前缀的列映射到一个resultMap中。 |
| notNullColumn | 默认情况下，当至少一个被映射到属性的列不为空时，子对象才会被创建。你可以在这个属性上指定非空的列来改变默认行为。可以使用逗号分隔来指定多个列。默认值：未设置（unset）。 |
| autoMapping | 开启或者关闭自动映射。注意：本属性对外部的结果映射无效，所以不能搭配select或resultMap元素使用。默认值：未设置（unset）。 |

使用外部的resultMap元素来映射关联：
```xml
<select id="selectBlog" resultMap="blogResult">
  select
    B.id            as blog_id,
    B.title         as blog_title,
    B.author_id     as blog_author_id,
    A.id            as author_id,
    A.username      as author_username,
    A.password      as author_password,
    A.email         as author_email,
    A.bio           as author_bio
  from Blog B left outer join Author A on B.author_id = A.id
  where B.id = #{id}
</select>


<resultMap id="blogResult" type="Blog">
  <id property="id" column="blog_id" />
  <result property="title" column="blog_title"/>
  <association property="author" column="blog_author_id" javaType="Author" resultMap="authorResult"/>
</resultMap>

<resultMap id="authorResult" type="Author">
  <id property="id" column="author_id"/>
  <result property="username" column="author_username"/>
  <result property="password" column="author_password"/>
  <result property="email" column="author_email"/>
  <result property="bio" column="author_bio"/>
</resultMap>
```

直接将resultMap作为子元素嵌套在内：
```xml
<resultMap id="blogResult" type="Blog">
  <id property="id" column="blog_id" />
  <result property="title" column="blog_title"/>
  <association property="author" javaType="Author">
    <id property="id" column="author_id"/>
    <result property="username" column="author_username"/>
    <result property="password" column="author_password"/>
    <result property="email" column="author_email"/>
    <result property="bio" column="author_bio"/>
  </association>
</resultMap>
```

指定columnPrefix以便重复使用该结果映射来映射co-author的结果：
```xml
<select id="selectBlog" resultMap="blogResult">
  select
    B.id            as blog_id,
    B.title         as blog_title,
    A.id            as author_id,
    A.username      as author_username,
    A.password      as author_password,
    A.email         as author_email,
    A.bio           as author_bio,
    CA.id           as co_author_id,
    CA.username     as co_author_username,
    CA.password     as co_author_password,
    CA.email        as co_author_email,
    CA.bio          as co_author_bio
  from Blog B
  left outer join Author A on B.author_id = A.id
  left outer join Author CA on B.co_author_id = CA.id
  where B.id = #{id}
</select>


<resultMap id="blogResult" type="Blog">
  <id property="id" column="blog_id" />
  <result property="title" column="blog_title"/>
  <association property="author"
    resultMap="authorResult" />
  <association property="coAuthor"
    resultMap="authorResult"
    columnPrefix="co_" />
</resultMap>
```

### collection

collection元素用于处理“has-many”类型的关系。

在Blog类中可以这样写：
```java
private List<Post> posts;
```

#### collection的嵌套Select查询

```xml
<resultMap id="blogResult" type="Blog">
  <collection property="posts" javaType="ArrayList" column="id" ofType="Post" select="selectPostsForBlog"/>
</resultMap>

<select id="selectBlog" resultMap="blogResult">
  SELECT * FROM BLOG WHERE ID = #{id}
</select>

<select id="selectPostsForBlog" resultType="Post">
  SELECT * FROM POST WHERE BLOG_ID = #{id}
</select>
```

`ofType`属性用来将JavaBean属性的类型和集合存储的类型区分开来。
一般情况下MyBatis可以推断javaType属性，因此可以省略。

#### collection的嵌套结果映射

collection的嵌套结果映射除了新增的`ofType`属性，它和association完全相同。

```xml
<select id="selectBlog" resultMap="blogResult">
  select
  B.id as blog_id,
  B.title as blog_title,
  B.author_id as blog_author_id,
  P.id as post_id,
  P.subject as post_subject,
  P.body as post_body,
  from Blog B
  left outer join Post P on B.id = P.blog_id
  where B.id = #{id}
</select>

<resultMap id="blogResult" type="Blog">
  <id property="id" column="blog_id" />
  <result property="title" column="blog_title"/>
  <collection property="posts" ofType="Post">
    <id property="id" column="post_id"/>
    <result property="subject" column="post_subject"/>
    <result property="body" column="post_body"/>
  </collection>
</resultMap>
```

外部resultMap用法：
```xml
<resultMap id="blogResult" type="Blog">
  <id property="id" column="blog_id" />
  <result property="title" column="blog_title"/>
  <collection property="posts" ofType="Post" resultMap="blogPostResult" columnPrefix="post_"/>
</resultMap>

<resultMap id="blogPostResult" type="Post">
  <id property="id" column="id"/>
  <result property="subject" column="subject"/>
  <result property="body" column="body"/>
</resultMap>
```

## 参考资料

[Mapper XML Files](https://mybatis.org/mybatis-3/sqlmap-xml.html)
