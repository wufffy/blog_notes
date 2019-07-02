##MySQL 学习之索引
- 逻辑分类
- 数据结构角度分类(不同的储存引擎使用不同的索引数据结构)
  - B+树  InnoDB
  - hash  
- 物理储存角度
  - 聚簇索引
    - 主键索引,叶子节点上储存的是数据库全部列的数据
  - 二级索引
    - 其他索引,叶子节点上储存的是索引对应主键id的值
- 逻辑角度
  - 普通索引或者单列索引
  - 唯一索引
  - 主键索引
  - 复合索引
- 需要理解的概念
  - 索引覆盖
  - 最左前缀索引
  - 索引下推
- B+ 树这种索引结构 概念
- 如何正确的重建索引


## 常用命令和语句
SHOW INDEX FROM 表名 查询索引的统计信息 https://juejin.im/book/5bffcbc9f265da614b11b731/section/5c061b43f265da612859e3fd


SHOW TABLE STATUS where name='table_name' 表数据的统计信息

SHOW VARIABLES LIKE '%dive%'; 查询MySQL的配置文件的参数

SET GLOBAL innodb_old_blocks_pct = 40; 设置参数,等同于修改配置文件


SET GLOBAL innodb_buffer_pool_size=268435456; 设置缓存大小

### mysql 事务
#### 隔离级别