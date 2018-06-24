[TOC]

# HBase Shell

### Shell 通用命令

- status：提供HBase的状态， 例如服务器的数量
- version：提供正在使用HBase版本
- table_help：表引用命令提供帮助
- whoami：提供有关用户的信息

### Shell 数据定义语言（DDL）

列举HBase Shell支持的可以在表中操作的命令

- create：用于创建一个表
- list：用于列出HBase的所有表
- disable：用于禁用表
- is_disabled：用于验证表是否被禁用
- enable：用于启用一个表
- is_enabled：用于验证是否已启用
- describe：用于提供一个表的描述
- alter：用于改变一个表
- exists：用于验证表是否存在
- drop：用于从HBase中删除表
- drop_all：用于丢弃在命令中给出匹配“regex”的表

### Shell 数据操作语言（DML）

- put：用于把指定列在指定的行中单元格的值放入特定的表
- get：用于取行或者单元格的内容
- delete：用于删除表中的单元格的值
- deleteall：用于删除给定行的所有单元格
- scan：用于扫描并返回表数据
- count：用于技术并返回表中的行的数目
- truncate：用于禁用，删除和重新创建一个指定的表

# HBase命名空间

### HBase命名空间

HBase命名空间namespace是与关系数据库系统中的数据库类似的表的逻辑分组，这种抽象为即将出现的多租户相关功能奠定了基础：

- 配额管理（Quota Management）限制命名空间可占用的资源量（即区域，表）
- 命名空间安全管理（Namespace Security Administration）为租户提供另一级别的安全管理
- 区域服务器组（Region server groups）命名空间/表可以固定在RegionServers的子集上，从而保证粗略的隔离级别

### 命名空间管理

可以创建，删除或更改命名空间，通过制定表单的完全限定表名，在创建表时确定命名空间成员权限：

``` sql
<table namespace>:<table qualifier>
```

例子：

``` sql
create_namespace 'my_ns'
create 'my_ns:my_table', 'fam'
drop_namespace 'my_ns'
alter_namespace 'my_ns', {METHOD => 'set', 'PROPERTY_NAME' => 'PROPERTY_VALUE'}
```

### HBase预定义命名空间

在HBase中有两个预定义的特殊命名空间：

- hbase：系统明明空间，用于包含HBase内部表
- default：没有显示指定命名空间的表将自动落入此命名空间

例子：

``` sql
create 'foo:bar', 'fam'
create 'bar', 'fam'
```

# HBase表，行与列族

### 表

HBase中表时在schema定义时被预先声明的，可以使用以下的命令来创建一个表，在这里必须指定**表名** 和**列族名** 。语法如下：

``` sql
create '<table_name>', '<column family>'
```

### HBase行

HBase中的行是逻辑上的行，物理上模型上行是按列族（colomn family）分别存取的，行键是未解释的字节，行是按字母顺序排序的，最低顺序首先出现在表中，空字节数组用于表示表命名空间的开始和结束

### HBase列族

HBase中的列族分组为列族，列族的所有列成员具有相同的前缀。必须在schema定义时提前声明列族，但可以在表启动并运行时动态的变为列

### HBase Cell

由{row key， column(=\<family> + \<label>)， version}唯一确定的单元格。