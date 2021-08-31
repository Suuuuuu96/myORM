# myORM
myORM是一个Golang ORM框架。本框架的实现，参考了XORM、GORM和GeeORM的开发思路和源代码。在此，我需要先向这些库的开发人员和相关贡献者表达我的敬意。
## 前言
对象关系映射（Object Relational Mapping，简写为ORM），是一种程序设计技术，用于实现面向对象编程语言里不同类型系统的数据之间的转换。对象关系映射，是关系型数据库和程序设计中的对象之间的映射：
* 数据库的表（table） --> 类（class）
* 记录（record，行数据）--> 对象（object）
* 字段（field）--> 对象的属性（attribute）
ORM 使用对象，封装了数据库操作，可以减少SQL语局的使用。开发者只使用面向对象编程，与数据对象直接交互，不用关心底层数据库。
但是，ORM并不能完全取代SQL语句的使用。因为，对于复杂的查询，ORM 要么是无法表达，要么是性能不如原生的 SQL。
Golang并没有自带的ORM框架，比较流行的第三方框架有XORM、GORM等等。参考了XORM、GORM和GeeORM等框架，笔者也开发了一个简单的ORM框架——myORM，目前已具备了常见ORM需要的基础功能。
## 功能
* 对象与表框架的映射：本框架能基于传入对象解析其结构体（struct），在数据库中建立同名的数据表，根据结构体的字段设置同名、对应类型的数据表字段（属性）。
* 记录的插入：本框架能根据传入对象（或对象的切片）在数据表中插入一条（或多条）对应的记录。
* 记录的删除：本框架能接收参数并根据参数设置的条件删除数据表中符合条件的所有记录。
* 记录的修改：本框架能接收参数并根据参数设置的条件更新数据表中符合条件的所有记录。
* 记录的查询：本框架能能查询数据表中符合条件的所有记录并将其追加到指定的结构体切片。
* 钩子：本框架支持用户自定义八种钩子函数，分别位于增删改查四种操作的之前或之后。
* 迁移：结构体成员变更时，对应同名数据库表的字段将自动修改、更新。
* 事务：用户能自定义一系列操作，并将这些操作聚合成一个事务，该事务具备 ACID 四个属性。
## 框架重要概念
* Engine/引擎：用于连接数据库，一个引擎对应一个数据库。
* Session/会话：用于操作数据表（包括建立/删除表格、执行SQL语句、建立事务），一个会话对应一个数据表。一个引擎可以对应多个会话。
* Dialect/方言：不同的关系型数据库管理系统，使用的SQL语句可能有所不同。数据库的数据类型和Golang的数据类型也有差异（Golang的Int、Int8、Int16、Int32对应数据库的integer）。这些所有的差异均由Dialect来处理，之后各种操作均不需要考虑具体语言或数据的差异。一种数据库管理系统，对应一个方言。
* generator/生成器：生成器负责生成SQL的各部分（如"WHERE ..."或"LIMIT ..."）。
* Clause/分句：一个Clause就是一条SQL语句的各部分的集合。
* Field/字段：包含列名、类型和注解。一个字段对应数据库中的一个属性（一列），
* Schema/表框架：即数据表的组织和结构，包含程序中的对应模型、表名、各字段信息、全体列名。一个数据表对应一个表框架。
