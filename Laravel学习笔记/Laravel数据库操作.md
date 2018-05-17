Laravel 提供如下三种方式对数据库进行操作：

1. 原生 SQL
2. 查询构造器
3. Eloquent ORM

原生 SQL 这里不做讨论。

# 查询构造器

# Eloquent ORM
Eloquent 模型可以当做查询构造器，可以在 Eloquent 查询中使用查询构造器中的任何方法。

## Eloquent: Relationships
Laravel 提供的多对多、远程一对多、多态关联支持，其实也是为我们在遇到类似需求是如何设计数据库做了一个指引，提供了一种解决方案。

这就是 Laravel 真正强大的地方，作为框架，它不仅仅是把一些东西集合在了一起，更重要的是对开发中的常见的需求都为我们提供了一种解决方案，设计思路。

### 多对多
如，一个用户可能拥有多种身份，而一种身份能同时被多个用户拥有。举例来说，很多用户都拥有「管理员」的身份。要定义这种关联，需要使用三个数据表：users、roles 和 role_user。role_user 表命名是以相关联的两个模型数据表来依照字母顺序命名，并包含了 user_id 和 role_id 字段

这个需要注意的一点是它需要一个中间表。

### 远程一对多
「远层一对多」提供了方便简短的方法来通过中间的关联获取远层的关联。例如，一个 Country 模型可能通过中间的 Users 模型关联到多个 Posts 模型。

### 多态关联
多态关联允许一个模型在单个关联中从属一个以上其它模型。举个例子，想象一下使用你应用的用户可以「评论」文章和视频。使用多态关联关系，您可以使用一个 comments 数据表就可以同时满足两个使用场景。

    posts
        id - integer
        title - string
        body - text

    videos
        id - integer
        title - string
        url - string

    comments
        id - integer
        body - text
        commentable_id - integer
        commentable_type - string

其中， commentable_id 用于存放文章或者视频的 id ，而 commentable_type 用于存放所属模型的类名。注意的是， commentable_type 是当我们访问 commentable 关联时， ORM 用于判断所属的模型是哪个「类型」。

### 多态多对多关联


# 定义、增删查改

## define model
Eloquent 在定义 Model 时在如下方面有一些默认的约定(Convention)：

1. 表名称
2. 主键
    字段名、类型、自增
3. Timestamps
4. 数据库连接配置


分块
游标

动态属性(Dynamic Properties)
    
# 关联
Since all types of Eloquent relationships are defined via functions, you may call those functions to obtain an instance of the relationship without actually executing the relationship queries.

上面这句话很重要，消除了我心中对 relationship 的一些误解.

等值联结 inner join

返回笛卡尔积的连接，又称为 cross join

outer join