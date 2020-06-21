#   欢迎关注小诚信驿站,公众号【小诚信驿站】，博客小诚信驿站（wolf_love666）
[toc]

手册：http://support.typora.io/

#  第1章、Mybatis简介

## 1.1、什么是Mybatis？

MyBatis 的前身是 iBatis，其是 Apache 软件基金会下的一个开源项目。2010年该项目从 Apache 基金会迁出，并改名为 MyBatis。同期，iBatis 停止维护。

MyBatis 是一种半自动化的 Java 持久层框架（persistence framework），其通过注解或 XML 的方式将对象和 SQL 关联起来。之所以说它是半自动的，是因为和 Hibernate 等一些可自动生成 SQL 的 ORM(Object Relational Mapping) 框架相比，使用 MyBatis 需要用户自行维护 SQL。维护 SQL 的工作比较繁琐，但也有好处。比如我们可控制 SQL 逻辑，可对其进行优化，以提高效率。

## 1.2、为什么要用Mybatis？

技术之间通常没有高下之分,根据主流的市场会决定你的一些技术栈的选型。

常用的ORM框架有Hibernate和MyBatis，也就是ssh组合和ssm组合中的h与m。

它们的特点和区别如下：
Hibernate对数据库结构提供了完整的封装，实现了POJO对象与数据库表之间的映射，能够自动生成并执行SQL语句。只要定义了POJO 到数据库表的映射关系，就可以通过Hibernate提供的方法完成数据库操作。Hibernate符合JPA规范，就是Java持久层API。

MyBatis通过映射配置文件，将SQL所需的参数和返回的结果字段映射到指定对象，MyBatis不会自动生成SQL，需要自己定义SQL语句，不过更方便对SQL语句进行优化。

总结起来：

1. Hibernate配置要比mybatis复杂的多，学习成本也比MyBatis高。MyBatis，简单、高效、灵活，但是需要自己维护SQL；
2. Hibernate功能强大、全自动、适配不同数据库，但是非常复杂，灵活性稍差。

MyBatis 是一个容易上手的持久层框架，使用者通过简单的学习即可掌握其常用特性的用法。这也是 MyBatis 被广泛使用的一个原因。

## 1.3、为什么要看源码？

仁者见仁，智者见智。对于1-3年的工作人员可能更希望的是面试以及提升自己的认知和思想。对于3-5年工作者，或多或少如果想要挑战更高的角色，资深技术专家或者高级技术专家，会负责架构设计，业务模块设计，技术转型等，指导新人等，则需要对于技术有着深刻的理解。而源码如何设计的架构和思想就会帮助你提升很多，毕竟融入了非常多优秀的人的结晶智慧。

## 1.4、如何阅读源码？(待完善2020-01-21)

1. 粗了解：

   对于开源框架的一些框架组成介绍，看看这个框架有几个模块，各个模块是做什么的，有什么联系，每个模块都有哪些核心类。

   可以看下书，或者找官网看下源码模块，这里我建议直接看源码踏实（书的版本会比源码的旧）。

2. 踩点挖：

   对于某个模块的核心类，debug一遍，画出时序图，对于生命线有个了解，以及调用逻辑。

3. 细整理：

   梳理涉及到的类图和时序图。

4. 常分享：

   可以通过回顾知识，或者排查问题经常过一遍加深自己的理解和掌握。

   

【Tips】时间不充裕的情况下，想要练手：建议先找一个demo,分析运行结果，然后debug进入，在debug过程中要干两件事情，第一画出模块类图结构，第二画出时序图。好处在于debug完后你会清楚的看到这个框架的这块功能是有哪些模块类组成，每个模块类对这个功能的实现提供了哪些能力等等，这对你理解这个功能的设计有一定帮助，另外时序图有助于你了解这个功能的执行逻辑，当你下一次想看这个功能的某些实现时候，你可以根据时序图直接找的你感兴趣的类，直接进入研究

## 1.5、源码前的准备

### 1.5.1、工具

选择合适的IDE，工欲善其事，必先利其器。这里的语言是Java，则推荐jetbrains idea.

选择合适的版本管理工具，虽然有svn，gi t可选，这里推荐gi t。

安装基本的Java和maven环境，这里不再赘述。

### 1.5.2、源码

源码地址：https://github.com/mybatis/mybatis-3

本次的编写基于如下版本：

![image-20200120175752037](/Users/didi/Library/Application Support/typora-user-images/image-20200120175752037.png)

### 1.5.3、idea安装插件

https://plugins.jetbrains.com/plugin/4509-statistic

简要说明下：安装该插件主要是方便自己制定计划进行学习，可以统计代码量，源码量，注释量方便自己排期学习。

## 1.6、小结（todo）

# 第2章、Mybatis框架拆解

## 2.1、架构图

Mybatis作为ORM框架在应用中的角色扮演，以及对于源码包的逻辑执行图。

![image-20200329175517729](/Users/didi/Library/Application Support/typora-user-images/image-20200329175517729.png)

![img](http://dl.iteye.com/upload/attachment/0065/2605/6a59d52b-db76-3737-b164-f366bb2fbce7.png)

![image-20200313191546150](/Users/didi/Library/Application Support/typora-user-images/image-20200313191546150.png)

### 2.1.1、Mybatis主要分为三层

![image-20200313191647083](/Users/didi/Library/Application Support/typora-user-images/image-20200313191647083.png)

#### 2.1.1.1、API接口层源码包

* 提供给外部使用

* 模块介绍

  | 模块                      | 功能说明 | NO   | 核心 |
  | ------------------------- | -------- | ---- | ---- |
  | org.apache.ibatis.session | 会话模块 | 20   | Y    |

#### 2.1.1.2、核心数据处理层源码包

* 实现Mybatis内部流程

* 模块介绍

  | 模块                        | 功能说明        | NO   |
  | --------------------------- | --------------- | ---- |
  | org.apache.ibatis.builder   | 构建器配置解析  | 19   |
  | org.apache.ibatis.cursor    | 结果集接口模块  | 2    |
  | org.apache.ibatis.executor  | SQL执行器       | 9    |
  | org.apache.ibatis.mapping   | SQL mapping映射 | 10   |
  | org.apache.ibatis.plugin    | 插件扩展点模块  | 15   |
  | org.apache.ibatis.scripting | 动态脚本SQL解析 | 11   |
  |                             |                 |      |

#### 2.1.1.3、基础模块层源码包

* 提供通用的模块功能，作为基础支撑

* 模块介绍

  | 模块                          |        功能说明        | NO   |
  | :---------------------------- | :--------------------: | ---- |
  | org.apache.ibatis.annotations |        注解模块        | 17   |
  | org.apache.ibatis.binding     |      注解实现模块      | 16   |
  | org.apache.ibatis.cache       |      缓存实现模块      | 14   |
  | org.apache.ibatis.datasource  | 数据源及数据源工厂模块 | 6    |
  | org.apache.ibatis.exceptions  |      异常定义模块      | 3    |
  | org.apache.ibatis.io          |    资源加载（工具）    | 12   |
  | org.apache.ibatis.jdbc        |      Jdbc工具模块      | 4    |
  | org.apache.ibatis.lang        |      Java版本模块      | 1    |
  | org.apache.ibatis.logging     |      日志功能模块      | 13   |
  | org.apache.ibatis.parsing     |     XML解析器模块      | 18   |
  | org.apache.ibatis.transaction |      事务支持模块      | 7    |
  | org.apache.ibatis.type        |      类型转化模块      | 5    |
  | org.apache.ibatis.reflection  | 类元数据，反射实现模块 | 8    |

![分层图](http://static.iocoder.cn/images/MyBatis/2020_01_04/07.png)

## 2.2、架构流程图(待完善2020-01-20)

![架构流程图](https://segmentfault.com/img/remote/1460000019898938)

## 2.3、核心文件说明

![image-20200313191745449](/Users/didi/Library/Application Support/typora-user-images/image-20200313191745449.png)

1. Mybatis配置文件

   - SqlMapConfig.xml，此文件作为mybatis的全局配置文件，配置了mybatis的运行环境等信息。
   - Mapper.xml，此文件作为mybatis的sql映射文件，文件中配置了操作数据库的sql语句。此文件需要在 SqlMapConfig.xml中加载。

2. SqlSessionFactory

   - 通过mybatis环境等配置信息构造SqlSessionFactory，即会话工厂。

3. SqlSession

   - 通过会话工厂创建sqlSession即会话，程序员通过sqlsession会话接口对数据库进行增删改查操作。

4. Executor执行器

   - mybatis底层自定义了Executor执行器接口来具体操作数据库，Executor接口有两个实现，一个是基本执行器（默认）、一个是缓存执行器，sqlsession底层是通过executor接口操作数据库的。

5. MappedStatement

   - 它也是mybatis一个底层封装对象，它包装了mybatis配置信息及sql映射信息等。mapper.xml文件中一个selectinsertupdatedelete标签对应一个Mapped Statement对象，selectinsertupdatedelete标签的id即是Mapped statement的id。
   - Mapped Statement对sql执行输入参数进行定义，包括HashMap、基本类型、pojo，Executor通过MappedStatement在执行sql前将输入的java对象映射至sql中，输入参数映射就是jdbc编程中对preparedStatement设置参数。
   - Mapped Statement对sql执行输出结果进行定义，包括HashMap、基本类型、pojo，Executor通过MappedStatement在执行sql后将输出结果映射至java对象中，输出结果映射过程相当于jdbc编程中对结果的解析处理过程。

6. Executor
       

           *  MyBatis执行器，是MyBatis 调度的核心，负责SQL语句的生成和查询缓存的维护

7. StatementHandler
       *    封装了JDBC Statement操作，负责对JDBC statement 的操作，如设置参数、将Statement结果集转换成List集合

8. ParameterHandler
       *   负责对用户传递的参数转换成JDBC Statement 所需要的参数

9. ResultSetHandler
       *  负责将JDBC返回的ResultSet结果集对象转换成List类型的集合

10. TypeHandler

       - 负责java数据类型和jdbc数据类型之间的映射和转换

11. SqlSource

       - 负责根据用户传递的parameterObject，动态地生成SQL语句，将信息封装到BoundSql对象中，并返回BoundSql表示动态生成的SQL语句以及相应的参数信息



![image-20200313193136097](/Users/didi/Library/Application Support/typora-user-images/image-20200313193136097.png)

![image-20200313194417503](/Users/didi/Library/Application Support/typora-user-images/image-20200313194417503.png)

![image-20200313202445903](/Users/didi/Library/Application Support/typora-user-images/image-20200313202445903.png)

# 第3章、API接口层

## 3.1、session模块[属于基础对外接口会话模块]

### 3.1.1、文件依赖关系

![image-20200120205105692](/Users/didi/Library/Application Support/typora-user-images/image-20200120205105692.png)

### 3.1.2、AutoMappingUnknownColumnBehavior自动映射未知列行为

```java
/**
 *  指定这个行为 当发现一个自动映射目标的未知列
 * Specify the behavior when detects an unknown column (or unknown property type) of automatic mapping target.
 *
 * @since 3.4.0
 * @author Kazuki Shimizu
 */
public enum AutoMappingUnknownColumnBehavior {

  /**
   * 默认什么也不做
   * Do nothing (Default).
   */
  NONE {
    @Override
    public void doAction(MappedStatement mappedStatement, String columnName, String property, Class<?> propertyType) {
      // do nothing
    }
  },

  /**
   * 输出警告日志
   * Output warning log.
   * Note: The log level of {@code 'org.apache.ibatis.session.AutoMappingUnknownColumnBehavior'} must be set to {@code WARN}.
   */
  WARNING {
    @Override
    public void doAction(MappedStatement mappedStatement, String columnName, String property, Class<?> propertyType) {
      LogHolder.log.warn(buildMessage(mappedStatement, columnName, property, propertyType));
    }
  },

  /**
   * 输出失败映射
   * Fail mapping.
   * Note: throw {@link SqlSessionException}.
   */
  FAILING {
    @Override
    public void doAction(MappedStatement mappedStatement, String columnName, String property, Class<?> propertyType) {
      throw new SqlSessionException(buildMessage(mappedStatement, columnName, property, propertyType));
    }
  };

  /**
   * 执行这个动作 当发现自动映射目标的一个未知列
   * Perform the action when detects an unknown column (or unknown property type) of automatic mapping target.
   * @param mappedStatement current mapped statement
   * @param columnName column name for mapping target
   * @param propertyName property name for mapping target
   * @param propertyType property type for mapping target (If this argument is not null, {@link org.apache.ibatis.type.TypeHandler} for property type is not registered)
     */
  public abstract void doAction(MappedStatement mappedStatement, String columnName, String propertyName, Class<?> propertyType);

  /**
   * 构建错误信息
   * build error message.
   */
  private static String buildMessage(MappedStatement mappedStatement, String columnName, String property, Class<?> propertyType) {
    return new StringBuilder("Unknown column is detected on '")
      .append(mappedStatement.getId())
      .append("' auto-mapping. Mapping parameters are ")
      .append("[")
      .append("columnName=").append(columnName)
      .append(",").append("propertyName=").append(property)
      .append(",").append("propertyType=").append(propertyType != null ? propertyType.getName() : null)
      .append("]")
      .toString();
  }
  //日志持有器
  private static class LogHolder {
    private static final Log log = LogFactory.getLog(AutoMappingUnknownColumnBehavior.class);
  }

}
```

### 3.1.3、AutoMappingBehavior自动映射行为

```java
/**
 * 指定MyBatis应该怎么自动映射列到属性
 * Specifies if and how MyBatis should automatically map columns to fields/properties.
 *
 * @author Eduardo Macarron
 */
public enum AutoMappingBehavior {

  /**
   * 不自动映射
   * Disables auto-mapping.
   */
  NONE,

  /**
   * 没有嵌套结果映射定义的时候自动映射结果
   * Will only auto-map results with no nested result mappings defined inside.
   */
  PARTIAL,

  /**
   * 复杂的自动映射
   * Will auto-map result mappings of any complexity (containing nested or otherwise).
   */
  FULL
}
```

### 3.1.4、ExecutorType执行类型

```java
/**
 * 执行器类型
 * @author Clinton Begin
 */
public enum ExecutorType {
  //简单，重用，批量
  SIMPLE, REUSE, BATCH
}
```

### 3.1.5、LocalCacheScope本地缓存范围

```java
/**
 * 本地缓存范围
 * @author Eduardo Macarron
 */
public enum LocalCacheScope {
  //session缓存一级，会话缓存二级
  SESSION,STATEMENT
}
```

### 3.1.6、TransactionIsolationLevel事务隔离级别

```java
/**
 * 事务隔离级别
 * @author Clinton Begin
 */
public enum TransactionIsolationLevel {
  //无事务隔离
  NONE(Connection.TRANSACTION_NONE),
  //读取已提交
  READ_COMMITTED(Connection.TRANSACTION_READ_COMMITTED),
  //读取未提交
  READ_UNCOMMITTED(Connection.TRANSACTION_READ_UNCOMMITTED),
  //可重复读
  REPEATABLE_READ(Connection.TRANSACTION_REPEATABLE_READ),
  //序列化
  SERIALIZABLE(Connection.TRANSACTION_SERIALIZABLE);
  //级别
  private final int level;
  //级别构造函数
  TransactionIsolationLevel(int level) {
    this.level = level;
  }
  //获取级别
  public int getLevel() {
    return level;
  }
}
```

### 3.1.7、SqlSessionException

```java
/**
 * SQlSession异常继承运行期异常
 * @author Clinton Begin
 */
public class SqlSessionException extends PersistenceException {

  private static final long serialVersionUID = 3833184690240265047L;
  //空构造函数
  public SqlSessionException() {
    super();
  }
  //传递信息的构造函数
  public SqlSessionException(String message) {
    super(message);
  }
  //传递信息和throwable的构造函数
  public SqlSessionException(String message, Throwable cause) {
    super(message, cause);
  }
  //传递throwable的构造函数
  public SqlSessionException(Throwable cause) {
    super(cause);
  }
}
```

### 3.1.8、RowBounds行边界

```java
/**
 * 行边界
 * @author Clinton Begin
 */
public class RowBounds {
  //没有行下标
  public static final int NO_ROW_OFFSET = 0;
  //最大限制行
  public static final int NO_ROW_LIMIT = Integer.MAX_VALUE;
  //默认的行对象
  public static final RowBounds DEFAULT = new RowBounds();
  //行下标
  private final int offset;
  //限制行数
  private final int limit;
  //构造函数
  public RowBounds() {
    this.offset = NO_ROW_OFFSET;
    this.limit = NO_ROW_LIMIT;
  }
  //构造函数
  public RowBounds(int offset, int limit) {
    this.offset = offset;
    this.limit = limit;
  }
  //获取页码
  public int getOffset() {
    return offset;
  }
  //获取每页数量
  public int getLimit() {
    return limit;
  }

}
```

### 3.1.9、ResultContext结果上下文

```java
/**
 * 结果上下文
 * @author Clinton Begin
 */
public interface ResultContext<T> {
  //获取对象结果
  T getResultObject();
  //获取对象条数
  int getResultCount();
  //是否停止了
  boolean isStopped();
  //停止获取
  void stop();

}
```

### 3.1.10、ResultHandler结果处理器

```java
/**
 * 结果处理器
 * @author Clinton Begin
 */
public interface ResultHandler<T> {
  //处理结果上下文
  void handleResult(ResultContext<? extends T> resultContext);

}
```

### 3.1.11、SqlSession会话

```java
/**
 * mybatis工作的主要Java接口。通过这个接口，你可以执行命令，获取mapper和管理事务
 * The primary Java interface for working with MyBatis.
 * Through this interface you can execute commands, get mappers and manage transactions.
 *
 * @author Clinton Begin
 */
public interface SqlSession extends Closeable {

  /**
   * 检索从会话键映射的单行
   * Retrieve a single row mapped from the statement key.
   * @param <T> the returned object type
   * @param statement
   * @return Mapped object
   */
  <T> T selectOne(String statement);

  /**
   * 检索从会话键和参数映射的单行
   * Retrieve a single row mapped from the statement key and parameter.
   * @param <T> the returned object type
   * @param statement Unique identifier matching the statement to use.
   * @param parameter A parameter object to pass to the statement.
   * @return Mapped object
   */
  <T> T selectOne(String statement, Object parameter);

  /**
   * 检索从会话键映射的多行
   * Retrieve a list of mapped objects from the statement key and parameter.
   * @param <E> the returned list element type
   * @param statement Unique identifier matching the statement to use.
   * @return List of mapped object
   */
  <E> List<E> selectList(String statement);

  /**
   * 检索从会话键和参数映射的多行
   * Retrieve a list of mapped objects from the statement key and parameter.
   * @param <E> the returned list element type
   * @param statement Unique identifier matching the statement to use.
   * @param parameter A parameter object to pass to the statement.
   * @return List of mapped object
   */
  <E> List<E> selectList(String statement, Object parameter);

  /**
   * 检索从会话键和参数映射的多行带有分页的
   * Retrieve a list of mapped objects from the statement key and parameter,
   * within the specified row bounds.
   * @param <E> the returned list element type
   * @param statement Unique identifier matching the statement to use.
   * @param parameter A parameter object to pass to the statement.
   * @param rowBounds  Bounds to limit object retrieval
   * @return List of mapped object
   */
  <E> List<E> selectList(String statement, Object parameter, RowBounds rowBounds);

  /**
   * selectMap是一个特殊的场景，被设计转换成list的结果成为map基于在结果对象的属性。举个例子：
   * 返回 Map[Integer,Author] for selectMap("selectAuthors","id")
   * The selectMap is a special case in that it is designed to convert a list
   * of results into a Map based on one of the properties in the resulting
   * objects.
   * Eg. Return a of Map[Integer,Author] for selectMap("selectAuthors","id")
   * @param <K> the returned Map keys type
   * @param <V> the returned Map values type
   * @param statement Unique identifier matching the statement to use.
   * @param mapKey The property to use as key for each value in the list.
   * @return Map containing key pair data.
   */
  <K, V> Map<K, V> selectMap(String statement, String mapKey);

  /**
   * selectMap是一个特殊的场景，被设计转换成list的结果成为map基于在结果对象的属性。
   * The selectMap is a special case in that it is designed to convert a list
   * of results into a Map based on one of the properties in the resulting
   * objects.
   * @param <K> the returned Map keys type
   * @param <V> the returned Map values type
   * @param statement Unique identifier matching the statement to use.
   * @param parameter A parameter object to pass to the statement.
   * @param mapKey The property to use as key for each value in the list.
   * @return Map containing key pair data.
   */
  <K, V> Map<K, V> selectMap(String statement, Object parameter, String mapKey);

  /**
   * selectMap是一个特殊的场景，被设计转换成list的结果成为map基于在结果对象的属性。
   * The selectMap is a special case in that it is designed to convert a list
   * of results into a Map based on one of the properties in the resulting
   * objects.
   * @param <K> the returned Map keys type
   * @param <V> the returned Map values type
   * @param statement Unique identifier matching the statement to use.
   * @param parameter A parameter object to pass to the statement.
   * @param mapKey The property to use as key for each value in the list.
   * @param rowBounds  Bounds to limit object retrieval
   * @return Map containing key pair data.
   */
  <K, V> Map<K, V> selectMap(String statement, Object parameter, String mapKey, RowBounds rowBounds);

  /**
   * 一个游标集的提供相同的结果作为list，除非她使用迭代器懒加载拉取数据
   * A Cursor offers the same results as a List, except it fetches data lazily using an Iterator.
   * @param <T> the returned cursor element type.
   * @param statement Unique identifier matching the statement to use.
   * @return Cursor of mapped objects
   */
  <T> Cursor<T> selectCursor(String statement);

  /**
   * 一个游标集提供相同的结果作为集合，除非她使用迭代器懒加载拉取数据
   * A Cursor offers the same results as a List, except it fetches data lazily using an Iterator.
   * @param <T> the returned cursor element type.
   * @param statement Unique identifier matching the statement to use.
   * @param parameter A parameter object to pass to the statement.
   * @return Cursor of mapped objects
   */
  <T> Cursor<T> selectCursor(String statement, Object parameter);

  /**
   * 一个游标集提供相同的结果作为集合，除非她使用迭代器懒加载拉取数据
   * A Cursor offers the same results as a List, except it fetches data lazily using an Iterator.
   * @param <T> the returned cursor element type.
   * @param statement Unique identifier matching the statement to use.
   * @param parameter A parameter object to pass to the statement.
   * @param rowBounds  Bounds to limit object retrieval
   * @return Cursor of mapped objects
   */
  <T> Cursor<T> selectCursor(String statement, Object parameter, RowBounds rowBounds);

  /**
   * 使用结果处理器 检索单个行映射从会话键和参数中。
   * Retrieve a single row mapped from the statement key and parameter
   * using a {@code ResultHandler}.
   * @param statement Unique identifier matching the statement to use.
   * @param parameter A parameter object to pass to the statement.
   * @param handler ResultHandler that will handle each retrieved row
   */
  void select(String statement, Object parameter, ResultHandler handler);

  /**
   * 从会话使用结果处理器检索单行映射
   * Retrieve a single row mapped from the statement
   * using a {@code ResultHandler}.
   * @param statement Unique identifier matching the statement to use.
   * @param handler ResultHandler that will handle each retrieved row
   */
  void select(String statement, ResultHandler handler);

  /**
   * 使用结果处理器和行边界 检索单行映射从会话键和参数
   * Retrieve a single row mapped from the statement key and parameter
   * using a {@code ResultHandler} and {@code RowBounds}.
   * @param statement Unique identifier matching the statement to use.
   * @param rowBounds RowBound instance to limit the query results
   * @param handler ResultHandler that will handle each retrieved row
   */
  void select(String statement, Object parameter, RowBounds rowBounds, ResultHandler handler);

  /**
   * 执行一个插入会话
   * Execute an insert statement.
   * @param statement Unique identifier matching the statement to execute.
   * @return int The number of rows affected by the insert.
   */
  int insert(String statement);

  /**
   * 执行一个带有参数的插入会话。任何生成自动创建值，或者查询实体对象将修改这个提供的参数对象属性。仅仅返回影响的行数。
   * 如何理解：比如执行插入生成主键的时候，会将主键ID赋值给入参对象的主键ID
   * Execute an insert statement with the given parameter object. Any generated
   * autoincrement values or selectKey entries will modify the given parameter
   * object properties. Only the number of rows affected will be returned.
   * @param statement Unique identifier matching the statement to execute.
   * @param parameter A parameter object to pass to the statement.
   * @return int The number of rows affected by the insert.
   */
  int insert(String statement, Object parameter);

  /**
   * 执行一个更新的会话。返回影响的行数
   * Execute an update statement. The number of rows affected will be returned.
   * @param statement Unique identifier matching the statement to execute.
   * @return int The number of rows affected by the update.
   */
  int update(String statement);

  /**
   * 执行一个更新的会话，将返回影响的更新行数
   * Execute an update statement. The number of rows affected will be returned.
   * @param statement Unique identifier matching the statement to execute.
   * @param parameter A parameter object to pass to the statement.
   * @return int The number of rows affected by the update.
   */
  int update(String statement, Object parameter);

  /**
   * 执行一个删除会话，返回影响的行数
   * Execute a delete statement. The number of rows affected will be returned.
   * @param statement Unique identifier matching the statement to execute.
   * @return int The number of rows affected by the delete.
   */
  int delete(String statement);

  /**
   * 执行删除会话，返回影响的函数
   * Execute a delete statement. The number of rows affected will be returned.
   * @param statement Unique identifier matching the statement to execute.
   * @param parameter A parameter object to pass to the statement.
   * @return int The number of rows affected by the delete.
   */
  int delete(String statement, Object parameter);

  /**
   * 刷新批量会话和提交数据库链接。注意，数据库链接如果没有updates/deletes/inserts调用，不会被提交
   * Flushes batch statements and commits database connection.
   * Note that database connection will not be committed if no updates/deletes/inserts were called.
   * To force the commit call {@link SqlSession#commit(boolean)}
   */
  void commit();

  /**
   * 刷新批量会话和提交数据库链接
   * Flushes batch statements and commits database connection.
   * @param force forces connection commit
   */
  void commit(boolean force);

  /**
   * 丢弃等待的批量会话和回滚数据库链接。注意：数据库链接如果没有updates/deletes/inserts被调用，则不会被回滚。
   * Discards pending batch statements and rolls database connection back.
   * Note that database connection will not be rolled back if no updates/deletes/inserts were called.
   * To force the rollback call {@link SqlSession#rollback(boolean)}
   */
  void rollback();

  /**
   * 丢弃等待的批量会话并且回滚数据库链接
   * Discards pending batch statements and rolls database connection back.
   * Note that database connection will not be rolled back if no updates/deletes/inserts were called.
   * @param force forces connection rollback
   */
  void rollback(boolean force);

  /**
   * 刷新批量会话，返回更新记录的批量结果
   * Flushes batch statements.
   * @return BatchResult list of updated records
   * @since 3.0.6
   */
  List<BatchResult> flushStatements();

  /**
   * 关闭会话
   * Closes the session.
   */
  @Override
  void close();

  /**
   * 清理本地会话缓存
   * Clears local session cache.
   */
  void clearCache();

  /**
   * 检索当前的全局配置
   * Retrieves current configuration.
   * @return Configuration
   */
  Configuration getConfiguration();

  /**
   * 检索一个mapper接口根据类型
   * Retrieves a mapper.
   * @param <T> the mapper type
   * @param type Mapper interface class
   * @return a mapper bound to this SqlSession
   */
  <T> T getMapper(Class<T> type);

  /**
   * 检索内部的数据库链接
   * Retrieves inner database connection.
   * @return Connection
   */
  Connection getConnection();
}
```

### 3.1.12、SqlSessionFactory会话工厂

```java
/**
 * 从连接或数据源创建{@link SqlSession}
 * Creates an {@link SqlSession} out of a connection or a DataSource
 *
 * @author Clinton Begin
 */
public interface SqlSessionFactory {
  //打开会话
  SqlSession openSession();
  //打开会话是否自动提交
  SqlSession openSession(boolean autoCommit);
  //打开链接的session
  SqlSession openSession(Connection connection);
  //打开事务隔离级别的session
  SqlSession openSession(TransactionIsolationLevel level);
  //打开执行器类型的session
  SqlSession openSession(ExecutorType execType);
  //打开执行器类型，是否自动提交的session
  SqlSession openSession(ExecutorType execType, boolean autoCommit);
  //打开执行器和事务隔离级别机制的session
  SqlSession openSession(ExecutorType execType, TransactionIsolationLevel level);
  //打开执行器，链接的session
  SqlSession openSession(ExecutorType execType, Connection connection);
  //获取全局配置
  Configuration getConfiguration();

}
```

### 3.1.13、SqlSessionFactoryBuilder会话工厂构建器

```java
/**
 * 构建{@link SqlSession}实例
 * Builds {@link SqlSession} instances.
 *
 * @author Clinton Begin
 */
public class SqlSessionFactoryBuilder {
  //根据字符流构建会话工厂
  public SqlSessionFactory build(Reader reader) {
    return build(reader, null, null);
  }
  //根据字符流和环境构建会话工厂
  public SqlSessionFactory build(Reader reader, String environment) {
    return build(reader, environment, null);
  }
  //根据字符流和属性构建会话工厂
  public SqlSessionFactory build(Reader reader, Properties properties) {
    return build(reader, null, properties);
  }
  //根据字符流和环境和属性构建会话工厂
  public SqlSessionFactory build(Reader reader, String environment, Properties properties) {
    try {
      //利用xml配置构建器
      XMLConfigBuilder parser = new XMLConfigBuilder(reader, environment, properties);
      return build(parser.parse());
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error building SqlSession.", e);
    } finally {
      ErrorContext.instance().reset();
      try {
        reader.close();
      } catch (IOException e) {
        // Intentionally ignore. Prefer previous error.
      }
    }
  }
  //根据字节流构建会话工厂
  public SqlSessionFactory build(InputStream inputStream) {
    return build(inputStream, null, null);
  }
  //根据字节流和环境构建会话工厂
  public SqlSessionFactory build(InputStream inputStream, String environment) {
    return build(inputStream, environment, null);
  }
  //根据字节流和属性文件构建会话工厂
  public SqlSessionFactory build(InputStream inputStream, Properties properties) {
    return build(inputStream, null, properties);
  }
  //构建会话工厂
  public SqlSessionFactory build(InputStream inputStream, String environment, Properties properties) {
    try {
      //解析字节流和环境和属性文件
      XMLConfigBuilder parser = new XMLConfigBuilder(inputStream, environment, properties);
      return build(parser.parse());
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error building SqlSession.", e);
    } finally {
      ErrorContext.instance().reset();
      try {
        inputStream.close();
      } catch (IOException e) {
        // Intentionally ignore. Prefer previous error.
      }
    }
  }
  //根据全局配置构建会话工厂，走默认的会话工厂
  public SqlSessionFactory build(Configuration config) {
    return new DefaultSqlSessionFactory(config);
  }

}
```

### 3.1.14、SqlSessionManager

```java
/**
 * 会话管理器
 * @author Larry Meadors
 */
public class SqlSessionManager implements SqlSessionFactory, SqlSession {
  //会话工厂
  private final SqlSessionFactory sqlSessionFactory;
  //会话代理
  private final SqlSession sqlSessionProxy;
  //本地会话--利用ThreadLocal 解决共享环境变量会话问题
  private final ThreadLocal<SqlSession> localSqlSession = new ThreadLocal<>();
  //构造函数
  private SqlSessionManager(SqlSessionFactory sqlSessionFactory) {
    this.sqlSessionFactory = sqlSessionFactory;
    this.sqlSessionProxy = (SqlSession) Proxy.newProxyInstance(
        SqlSessionFactory.class.getClassLoader(),
        new Class[]{SqlSession.class},
        new SqlSessionInterceptor());
  }
  //根据字符流构建会话管理器实例
  public static SqlSessionManager newInstance(Reader reader) {
    return new SqlSessionManager(new SqlSessionFactoryBuilder().build(reader, null, null));
  }
  //根据字符流和环境构建会话管理器实例
  public static SqlSessionManager newInstance(Reader reader, String environment) {
    return new SqlSessionManager(new SqlSessionFactoryBuilder().build(reader, environment, null));
  }
  //根据字符流和属性文件构建会话管理器实例
  public static SqlSessionManager newInstance(Reader reader, Properties properties) {
    return new SqlSessionManager(new SqlSessionFactoryBuilder().build(reader, null, properties));
  }
  //根据字节流构建会话管理器实例
  public static SqlSessionManager newInstance(InputStream inputStream) {
    return new SqlSessionManager(new SqlSessionFactoryBuilder().build(inputStream, null, null));
  }
  //根据字节流和环境构建会话管理器实例
  public static SqlSessionManager newInstance(InputStream inputStream, String environment) {
    return new SqlSessionManager(new SqlSessionFactoryBuilder().build(inputStream, environment, null));
  }
  //根据字节流和属性构建会话管理器实例
  public static SqlSessionManager newInstance(InputStream inputStream, Properties properties) {
    return new SqlSessionManager(new SqlSessionFactoryBuilder().build(inputStream, null, properties));
  }
  //根据会话工厂构建会话管理器
  public static SqlSessionManager newInstance(SqlSessionFactory sqlSessionFactory) {
    return new SqlSessionManager(sqlSessionFactory);
  }
  //开启管理会话
  public void startManagedSession() {
    this.localSqlSession.set(openSession());
  }
  //开启自动提交管理会话
  public void startManagedSession(boolean autoCommit) {
    this.localSqlSession.set(openSession(autoCommit));
  }
  //开启链接下的管理会话
  public void startManagedSession(Connection connection) {
    this.localSqlSession.set(openSession(connection));
  }
  //开启事务隔离级别的会话
  public void startManagedSession(TransactionIsolationLevel level) {
    this.localSqlSession.set(openSession(level));
  }
  //开启执行器管理会话
  public void startManagedSession(ExecutorType execType) {
    this.localSqlSession.set(openSession(execType));
  }
  //开启执行器和自动提交的管理会话
  public void startManagedSession(ExecutorType execType, boolean autoCommit) {
    this.localSqlSession.set(openSession(execType, autoCommit));
  }
  //开启执行器类型和事务隔离级别的管理会话
  public void startManagedSession(ExecutorType execType, TransactionIsolationLevel level) {
    this.localSqlSession.set(openSession(execType, level));
  }
  //开启执行器类型和链接的管理会话
  public void startManagedSession(ExecutorType execType, Connection connection) {
    this.localSqlSession.set(openSession(execType, connection));
  }
  //是否开启了管理会话
  public boolean isManagedSessionStarted() {
    return this.localSqlSession.get() != null;
  }
  //打开会话
  @Override
  public SqlSession openSession() {
    return sqlSessionFactory.openSession();
  }
  //打开自动提交的会话
  @Override
  public SqlSession openSession(boolean autoCommit) {
    return sqlSessionFactory.openSession(autoCommit);
  }
  //打开链接的会话
  @Override
  public SqlSession openSession(Connection connection) {
    return sqlSessionFactory.openSession(connection);
  }
  //打开事务隔离级别的会话
  @Override
  public SqlSession openSession(TransactionIsolationLevel level) {
    return sqlSessionFactory.openSession(level);
  }
  //获取执行器类型的会话
  @Override
  public SqlSession openSession(ExecutorType execType) {
    return sqlSessionFactory.openSession(execType);
  }
  //获取执行器类型和自动提交的会话
  @Override
  public SqlSession openSession(ExecutorType execType, boolean autoCommit) {
    return sqlSessionFactory.openSession(execType, autoCommit);
  }
  //获取执行器类型和事务隔离级别的会话
  @Override
  public SqlSession openSession(ExecutorType execType, TransactionIsolationLevel level) {
    return sqlSessionFactory.openSession(execType, level);
  }
  //获取执行器类型和链接的会话
  @Override
  public SqlSession openSession(ExecutorType execType, Connection connection) {
    return sqlSessionFactory.openSession(execType, connection);
  }
  //获取全局额配置
  @Override
  public Configuration getConfiguration() {
    return sqlSessionFactory.getConfiguration();
  }
  //通过代理会话查询
  @Override
  public <T> T selectOne(String statement) {
    return sqlSessionProxy.selectOne(statement);
  }
  //通过代理会话查询
  @Override
  public <T> T selectOne(String statement, Object parameter) {
    return sqlSessionProxy.selectOne(statement, parameter);
  }
  //通过代理会话查询
  @Override
  public <K, V> Map<K, V> selectMap(String statement, String mapKey) {
    return sqlSessionProxy.selectMap(statement, mapKey);
  }
  //通过代理会话查询
  @Override
  public <K, V> Map<K, V> selectMap(String statement, Object parameter, String mapKey) {
    return sqlSessionProxy.selectMap(statement, parameter, mapKey);
  }
  //通过代理会话查询
  @Override
  public <K, V> Map<K, V> selectMap(String statement, Object parameter, String mapKey, RowBounds rowBounds) {
    return sqlSessionProxy.selectMap(statement, parameter, mapKey, rowBounds);
  }
  //通过代理会话查询
  @Override
  public <T> Cursor<T> selectCursor(String statement) {
    return sqlSessionProxy.selectCursor(statement);
  }
  //通过代理会话查询
  @Override
  public <T> Cursor<T> selectCursor(String statement, Object parameter) {
    return sqlSessionProxy.selectCursor(statement, parameter);
  }
  //通过代理会话查询
  @Override
  public <T> Cursor<T> selectCursor(String statement, Object parameter, RowBounds rowBounds) {
    return sqlSessionProxy.selectCursor(statement, parameter, rowBounds);
  }
  //通过代理会话查询
  @Override
  public <E> List<E> selectList(String statement) {
    return sqlSessionProxy.selectList(statement);
  }
  //通过代理会话查询

  @Override
  public <E> List<E> selectList(String statement, Object parameter) {
    return sqlSessionProxy.selectList(statement, parameter);
  }
  //通过代理会话查询

  @Override
  public <E> List<E> selectList(String statement, Object parameter, RowBounds rowBounds) {
    return sqlSessionProxy.selectList(statement, parameter, rowBounds);
  }
  //通过代理会话查询

  @Override
  public void select(String statement, ResultHandler handler) {
    sqlSessionProxy.select(statement, handler);
  }
  //通过代理会话查询

  @Override
  public void select(String statement, Object parameter, ResultHandler handler) {
    sqlSessionProxy.select(statement, parameter, handler);
  }
  //通过代理会话查询

  @Override
  public void select(String statement, Object parameter, RowBounds rowBounds, ResultHandler handler) {
    sqlSessionProxy.select(statement, parameter, rowBounds, handler);
  }
  //通过代理会话插入

  @Override
  public int insert(String statement) {
    return sqlSessionProxy.insert(statement);
  }
  //通过代理会话插入

  @Override
  public int insert(String statement, Object parameter) {
    return sqlSessionProxy.insert(statement, parameter);
  }
  //通过代理会话更新

  @Override
  public int update(String statement) {
    return sqlSessionProxy.update(statement);
  }
  //通过代理会话更新

  @Override
  public int update(String statement, Object parameter) {
    return sqlSessionProxy.update(statement, parameter);
  }
  //通过代理会话删除
  @Override
  public int delete(String statement) {
    return sqlSessionProxy.delete(statement);
  }
  //通过代理会话删除

  @Override
  public int delete(String statement, Object parameter) {
    return sqlSessionProxy.delete(statement, parameter);
  }
  //从全局配置本地的会话中获取链接，再获取对应的mapper接口
  @Override
  public <T> T getMapper(Class<T> type) {
    return getConfiguration().getMapper(type, this);
  }
  //获取链接
  @Override
  public Connection getConnection() {
    final SqlSession sqlSession = localSqlSession.get();
    if (sqlSession == null) {
      throw new SqlSessionException("Error:  Cannot get connection.  No managed session is started.");
    }
    return sqlSession.getConnection();
  }
  //清除缓存
  @Override
  public void clearCache() {
    final SqlSession sqlSession = localSqlSession.get();
    if (sqlSession == null) {
      throw new SqlSessionException("Error:  Cannot clear the cache.  No managed session is started.");
    }
    sqlSession.clearCache();
  }
  //提交会话操作
  @Override
  public void commit() {
    final SqlSession sqlSession = localSqlSession.get();
    if (sqlSession == null) {
      throw new SqlSessionException("Error:  Cannot commit.  No managed session is started.");
    }
    sqlSession.commit();
  }
  //强制提交会话操作
  @Override
  public void commit(boolean force) {
    final SqlSession sqlSession = localSqlSession.get();
    if (sqlSession == null) {
      throw new SqlSessionException("Error:  Cannot commit.  No managed session is started.");
    }
    sqlSession.commit(force);
  }
  //回滚会话
  @Override
  public void rollback() {
    final SqlSession sqlSession = localSqlSession.get();
    if (sqlSession == null) {
      throw new SqlSessionException("Error:  Cannot rollback.  No managed session is started.");
    }
    sqlSession.rollback();
  }
  //强制回滚会话
  @Override
  public void rollback(boolean force) {
    final SqlSession sqlSession = localSqlSession.get();
    if (sqlSession == null) {
      throw new SqlSessionException("Error:  Cannot rollback.  No managed session is started.");
    }
    sqlSession.rollback(force);
  }
  //刷新会话
  @Override
  public List<BatchResult> flushStatements() {
    final SqlSession sqlSession = localSqlSession.get();
    if (sqlSession == null) {
      throw new SqlSessionException("Error:  Cannot rollback.  No managed session is started.");
    }
    return sqlSession.flushStatements();
  }
  //关闭会话
  @Override
  public void close() {
    final SqlSession sqlSession = localSqlSession.get();
    if (sqlSession == null) {
      throw new SqlSessionException("Error:  Cannot close.  No managed session is started.");
    }
    try {
      sqlSession.close();
    } finally {
      localSqlSession.set(null);
    }
  }
  //会话拦截器，没有会话则走默认的会话
  private class SqlSessionInterceptor implements InvocationHandler {
    public SqlSessionInterceptor() {
        // Prevent Synthetic Access
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
      final SqlSession sqlSession = SqlSessionManager.this.localSqlSession.get();
      if (sqlSession != null) {
        try {
          return method.invoke(sqlSession, args);
        } catch (Throwable t) {
          throw ExceptionUtil.unwrapThrowable(t);
        }
      } else {
        try (SqlSession autoSqlSession = openSession()) {
          try {
            final Object result = method.invoke(autoSqlSession, args);
            autoSqlSession.commit();
            return result;
          } catch (Throwable t) {
            autoSqlSession.rollback();
            throw ExceptionUtil.unwrapThrowable(t);
          }
        }
      }
    }
  }

}
```

### 3.1.15、Configuration全局配置

```java
/**
 * 全局配置--mybatis的总管家
 * @author Clinton Begin
 */
public class Configuration {
  //环境对象
  protected Environment environment;
  //是否开启边界
  protected boolean safeRowBoundsEnabled;
  //是否开启结果处理器
  protected boolean safeResultHandlerEnabled = true;
  //是否开启下划线映射到驼峰标示
  protected boolean mapUnderscoreToCamelCase;
  //是否懒加载
  protected boolean aggressiveLazyLoading;
  //是否开启多个结果集
  protected boolean multipleResultSetsEnabled = true;
  //是否启用生成主键策略
  protected boolean useGeneratedKeys;
  //是否使用列标签
  protected boolean useColumnLabel = true;
  //是否开启缓存
  protected boolean cacheEnabled = true;
  //是否开启对空值调用setter
  protected boolean callSettersOnNulls;
  //是否使用实际的参数名字
  protected boolean useActualParamName = true;
  //返回空行实例
  protected boolean returnInstanceForEmptyRow;
  //日志前缀
  protected String logPrefix;
  //日志实现
  protected Class<? extends Log> logImpl;
  //vfs实现
  protected Class<? extends VFS> vfsImpl;
  //本地缓存范围--默认session
  protected LocalCacheScope localCacheScope = LocalCacheScope.SESSION;
  //jdbc类型对null值的处理
  protected JdbcType jdbcTypeForNull = JdbcType.OTHER;
  //懒加载触发方法
  protected Set<String> lazyLoadTriggerMethods = new HashSet<>(Arrays.asList("equals", "clone", "hashCode", "toString"));
  //默认的会话超时
  protected Integer defaultStatementTimeout;
  //默认的拉取数量
  protected Integer defaultFetchSize;
  //默认的结果集类型
  protected ResultSetType defaultResultSetType;
  //默认的执行类型
  protected ExecutorType defaultExecutorType = ExecutorType.SIMPLE;
  //自动映射行为
  protected AutoMappingBehavior autoMappingBehavior = AutoMappingBehavior.PARTIAL;
  //自动映射未知列行为
  protected AutoMappingUnknownColumnBehavior autoMappingUnknownColumnBehavior = AutoMappingUnknownColumnBehavior.NONE;

  //变量
  protected Properties variables = new Properties();
  //反射工厂
  protected ReflectorFactory reflectorFactory = new DefaultReflectorFactory();
  //对象工厂
  protected ObjectFactory objectFactory = new DefaultObjectFactory();
  //对象包装工厂
  protected ObjectWrapperFactory objectWrapperFactory = new DefaultObjectWrapperFactory();
  //是否启动懒加载
  protected boolean lazyLoadingEnabled = false;
  //代理工厂
  protected ProxyFactory proxyFactory = new JavassistProxyFactory(); // #224 Using internal Javassist instead of OGNL
  //数据库ID
  protected String databaseId;
  /**
   * 全局配置工厂类--通常用于创建加载反序列化未读属性的配置
   * Configuration factory class.
   * Used to create Configuration for loading deserialized unread properties.
   *
   * @see <a href='https://code.google.com/p/mybatis/issues/detail?id=300'>Issue 300 (google code)</a>
   */
  protected Class<?> configurationFactory;
  //mapper注册器
  protected final MapperRegistry mapperRegistry = new MapperRegistry(this);
  //拦截器链
  protected final InterceptorChain interceptorChain = new InterceptorChain();
  //类型处理器注册器
  protected final TypeHandlerRegistry typeHandlerRegistry = new TypeHandlerRegistry(this);
  //类型别名注册器
  protected final TypeAliasRegistry typeAliasRegistry = new TypeAliasRegistry();
  //语言驱动注册
  protected final LanguageDriverRegistry languageRegistry = new LanguageDriverRegistry();
  //映射的会话
  protected final Map<String, MappedStatement> mappedStatements = new StrictMap<MappedStatement>("Mapped Statements collection")
      .conflictMessageProducer((savedValue, targetValue) ->
          ". please check " + savedValue.getResource() + " and " + targetValue.getResource());
  //缓存集合
  protected final Map<String, Cache> caches = new StrictMap<>("Caches collection");
  //映射结果集合
  protected final Map<String, ResultMap> resultMaps = new StrictMap<>("Result Maps collection");
  //参数映射集合
  protected final Map<String, ParameterMap> parameterMaps = new StrictMap<>("Parameter Maps collection");
  //主键生成集合
  protected final Map<String, KeyGenerator> keyGenerators = new StrictMap<>("Key Generators collection");
  //加载资源
  protected final Set<String> loadedResources = new HashSet<>();
  //sql片段
  protected final Map<String, XNode> sqlFragments = new StrictMap<>("XML fragments parsed from previous mappers");
  //不完整的会话
  protected final Collection<XMLStatementBuilder> incompleteStatements = new LinkedList<>();
  //不完整的缓存引用
  protected final Collection<CacheRefResolver> incompleteCacheRefs = new LinkedList<>();
  //不完整的结果映射集合
  protected final Collection<ResultMapResolver> incompleteResultMaps = new LinkedList<>();
  //不完整的方法集合
  protected final Collection<MethodResolver> incompleteMethods = new LinkedList<>();

  /*
  持有缓存引用的map，这个key是引用缓存绑定另外一个命名空间和实际缓存绑定的命名空间的值的命名空间
   * A map holds cache-ref relationship. The key is the namespace that
   * references a cache bound to another namespace and the value is the
   * namespace which the actual cache is bound to.
   */
  protected final Map<String, String> cacheRefMap = new HashMap<>();
  //构造函数
  public Configuration(Environment environment) {
    this();
    this.environment = environment;
  }
  //全局配置
  public Configuration() {
    //类型别名注册器
    typeAliasRegistry.registerAlias("JDBC", JdbcTransactionFactory.class);
    typeAliasRegistry.registerAlias("MANAGED", ManagedTransactionFactory.class);

    typeAliasRegistry.registerAlias("JNDI", JndiDataSourceFactory.class);
    typeAliasRegistry.registerAlias("POOLED", PooledDataSourceFactory.class);
    typeAliasRegistry.registerAlias("UNPOOLED", UnpooledDataSourceFactory.class);

    typeAliasRegistry.registerAlias("PERPETUAL", PerpetualCache.class);
    typeAliasRegistry.registerAlias("FIFO", FifoCache.class);
    typeAliasRegistry.registerAlias("LRU", LruCache.class);
    typeAliasRegistry.registerAlias("SOFT", SoftCache.class);
    typeAliasRegistry.registerAlias("WEAK", WeakCache.class);

    typeAliasRegistry.registerAlias("DB_VENDOR", VendorDatabaseIdProvider.class);

    typeAliasRegistry.registerAlias("XML", XMLLanguageDriver.class);
    typeAliasRegistry.registerAlias("RAW", RawLanguageDriver.class);

    typeAliasRegistry.registerAlias("SLF4J", Slf4jImpl.class);
    typeAliasRegistry.registerAlias("COMMONS_LOGGING", JakartaCommonsLoggingImpl.class);
    typeAliasRegistry.registerAlias("LOG4J", Log4jImpl.class);
    typeAliasRegistry.registerAlias("LOG4J2", Log4j2Impl.class);
    typeAliasRegistry.registerAlias("JDK_LOGGING", Jdk14LoggingImpl.class);
    typeAliasRegistry.registerAlias("STDOUT_LOGGING", StdOutImpl.class);
    typeAliasRegistry.registerAlias("NO_LOGGING", NoLoggingImpl.class);

    typeAliasRegistry.registerAlias("CGLIB", CglibProxyFactory.class);
    typeAliasRegistry.registerAlias("JAVASSIST", JavassistProxyFactory.class);

    languageRegistry.setDefaultDriverClass(XMLLanguageDriver.class);
    languageRegistry.register(RawLanguageDriver.class);
  }
  //获取日志前缀
  public String getLogPrefix() {
    return logPrefix;
  }
  //设置日志前缀
  public void setLogPrefix(String logPrefix) {
    this.logPrefix = logPrefix;
  }
  //获取日志实现类
  public Class<? extends Log> getLogImpl() {
    return logImpl;
  }
  //设置日志实现类
  public void setLogImpl(Class<? extends Log> logImpl) {
    if (logImpl != null) {
      this.logImpl = logImpl;
      LogFactory.useCustomLogging(this.logImpl);
    }
  }
  //获取vfs实现类
  public Class<? extends VFS> getVfsImpl() {
    return this.vfsImpl;
  }
  //设置vfs实现类
  public void setVfsImpl(Class<? extends VFS> vfsImpl) {
    if (vfsImpl != null) {
      this.vfsImpl = vfsImpl;
      VFS.addImplClass(this.vfsImpl);
    }
  }
  //
  public boolean isCallSettersOnNulls() {
    return callSettersOnNulls;
  }

  public void setCallSettersOnNulls(boolean callSettersOnNulls) {
    this.callSettersOnNulls = callSettersOnNulls;
  }

  public boolean isUseActualParamName() {
    return useActualParamName;
  }

  public void setUseActualParamName(boolean useActualParamName) {
    this.useActualParamName = useActualParamName;
  }

  public boolean isReturnInstanceForEmptyRow() {
    return returnInstanceForEmptyRow;
  }

  public void setReturnInstanceForEmptyRow(boolean returnEmptyInstance) {
    this.returnInstanceForEmptyRow = returnEmptyInstance;
  }

  public String getDatabaseId() {
    return databaseId;
  }

  public void setDatabaseId(String databaseId) {
    this.databaseId = databaseId;
  }

  public Class<?> getConfigurationFactory() {
    return configurationFactory;
  }

  public void setConfigurationFactory(Class<?> configurationFactory) {
    this.configurationFactory = configurationFactory;
  }

  public boolean isSafeResultHandlerEnabled() {
    return safeResultHandlerEnabled;
  }

  public void setSafeResultHandlerEnabled(boolean safeResultHandlerEnabled) {
    this.safeResultHandlerEnabled = safeResultHandlerEnabled;
  }

  public boolean isSafeRowBoundsEnabled() {
    return safeRowBoundsEnabled;
  }

  public void setSafeRowBoundsEnabled(boolean safeRowBoundsEnabled) {
    this.safeRowBoundsEnabled = safeRowBoundsEnabled;
  }

  public boolean isMapUnderscoreToCamelCase() {
    return mapUnderscoreToCamelCase;
  }

  public void setMapUnderscoreToCamelCase(boolean mapUnderscoreToCamelCase) {
    this.mapUnderscoreToCamelCase = mapUnderscoreToCamelCase;
  }

  public void addLoadedResource(String resource) {
    loadedResources.add(resource);
  }

  public boolean isResourceLoaded(String resource) {
    return loadedResources.contains(resource);
  }

  public Environment getEnvironment() {
    return environment;
  }

  public void setEnvironment(Environment environment) {
    this.environment = environment;
  }

  public AutoMappingBehavior getAutoMappingBehavior() {
    return autoMappingBehavior;
  }

  public void setAutoMappingBehavior(AutoMappingBehavior autoMappingBehavior) {
    this.autoMappingBehavior = autoMappingBehavior;
  }

  /**
   *
   * @since 3.4.0
   */
  public AutoMappingUnknownColumnBehavior getAutoMappingUnknownColumnBehavior() {
    return autoMappingUnknownColumnBehavior;
  }

  /**
   * @since 3.4.0
   */
  public void setAutoMappingUnknownColumnBehavior(AutoMappingUnknownColumnBehavior autoMappingUnknownColumnBehavior) {
    this.autoMappingUnknownColumnBehavior = autoMappingUnknownColumnBehavior;
  }

  public boolean isLazyLoadingEnabled() {
    return lazyLoadingEnabled;
  }

  public void setLazyLoadingEnabled(boolean lazyLoadingEnabled) {
    this.lazyLoadingEnabled = lazyLoadingEnabled;
  }

  public ProxyFactory getProxyFactory() {
    return proxyFactory;
  }

  public void setProxyFactory(ProxyFactory proxyFactory) {
    if (proxyFactory == null) {
      proxyFactory = new JavassistProxyFactory();
    }
    this.proxyFactory = proxyFactory;
  }

  public boolean isAggressiveLazyLoading() {
    return aggressiveLazyLoading;
  }

  public void setAggressiveLazyLoading(boolean aggressiveLazyLoading) {
    this.aggressiveLazyLoading = aggressiveLazyLoading;
  }

  public boolean isMultipleResultSetsEnabled() {
    return multipleResultSetsEnabled;
  }

  public void setMultipleResultSetsEnabled(boolean multipleResultSetsEnabled) {
    this.multipleResultSetsEnabled = multipleResultSetsEnabled;
  }

  public Set<String> getLazyLoadTriggerMethods() {
    return lazyLoadTriggerMethods;
  }

  public void setLazyLoadTriggerMethods(Set<String> lazyLoadTriggerMethods) {
    this.lazyLoadTriggerMethods = lazyLoadTriggerMethods;
  }

  public boolean isUseGeneratedKeys() {
    return useGeneratedKeys;
  }

  public void setUseGeneratedKeys(boolean useGeneratedKeys) {
    this.useGeneratedKeys = useGeneratedKeys;
  }

  public ExecutorType getDefaultExecutorType() {
    return defaultExecutorType;
  }

  public void setDefaultExecutorType(ExecutorType defaultExecutorType) {
    this.defaultExecutorType = defaultExecutorType;
  }

  public boolean isCacheEnabled() {
    return cacheEnabled;
  }

  public void setCacheEnabled(boolean cacheEnabled) {
    this.cacheEnabled = cacheEnabled;
  }

  public Integer getDefaultStatementTimeout() {
    return defaultStatementTimeout;
  }

  public void setDefaultStatementTimeout(Integer defaultStatementTimeout) {
    this.defaultStatementTimeout = defaultStatementTimeout;
  }

  /**
   * @since 3.3.0
   */
  public Integer getDefaultFetchSize() {
    return defaultFetchSize;
  }

  /**
   * @since 3.3.0
   */
  public void setDefaultFetchSize(Integer defaultFetchSize) {
    this.defaultFetchSize = defaultFetchSize;
  }

  /**
   * @since 3.5.2
   */
  public ResultSetType getDefaultResultSetType() {
    return defaultResultSetType;
  }

  /**
   * @since 3.5.2
   */
  public void setDefaultResultSetType(ResultSetType defaultResultSetType) {
    this.defaultResultSetType = defaultResultSetType;
  }

  public boolean isUseColumnLabel() {
    return useColumnLabel;
  }

  public void setUseColumnLabel(boolean useColumnLabel) {
    this.useColumnLabel = useColumnLabel;
  }

  public LocalCacheScope getLocalCacheScope() {
    return localCacheScope;
  }

  public void setLocalCacheScope(LocalCacheScope localCacheScope) {
    this.localCacheScope = localCacheScope;
  }

  public JdbcType getJdbcTypeForNull() {
    return jdbcTypeForNull;
  }

  public void setJdbcTypeForNull(JdbcType jdbcTypeForNull) {
    this.jdbcTypeForNull = jdbcTypeForNull;
  }

  public Properties getVariables() {
    return variables;
  }

  public void setVariables(Properties variables) {
    this.variables = variables;
  }

  public TypeHandlerRegistry getTypeHandlerRegistry() {
    return typeHandlerRegistry;
  }

  /**
   * Set a default {@link TypeHandler} class for {@link Enum}.
   * A default {@link TypeHandler} is {@link org.apache.ibatis.type.EnumTypeHandler}.
   * @param typeHandler a type handler class for {@link Enum}
   * @since 3.4.5
   */
  public void setDefaultEnumTypeHandler(Class<? extends TypeHandler> typeHandler) {
    if (typeHandler != null) {
      getTypeHandlerRegistry().setDefaultEnumTypeHandler(typeHandler);
    }
  }

  public TypeAliasRegistry getTypeAliasRegistry() {
    return typeAliasRegistry;
  }

  /**
   * @since 3.2.2
   */
  public MapperRegistry getMapperRegistry() {
    return mapperRegistry;
  }

  public ReflectorFactory getReflectorFactory() {
    return reflectorFactory;
  }

  public void setReflectorFactory(ReflectorFactory reflectorFactory) {
    this.reflectorFactory = reflectorFactory;
  }

  public ObjectFactory getObjectFactory() {
    return objectFactory;
  }

  public void setObjectFactory(ObjectFactory objectFactory) {
    this.objectFactory = objectFactory;
  }

  public ObjectWrapperFactory getObjectWrapperFactory() {
    return objectWrapperFactory;
  }

  public void setObjectWrapperFactory(ObjectWrapperFactory objectWrapperFactory) {
    this.objectWrapperFactory = objectWrapperFactory;
  }

  /**
   * @since 3.2.2
   */
  public List<Interceptor> getInterceptors() {
    return interceptorChain.getInterceptors();
  }

  public LanguageDriverRegistry getLanguageRegistry() {
    return languageRegistry;
  }

  public void setDefaultScriptingLanguage(Class<? extends LanguageDriver> driver) {
    if (driver == null) {
      driver = XMLLanguageDriver.class;
    }
    getLanguageRegistry().setDefaultDriverClass(driver);
  }

  public LanguageDriver getDefaultScriptingLanguageInstance() {
    return languageRegistry.getDefaultDriver();
  }

  /**
   * @since 3.5.1
   */
  public LanguageDriver getLanguageDriver(Class<? extends LanguageDriver> langClass) {
    if (langClass == null) {
      return languageRegistry.getDefaultDriver();
    }
    languageRegistry.register(langClass);
    return languageRegistry.getDriver(langClass);
  }

  /**
   * @deprecated Use {@link #getDefaultScriptingLanguageInstance()}
   */
  @Deprecated
  public LanguageDriver getDefaultScriptingLanuageInstance() {
    return getDefaultScriptingLanguageInstance();
  }

  public MetaObject newMetaObject(Object object) {
    return MetaObject.forObject(object, objectFactory, objectWrapperFactory, reflectorFactory);
  }

  public ParameterHandler newParameterHandler(MappedStatement mappedStatement, Object parameterObject, BoundSql boundSql) {
    ParameterHandler parameterHandler = mappedStatement.getLang().createParameterHandler(mappedStatement, parameterObject, boundSql);
    parameterHandler = (ParameterHandler) interceptorChain.pluginAll(parameterHandler);
    return parameterHandler;
  }

  public ResultSetHandler newResultSetHandler(Executor executor, MappedStatement mappedStatement, RowBounds rowBounds, ParameterHandler parameterHandler,
      ResultHandler resultHandler, BoundSql boundSql) {
    ResultSetHandler resultSetHandler = new DefaultResultSetHandler(executor, mappedStatement, parameterHandler, resultHandler, boundSql, rowBounds);
    resultSetHandler = (ResultSetHandler) interceptorChain.pluginAll(resultSetHandler);
    return resultSetHandler;
  }

  public StatementHandler newStatementHandler(Executor executor, MappedStatement mappedStatement, Object parameterObject, RowBounds rowBounds, ResultHandler resultHandler, BoundSql boundSql) {
    StatementHandler statementHandler = new RoutingStatementHandler(executor, mappedStatement, parameterObject, rowBounds, resultHandler, boundSql);
    statementHandler = (StatementHandler) interceptorChain.pluginAll(statementHandler);
    return statementHandler;
  }

  public Executor newExecutor(Transaction transaction) {
    return newExecutor(transaction, defaultExecutorType);
  }

  public Executor newExecutor(Transaction transaction, ExecutorType executorType) {
    executorType = executorType == null ? defaultExecutorType : executorType;
    executorType = executorType == null ? ExecutorType.SIMPLE : executorType;
    Executor executor;
    if (ExecutorType.BATCH == executorType) {
      executor = new BatchExecutor(this, transaction);
    } else if (ExecutorType.REUSE == executorType) {
      executor = new ReuseExecutor(this, transaction);
    } else {
      executor = new SimpleExecutor(this, transaction);
    }
    if (cacheEnabled) {
      executor = new CachingExecutor(executor);
    }
    executor = (Executor) interceptorChain.pluginAll(executor);
    return executor;
  }

  public void addKeyGenerator(String id, KeyGenerator keyGenerator) {
    keyGenerators.put(id, keyGenerator);
  }

  public Collection<String> getKeyGeneratorNames() {
    return keyGenerators.keySet();
  }

  public Collection<KeyGenerator> getKeyGenerators() {
    return keyGenerators.values();
  }

  public KeyGenerator getKeyGenerator(String id) {
    return keyGenerators.get(id);
  }

  public boolean hasKeyGenerator(String id) {
    return keyGenerators.containsKey(id);
  }

  public void addCache(Cache cache) {
    caches.put(cache.getId(), cache);
  }

  public Collection<String> getCacheNames() {
    return caches.keySet();
  }

  public Collection<Cache> getCaches() {
    return caches.values();
  }

  public Cache getCache(String id) {
    return caches.get(id);
  }

  public boolean hasCache(String id) {
    return caches.containsKey(id);
  }

  public void addResultMap(ResultMap rm) {
    resultMaps.put(rm.getId(), rm);
    checkLocallyForDiscriminatedNestedResultMaps(rm);
    checkGloballyForDiscriminatedNestedResultMaps(rm);
  }

  public Collection<String> getResultMapNames() {
    return resultMaps.keySet();
  }

  public Collection<ResultMap> getResultMaps() {
    return resultMaps.values();
  }

  public ResultMap getResultMap(String id) {
    return resultMaps.get(id);
  }

  public boolean hasResultMap(String id) {
    return resultMaps.containsKey(id);
  }

  public void addParameterMap(ParameterMap pm) {
    parameterMaps.put(pm.getId(), pm);
  }

  public Collection<String> getParameterMapNames() {
    return parameterMaps.keySet();
  }

  public Collection<ParameterMap> getParameterMaps() {
    return parameterMaps.values();
  }

  public ParameterMap getParameterMap(String id) {
    return parameterMaps.get(id);
  }

  public boolean hasParameterMap(String id) {
    return parameterMaps.containsKey(id);
  }

  public void addMappedStatement(MappedStatement ms) {
    mappedStatements.put(ms.getId(), ms);
  }

  public Collection<String> getMappedStatementNames() {
    buildAllStatements();
    return mappedStatements.keySet();
  }

  public Collection<MappedStatement> getMappedStatements() {
    buildAllStatements();
    return mappedStatements.values();
  }

  public Collection<XMLStatementBuilder> getIncompleteStatements() {
    return incompleteStatements;
  }

  public void addIncompleteStatement(XMLStatementBuilder incompleteStatement) {
    incompleteStatements.add(incompleteStatement);
  }

  public Collection<CacheRefResolver> getIncompleteCacheRefs() {
    return incompleteCacheRefs;
  }

  public void addIncompleteCacheRef(CacheRefResolver incompleteCacheRef) {
    incompleteCacheRefs.add(incompleteCacheRef);
  }

  public Collection<ResultMapResolver> getIncompleteResultMaps() {
    return incompleteResultMaps;
  }

  public void addIncompleteResultMap(ResultMapResolver resultMapResolver) {
    incompleteResultMaps.add(resultMapResolver);
  }

  public void addIncompleteMethod(MethodResolver builder) {
    incompleteMethods.add(builder);
  }

  public Collection<MethodResolver> getIncompleteMethods() {
    return incompleteMethods;
  }

  public MappedStatement getMappedStatement(String id) {
    return this.getMappedStatement(id, true);
  }

  public MappedStatement getMappedStatement(String id, boolean validateIncompleteStatements) {
    if (validateIncompleteStatements) {
      buildAllStatements();
    }
    return mappedStatements.get(id);
  }

  public Map<String, XNode> getSqlFragments() {
    return sqlFragments;
  }

  public void addInterceptor(Interceptor interceptor) {
    interceptorChain.addInterceptor(interceptor);
  }

  public void addMappers(String packageName, Class<?> superType) {
    mapperRegistry.addMappers(packageName, superType);
  }

  public void addMappers(String packageName) {
    mapperRegistry.addMappers(packageName);
  }

  public <T> void addMapper(Class<T> type) {
    mapperRegistry.addMapper(type);
  }

  public <T> T getMapper(Class<T> type, SqlSession sqlSession) {
    return mapperRegistry.getMapper(type, sqlSession);
  }

  public boolean hasMapper(Class<?> type) {
    return mapperRegistry.hasMapper(type);
  }

  public boolean hasStatement(String statementName) {
    return hasStatement(statementName, true);
  }

  public boolean hasStatement(String statementName, boolean validateIncompleteStatements) {
    if (validateIncompleteStatements) {
      buildAllStatements();
    }
    return mappedStatements.containsKey(statementName);
  }

  public void addCacheRef(String namespace, String referencedNamespace) {
    cacheRefMap.put(namespace, referencedNamespace);
  }

  /*
    解析在缓存中的所有的未进行的会话节点。当她提供快速会话校验的时候 被推荐使用调用这个方法一次所有的mapper被加载，
   * Parses all the unprocessed statement nodes in the cache. It is recommended
   * to call this method once all the mappers are added as it provides fail-fast
   * statement validation.
   */
  protected void buildAllStatements() {
    parsePendingResultMaps();
    if (!incompleteCacheRefs.isEmpty()) {
      synchronized (incompleteCacheRefs) {
        incompleteCacheRefs.removeIf(x -> x.resolveCacheRef() != null);
      }
    }
    if (!incompleteStatements.isEmpty()) {
      synchronized (incompleteStatements) {
        incompleteStatements.removeIf(x -> {
          x.parseStatementNode();
          return true;
        });
      }
    }
    if (!incompleteMethods.isEmpty()) {
      synchronized (incompleteMethods) {
        incompleteMethods.removeIf(x -> {
          x.resolve();
          return true;
        });
      }
    }
  }
  //解析等待的结果map集
  private void parsePendingResultMaps() {
    if (incompleteResultMaps.isEmpty()) {
      return;
    }
    synchronized (incompleteResultMaps) {
      boolean resolved;
      IncompleteElementException ex = null;
      do {
        resolved = false;
        Iterator<ResultMapResolver> iterator = incompleteResultMaps.iterator();
        while (iterator.hasNext()) {
          try {
            iterator.next().resolve();
            iterator.remove();
            resolved = true;
          } catch (IncompleteElementException e) {
            ex = e;
          }
        }
      } while (resolved);
      if (!incompleteResultMaps.isEmpty() && ex != null) {
        // At least one result map is unresolvable.
        throw ex;
      }
    }
  }

  /**
   * 提取命名空间从全指定的会话ID
   * Extracts namespace from fully qualified statement id.
   *
   * @param statementId
   * @return namespace or null when id does not contain period.
   */
  protected String extractNamespace(String statementId) {
    int lastPeriod = statementId.lastIndexOf('.');
    return lastPeriod > 0 ? statementId.substring(0, lastPeriod) : null;
  }
  // 慢，但一次性的时间花费。期待更好的方法
  // Slow but a one time cost. A better solution is welcome.
  protected void checkGloballyForDiscriminatedNestedResultMaps(ResultMap rm) {
    if (rm.hasNestedResultMaps()) {
      for (Map.Entry<String, ResultMap> entry : resultMaps.entrySet()) {
        Object value = entry.getValue();
        if (value instanceof ResultMap) {
          ResultMap entryResultMap = (ResultMap) value;
          if (!entryResultMap.hasNestedResultMaps() && entryResultMap.getDiscriminator() != null) {
            Collection<String> discriminatedResultMapNames = entryResultMap.getDiscriminator().getDiscriminatorMap().values();
            if (discriminatedResultMapNames.contains(rm.getId())) {
              entryResultMap.forceNestedResultMaps();
            }
          }
        }
      }
    }
  }
  // 慢，但一次性的时间花费。期待更好的方法
  // Slow but a one time cost. A better solution is welcome.
  protected void checkLocallyForDiscriminatedNestedResultMaps(ResultMap rm) {
    if (!rm.hasNestedResultMaps() && rm.getDiscriminator() != null) {
      for (Map.Entry<String, String> entry : rm.getDiscriminator().getDiscriminatorMap().entrySet()) {
        String discriminatedResultMapName = entry.getValue();
        if (hasResultMap(discriminatedResultMapName)) {
          ResultMap discriminatedResultMap = resultMaps.get(discriminatedResultMapName);
          if (discriminatedResultMap.hasNestedResultMaps()) {
            rm.forceNestedResultMaps();
            break;
          }
        }
      }
    }
  }
  //严格的map内部实现类
  protected static class StrictMap<V> extends HashMap<String, V> {

    private static final long serialVersionUID = -4950446264854982944L;
    private final String name;
    private BiFunction<V, V, String> conflictMessageProducer;

    public StrictMap(String name, int initialCapacity, float loadFactor) {
      super(initialCapacity, loadFactor);
      this.name = name;
    }

    public StrictMap(String name, int initialCapacity) {
      super(initialCapacity);
      this.name = name;
    }

    public StrictMap(String name) {
      super();
      this.name = name;
    }

    public StrictMap(String name, Map<String, ? extends V> m) {
      super(m);
      this.name = name;
    }

    /**
     * 分配一个函数 产生一个冲突错误信息当包含相同key和值
     * Assign a function for producing a conflict error message when contains value with the same key.
     * <p>
     * function arguments are 1st is saved value and 2nd is target value.
     * @param conflictMessageProducer A function for producing a conflict error message
     * @return a conflict error message
     * @since 3.5.0
     */
    public StrictMap<V> conflictMessageProducer(BiFunction<V, V, String> conflictMessageProducer) {
      this.conflictMessageProducer = conflictMessageProducer;
      return this;
    }
   //存放属性和对象
    @Override
    @SuppressWarnings("unchecked")
    public V put(String key, V value) {
      if (containsKey(key)) {
        throw new IllegalArgumentException(name + " already contains value for " + key
            + (conflictMessageProducer == null ? "" : conflictMessageProducer.apply(super.get(key), value)));
      }
      if (key.contains(".")) {
        final String shortKey = getShortName(key);
        if (super.get(shortKey) == null) {
          super.put(shortKey, value);
        } else {
          super.put(shortKey, (V) new Ambiguity(shortKey));
        }
      }
      return super.put(key, value);
    }
   //获取全局配置的属性对应的值
    @Override
    public V get(Object key) {
      V value = super.get(key);
      if (value == null) {
        throw new IllegalArgumentException(name + " does not contain value for " + key);
      }
      if (value instanceof Ambiguity) {
        throw new IllegalArgumentException(((Ambiguity) value).getSubject() + " is ambiguous in " + name
            + " (try using the full name including the namespace, or rename one of the entries)");
      }
      return value;
    }
  //内部语义不明的类
    protected static class Ambiguity {
      final private String subject;

      public Ambiguity(String subject) {
        this.subject = subject;
      }

      public String getSubject() {
        return subject;
      }
    }

    private String getShortName(String key) {
      final String[] keyParts = key.split("\\.");
      return keyParts[keyParts.length - 1];
    }
  }

}
```

### 3.1.16、DefaultSqlSessionFactory默认的会话工厂

```java
/**
 * 默认的会话工厂
 * @author Clinton Begin
 */
public class DefaultSqlSessionFactory implements SqlSessionFactory {
  //全局配置
  private final Configuration configuration;
  //构造函数
  public DefaultSqlSessionFactory(Configuration configuration) {
    this.configuration = configuration;
  }
  //打开会话从数据源
  @Override
  public SqlSession openSession() {
    return openSessionFromDataSource(configuration.getDefaultExecutorType(), null, false);
  }
  //打开从数据源是否自动提交的会话
  @Override
  public SqlSession openSession(boolean autoCommit) {
    return openSessionFromDataSource(configuration.getDefaultExecutorType(), null, autoCommit);
  }
  //打开执行类型的会话
  @Override
  public SqlSession openSession(ExecutorType execType) {
    return openSessionFromDataSource(execType, null, false);
  }
  //打开事务隔离级别的会话
  @Override
  public SqlSession openSession(TransactionIsolationLevel level) {
    return openSessionFromDataSource(configuration.getDefaultExecutorType(), level, false);
  }
  //打开执行类型和事务隔离级别的会话
  @Override
  public SqlSession openSession(ExecutorType execType, TransactionIsolationLevel level) {
    return openSessionFromDataSource(execType, level, false);
  }
  //打开执行类型和是否自动提交的会话
  @Override
  public SqlSession openSession(ExecutorType execType, boolean autoCommit) {
    return openSessionFromDataSource(execType, null, autoCommit);
  }
 //打开链接的会话
  @Override
  public SqlSession openSession(Connection connection) {
    return openSessionFromConnection(configuration.getDefaultExecutorType(), connection);
  }
  //打开执行类型起和链接的会话
  @Override
  public SqlSession openSession(ExecutorType execType, Connection connection) {
    return openSessionFromConnection(execType, connection);
  }
  //获取全局配置
  @Override
  public Configuration getConfiguration() {
    return configuration;
  }
  //打开从数据源的会话
  private SqlSession openSessionFromDataSource(ExecutorType execType, TransactionIsolationLevel level, boolean autoCommit) {
    Transaction tx = null;
    try {
      final Environment environment = configuration.getEnvironment();
      final TransactionFactory transactionFactory = getTransactionFactoryFromEnvironment(environment);
      tx = transactionFactory.newTransaction(environment.getDataSource(), level, autoCommit);
      final Executor executor = configuration.newExecutor(tx, execType);
      return new DefaultSqlSession(configuration, executor, autoCommit);
    } catch (Exception e) {
      closeTransaction(tx); // may have fetched a connection so lets call close()
      throw ExceptionFactory.wrapException("Error opening session.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
  }
  //打开从链接的会话
  private SqlSession openSessionFromConnection(ExecutorType execType, Connection connection) {
    try {
      boolean autoCommit;
      try {
        autoCommit = connection.getAutoCommit();
      } catch (SQLException e) {
        // Failover to true, as most poor drivers
        // or databases won't support transactions
        autoCommit = true;
      }
      final Environment environment = configuration.getEnvironment();
      final TransactionFactory transactionFactory = getTransactionFactoryFromEnvironment(environment);
      final Transaction tx = transactionFactory.newTransaction(connection);
      final Executor executor = configuration.newExecutor(tx, execType);
      return new DefaultSqlSession(configuration, executor, autoCommit);
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error opening session.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
  }
  //获取事务工厂
  private TransactionFactory getTransactionFactoryFromEnvironment(Environment environment) {
    if (environment == null || environment.getTransactionFactory() == null) {
      return new ManagedTransactionFactory();
    }
    return environment.getTransactionFactory();
  }
  //关闭事务
  private void closeTransaction(Transaction tx) {
    if (tx != null) {
      try {
        tx.close();
      } catch (SQLException ignore) {
        // Intentionally ignore. Prefer previous error.
      }
    }
  }

}
```

### 3.1.17、DefaultSqlSession默认的会话

```java
/**
 * 默认的会话实现类，这个类非安全
 * The default implementation for {@link SqlSession}.
 * Note that this class is not Thread-Safe.
 *
 * @author Clinton Begin
 */
public class DefaultSqlSession implements SqlSession {
  //全局配置
  private final Configuration configuration;
  //执行器
  private final Executor executor;
  //是否自动提交
  private final boolean autoCommit;
  //是否脏数据
  private boolean dirty;
  //游标结果集
  private List<Cursor<?>> cursorList;
  //构造函数
  public DefaultSqlSession(Configuration configuration, Executor executor, boolean autoCommit) {
    this.configuration = configuration;
    this.executor = executor;
    this.dirty = false;
    this.autoCommit = autoCommit;
  }
  //构造函数
  public DefaultSqlSession(Configuration configuration, Executor executor) {
    this(configuration, executor, false);
  }
  //查询单条数据
  @Override
  public <T> T selectOne(String statement) {
    return this.selectOne(statement, null);
  }
  //查询单条数据
  @Override
  public <T> T selectOne(String statement, Object parameter) {
    // Popular vote was to return null on 0 results and throw exception on too many.
    List<T> list = this.selectList(statement, parameter);
    if (list.size() == 1) {
      return list.get(0);
    } else if (list.size() > 1) {
      throw new TooManyResultsException("Expected one result (or null) to be returned by selectOne(), but found: " + list.size());
    } else {
      return null;
    }
  }
  //查询map
  @Override
  public <K, V> Map<K, V> selectMap(String statement, String mapKey) {
    return this.selectMap(statement, null, mapKey, RowBounds.DEFAULT);
  }
  //查询map

  @Override
  public <K, V> Map<K, V> selectMap(String statement, Object parameter, String mapKey) {
    return this.selectMap(statement, parameter, mapKey, RowBounds.DEFAULT);
  }
  //查询map

  @Override
  public <K, V> Map<K, V> selectMap(String statement, Object parameter, String mapKey, RowBounds rowBounds) {
    final List<? extends V> list = selectList(statement, parameter, rowBounds);
    final DefaultMapResultHandler<K, V> mapResultHandler = new DefaultMapResultHandler<>(mapKey,
            configuration.getObjectFactory(), configuration.getObjectWrapperFactory(), configuration.getReflectorFactory());
    final DefaultResultContext<V> context = new DefaultResultContext<>();
    for (V o : list) {
      context.nextResultObject(o);
      mapResultHandler.handleResult(context);
    }
    return mapResultHandler.getMappedResults();
  }
  //查询游标集
  @Override
  public <T> Cursor<T> selectCursor(String statement) {
    return selectCursor(statement, null);
  }
  //查询游标集

  @Override
  public <T> Cursor<T> selectCursor(String statement, Object parameter) {
    return selectCursor(statement, parameter, RowBounds.DEFAULT);
  }
  //查询游标集

  @Override
  public <T> Cursor<T> selectCursor(String statement, Object parameter, RowBounds rowBounds) {
    try {
      MappedStatement ms = configuration.getMappedStatement(statement);
      Cursor<T> cursor = executor.queryCursor(ms, wrapCollection(parameter), rowBounds);
      registerCursor(cursor);
      return cursor;
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error querying database.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
  }
  //查询批量结果

  @Override
  public <E> List<E> selectList(String statement) {
    return this.selectList(statement, null);
  }
  //查询批量结果

  @Override
  public <E> List<E> selectList(String statement, Object parameter) {
    return this.selectList(statement, parameter, RowBounds.DEFAULT);
  }
  //查询批量结果

  @Override
  public <E> List<E> selectList(String statement, Object parameter, RowBounds rowBounds) {
    try {
      MappedStatement ms = configuration.getMappedStatement(statement);
      return executor.query(ms, wrapCollection(parameter), rowBounds, Executor.NO_RESULT_HANDLER);
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error querying database.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
  }
  //查询会话

  @Override
  public void select(String statement, Object parameter, ResultHandler handler) {
    select(statement, parameter, RowBounds.DEFAULT, handler);
  }
  //查询会话

  @Override
  public void select(String statement, ResultHandler handler) {
    select(statement, null, RowBounds.DEFAULT, handler);
  }
  //查询会话

  @Override
  public void select(String statement, Object parameter, RowBounds rowBounds, ResultHandler handler) {
    try {
      MappedStatement ms = configuration.getMappedStatement(statement);
      executor.query(ms, wrapCollection(parameter), rowBounds, handler);
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error querying database.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
  }
  //插入会话

  @Override
  public int insert(String statement) {
    return insert(statement, null);
  }
  //插入会话

  @Override
  public int insert(String statement, Object parameter) {
    return update(statement, parameter);
  }
  //更新会话

  @Override
  public int update(String statement) {
    return update(statement, null);
  }
  //更新会话

  @Override
  public int update(String statement, Object parameter) {
    try {
      dirty = true;
      MappedStatement ms = configuration.getMappedStatement(statement);
      return executor.update(ms, wrapCollection(parameter));
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error updating database.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
  }
  //删除会话

  @Override
  public int delete(String statement) {
    return update(statement, null);
  }
  //删除会话

  @Override
  public int delete(String statement, Object parameter) {
    return update(statement, parameter);
  }
  //提交会话

  @Override
  public void commit() {
    commit(false);
  }
  //提交会话

  @Override
  public void commit(boolean force) {
    try {
      executor.commit(isCommitOrRollbackRequired(force));
      dirty = false;
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error committing transaction.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
  }
  //回滚会话

  @Override
  public void rollback() {
    rollback(false);
  }
  //回滚会话

  @Override
  public void rollback(boolean force) {
    try {
      executor.rollback(isCommitOrRollbackRequired(force));
      dirty = false;
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error rolling back transaction.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
  }
  //刷新会话

  @Override
  public List<BatchResult> flushStatements() {
    try {
      return executor.flushStatements();
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error flushing statements.  Cause: " + e, e);
    } finally {
      ErrorContext.instance().reset();
    }
  }
  //关闭会话

  @Override
  public void close() {
    try {
      executor.close(isCommitOrRollbackRequired(false));
      closeCursors();
      dirty = false;
    } finally {
      ErrorContext.instance().reset();
    }
  }
//关闭游标集
  private void closeCursors() {
    if (cursorList != null && !cursorList.isEmpty()) {
      for (Cursor<?> cursor : cursorList) {
        try {
          cursor.close();
        } catch (IOException e) {
          throw ExceptionFactory.wrapException("Error closing cursor.  Cause: " + e, e);
        }
      }
      cursorList.clear();
    }
  }
  //获取配置会话

  @Override
  public Configuration getConfiguration() {
    return configuration;
  }
 //获取mapper会话
  @Override
  public <T> T getMapper(Class<T> type) {
    return configuration.getMapper(type, this);
  }
 //获取链接会话
  @Override
  public Connection getConnection() {
    try {
      return executor.getTransaction().getConnection();
    } catch (SQLException e) {
      throw ExceptionFactory.wrapException("Error getting a new connection.  Cause: " + e, e);
    }
  }
  //清除缓存
  @Override
  public void clearCache() {
    executor.clearLocalCache();
  }
  //注册游标
  private <T> void registerCursor(Cursor<T> cursor) {
    if (cursorList == null) {
      cursorList = new ArrayList<>();
    }
    cursorList.add(cursor);
  }
 //是否有必要提交或者回滚
  private boolean isCommitOrRollbackRequired(boolean force) {
    return (!autoCommit && dirty) || force;
  }
  //封装集合
  private Object wrapCollection(final Object object) {
    if (object instanceof Collection) {
      StrictMap<Object> map = new StrictMap<>();
      map.put("collection", object);
      if (object instanceof List) {
        map.put("list", object);
      }
      return map;
    } else if (object != null && object.getClass().isArray()) {
      StrictMap<Object> map = new StrictMap<>();
      map.put("array", object);
      return map;
    }
    return object;
  }
  //严格map
  public static class StrictMap<V> extends HashMap<String, V> {

    private static final long serialVersionUID = -5741767162221585340L;

    @Override
    public V get(Object key) {
      if (!super.containsKey(key)) {
        throw new BindingException("Parameter '" + key + "' not found. Available parameters are " + this.keySet());
      }
      return super.get(key);
    }

  }

}
```

每个基于 MyBatis 的应用都是以一个 SqlSessionFactory 的实例为核心的。SqlSessionFactory 的实例可以通过 SqlSessionFactoryBuilder 获得。而 SqlSessionFactoryBuilder 则可以从 XML 配置文件或一个预先定制的 Configuration 的实例构建出 SqlSessionFactory 的实例。

![image-20200121144303137](/Users/didi/Library/Application Support/typora-user-images/image-20200121144303137.png)

## SqlSessionFactoryBuilder

顾名思义，根据名字的命名我们了解的是SqlSessionFactory的构建器。该构建器最核心的是2个构建函数，其他作为衍伸的重载构建函数。

![image-20200122124606076](/Users/didi/Library/Application Support/typora-user-images/image-20200122124606076.png)

### 以字符流Reader作为输入

```java
public SqlSessionFactory build(Reader reader, String environment, Properties properties) {
    try {
      XMLConfigBuilder parser = new XMLConfigBuilder(reader, environment, properties);
      return build(parser.parse());
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error building SqlSession.", e);
    } finally {
      ErrorContext.instance().reset();
      try {
        reader.close();
      } catch (IOException e) {
        // Intentionally ignore. Prefer previous error.
      }
    }
  }

```



### 以字节流InputStream作为输入

```java
public SqlSessionFactory build(InputStream inputStream, String environment, Properties properties) {
    try {
      XMLConfigBuilder parser = new XMLConfigBuilder(inputStream, environment, properties);
      return build(parser.parse());
    } catch (Exception e) {
      throw ExceptionFactory.wrapException("Error building SqlSession.", e);
    } finally {
      ErrorContext.instance().reset();
      try {
        inputStream.close();
      } catch (IOException e) {
        // Intentionally ignore. Prefer previous error.
      }
    }
  }
```





## SqlSessionFactory

###  从 XML 配置文件中构建 SqlSessionFactory

从 XML 文件中构建 SqlSessionFactory 的实例非常简单，建议使用类路径下的资源文件进行配置。 但是也可以使用任意的输入流（InputStream）实例，包括字符串形式的文件路径或者 file:// 的 URL 形式的文件路径来配置。MyBatis 包含一个名叫 Resources 的工具类，它包含一些实用方法，可使从 classpath 或其他位置加载资源文件更加容易。

```java
String resource = "org/mybatis/example/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

XML 配置文件中包含了对 MyBatis 系统的核心设置，包含获取数据库连接实例的数据源（DataSource）和决定事务作用域和控制方式的事务管理器（TransactionManager）。 XML 配置文件的详细内容后面再探讨，这里先给出一个简单的示例(也是核心关键的部分)：

```xml-dtd
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

###  预装Configuration实例构建SqlSessionFactory

直接从 Java 代码而不是 XML 文件中创建配置，Mybatis也提供所有和 XML 文件相同功能的配置项。

```java
DataSource dataSource = BlogDataSourceFactory.getBlogDataSource();
TransactionFactory transactionFactory = new JdbcTransactionFactory();
Environment environment = new Environment("development", transactionFactory, dataSource);
Configuration configuration = new Configuration(environment);
configuration.addMapper(BlogMapper.class);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(configuration);
```

注意该例中，configuration 添加了一个映射器类（mapper class）。映射器类是 Java 类，它们包含 SQL 映射语句的注解从而避免依赖 XML 文件。不过，由于 Java 注解的一些限制以及某些 MyBatis 映射的复杂性，要使用大多数高级映射（比如：嵌套联合映射），仍然需要使用 XML 配置。有鉴于此，如果存在一个同名 XML 配置文件，MyBatis 会自动查找并加载它.

【关键点】同名的XML文件优先于同名的class类加载。约定大于配置。

##  SqlSession

### 从 SqlSessionFactory 中获取 SqlSession.

SqlSession 完全包含了面向数据库执行 SQL 命令所需的所有方法。你可以通过 SqlSession 实例来直接执行已映射的 SQL 语句

老版本写法：

```java
try (SqlSession session = sqlSessionFactory.openSession()) {
  Blog blog = (Blog) session.selectOne("org.mybatis.example.BlogMapper.selectBlog", 101);
}
```

新版本写法：

```java
try (SqlSession session = sqlSessionFactory.openSession()) {
  BlogMapper mapper = session.getMapper(BlogMapper.class);
  Blog blog = mapper.selectBlog(101);
}
```









# 第4章、核心数据处理层

## 4.1、org.apache.ibatis.builder（构建器）

### 4.1.1、基础

#### 4.1.1.1、BuilderException

```java
/**
 * 构建器异常
 * @author Clinton Begin
 */
public class BuilderException extends PersistenceException {

  private static final long serialVersionUID = -3885164021020443281L;
  //空构造函数
  public BuilderException() {
    super();
  }
  //传递信息的构造函数
  public BuilderException(String message) {
    super(message);
  }
  //传递信息和throwable的构造函数
  public BuilderException(String message, Throwable cause) {
    super(message, cause);
  }
  //传递throwable的构造函数
  public BuilderException(Throwable cause) {
    super(cause);
  }
}
```

#### 4.1.1.2、IncompleteElementException不完整元素异常

```java
/**
 * 不完整的元素异常
 * @author Eduardo Macarron
 */
public class IncompleteElementException extends BuilderException {
  private static final long serialVersionUID = -3697292286890900315L;
  //空构造函数
  public IncompleteElementException() {
    super();
  }
  //传递信息和throwable的构造函数
  public IncompleteElementException(String message, Throwable cause) {
    super(message, cause);
  }
  //传递信息的构造函数
  public IncompleteElementException(String message) {
    super(message);
  }
  //传递throwable的构造函数
  public IncompleteElementException(Throwable cause) {
    super(cause);
  }

}
```

#### 4.1.1.3、InitialzingObject初始化对象

```java
/**
 * 提供一个初始化方法的接口
 * Interface that indicate to provide an initialization method.
 *
 * @since 3.4.2
 * @author Kazuki Shimizu
 */
public interface InitializingObject {

  /**
   * 初始化一个实例对象
   * Initialize an instance.
   * <p>
   * This method will be invoked after it has set all properties.
   * </p>
   * @throws Exception in the event of misconfiguration (such as failure to set an essential property) or if initialization fails
   */
  void initialize() throws Exception;

}
```

#### 4.1.1.4、CacheRefResolver缓存引用解析器

```java
/**
 * 缓存引用解析器
 * @author Clinton Begin
 */
public class CacheRefResolver {
  //mapper构建器助手
  private final MapperBuilderAssistant assistant;
  //缓存引用命名空间
  private final String cacheRefNamespace;
  //构造函数
  public CacheRefResolver(MapperBuilderAssistant assistant, String cacheRefNamespace) {
    this.assistant = assistant;
    this.cacheRefNamespace = cacheRefNamespace;
  }
  //助手使用缓存引用命名空间
  public Cache resolveCacheRef() {
    return assistant.useCacheRef(cacheRefNamespace);
  }
}
```

#### 4.1.1.5、ParameterExpression参数表达式（todo二次）

```java
/**
 *内联参数表达式分析器。支持的语法（简体）：
 * Inline parameter expression parser. Supported grammar (simplified):
 *
 * <pre>
 * inline-parameter = (propertyName | expression) oldJdbcType attributes
 * propertyName = /expression language's property navigation path/
 * expression = '(' /expression language's expression/ ')'
 * oldJdbcType = ':' /any valid jdbc type/
 * attributes = (',' attribute)*
 * attribute = name '=' value
 * </pre>
 *
 * @author Frank D. Martinez [mnesarco]
 */
public class ParameterExpression extends HashMap<String, String> {

  private static final long serialVersionUID = -2417552199605158680L;
  //构造函数
  public ParameterExpression(String expression) {
    parse(expression);
  }
  //解析表达式
  private void parse(String expression) {
    int p = skipWS(expression, 0);
    if (expression.charAt(p) == '(') {
      expression(expression, p + 1);
    } else {
      property(expression, p);
    }
  }
  //解析表达式
  private void expression(String expression, int left) {
    int match = 1;
    int right = left + 1;
    while (match > 0) {
      if (expression.charAt(right) == ')') {
        match--;
      } else if (expression.charAt(right) == '(') {
        match++;
      }
      right++;
    }
    put("expression", expression.substring(left, right - 1));
    jdbcTypeOpt(expression, right);
  }
  //属性表达式
  private void property(String expression, int left) {
    if (left < expression.length()) {
      int right = skipUntil(expression, left, ",:");
      put("property", trimmedStr(expression, left, right));
      jdbcTypeOpt(expression, right);
    }
  }
  //跳过
  private int skipWS(String expression, int p) {
    for (int i = p; i < expression.length(); i++) {
      if (expression.charAt(i) > 0x20) {
        return i;
      }
    }
    return expression.length();
  }
 //跳过
  private int skipUntil(String expression, int p, final String endChars) {
    for (int i = p; i < expression.length(); i++) {
      char c = expression.charAt(i);
      if (endChars.indexOf(c) > -1) {
        return i;
      }
    }
    return expression.length();
  }
  //
  private void jdbcTypeOpt(String expression, int p) {
    p = skipWS(expression, p);
    if (p < expression.length()) {
      if (expression.charAt(p) == ':') {
        jdbcType(expression, p + 1);
      } else if (expression.charAt(p) == ',') {
        option(expression, p + 1);
      } else {
        throw new BuilderException("Parsing error in {" + expression + "} in position " + p);
      }
    }
  }

  private void jdbcType(String expression, int p) {
    int left = skipWS(expression, p);
    int right = skipUntil(expression, left, ",");
    if (right > left) {
      put("jdbcType", trimmedStr(expression, left, right));
    } else {
      throw new BuilderException("Parsing error in {" + expression + "} in position " + p);
    }
    option(expression, right + 1);
  }
  //可选
  private void option(String expression, int p) {
    int left = skipWS(expression, p);
    if (left < expression.length()) {
      int right = skipUntil(expression, left, "=");
      String name = trimmedStr(expression, left, right);
      left = right + 1;
      right = skipUntil(expression, left, ",");
      String value = trimmedStr(expression, left, right);
      put(name, value);
      option(expression, right + 1);
    }
  }
  //被修正的string
  private String trimmedStr(String str, int start, int end) {
    while (str.charAt(start) <= 0x20) {
      start++;
    }
    while (str.charAt(end - 1) <= 0x20) {
      end--;
    }
    return start >= end ? "" : str.substring(start, end);
  }

}
```

#### 4.1.1.6、BaseBuilder抽象构建器

```java
/**
 * 抽象构建器
 * @author Clinton Begin
 */
public abstract class BaseBuilder {
  //全局配置
  protected final Configuration configuration;
  //类型别名注册器
  protected final TypeAliasRegistry typeAliasRegistry;
  //类型处理器注册器
  protected final TypeHandlerRegistry typeHandlerRegistry;
  //构造函数
  public BaseBuilder(Configuration configuration) {
    this.configuration = configuration;
    this.typeAliasRegistry = this.configuration.getTypeAliasRegistry();
    this.typeHandlerRegistry = this.configuration.getTypeHandlerRegistry();
  }
  //获取全局配置
  public Configuration getConfiguration() {
    return configuration;
  }
  //解析正则表达式
  protected Pattern parseExpression(String regex, String defaultValue) {
    return Pattern.compile(regex == null ? defaultValue : regex);
  }
  //获取boolean值
  protected Boolean booleanValueOf(String value, Boolean defaultValue) {
    return value == null ? defaultValue : Boolean.valueOf(value);
  }
  //获取int值
  protected Integer integerValueOf(String value, Integer defaultValue) {
    return value == null ? defaultValue : Integer.valueOf(value);
  }
  //获取set值
  protected Set<String> stringSetValueOf(String value, String defaultValue) {
    value = value == null ? defaultValue : value;
    return new HashSet<>(Arrays.asList(value.split(",")));
  }
  //解析jdbc类型
  protected JdbcType resolveJdbcType(String alias) {
    if (alias == null) {
      return null;
    }
    try {
      return JdbcType.valueOf(alias);
    } catch (IllegalArgumentException e) {
      throw new BuilderException("Error resolving JdbcType. Cause: " + e, e);
    }
  }
  //解析结果集类型
  protected ResultSetType resolveResultSetType(String alias) {
    if (alias == null) {
      return null;
    }
    try {
      return ResultSetType.valueOf(alias);
    } catch (IllegalArgumentException e) {
      throw new BuilderException("Error resolving ResultSetType. Cause: " + e, e);
    }
  }
  //解析参数风格
  protected ParameterMode resolveParameterMode(String alias) {
    if (alias == null) {
      return null;
    }
    try {
      return ParameterMode.valueOf(alias);
    } catch (IllegalArgumentException e) {
      throw new BuilderException("Error resolving ParameterMode. Cause: " + e, e);
    }
  }
  //创建实例
  protected Object createInstance(String alias) {
    Class<?> clazz = resolveClass(alias);
    if (clazz == null) {
      return null;
    }
    try {
      return clazz.getDeclaredConstructor().newInstance();
    } catch (Exception e) {
      throw new BuilderException("Error creating instance. Cause: " + e, e);
    }
  }
  //根据别名解析类
  protected <T> Class<? extends T> resolveClass(String alias) {
    if (alias == null) {
      return null;
    }
    try {
      return resolveAlias(alias);
    } catch (Exception e) {
      throw new BuilderException("Error resolving class. Cause: " + e, e);
    }
  }
  //解析类型处理器
  protected TypeHandler<?> resolveTypeHandler(Class<?> javaType, String typeHandlerAlias) {
    if (typeHandlerAlias == null) {
      return null;
    }
    Class<?> type = resolveClass(typeHandlerAlias);
    if (type != null && !TypeHandler.class.isAssignableFrom(type)) {
      throw new BuilderException("Type " + type.getName() + " is not a valid TypeHandler because it does not implement TypeHandler interface");
    }
    @SuppressWarnings("unchecked") // already verified it is a TypeHandler
    Class<? extends TypeHandler<?>> typeHandlerType = (Class<? extends TypeHandler<?>>) type;
    return resolveTypeHandler(javaType, typeHandlerType);
  }
  //解析类型处理器
  protected TypeHandler<?> resolveTypeHandler(Class<?> javaType, Class<? extends TypeHandler<?>> typeHandlerType) {
    if (typeHandlerType == null) {
      return null;
    }
    // javaType ignored for injected handlers see issue #746 for full detail
    TypeHandler<?> handler = typeHandlerRegistry.getMappingTypeHandler(typeHandlerType);
    if (handler == null) {
      // not in registry, create a new one
      handler = typeHandlerRegistry.getInstance(javaType, typeHandlerType);
    }
    return handler;
  }
  //解析别名
  protected <T> Class<? extends T> resolveAlias(String alias) {
    return typeAliasRegistry.resolveAlias(alias);
  }
}
```

#### 4.1.1.7、MapperBuilderAssistant接口构建器助手

```java
/**
 * mapper构建器助手
 * @author Clinton Begin
 */
public class MapperBuilderAssistant extends BaseBuilder {
  //当前命名空间
  private String currentNamespace;
  //资源
  private final String resource;
  //当前缓存
  private Cache currentCache;
  //是否未解决缓存引用
  private boolean unresolvedCacheRef; // issue #676
  //构造器
  public MapperBuilderAssistant(Configuration configuration, String resource) {
    super(configuration);
    //错误上下文构建实例资源
    ErrorContext.instance().resource(resource);
    this.resource = resource;
  }
  //获取当前命名空间
  public String getCurrentNamespace() {
    return currentNamespace;
  }
  //设置当前命名空间
  public void setCurrentNamespace(String currentNamespace) {
    if (currentNamespace == null) {
      throw new BuilderException("The mapper element requires a namespace attribute to be specified.");
    }

    if (this.currentNamespace != null && !this.currentNamespace.equals(currentNamespace)) {
      throw new BuilderException("Wrong namespace. Expected '"
          + this.currentNamespace + "' but found '" + currentNamespace + "'.");
    }

    this.currentNamespace = currentNamespace;
  }
  //应用当前的命名空间
  public String applyCurrentNamespace(String base, boolean isReference) {
    if (base == null) {
      return null;
    }
    if (isReference) {
      // is it qualified with any namespace yet?
      if (base.contains(".")) {
        return base;
      }
    } else {
      // is it qualified with this namespace yet?
      if (base.startsWith(currentNamespace + ".")) {
        return base;
      }
      if (base.contains(".")) {
        throw new BuilderException("Dots are not allowed in element names, please remove it from " + base);
      }
    }
    return currentNamespace + "." + base;
  }
  //使用命名空间的缓存引用
  public Cache useCacheRef(String namespace) {
    if (namespace == null) {
      throw new BuilderException("cache-ref element requires a namespace attribute.");
    }
    try {
      unresolvedCacheRef = true;
      //获取缓存
      Cache cache = configuration.getCache(namespace);
      if (cache == null) {
        throw new IncompleteElementException("No cache for namespace '" + namespace + "' could be found.");
      }
      currentCache = cache;
      unresolvedCacheRef = false;
      return cache;
    } catch (IllegalArgumentException e) {
      throw new IncompleteElementException("No cache for namespace '" + namespace + "' could be found.", e);
    }
  }
  //使用新缓存
  public Cache useNewCache(Class<? extends Cache> typeClass,
      Class<? extends Cache> evictionClass,
      Long flushInterval,
      Integer size,
      boolean readWrite,
      boolean blocking,
      Properties props) {
    //构建缓存
    Cache cache = new CacheBuilder(currentNamespace)
        .implementation(valueOrDefault(typeClass, PerpetualCache.class))
        .addDecorator(valueOrDefault(evictionClass, LruCache.class))
        .clearInterval(flushInterval)
        .size(size)
        .readWrite(readWrite)
        .blocking(blocking)
        .properties(props)
        .build();
    configuration.addCache(cache);
    currentCache = cache;
    return cache;
  }
  //添加参数map
  public ParameterMap addParameterMap(String id, Class<?> parameterClass, List<ParameterMapping> parameterMappings) {
    id = applyCurrentNamespace(id, false);
    //全局配置添加参数集合
    ParameterMap parameterMap = new ParameterMap.Builder(configuration, id, parameterClass, parameterMappings).build();
    configuration.addParameterMap(parameterMap);
    return parameterMap;
  }
  //构建参数映射
  public ParameterMapping buildParameterMapping(
      Class<?> parameterType,
      String property,
      Class<?> javaType,
      JdbcType jdbcType,
      String resultMap,
      ParameterMode parameterMode,
      Class<? extends TypeHandler<?>> typeHandler,
      Integer numericScale) {
    resultMap = applyCurrentNamespace(resultMap, true);
   //解析参数Java类型
    // Class parameterType = parameterMapBuilder.type();
    Class<?> javaTypeClass = resolveParameterJavaType(parameterType, property, javaType, jdbcType);
    TypeHandler<?> typeHandlerInstance = resolveTypeHandler(javaTypeClass, typeHandler);

    return new ParameterMapping.Builder(configuration, property, javaTypeClass)
        .jdbcType(jdbcType)
        .resultMapId(resultMap)
        .mode(parameterMode)
        .numericScale(numericScale)
        .typeHandler(typeHandlerInstance)
        .build();
  }
  //添加结果map
  public ResultMap addResultMap(
      String id,
      Class<?> type,
      String extend,
      Discriminator discriminator,
      List<ResultMapping> resultMappings,
      Boolean autoMapping) {
    id = applyCurrentNamespace(id, false);
    extend = applyCurrentNamespace(extend, true);

    if (extend != null) {
      if (!configuration.hasResultMap(extend)) {
        throw new IncompleteElementException("Could not find a parent resultmap with id '" + extend + "'");
      }
      ResultMap resultMap = configuration.getResultMap(extend);
      List<ResultMapping> extendedResultMappings = new ArrayList<>(resultMap.getResultMappings());
      extendedResultMappings.removeAll(resultMappings);
      // Remove parent constructor if this resultMap declares a constructor.
      boolean declaresConstructor = false;
      for (ResultMapping resultMapping : resultMappings) {
        if (resultMapping.getFlags().contains(ResultFlag.CONSTRUCTOR)) {
          declaresConstructor = true;
          break;
        }
      }
      //声明的构造器
      if (declaresConstructor) {
        extendedResultMappings.removeIf(resultMapping -> resultMapping.getFlags().contains(ResultFlag.CONSTRUCTOR));
      }
      resultMappings.addAll(extendedResultMappings);
    }
    ResultMap resultMap = new ResultMap.Builder(configuration, id, type, resultMappings, autoMapping)
        .discriminator(discriminator)
        .build();
    configuration.addResultMap(resultMap);
    return resultMap;
  }
  //枸杞鉴别器
  public Discriminator buildDiscriminator(
      Class<?> resultType,
      String column,
      Class<?> javaType,
      JdbcType jdbcType,
      Class<? extends TypeHandler<?>> typeHandler,
      Map<String, String> discriminatorMap) {
    ResultMapping resultMapping = buildResultMapping(
        resultType,
        null,
        column,
        javaType,
        jdbcType,
        null,
        null,
        null,
        null,
        typeHandler,
        new ArrayList<>(),
        null,
        null,
        false);
    Map<String, String> namespaceDiscriminatorMap = new HashMap<>();
    for (Map.Entry<String, String> e : discriminatorMap.entrySet()) {
      String resultMap = e.getValue();
      resultMap = applyCurrentNamespace(resultMap, true);
      namespaceDiscriminatorMap.put(e.getKey(), resultMap);
    }
    return new Discriminator.Builder(configuration, resultMapping, namespaceDiscriminatorMap).build();
  }
  //添加映射的会话
  public MappedStatement addMappedStatement(
      String id,
      SqlSource sqlSource,
      StatementType statementType,
      SqlCommandType sqlCommandType,
      Integer fetchSize,
      Integer timeout,
      String parameterMap,
      Class<?> parameterType,
      String resultMap,
      Class<?> resultType,
      ResultSetType resultSetType,
      boolean flushCache,
      boolean useCache,
      boolean resultOrdered,
      KeyGenerator keyGenerator,
      String keyProperty,
      String keyColumn,
      String databaseId,
      LanguageDriver lang,
      String resultSets) {

    if (unresolvedCacheRef) {
      throw new IncompleteElementException("Cache-ref not yet resolved");
    }

    id = applyCurrentNamespace(id, false);
    boolean isSelect = sqlCommandType == SqlCommandType.SELECT;

    MappedStatement.Builder statementBuilder = new MappedStatement.Builder(configuration, id, sqlSource, sqlCommandType)
        .resource(resource)
        .fetchSize(fetchSize)
        .timeout(timeout)
        .statementType(statementType)
        .keyGenerator(keyGenerator)
        .keyProperty(keyProperty)
        .keyColumn(keyColumn)
        .databaseId(databaseId)
        .lang(lang)
        .resultOrdered(resultOrdered)
        .resultSets(resultSets)
        .resultMaps(getStatementResultMaps(resultMap, resultType, id))
        .resultSetType(resultSetType)
        .flushCacheRequired(valueOrDefault(flushCache, !isSelect))
        .useCache(valueOrDefault(useCache, isSelect))
        .cache(currentCache);

    ParameterMap statementParameterMap = getStatementParameterMap(parameterMap, parameterType, id);
    if (statementParameterMap != null) {
      statementBuilder.parameterMap(statementParameterMap);
    }

    MappedStatement statement = statementBuilder.build();
    configuration.addMappedStatement(statement);
    return statement;
  }
   //获取值或者默认值
  private <T> T valueOrDefault(T value, T defaultValue) {
    return value == null ? defaultValue : value;
  }
  //获取会话参数map
  private ParameterMap getStatementParameterMap(
      String parameterMapName,
      Class<?> parameterTypeClass,
      String statementId) {
    parameterMapName = applyCurrentNamespace(parameterMapName, true);
    ParameterMap parameterMap = null;
    if (parameterMapName != null) {
      try {
        parameterMap = configuration.getParameterMap(parameterMapName);
      } catch (IllegalArgumentException e) {
        throw new IncompleteElementException("Could not find parameter map " + parameterMapName, e);
      }
    } else if (parameterTypeClass != null) {
      List<ParameterMapping> parameterMappings = new ArrayList<>();
      parameterMap = new ParameterMap.Builder(
          configuration,
          statementId + "-Inline",
          parameterTypeClass,
          parameterMappings).build();
    }
    return parameterMap;
  }
  //获取会话结果map
  private List<ResultMap> getStatementResultMaps(
      String resultMap,
      Class<?> resultType,
      String statementId) {
    resultMap = applyCurrentNamespace(resultMap, true);

    List<ResultMap> resultMaps = new ArrayList<>();
    if (resultMap != null) {
      String[] resultMapNames = resultMap.split(",");
      for (String resultMapName : resultMapNames) {
        try {
          resultMaps.add(configuration.getResultMap(resultMapName.trim()));
        } catch (IllegalArgumentException e) {
          throw new IncompleteElementException("Could not find result map '" + resultMapName + "' referenced from '" + statementId + "'", e);
        }
      }
    } else if (resultType != null) {
      ResultMap inlineResultMap = new ResultMap.Builder(
          configuration,
          statementId + "-Inline",
          resultType,
          new ArrayList<>(),
          null).build();
      resultMaps.add(inlineResultMap);
    }
    return resultMaps;
  }
  //构建结果映射
  public ResultMapping buildResultMapping(
      Class<?> resultType,
      String property,
      String column,
      Class<?> javaType,
      JdbcType jdbcType,
      String nestedSelect,
      String nestedResultMap,
      String notNullColumn,
      String columnPrefix,
      Class<? extends TypeHandler<?>> typeHandler,
      List<ResultFlag> flags,
      String resultSet,
      String foreignColumn,
      boolean lazy) {
    Class<?> javaTypeClass = resolveResultJavaType(resultType, property, javaType);
    TypeHandler<?> typeHandlerInstance = resolveTypeHandler(javaTypeClass, typeHandler);
    List<ResultMapping> composites;
    if ((nestedSelect == null || nestedSelect.isEmpty()) && (foreignColumn == null || foreignColumn.isEmpty())) {
      composites = Collections.emptyList();
    } else {
      composites = parseCompositeColumnName(column);
    }
    return new ResultMapping.Builder(configuration, property, column, javaTypeClass)
        .jdbcType(jdbcType)
        .nestedQueryId(applyCurrentNamespace(nestedSelect, true))
        .nestedResultMapId(applyCurrentNamespace(nestedResultMap, true))
        .resultSet(resultSet)
        .typeHandler(typeHandlerInstance)
        .flags(flags == null ? new ArrayList<>() : flags)
        .composites(composites)
        .notNullColumns(parseMultipleColumnNames(notNullColumn))
        .columnPrefix(columnPrefix)
        .foreignColumn(foreignColumn)
        .lazy(lazy)
        .build();
  }
  //解析多个列名字
  private Set<String> parseMultipleColumnNames(String columnName) {
    Set<String> columns = new HashSet<>();
    if (columnName != null) {
      if (columnName.indexOf(',') > -1) {
        StringTokenizer parser = new StringTokenizer(columnName, "{}, ", false);
        while (parser.hasMoreTokens()) {
          String column = parser.nextToken();
          columns.add(column);
        }
      } else {
        columns.add(columnName);
      }
    }
    return columns;
  } 
  //解析混合的列名字
  private List<ResultMapping> parseCompositeColumnName(String columnName) {
    List<ResultMapping> composites = new ArrayList<>();
    if (columnName != null && (columnName.indexOf('=') > -1 || columnName.indexOf(',') > -1)) {
      StringTokenizer parser = new StringTokenizer(columnName, "{}=, ", false);
      while (parser.hasMoreTokens()) {
        String property = parser.nextToken();
        String column = parser.nextToken();
        ResultMapping complexResultMapping = new ResultMapping.Builder(
            configuration, property, column, configuration.getTypeHandlerRegistry().getUnknownTypeHandler()).build();
        composites.add(complexResultMapping);
      }
    }
    return composites;
  }
  //解析结果Java类型
  private Class<?> resolveResultJavaType(Class<?> resultType, String property, Class<?> javaType) {
    if (javaType == null && property != null) {
      try {
        MetaClass metaResultType = MetaClass.forClass(resultType, configuration.getReflectorFactory());
        javaType = metaResultType.getSetterType(property);
      } catch (Exception e) {
        //ignore, following null check statement will deal with the situation
      }
    }
    if (javaType == null) {
      javaType = Object.class;
    }
    return javaType;
  }
  //解析参数Java类型
  private Class<?> resolveParameterJavaType(Class<?> resultType, String property, Class<?> javaType, JdbcType jdbcType) {
    if (javaType == null) {
      if (JdbcType.CURSOR.equals(jdbcType)) {
        javaType = java.sql.ResultSet.class;
      } else if (Map.class.isAssignableFrom(resultType)) {
        javaType = Object.class;
      } else {
        MetaClass metaResultType = MetaClass.forClass(resultType, configuration.getReflectorFactory());
        javaType = metaResultType.getGetterType(property);
      }
    }
    if (javaType == null) {
      javaType = Object.class;
    }
    return javaType;
  }
  //
  /** Backward compatibility signature. */
  public ResultMapping buildResultMapping(Class<?> resultType, String property, String column, Class<?> javaType,
      JdbcType jdbcType, String nestedSelect, String nestedResultMap, String notNullColumn, String columnPrefix,
      Class<? extends TypeHandler<?>> typeHandler, List<ResultFlag> flags) {
    return buildResultMapping(
      resultType, property, column, javaType, jdbcType, nestedSelect,
      nestedResultMap, notNullColumn, columnPrefix, typeHandler, flags, null, null, configuration.isLazyLoadingEnabled());
  }

  /**
   * @deprecated Use {@link Configuration#getLanguageDriver(Class)}
   */
  @Deprecated
  public LanguageDriver getLanguageDriver(Class<? extends LanguageDriver> langClass) {
    return configuration.getLanguageDriver(langClass);
  }

  /** Backward compatibility signature. */
  public MappedStatement addMappedStatement(String id, SqlSource sqlSource, StatementType statementType,
      SqlCommandType sqlCommandType, Integer fetchSize, Integer timeout, String parameterMap, Class<?> parameterType,
      String resultMap, Class<?> resultType, ResultSetType resultSetType, boolean flushCache, boolean useCache,
      boolean resultOrdered, KeyGenerator keyGenerator, String keyProperty, String keyColumn, String databaseId,
      LanguageDriver lang) {
    return addMappedStatement(
      id, sqlSource, statementType, sqlCommandType, fetchSize, timeout,
      parameterMap, parameterType, resultMap, resultType, resultSetType,
      flushCache, useCache, resultOrdered, keyGenerator, keyProperty,
      keyColumn, databaseId, lang, null);
  }

}
```

#### 4.1.1.8、SqlSourceBuilder

```java
/**
 * SQL源构建器
 * @author Clinton Begin
 */
public class SqlSourceBuilder extends BaseBuilder {
  //参数属性
  private static final String PARAMETER_PROPERTIES = "javaType,jdbcType,mode,numericScale,resultMap,typeHandler,jdbcTypeName";
  //构造函数
  public SqlSourceBuilder(Configuration configuration) {
    super(configuration);
  }
  //解析源SQL
  public SqlSource parse(String originalSql, Class<?> parameterType, Map<String, Object> additionalParameters) {
    ParameterMappingTokenHandler handler = new ParameterMappingTokenHandler(configuration, parameterType, additionalParameters);
    GenericTokenParser parser = new GenericTokenParser("#{", "}", handler);
    String sql = parser.parse(originalSql);
    return new StaticSqlSource(configuration, sql, handler.getParameterMappings());
  }
  //参数映射token处理器
  private static class ParameterMappingTokenHandler extends BaseBuilder implements TokenHandler {
    //参数映射集合
    private List<ParameterMapping> parameterMappings = new ArrayList<>();
    //参数类型
    private Class<?> parameterType;
    //元参数
    private MetaObject metaParameters;
    //构造函数
    public ParameterMappingTokenHandler(Configuration configuration, Class<?> parameterType, Map<String, Object> additionalParameters) {
      super(configuration);
      this.parameterType = parameterType;
      this.metaParameters = configuration.newMetaObject(additionalParameters);
    }
    //获取参数映射
    public List<ParameterMapping> getParameterMappings() {
      return parameterMappings;
    }
    //处理token
    @Override
    public String handleToken(String content) {
      parameterMappings.add(buildParameterMapping(content));
      return "?";
    }
    //构建参数映射
    private ParameterMapping buildParameterMapping(String content) {
      Map<String, String> propertiesMap = parseParameterMapping(content);
      String property = propertiesMap.get("property");
      Class<?> propertyType;
      //属性有get方法
      if (metaParameters.hasGetter(property)) { // issue #448 get type from additional params
        propertyType = metaParameters.getGetterType(property);
        //属性有类型处理器
      } else if (typeHandlerRegistry.hasTypeHandler(parameterType)) {
        propertyType = parameterType;
        //jdbc类型的游标集
      } else if (JdbcType.CURSOR.name().equals(propertiesMap.get("jdbcType"))) {
        propertyType = java.sql.ResultSet.class;
        //参数类型为map
      } else if (property == null || Map.class.isAssignableFrom(parameterType)) {
        propertyType = Object.class;
      } else {
        //获取元类
        MetaClass metaClass = MetaClass.forClass(parameterType, configuration.getReflectorFactory());
        if (metaClass.hasGetter(property)) {
          propertyType = metaClass.getGetterType(property);
        } else {
          propertyType = Object.class;
        }
      }
      //参数映射构建器
      ParameterMapping.Builder builder = new ParameterMapping.Builder(configuration, property, propertyType);
      Class<?> javaType = propertyType;
      String typeHandlerAlias = null;
      //属性遍历参数
      for (Map.Entry<String, String> entry : propertiesMap.entrySet()) {
        String name = entry.getKey();
        String value = entry.getValue();
        if ("javaType".equals(name)) {
          javaType = resolveClass(value);
          builder.javaType(javaType);
        } else if ("jdbcType".equals(name)) {
          builder.jdbcType(resolveJdbcType(value));
        } else if ("mode".equals(name)) {
          builder.mode(resolveParameterMode(value));
        } else if ("numericScale".equals(name)) {
          builder.numericScale(Integer.valueOf(value));
        } else if ("resultMap".equals(name)) {
          builder.resultMapId(value);
        } else if ("typeHandler".equals(name)) {
          typeHandlerAlias = value;
        } else if ("jdbcTypeName".equals(name)) {
          builder.jdbcTypeName(value);
        } else if ("property".equals(name)) {
          // Do Nothing
        } else if ("expression".equals(name)) {
          throw new BuilderException("Expression based parameters are not supported yet");
        } else {
          throw new BuilderException("An invalid property '" + name + "' was found in mapping #{" + content + "}.  Valid properties are " + PARAMETER_PROPERTIES);
        }
      }
      if (typeHandlerAlias != null) {
        builder.typeHandler(resolveTypeHandler(javaType, typeHandlerAlias));
      }
      return builder.build();
    }
    //解析参数映射
    private Map<String, String> parseParameterMapping(String content) {
      try {
        return new ParameterExpression(content);
      } catch (BuilderException ex) {
        throw ex;
      } catch (Exception ex) {
        throw new BuilderException("Parsing error was found in mapping #{" + content + "}.  Check syntax #{property|(expression), var1=value1, var2=value2, ...} ", ex);
      }
    }
  }

}
```

#### 4.1.1.9、StaticSqlSource静态SQL源

```java
/**
 * 静态的SQL源
 * @author Clinton Begin
 */
public class StaticSqlSource implements SqlSource {
  //sql
  private final String sql;
  //参数映射集合
  private final List<ParameterMapping> parameterMappings;
  //全局配置
  private final Configuration configuration;
  //构造函数
  public StaticSqlSource(Configuration configuration, String sql) {
    this(configuration, sql, null);
  }
  //构造函数
  public StaticSqlSource(Configuration configuration, String sql, List<ParameterMapping> parameterMappings) {
    this.sql = sql;
    this.parameterMappings = parameterMappings;
    this.configuration = configuration;
  } 
  //获取绑定参数的执行SQL
  @Override
  public BoundSql getBoundSql(Object parameterObject) {
    return new BoundSql(configuration, sql, parameterMappings, parameterObject);
  }

}
```

#### 4.1.1.10、ResultMapResolver结果映射解析器

```java
/**
 * 结果map解析器
 * @author Eduardo Macarron
 */
public class ResultMapResolver {
  //接口构建器助手
  private final MapperBuilderAssistant assistant;
  //id
  private final String id;
  //类型
  private final Class<?> type;
  //继承
  private final String extend;
  //鉴别器
  private final Discriminator discriminator;
  //结果映射集合
  private final List<ResultMapping> resultMappings;
  //是否自动映射
  private final Boolean autoMapping;
  //构造函数
  public ResultMapResolver(MapperBuilderAssistant assistant, String id, Class<?> type, String extend, Discriminator discriminator, List<ResultMapping> resultMappings, Boolean autoMapping) {
    this.assistant = assistant;
    this.id = id;
    this.type = type;
    this.extend = extend;
    this.discriminator = discriminator;
    this.resultMappings = resultMappings;
    this.autoMapping = autoMapping;
  }
  //解析为结果map
  public ResultMap resolve() {
    return assistant.addResultMap(this.id, this.type, this.extend, this.discriminator, this.resultMappings, this.autoMapping);
  }

}
```

### 4.1.2、注解

#### 4.1.2.1、MapperAnnotationBuilder

```java
/**
 * mapper注解构建器
 * @author Clinton Begin
 * @author Kazuki Shimizu
 */
public class MapperAnnotationBuilder {
  //SQL注解类型集合
  private static final Set<Class<? extends Annotation>> SQL_ANNOTATION_TYPES = new HashSet<>();
  //SQL服务注解类型集合
  private static final Set<Class<? extends Annotation>> SQL_PROVIDER_ANNOTATION_TYPES = new HashSet<>();
  //全局配置
  private final Configuration configuration;
  //mapper构建器助手
  private final MapperBuilderAssistant assistant;
  //类型
  private final Class<?> type;
  //静态代码块
  static {
    //方法注解的类型添加
    SQL_ANNOTATION_TYPES.add(Select.class);
    SQL_ANNOTATION_TYPES.add(Insert.class);
    SQL_ANNOTATION_TYPES.add(Update.class);
    SQL_ANNOTATION_TYPES.add(Delete.class);
    //服务方法注解的类型添加
    SQL_PROVIDER_ANNOTATION_TYPES.add(SelectProvider.class);
    SQL_PROVIDER_ANNOTATION_TYPES.add(InsertProvider.class);
    SQL_PROVIDER_ANNOTATION_TYPES.add(UpdateProvider.class);
    SQL_PROVIDER_ANNOTATION_TYPES.add(DeleteProvider.class);
  }
  //全局配置和类型的构造函数
  public MapperAnnotationBuilder(Configuration configuration, Class<?> type) {
    String resource = type.getName().replace('.', '/') + ".java (best guess)";
    this.assistant = new MapperBuilderAssistant(configuration, resource);
    this.configuration = configuration;
    this.type = type;
  }
  //解析
  public void parse() {
    //类型资源
    String resource = type.toString();
    //是否加载资源
    if (!configuration.isResourceLoaded(resource)) {
      //加载xml资源
      loadXmlResource();
      //添加加载的资源
      configuration.addLoadedResource(resource);
      //助手添加当前命名空间
      assistant.setCurrentNamespace(type.getName());
      //解析缓存
      parseCache();
      //解析缓存引用
      parseCacheRef();
      //遍历方法，解析会话
      Method[] methods = type.getMethods();
      for (Method method : methods) {
        try {
          // issue #237
          if (!method.isBridge()) {
            parseStatement(method);
          }
        } catch (IncompleteElementException e) {
          //全局配置不完整的方法
          configuration.addIncompleteMethod(new MethodResolver(this, method));
        }
      }
    }
    //解析等待的方法
    parsePendingMethods();
  }
  //解析等待的方法
  private void parsePendingMethods() {
    Collection<MethodResolver> incompleteMethods = configuration.getIncompleteMethods();
    synchronized (incompleteMethods) {
      //遍历不完整的方法移除
      Iterator<MethodResolver> iter = incompleteMethods.iterator();
      while (iter.hasNext()) {
        try {
          iter.next().resolve();
          iter.remove();
        } catch (IncompleteElementException e) {
          // This method is still missing a resource
        }
      }
    }
  }
  //加载xml资源
  private void loadXmlResource() {
    //Spring框架可能不知道真实的资源名字，因此我们检查下标记来预防一个资源加载2遍。这个标记是设定在XMLMapperBuilder#bindMapperForNamespace
    // Spring may not know the real resource name so we check a flag
    // to prevent loading again a resource twice
    // this flag is set at XMLMapperBuilder#bindMapperForNamespace
    if (!configuration.isResourceLoaded("namespace:" + type.getName())) {
      String xmlResource = type.getName().replace('.', '/') + ".xml";
      // #1347
      InputStream inputStream = type.getResourceAsStream("/" + xmlResource);
      if (inputStream == null) {
        //检索在类路径不再模块的mapper
        // Search XML mapper that is not in the module but in the classpath.
        try {
          //获取输入流
          inputStream = Resources.getResourceAsStream(type.getClassLoader(), xmlResource);
        } catch (IOException e2) {
          // ignore, resource is not required
        }
      }
      if (inputStream != null) {
        //xml解析流
        XMLMapperBuilder xmlParser = new XMLMapperBuilder(inputStream, assistant.getConfiguration(), xmlResource, configuration.getSqlFragments(), type.getName());
        xmlParser.parse();
      }
    }
  }
  //解析缓存
  private void parseCache() {
    //缓存命名空间
    CacheNamespace cacheDomain = type.getAnnotation(CacheNamespace.class);
    if (cacheDomain != null) {
      Integer size = cacheDomain.size() == 0 ? null : cacheDomain.size();
      Long flushInterval = cacheDomain.flushInterval() == 0 ? null : cacheDomain.flushInterval();
      Properties props = convertToProperties(cacheDomain.properties());
      assistant.useNewCache(cacheDomain.implementation(), cacheDomain.eviction(), flushInterval, size, cacheDomain.readWrite(), cacheDomain.blocking(), props);
    }
  }
  //转换为属性集合
  private Properties convertToProperties(Property[] properties) {
    if (properties.length == 0) {
      return null;
    }
    Properties props = new Properties();
    for (Property property : properties) {
      props.setProperty(property.name(),
          PropertyParser.parse(property.value(), configuration.getVariables()));
    }
    return props;
  }
  //解析缓存引用
  private void parseCacheRef() {
    //缓存命名引用
    CacheNamespaceRef cacheDomainRef = type.getAnnotation(CacheNamespaceRef.class);
    if (cacheDomainRef != null) {
      Class<?> refType = cacheDomainRef.value();
      String refName = cacheDomainRef.name();
      if (refType == void.class && refName.isEmpty()) {
        throw new BuilderException("Should be specified either value() or name() attribute in the @CacheNamespaceRef");
      }
      if (refType != void.class && !refName.isEmpty()) {
        throw new BuilderException("Cannot use both value() and name() attribute in the @CacheNamespaceRef");
      }
      String namespace = (refType != void.class) ? refType.getName() : refName;
      try {
        assistant.useCacheRef(namespace);
      } catch (IncompleteElementException e) {
        configuration.addIncompleteCacheRef(new CacheRefResolver(assistant, namespace));
      }
    }
  }
  //根据方法解析结果map
  private String parseResultMap(Method method) {
    Class<?> returnType = getReturnType(method);
    Arg[] args = method.getAnnotationsByType(Arg.class);
    Result[] results = method.getAnnotationsByType(Result.class);
    TypeDiscriminator typeDiscriminator = method.getAnnotation(TypeDiscriminator.class);
    String resultMapId = generateResultMapName(method);
    applyResultMap(resultMapId, returnType, args, results, typeDiscriminator);
    return resultMapId;
  }
  //根据方法生成结果map名字
  private String generateResultMapName(Method method) {
    Results results = method.getAnnotation(Results.class);
    if (results != null && !results.id().isEmpty()) {
      return type.getName() + "." + results.id();
    }
    StringBuilder suffix = new StringBuilder();
    for (Class<?> c : method.getParameterTypes()) {
      suffix.append("-");
      suffix.append(c.getSimpleName());
    }
    if (suffix.length() < 1) {
      suffix.append("-void");
    }
    return type.getName() + "." + method.getName() + suffix;
  }
  //应用结果map
  private void applyResultMap(String resultMapId, Class<?> returnType, Arg[] args, Result[] results, TypeDiscriminator discriminator) {
    List<ResultMapping> resultMappings = new ArrayList<>();
    applyConstructorArgs(args, returnType, resultMappings);
    applyResults(results, returnType, resultMappings);
    Discriminator disc = applyDiscriminator(resultMapId, returnType, discriminator);
    // TODO add AutoMappingBehaviour
    assistant.addResultMap(resultMapId, returnType, null, disc, resultMappings, null);
    createDiscriminatorResultMaps(resultMapId, returnType, discriminator);
  }
  //创建鉴别器结果map
  private void createDiscriminatorResultMaps(String resultMapId, Class<?> resultType, TypeDiscriminator discriminator) {
    if (discriminator != null) {
      for (Case c : discriminator.cases()) {
        String caseResultMapId = resultMapId + "-" + c.value();
        List<ResultMapping> resultMappings = new ArrayList<>();
        // issue #136
        applyConstructorArgs(c.constructArgs(), resultType, resultMappings);
        applyResults(c.results(), resultType, resultMappings);
        // TODO add AutoMappingBehaviour
        assistant.addResultMap(caseResultMapId, c.type(), resultMapId, null, resultMappings, null);
      }
    }
  }
  //应用鉴别器
  private Discriminator applyDiscriminator(String resultMapId, Class<?> resultType, TypeDiscriminator discriminator) {
    if (discriminator != null) {
      String column = discriminator.column();
      Class<?> javaType = discriminator.javaType() == void.class ? String.class : discriminator.javaType();
      JdbcType jdbcType = discriminator.jdbcType() == JdbcType.UNDEFINED ? null : discriminator.jdbcType();
      @SuppressWarnings("unchecked")
      Class<? extends TypeHandler<?>> typeHandler = (Class<? extends TypeHandler<?>>)
              (discriminator.typeHandler() == UnknownTypeHandler.class ? null : discriminator.typeHandler());
      Case[] cases = discriminator.cases();
      Map<String, String> discriminatorMap = new HashMap<>();
      for (Case c : cases) {
        String value = c.value();
        String caseResultMapId = resultMapId + "-" + value;
        discriminatorMap.put(value, caseResultMapId);
      }
      return assistant.buildDiscriminator(resultType, column, javaType, jdbcType, typeHandler, discriminatorMap);
    }
    return null;
  }
  //解析方法会话
  void parseStatement(Method method) {
    Class<?> parameterTypeClass = getParameterType(method);
    LanguageDriver languageDriver = getLanguageDriver(method);
    SqlSource sqlSource = getSqlSourceFromAnnotations(method, parameterTypeClass, languageDriver);
    if (sqlSource != null) {
      Options options = method.getAnnotation(Options.class);
      final String mappedStatementId = type.getName() + "." + method.getName();
      Integer fetchSize = null;
      Integer timeout = null;
      StatementType statementType = StatementType.PREPARED;
      ResultSetType resultSetType = configuration.getDefaultResultSetType();
      SqlCommandType sqlCommandType = getSqlCommandType(method);
      boolean isSelect = sqlCommandType == SqlCommandType.SELECT;
      boolean flushCache = !isSelect;
      boolean useCache = isSelect;

      KeyGenerator keyGenerator;
      String keyProperty = null;
      String keyColumn = null;
      if (SqlCommandType.INSERT.equals(sqlCommandType) || SqlCommandType.UPDATE.equals(sqlCommandType)) {
        // first check for SelectKey annotation - that overrides everything else
        SelectKey selectKey = method.getAnnotation(SelectKey.class);
        if (selectKey != null) {
          keyGenerator = handleSelectKeyAnnotation(selectKey, mappedStatementId, getParameterType(method), languageDriver);
          keyProperty = selectKey.keyProperty();
        } else if (options == null) {
          keyGenerator = configuration.isUseGeneratedKeys() ? Jdbc3KeyGenerator.INSTANCE : NoKeyGenerator.INSTANCE;
        } else {
          keyGenerator = options.useGeneratedKeys() ? Jdbc3KeyGenerator.INSTANCE : NoKeyGenerator.INSTANCE;
          keyProperty = options.keyProperty();
          keyColumn = options.keyColumn();
        }
      } else {
        keyGenerator = NoKeyGenerator.INSTANCE;
      }

      if (options != null) {
        if (FlushCachePolicy.TRUE.equals(options.flushCache())) {
          flushCache = true;
        } else if (FlushCachePolicy.FALSE.equals(options.flushCache())) {
          flushCache = false;
        }
        useCache = options.useCache();
        fetchSize = options.fetchSize() > -1 || options.fetchSize() == Integer.MIN_VALUE ? options.fetchSize() : null; //issue #348
        timeout = options.timeout() > -1 ? options.timeout() : null;
        statementType = options.statementType();
        if (options.resultSetType() != ResultSetType.DEFAULT) {
          resultSetType = options.resultSetType();
        }
      }

      String resultMapId = null;
      ResultMap resultMapAnnotation = method.getAnnotation(ResultMap.class);
      if (resultMapAnnotation != null) {
        resultMapId = String.join(",", resultMapAnnotation.value());
      } else if (isSelect) {
        resultMapId = parseResultMap(method);
      }

      assistant.addMappedStatement(
          mappedStatementId,
          sqlSource,
          statementType,
          sqlCommandType,
          fetchSize,
          timeout,
          // ParameterMapID
          null,
          parameterTypeClass,
          resultMapId,
          getReturnType(method),
          resultSetType,
          flushCache,
          useCache,
          // TODO gcode issue #577
          false,
          keyGenerator,
          keyProperty,
          keyColumn,
          // DatabaseID
          null,
          languageDriver,
          // ResultSets
          options != null ? nullOrEmpty(options.resultSets()) : null);
    }
  }
  //获取语言驱动
  private LanguageDriver getLanguageDriver(Method method) {
    Lang lang = method.getAnnotation(Lang.class);
    Class<? extends LanguageDriver> langClass = null;
    if (lang != null) {
      langClass = lang.value();
    }
    return configuration.getLanguageDriver(langClass);
  }
  //根据方法获取参数类型
  private Class<?> getParameterType(Method method) {
    Class<?> parameterType = null;
    Class<?>[] parameterTypes = method.getParameterTypes();
    for (Class<?> currentParameterType : parameterTypes) {
      if (!RowBounds.class.isAssignableFrom(currentParameterType) && !ResultHandler.class.isAssignableFrom(currentParameterType)) {
        if (parameterType == null) {
          parameterType = currentParameterType;
        } else {
          // issue #135
          parameterType = ParamMap.class;
        }
      }
    }
    return parameterType;
  }
  //根据方法获取返回类型
  private Class<?> getReturnType(Method method) {
    Class<?> returnType = method.getReturnType();
    Type resolvedReturnType = TypeParameterResolver.resolveReturnType(method, type);
    if (resolvedReturnType instanceof Class) {
      returnType = (Class<?>) resolvedReturnType;
      if (returnType.isArray()) {
        returnType = returnType.getComponentType();
      }
      // gcode issue #508
      if (void.class.equals(returnType)) {
        ResultType rt = method.getAnnotation(ResultType.class);
        if (rt != null) {
          returnType = rt.value();
        }
      }
    } else if (resolvedReturnType instanceof ParameterizedType) {
      ParameterizedType parameterizedType = (ParameterizedType) resolvedReturnType;
      Class<?> rawType = (Class<?>) parameterizedType.getRawType();
      if (Collection.class.isAssignableFrom(rawType) || Cursor.class.isAssignableFrom(rawType)) {
        Type[] actualTypeArguments = parameterizedType.getActualTypeArguments();
        if (actualTypeArguments != null && actualTypeArguments.length == 1) {
          Type returnTypeParameter = actualTypeArguments[0];
          if (returnTypeParameter instanceof Class<?>) {
            returnType = (Class<?>) returnTypeParameter;
          } else if (returnTypeParameter instanceof ParameterizedType) {
            // (gcode issue #443) actual type can be a also a parameterized type
            returnType = (Class<?>) ((ParameterizedType) returnTypeParameter).getRawType();
          } else if (returnTypeParameter instanceof GenericArrayType) {
            Class<?> componentType = (Class<?>) ((GenericArrayType) returnTypeParameter).getGenericComponentType();
            // (gcode issue #525) support List<byte[]>
            returnType = Array.newInstance(componentType, 0).getClass();
          }
        }
      } else if (method.isAnnotationPresent(MapKey.class) && Map.class.isAssignableFrom(rawType)) {
        // (gcode issue 504) Do not look into Maps if there is not MapKey annotation
        Type[] actualTypeArguments = parameterizedType.getActualTypeArguments();
        if (actualTypeArguments != null && actualTypeArguments.length == 2) {
          Type returnTypeParameter = actualTypeArguments[1];
          if (returnTypeParameter instanceof Class<?>) {
            returnType = (Class<?>) returnTypeParameter;
          } else if (returnTypeParameter instanceof ParameterizedType) {
            // (gcode issue 443) actual type can be a also a parameterized type
            returnType = (Class<?>) ((ParameterizedType) returnTypeParameter).getRawType();
          }
        }
      } else if (Optional.class.equals(rawType)) {
        Type[] actualTypeArguments = parameterizedType.getActualTypeArguments();
        Type returnTypeParameter = actualTypeArguments[0];
        if (returnTypeParameter instanceof Class<?>) {
          returnType = (Class<?>) returnTypeParameter;
        }
      }
    }

    return returnType;
  }
  //获取sql源从注解
  private SqlSource getSqlSourceFromAnnotations(Method method, Class<?> parameterType, LanguageDriver languageDriver) {
    try {
      Class<? extends Annotation> sqlAnnotationType = getSqlAnnotationType(method);
      Class<? extends Annotation> sqlProviderAnnotationType = getSqlProviderAnnotationType(method);
      if (sqlAnnotationType != null) {
        if (sqlProviderAnnotationType != null) {
          throw new BindingException("You cannot supply both a static SQL and SqlProvider to method named " + method.getName());
        }
        Annotation sqlAnnotation = method.getAnnotation(sqlAnnotationType);
        final String[] strings = (String[]) sqlAnnotation.getClass().getMethod("value").invoke(sqlAnnotation);
        return buildSqlSourceFromStrings(strings, parameterType, languageDriver);
      } else if (sqlProviderAnnotationType != null) {
        Annotation sqlProviderAnnotation = method.getAnnotation(sqlProviderAnnotationType);
        return new ProviderSqlSource(assistant.getConfiguration(), sqlProviderAnnotation, type, method);
      }
      return null;
    } catch (Exception e) {
      throw new BuilderException("Could not find value method on SQL annotation.  Cause: " + e, e);
    }
  }
  //构建sql源从string集合
  private SqlSource buildSqlSourceFromStrings(String[] strings, Class<?> parameterTypeClass, LanguageDriver languageDriver) {
    final StringBuilder sql = new StringBuilder();
    for (String fragment : strings) {
      sql.append(fragment);
      sql.append(" ");
    }
    return languageDriver.createSqlSource(configuration, sql.toString().trim(), parameterTypeClass);
  }
  //获取sql命令
  private SqlCommandType getSqlCommandType(Method method) {
    Class<? extends Annotation> type = getSqlAnnotationType(method);

    if (type == null) {
      type = getSqlProviderAnnotationType(method);

      if (type == null) {
        return SqlCommandType.UNKNOWN;
      }

      if (type == SelectProvider.class) {
        type = Select.class;
      } else if (type == InsertProvider.class) {
        type = Insert.class;
      } else if (type == UpdateProvider.class) {
        type = Update.class;
      } else if (type == DeleteProvider.class) {
        type = Delete.class;
      }
    }

    return SqlCommandType.valueOf(type.getSimpleName().toUpperCase(Locale.ENGLISH));
  }
  //获取sql注解类型
  private Class<? extends Annotation> getSqlAnnotationType(Method method) {
    return chooseAnnotationType(method, SQL_ANNOTATION_TYPES);
  }
  //获取sql服务注解类型
  private Class<? extends Annotation> getSqlProviderAnnotationType(Method method) {
    return chooseAnnotationType(method, SQL_PROVIDER_ANNOTATION_TYPES);
  }
  //选择注解类型
  private Class<? extends Annotation> chooseAnnotationType(Method method, Set<Class<? extends Annotation>> types) {
    for (Class<? extends Annotation> type : types) {
      Annotation annotation = method.getAnnotation(type);
      if (annotation != null) {
        return type;
      }
    }
    return null;
  }
  //应用结果
  private void applyResults(Result[] results, Class<?> resultType, List<ResultMapping> resultMappings) {
    for (Result result : results) {
      List<ResultFlag> flags = new ArrayList<>();
      if (result.id()) {
        flags.add(ResultFlag.ID);
      }
      @SuppressWarnings("unchecked")
      Class<? extends TypeHandler<?>> typeHandler = (Class<? extends TypeHandler<?>>)
              ((result.typeHandler() == UnknownTypeHandler.class) ? null : result.typeHandler());
      ResultMapping resultMapping = assistant.buildResultMapping(
          resultType,
          nullOrEmpty(result.property()),
          nullOrEmpty(result.column()),
          result.javaType() == void.class ? null : result.javaType(),
          result.jdbcType() == JdbcType.UNDEFINED ? null : result.jdbcType(),
          hasNestedSelect(result) ? nestedSelectId(result) : null,
          null,
          null,
          null,
          typeHandler,
          flags,
          null,
          null,
          isLazy(result));
      resultMappings.add(resultMapping);
    }
  }
  //嵌套查询ID
  private String nestedSelectId(Result result) {
    String nestedSelect = result.one().select();
    if (nestedSelect.length() < 1) {
      nestedSelect = result.many().select();
    }
    if (!nestedSelect.contains(".")) {
      nestedSelect = type.getName() + "." + nestedSelect;
    }
    return nestedSelect;
  }
  //是否延迟加载
  private boolean isLazy(Result result) {
    boolean isLazy = configuration.isLazyLoadingEnabled();
    if (result.one().select().length() > 0 && FetchType.DEFAULT != result.one().fetchType()) {
      isLazy = result.one().fetchType() == FetchType.LAZY;
    } else if (result.many().select().length() > 0 && FetchType.DEFAULT != result.many().fetchType()) {
      isLazy = result.many().fetchType() == FetchType.LAZY;
    }
    return isLazy;
  }
  //结果是否嵌套查询
  private boolean hasNestedSelect(Result result) {
    if (result.one().select().length() > 0 && result.many().select().length() > 0) {
      throw new BuilderException("Cannot use both @One and @Many annotations in the same @Result");
    }
    return result.one().select().length() > 0 || result.many().select().length() > 0;
  }
  //应用构造器参数
  private void applyConstructorArgs(Arg[] args, Class<?> resultType, List<ResultMapping> resultMappings) {
    for (Arg arg : args) {
      List<ResultFlag> flags = new ArrayList<>();
      flags.add(ResultFlag.CONSTRUCTOR);
      if (arg.id()) {
        flags.add(ResultFlag.ID);
      }
      @SuppressWarnings("unchecked")
      Class<? extends TypeHandler<?>> typeHandler = (Class<? extends TypeHandler<?>>)
              (arg.typeHandler() == UnknownTypeHandler.class ? null : arg.typeHandler());
      ResultMapping resultMapping = assistant.buildResultMapping(
          resultType,
          nullOrEmpty(arg.name()),
          nullOrEmpty(arg.column()),
          arg.javaType() == void.class ? null : arg.javaType(),
          arg.jdbcType() == JdbcType.UNDEFINED ? null : arg.jdbcType(),
          nullOrEmpty(arg.select()),
          nullOrEmpty(arg.resultMap()),
          null,
          nullOrEmpty(arg.columnPrefix()),
          typeHandler,
          flags,
          null,
          null,
          false);
      resultMappings.add(resultMapping);
    }
  }
  //null或者空
  private String nullOrEmpty(String value) {
    return value == null || value.trim().length() == 0 ? null : value;
  }
  //处理生成注解的注解
  private KeyGenerator handleSelectKeyAnnotation(SelectKey selectKeyAnnotation, String baseStatementId, Class<?> parameterTypeClass, LanguageDriver languageDriver) {
    String id = baseStatementId + SelectKeyGenerator.SELECT_KEY_SUFFIX;
    Class<?> resultTypeClass = selectKeyAnnotation.resultType();
    StatementType statementType = selectKeyAnnotation.statementType();
    String keyProperty = selectKeyAnnotation.keyProperty();
    String keyColumn = selectKeyAnnotation.keyColumn();
    boolean executeBefore = selectKeyAnnotation.before();

    // defaults
    boolean useCache = false;
    KeyGenerator keyGenerator = NoKeyGenerator.INSTANCE;
    Integer fetchSize = null;
    Integer timeout = null;
    boolean flushCache = false;
    String parameterMap = null;
    String resultMap = null;
    ResultSetType resultSetTypeEnum = null;

    SqlSource sqlSource = buildSqlSourceFromStrings(selectKeyAnnotation.statement(), parameterTypeClass, languageDriver);
    SqlCommandType sqlCommandType = SqlCommandType.SELECT;

    assistant.addMappedStatement(id, sqlSource, statementType, sqlCommandType, fetchSize, timeout, parameterMap, parameterTypeClass, resultMap, resultTypeClass, resultSetTypeEnum,
        flushCache, useCache, false,
        keyGenerator, keyProperty, keyColumn, null, languageDriver, null);

    id = assistant.applyCurrentNamespace(id, false);

    MappedStatement keyStatement = configuration.getMappedStatement(id, false);
    SelectKeyGenerator answer = new SelectKeyGenerator(keyStatement, executeBefore);
    configuration.addKeyGenerator(id, answer);
    return answer;
  }

}
```

#### 4.1.2.2、MethodResolver

```java
/**
 * 方法解析器
 * @author Eduardo Macarron
 */
public class MethodResolver {
  //mapper注解构建器
  private final MapperAnnotationBuilder annotationBuilder;
  //反射的Method方法
  private final Method method;
  //构造函数
  public MethodResolver(MapperAnnotationBuilder annotationBuilder, Method method) {
    this.annotationBuilder = annotationBuilder;
    this.method = method;
  }
  //解析注解的方法
  public void resolve() {
    annotationBuilder.parseStatement(method);
  }

}
```

#### 4.1.2.3、ProviderContext服务上下文

```java
/**
 * sql服务方法的上下文对象
 * The context object for sql provider method.
 *
 * @author Kazuki Shimizu
 * @since 3.4.5
 */
public final class ProviderContext {
  //mapper类型
  private final Class<?> mapperType;
  //mapper方法
  private final Method mapperMethod;
  //数据库ID
  private final String databaseId;

  /**
   * 构造函数
   * Constructor.
   *
   * @param mapperType A mapper interface type that specified provider
   * @param mapperMethod A mapper method that specified provider
   * @param databaseId A database id
   */
  ProviderContext(Class<?> mapperType, Method mapperMethod, String databaseId) {
    this.mapperType = mapperType;
    this.mapperMethod = mapperMethod;
    this.databaseId = databaseId;
  }

  /**
   * 获取指定服务的mapper接口
   * Get a mapper interface type that specified provider.
   *
   * @return A mapper interface type that specified provider
   */
  public Class<?> getMapperType() {
    return mapperType;
  }

  /**
   * 获取指定服务的mapper方法
   * Get a mapper method that specified provider.
   *
   * @return A mapper method that specified provider
   */
  public Method getMapperMethod() {
    return mapperMethod;
  }

  /**
   * 从数据库ID服务中获取数据库ID
   * Get a database id that provided from {@link org.apache.ibatis.mapping.DatabaseIdProvider}.
   *
   * @return A database id
   * @since 3.5.1
   */
  public String getDatabaseId() {
    return databaseId;
  }

}
```

#### 4.1.2.4、ProviderMethodResolver服务方法解析器接口

```java
/**
 * 解析SQL服务方法代理SQL服务类的接口
 * The interface that resolve an SQL provider method via an SQL provider class.
 * 这个接口需要实现在一个SQL服务类 和她需要定义默认的构造器创建实例
 * <p> This interface need to implements at an SQL provider class and
 * it need to define the default constructor for creating a new instance.
 *
 * @since 3.5.1
 * @author Kazuki Shimizu
 */
public interface ProviderMethodResolver {

  /**
   * 解析SQL服务的方法
   * Resolve an SQL provider method.
   *
   * <p> The default implementation return a method that matches following conditions.
   * <ul>
   *   <li>Method name matches with mapper method</li>
   *   <li>Return type matches the {@link CharSequence}({@link String}, {@link StringBuilder}, etc...)</li>
   * </ul>
   * If matched method is zero or multiple, it throws a {@link BuilderException}.
   *
   * @param context a context for SQL provider
   * @return an SQL provider method
   * @throws BuilderException Throws when cannot resolve a target method
   */
  default Method resolveMethod(ProviderContext context) {
    List<Method> sameNameMethods = Arrays.stream(getClass().getMethods())
        .filter(m -> m.getName().equals(context.getMapperMethod().getName()))
        .collect(Collectors.toList());
    if (sameNameMethods.isEmpty()) {
      throw new BuilderException("Cannot resolve the provider method because '"
          + context.getMapperMethod().getName() + "' not found in SqlProvider '" + getClass().getName() + "'.");
    }
    List<Method> targetMethods = sameNameMethods.stream()
        .filter(m -> CharSequence.class.isAssignableFrom(m.getReturnType()))
        .collect(Collectors.toList());
    if (targetMethods.size() == 1) {
      return targetMethods.get(0);
    }
    if (targetMethods.isEmpty()) {
      throw new BuilderException("Cannot resolve the provider method because '"
          + context.getMapperMethod().getName() + "' does not return the CharSequence or its subclass in SqlProvider '"
          + getClass().getName() + "'.");
    } else {
      throw new BuilderException("Cannot resolve the provider method because '"
          + context.getMapperMethod().getName() + "' is found multiple in SqlProvider '" + getClass().getName() + "'.");
    }
  }

}
```

####  4.1.2.5、ProviderSqlSource服务SQL源

```java
/**
 * 服务SQL源
 * @author Clinton Begin
 * @author Kazuki Shimizu
 */
public class ProviderSqlSource implements SqlSource {
  //全局配置
  private final Configuration configuration;
  
  //服务类型
  private final Class<?> providerType;
  //语言驱动
  private final LanguageDriver languageDriver;
  //mapper方法
  private final Method mapperMethod;
  //服务方法
  private final Method providerMethod;
  //服务方法参数名字
  private final String[] providerMethodArgumentNames;
  //服务方法参数类型
  private final Class<?>[] providerMethodParameterTypes;
  //服务上下文
  private final ProviderContext providerContext;
  //服务上下文索引
  private final Integer providerContextIndex;

  /**
   * 自动3。5。3版本以后，请使用 {@link #ProviderSqlSource(Configuration, Annotation, Class, Method)}代替这个方法
   * @deprecated Since 3.5.3, Please use the {@link #ProviderSqlSource(Configuration, Annotation, Class, Method)} instead of this.
   * This constructor will remove at a future version.
   */
  @Deprecated
  public ProviderSqlSource(Configuration configuration, Object provider) {
    this(configuration, provider, null, null);
  }

  /**
   * 自动3。5。3版本以后，请使用 {@link #ProviderSqlSource(Configuration, Annotation, Class, Method)}代替这个方法
   * @since 3.4.5
   * @deprecated Since 3.5.3, Please use the {@link #ProviderSqlSource(Configuration, Annotation, Class, Method)} instead of this.
   * This constructor will remove at a future version.
   */
  @Deprecated
  public ProviderSqlSource(Configuration configuration, Object provider, Class<?> mapperType, Method mapperMethod) {
    this(configuration, (Annotation) provider , mapperType, mapperMethod);
  }

  /**
   * 服务SQL源
   * @since 3.5.3
   */
  public ProviderSqlSource(Configuration configuration, Annotation provider, Class<?> mapperType, Method mapperMethod) {
    String candidateProviderMethodName;
    Method candidateProviderMethod = null;
    try {
      this.configuration = configuration;
      this.mapperMethod = mapperMethod;
      Lang lang = mapperMethod == null ? null : mapperMethod.getAnnotation(Lang.class);
      this.languageDriver = configuration.getLanguageDriver(lang == null ? null : lang.value());
      this.providerType = getProviderType(provider, mapperMethod);
      //获取方法的服务
      candidateProviderMethodName = (String) provider.annotationType().getMethod("method").invoke(provider);

      if (candidateProviderMethodName.length() == 0 && ProviderMethodResolver.class.isAssignableFrom(this.providerType)) {
        candidateProviderMethod = ((ProviderMethodResolver) this.providerType.getDeclaredConstructor().newInstance())
            .resolveMethod(new ProviderContext(mapperType, mapperMethod, configuration.getDatabaseId()));
      }
      if (candidateProviderMethod == null) {
        candidateProviderMethodName = candidateProviderMethodName.length() == 0 ? "provideSql" : candidateProviderMethodName;
        for (Method m : this.providerType.getMethods()) {
          if (candidateProviderMethodName.equals(m.getName()) && CharSequence.class.isAssignableFrom(m.getReturnType())) {
            if (candidateProviderMethod != null) {
              throw new BuilderException("Error creating SqlSource for SqlProvider. Method '"
                  + candidateProviderMethodName + "' is found multiple in SqlProvider '" + this.providerType.getName()
                  + "'. Sql provider method can not overload.");
            }
            candidateProviderMethod = m;
          }
        }
      }
    } catch (BuilderException e) {
      throw e;
    } catch (Exception e) {
      throw new BuilderException("Error creating SqlSource for SqlProvider.  Cause: " + e, e);
    }
    if (candidateProviderMethod == null) {
      throw new BuilderException("Error creating SqlSource for SqlProvider. Method '"
          + candidateProviderMethodName + "' not found in SqlProvider '" + this.providerType.getName() + "'.");
    }
    this.providerMethod = candidateProviderMethod;
    this.providerMethodArgumentNames = new ParamNameResolver(configuration, this.providerMethod).getNames();
    this.providerMethodParameterTypes = this.providerMethod.getParameterTypes();

    ProviderContext candidateProviderContext = null;
    Integer candidateProviderContextIndex = null;
    for (int i = 0; i < this.providerMethodParameterTypes.length; i++) {
      Class<?> parameterType = this.providerMethodParameterTypes[i];
      if (parameterType == ProviderContext.class) {
        if (candidateProviderContext != null) {
          throw new BuilderException("Error creating SqlSource for SqlProvider. ProviderContext found multiple in SqlProvider method ("
              + this.providerType.getName() + "." + providerMethod.getName()
              + "). ProviderContext can not define multiple in SqlProvider method argument.");
        }
        candidateProviderContext = new ProviderContext(mapperType, mapperMethod, configuration.getDatabaseId());
        candidateProviderContextIndex = i;
      }
    }
    this.providerContext = candidateProviderContext;
    this.providerContextIndex = candidateProviderContextIndex;
  }
  //获取参数对象绑定的SQL
  @Override
  public BoundSql getBoundSql(Object parameterObject) {
    SqlSource sqlSource = createSqlSource(parameterObject);
    return sqlSource.getBoundSql(parameterObject);
  }
  //创建SQL源
  private SqlSource createSqlSource(Object parameterObject) {
    try {
      String sql;
      //参数对象为map
      if (parameterObject instanceof Map) {
        int bindParameterCount = providerMethodParameterTypes.length - (providerContext == null ? 0 : 1);
        if (bindParameterCount == 1 &&
          (providerMethodParameterTypes[Integer.valueOf(0).equals(providerContextIndex) ? 1 : 0].isAssignableFrom(parameterObject.getClass()))) {
          sql = invokeProviderMethod(extractProviderMethodArguments(parameterObject));
        } else {
          @SuppressWarnings("unchecked")
          Map<String, Object> params = (Map<String, Object>) parameterObject;
          sql = invokeProviderMethod(extractProviderMethodArguments(params, providerMethodArgumentNames));
        }
        //参数类型=0
      } else if (providerMethodParameterTypes.length == 0) {
        sql = invokeProviderMethod();
        //参数类型=1
      } else if (providerMethodParameterTypes.length == 1) {
        if (providerContext == null) {
          sql = invokeProviderMethod(parameterObject);
        } else {
          sql = invokeProviderMethod(providerContext);
        }
        //参数类型=2
      } else if (providerMethodParameterTypes.length == 2) {
        sql = invokeProviderMethod(extractProviderMethodArguments(parameterObject));
      } else {
        throw new BuilderException("Cannot invoke SqlProvider method '" + providerMethod
          + "' with specify parameter '" + (parameterObject == null ? null : parameterObject.getClass())
          + "' because SqlProvider method arguments for '" + mapperMethod + "' is an invalid combination.");
      }
      Class<?> parameterType = parameterObject == null ? Object.class : parameterObject.getClass();
      //语言驱动创建SQL源
      return languageDriver.createSqlSource(configuration, sql, parameterType);
    } catch (BuilderException e) {
      throw e;
    } catch (Exception e) {
      throw new BuilderException("Error invoking SqlProvider method '" + providerMethod
          + "' with specify parameter '" + (parameterObject == null ? null : parameterObject.getClass()) + "'.  Cause: " + extractRootCause(e), e);
    }
  }
  //提取根异常
  private Throwable extractRootCause(Exception e) {
    Throwable cause = e;
    while(cause.getCause() != null) {
      cause = cause.getCause();
    }
    return cause;
  }
  //提取服务方法参数
  private Object[] extractProviderMethodArguments(Object parameterObject) {
    if (providerContext != null) {
      Object[] args = new Object[2];
      args[providerContextIndex == 0 ? 1 : 0] = parameterObject;
      args[providerContextIndex] = providerContext;
      return args;
    } else {
      return new Object[] { parameterObject };
    }
  }
  //提取服务方法参数
  private Object[] extractProviderMethodArguments(Map<String, Object> params, String[] argumentNames) {
    Object[] args = new Object[argumentNames.length];
    for (int i = 0; i < args.length; i++) {
      if (providerContextIndex != null && providerContextIndex == i) {
        args[i] = providerContext;
      } else {
        args[i] = params.get(argumentNames[i]);
      }
    }
    return args;
  }
  //执行服务方法
  private String invokeProviderMethod(Object... args) throws Exception {
    Object targetObject = null;
    if (!Modifier.isStatic(providerMethod.getModifiers())) {
      targetObject = providerType.getDeclaredConstructor().newInstance();
    }
    CharSequence sql = (CharSequence) providerMethod.invoke(targetObject, args);
    return sql != null ? sql.toString() : null;
  }
  //获取服务类型
  private Class<?> getProviderType(Annotation providerAnnotation, Method mapperMethod)
      throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {
    Class<?> type = (Class<?>) providerAnnotation.annotationType().getMethod("type").invoke(providerAnnotation);
    Class<?> value = (Class<?>) providerAnnotation.annotationType().getMethod("value").invoke(providerAnnotation);
    if (value == void.class && type == void.class) {
      throw new BuilderException("Please specify either 'value' or 'type' attribute of @"
          + providerAnnotation.annotationType().getSimpleName()
          + " at the '" + mapperMethod.toString() + "'.");
    }
    if (value != void.class && type != void.class && value != type) {
      throw new BuilderException("Cannot specify different class on 'value' and 'type' attribute of @"
          + providerAnnotation.annotationType().getSimpleName()
          + " at the '" + mapperMethod.toString() + "'.");
    }
    return value == void.class ? type : value;
  }

}
```

### 4.1.3、xml

#### 4.1.3.1、XMLConfigBuilder全局配置构建器

```java
/**
 * xml配置构建器
 * @author Clinton Begin
 * @author Kazuki Shimizu
 */
public class XMLConfigBuilder extends BaseBuilder {
  //是否解析
  private boolean parsed;
  //解析器
  private final XPathParser parser;
  //环境
  private String environment;
  //本地反射工厂
  private final ReflectorFactory localReflectorFactory = new DefaultReflectorFactory();
  //构造函数
  public XMLConfigBuilder(Reader reader) {
    this(reader, null, null);
  }
  //构造函数
  public XMLConfigBuilder(Reader reader, String environment) {
    this(reader, environment, null);
  }
  //构造函数
  public XMLConfigBuilder(Reader reader, String environment, Properties props) {
    this(new XPathParser(reader, true, props, new XMLMapperEntityResolver()), environment, props);
  }
  //构造函数
  public XMLConfigBuilder(InputStream inputStream) {
    this(inputStream, null, null);
  }
  //构造函数
  public XMLConfigBuilder(InputStream inputStream, String environment) {
    this(inputStream, environment, null);
  }
   //构造函数
  public XMLConfigBuilder(InputStream inputStream, String environment, Properties props) {
    this(new XPathParser(inputStream, true, props, new XMLMapperEntityResolver()), environment, props);
  }
   //私有构造函数
  private XMLConfigBuilder(XPathParser parser, String environment, Properties props) {
    super(new Configuration());
    ErrorContext.instance().resource("SQL Mapper Configuration");
    this.configuration.setVariables(props);
    this.parsed = false;
    this.environment = environment;
    this.parser = parser;
  }
  //解析全局配置对象
  public Configuration parse() {
    if (parsed) {
      throw new BuilderException("Each XMLConfigBuilder can only be used once.");
    }
    parsed = true;
    parseConfiguration(parser.evalNode("/configuration"));
    return configuration;
  }
   //解析全局配置对象
  private void parseConfiguration(XNode root) {
    try {
      //issue #117 read properties first
      propertiesElement(root.evalNode("properties"));
      Properties settings = settingsAsProperties(root.evalNode("settings"));
      loadCustomVfs(settings);
      loadCustomLogImpl(settings);
      typeAliasesElement(root.evalNode("typeAliases"));
      pluginElement(root.evalNode("plugins"));
      objectFactoryElement(root.evalNode("objectFactory"));
      objectWrapperFactoryElement(root.evalNode("objectWrapperFactory"));
      reflectorFactoryElement(root.evalNode("reflectorFactory"));
      settingsElement(settings);
      // read it after objectFactory and objectWrapperFactory issue #631
      environmentsElement(root.evalNode("environments"));
      databaseIdProviderElement(root.evalNode("databaseIdProvider"));
      typeHandlerElement(root.evalNode("typeHandlers"));
      mapperElement(root.evalNode("mappers"));
    } catch (Exception e) {
      throw new BuilderException("Error parsing SQL Mapper Configuration. Cause: " + e, e);
    }
  }
  //设置属性
  private Properties settingsAsProperties(XNode context) {
    if (context == null) {
      return new Properties();
    }
    Properties props = context.getChildrenAsProperties();
    // Check that all settings are known to the configuration class
    MetaClass metaConfig = MetaClass.forClass(Configuration.class, localReflectorFactory);
    for (Object key : props.keySet()) {
      if (!metaConfig.hasSetter(String.valueOf(key))) {
        throw new BuilderException("The setting " + key + " is not known.  Make sure you spelled it correctly (case sensitive).");
      }
    }
    return props;
  }
   //加载定制vfs解析实现类
  private void loadCustomVfs(Properties props) throws ClassNotFoundException {
    String value = props.getProperty("vfsImpl");
    if (value != null) {
      String[] clazzes = value.split(",");
      for (String clazz : clazzes) {
        if (!clazz.isEmpty()) {
          @SuppressWarnings("unchecked")
          Class<? extends VFS> vfsImpl = (Class<? extends VFS>)Resources.classForName(clazz);
          configuration.setVfsImpl(vfsImpl);
        }
      }
    }
  }
  //加载定制实现类
  private void loadCustomLogImpl(Properties props) {
    Class<? extends Log> logImpl = resolveClass(props.getProperty("logImpl"));
    configuration.setLogImpl(logImpl);
  }
  //类型别名元素
  private void typeAliasesElement(XNode parent) {
    if (parent != null) {
      for (XNode child : parent.getChildren()) {
        if ("package".equals(child.getName())) {
          String typeAliasPackage = child.getStringAttribute("name");
          configuration.getTypeAliasRegistry().registerAliases(typeAliasPackage);
        } else {
          String alias = child.getStringAttribute("alias");
          String type = child.getStringAttribute("type");
          try {
            Class<?> clazz = Resources.classForName(type);
            if (alias == null) {
              typeAliasRegistry.registerAlias(clazz);
            } else {
              typeAliasRegistry.registerAlias(alias, clazz);
            }
          } catch (ClassNotFoundException e) {
            throw new BuilderException("Error registering typeAlias for '" + alias + "'. Cause: " + e, e);
          }
        }
      }
    }
  }
 //插件元素
  private void pluginElement(XNode parent) throws Exception {
    if (parent != null) {
      for (XNode child : parent.getChildren()) {
        String interceptor = child.getStringAttribute("interceptor");
        Properties properties = child.getChildrenAsProperties();
        Interceptor interceptorInstance = (Interceptor) resolveClass(interceptor).getDeclaredConstructor().newInstance();
        interceptorInstance.setProperties(properties);
        configuration.addInterceptor(interceptorInstance);
      }
    }
  }
  //对象工厂元素
  private void objectFactoryElement(XNode context) throws Exception {
    if (context != null) {
      String type = context.getStringAttribute("type");
      Properties properties = context.getChildrenAsProperties();
      ObjectFactory factory = (ObjectFactory) resolveClass(type).getDeclaredConstructor().newInstance();
      factory.setProperties(properties);
      configuration.setObjectFactory(factory);
    }
  }
  //对象包装工厂元素
  private void objectWrapperFactoryElement(XNode context) throws Exception {
    if (context != null) {
      String type = context.getStringAttribute("type");
      ObjectWrapperFactory factory = (ObjectWrapperFactory) resolveClass(type).getDeclaredConstructor().newInstance();
      configuration.setObjectWrapperFactory(factory);
    }
  }
  //反射工厂元素
  private void reflectorFactoryElement(XNode context) throws Exception {
    if (context != null) {
      String type = context.getStringAttribute("type");
      ReflectorFactory factory = (ReflectorFactory) resolveClass(type).getDeclaredConstructor().newInstance();
      configuration.setReflectorFactory(factory);
    }
  }
//属性元素
  private void propertiesElement(XNode context) throws Exception {
    if (context != null) {
      Properties defaults = context.getChildrenAsProperties();
      String resource = context.getStringAttribute("resource");
      String url = context.getStringAttribute("url");
      if (resource != null && url != null) {
        throw new BuilderException("The properties element cannot specify both a URL and a resource based property file reference.  Please specify one or the other.");
      }
      if (resource != null) {
        defaults.putAll(Resources.getResourceAsProperties(resource));
      } else if (url != null) {
        defaults.putAll(Resources.getUrlAsProperties(url));
      }
      Properties vars = configuration.getVariables();
      if (vars != null) {
        defaults.putAll(vars);
      }
      parser.setVariables(defaults);
      configuration.setVariables(defaults);
    }
  }
  //设置属性
  private void settingsElement(Properties props) {
    configuration.setAutoMappingBehavior(AutoMappingBehavior.valueOf(props.getProperty("autoMappingBehavior", "PARTIAL")));
    configuration.setAutoMappingUnknownColumnBehavior(AutoMappingUnknownColumnBehavior.valueOf(props.getProperty("autoMappingUnknownColumnBehavior", "NONE")));
    configuration.setCacheEnabled(booleanValueOf(props.getProperty("cacheEnabled"), true));
    configuration.setProxyFactory((ProxyFactory) createInstance(props.getProperty("proxyFactory")));
    configuration.setLazyLoadingEnabled(booleanValueOf(props.getProperty("lazyLoadingEnabled"), false));
    configuration.setAggressiveLazyLoading(booleanValueOf(props.getProperty("aggressiveLazyLoading"), false));
    configuration.setMultipleResultSetsEnabled(booleanValueOf(props.getProperty("multipleResultSetsEnabled"), true));
    configuration.setUseColumnLabel(booleanValueOf(props.getProperty("useColumnLabel"), true));
    configuration.setUseGeneratedKeys(booleanValueOf(props.getProperty("useGeneratedKeys"), false));
    configuration.setDefaultExecutorType(ExecutorType.valueOf(props.getProperty("defaultExecutorType", "SIMPLE")));
    configuration.setDefaultStatementTimeout(integerValueOf(props.getProperty("defaultStatementTimeout"), null));
    configuration.setDefaultFetchSize(integerValueOf(props.getProperty("defaultFetchSize"), null));
    configuration.setDefaultResultSetType(resolveResultSetType(props.getProperty("defaultResultSetType")));
    configuration.setMapUnderscoreToCamelCase(booleanValueOf(props.getProperty("mapUnderscoreToCamelCase"), false));
    configuration.setSafeRowBoundsEnabled(booleanValueOf(props.getProperty("safeRowBoundsEnabled"), false));
    configuration.setLocalCacheScope(LocalCacheScope.valueOf(props.getProperty("localCacheScope", "SESSION")));
    configuration.setJdbcTypeForNull(JdbcType.valueOf(props.getProperty("jdbcTypeForNull", "OTHER")));
    configuration.setLazyLoadTriggerMethods(stringSetValueOf(props.getProperty("lazyLoadTriggerMethods"), "equals,clone,hashCode,toString"));
    configuration.setSafeResultHandlerEnabled(booleanValueOf(props.getProperty("safeResultHandlerEnabled"), true));
    configuration.setDefaultScriptingLanguage(resolveClass(props.getProperty("defaultScriptingLanguage")));
    configuration.setDefaultEnumTypeHandler(resolveClass(props.getProperty("defaultEnumTypeHandler")));
    configuration.setCallSettersOnNulls(booleanValueOf(props.getProperty("callSettersOnNulls"), false));
    configuration.setUseActualParamName(booleanValueOf(props.getProperty("useActualParamName"), true));
    configuration.setReturnInstanceForEmptyRow(booleanValueOf(props.getProperty("returnInstanceForEmptyRow"), false));
    configuration.setLogPrefix(props.getProperty("logPrefix"));
    configuration.setConfigurationFactory(resolveClass(props.getProperty("configurationFactory")));
  }
  //环境元素
  private void environmentsElement(XNode context) throws Exception {
    if (context != null) {
      if (environment == null) {
        environment = context.getStringAttribute("default");
      }
      for (XNode child : context.getChildren()) {
        String id = child.getStringAttribute("id");
        if (isSpecifiedEnvironment(id)) {
          TransactionFactory txFactory = transactionManagerElement(child.evalNode("transactionManager"));
          DataSourceFactory dsFactory = dataSourceElement(child.evalNode("dataSource"));
          DataSource dataSource = dsFactory.getDataSource();
          Environment.Builder environmentBuilder = new Environment.Builder(id)
              .transactionFactory(txFactory)
              .dataSource(dataSource);
          configuration.setEnvironment(environmentBuilder.build());
        }
      }
    }
  }
  //数据库ID服务元素
  private void databaseIdProviderElement(XNode context) throws Exception {
    DatabaseIdProvider databaseIdProvider = null;
    if (context != null) {
      String type = context.getStringAttribute("type");
      // awful patch to keep backward compatibility
      if ("VENDOR".equals(type)) {
        type = "DB_VENDOR";
      }
      Properties properties = context.getChildrenAsProperties();
      databaseIdProvider = (DatabaseIdProvider) resolveClass(type).getDeclaredConstructor().newInstance();
      databaseIdProvider.setProperties(properties);
    }
    Environment environment = configuration.getEnvironment();
    if (environment != null && databaseIdProvider != null) {
      String databaseId = databaseIdProvider.getDatabaseId(environment.getDataSource());
      configuration.setDatabaseId(databaseId);
    }
  }
  //事务处理的元素
  private TransactionFactory transactionManagerElement(XNode context) throws Exception {
    if (context != null) {
      String type = context.getStringAttribute("type");
      Properties props = context.getChildrenAsProperties();
      TransactionFactory factory = (TransactionFactory) resolveClass(type).getDeclaredConstructor().newInstance();
      factory.setProperties(props);
      return factory;
    }
    throw new BuilderException("Environment declaration requires a TransactionFactory.");
  }
  //数据源处理的元素
  private DataSourceFactory dataSourceElement(XNode context) throws Exception {
    if (context != null) {
      String type = context.getStringAttribute("type");
      Properties props = context.getChildrenAsProperties();
      DataSourceFactory factory = (DataSourceFactory) resolveClass(type).getDeclaredConstructor().newInstance();
      factory.setProperties(props);
      return factory;
    }
    throw new BuilderException("Environment declaration requires a DataSourceFactory.");
  }
  //类型处理器的元素
  private void typeHandlerElement(XNode parent) {
    if (parent != null) {
      for (XNode child : parent.getChildren()) {
        if ("package".equals(child.getName())) {
          String typeHandlerPackage = child.getStringAttribute("name");
          typeHandlerRegistry.register(typeHandlerPackage);
        } else {
          String javaTypeName = child.getStringAttribute("javaType");
          String jdbcTypeName = child.getStringAttribute("jdbcType");
          String handlerTypeName = child.getStringAttribute("handler");
          Class<?> javaTypeClass = resolveClass(javaTypeName);
          JdbcType jdbcType = resolveJdbcType(jdbcTypeName);
          Class<?> typeHandlerClass = resolveClass(handlerTypeName);
          if (javaTypeClass != null) {
            if (jdbcType == null) {
              typeHandlerRegistry.register(javaTypeClass, typeHandlerClass);
            } else {
              typeHandlerRegistry.register(javaTypeClass, jdbcType, typeHandlerClass);
            }
          } else {
            typeHandlerRegistry.register(typeHandlerClass);
          }
        }
      }
    }
  }
  //mapper文件解析元素
  private void mapperElement(XNode parent) throws Exception {
    if (parent != null) {
      for (XNode child : parent.getChildren()) {
        if ("package".equals(child.getName())) {
          String mapperPackage = child.getStringAttribute("name");
          configuration.addMappers(mapperPackage);
        } else {
          String resource = child.getStringAttribute("resource");
          String url = child.getStringAttribute("url");
          String mapperClass = child.getStringAttribute("class");
          if (resource != null && url == null && mapperClass == null) {
            ErrorContext.instance().resource(resource);
            InputStream inputStream = Resources.getResourceAsStream(resource);
            XMLMapperBuilder mapperParser = new XMLMapperBuilder(inputStream, configuration, resource, configuration.getSqlFragments());
            mapperParser.parse();
          } else if (resource == null && url != null && mapperClass == null) {
            ErrorContext.instance().resource(url);
            InputStream inputStream = Resources.getUrlAsStream(url);
            XMLMapperBuilder mapperParser = new XMLMapperBuilder(inputStream, configuration, url, configuration.getSqlFragments());
            mapperParser.parse();
          } else if (resource == null && url == null && mapperClass != null) {
            Class<?> mapperInterface = Resources.classForName(mapperClass);
            configuration.addMapper(mapperInterface);
          } else {
            throw new BuilderException("A mapper element may only specify a url, resource or class, but not more than one.");
          }
        }
      }
    }
  }
  //是否指定环境
  private boolean isSpecifiedEnvironment(String id) {
    if (environment == null) {
      throw new BuilderException("No environment specified.");
    } else if (id == null) {
      throw new BuilderException("Environment requires an id attribute.");
    } else if (environment.equals(id)) {
      return true;
    }
    return false;
  }

}
```

#### 4.1.3.2、XMLIncludeTransformer转换器

```java
/**
 * xml包含的转换器
 * @author Frank D. Martinez [mnesarco]
 */
public class XMLIncludeTransformer {
   //全局配置
  private final Configuration configuration;
  //mapper接口构建助手
  private final MapperBuilderAssistant builderAssistant;
  //构造函数
  public XMLIncludeTransformer(Configuration configuration, MapperBuilderAssistant builderAssistant) {
    this.configuration = configuration;
    this.builderAssistant = builderAssistant;
  }
  //应用节点包含的信息
  public void applyIncludes(Node source) {
    Properties variablesContext = new Properties();
    Properties configurationVariables = configuration.getVariables();
    Optional.ofNullable(configurationVariables).ifPresent(variablesContext::putAll);
    applyIncludes(source, variablesContext, false);
  }

  /**
   * 递归地通过所有SQL片段应用includes。
   * Recursively apply includes through all SQL fragments.
   * @param source Include node in DOM tree
   * @param variablesContext Current context for static variables with values
   */
  private void applyIncludes(Node source, final Properties variablesContext, boolean included) {
    if (source.getNodeName().equals("include")) {
      Node toInclude = findSqlFragment(getStringAttribute(source, "refid"), variablesContext);
      Properties toIncludeContext = getVariablesContext(source, variablesContext);
      applyIncludes(toInclude, toIncludeContext, true);
      if (toInclude.getOwnerDocument() != source.getOwnerDocument()) {
        toInclude = source.getOwnerDocument().importNode(toInclude, true);
      }
      source.getParentNode().replaceChild(toInclude, source);
      while (toInclude.hasChildNodes()) {
        toInclude.getParentNode().insertBefore(toInclude.getFirstChild(), toInclude);
      }
      toInclude.getParentNode().removeChild(toInclude);
    } else if (source.getNodeType() == Node.ELEMENT_NODE) {
      if (included && !variablesContext.isEmpty()) {
        // replace variables in attribute values
        NamedNodeMap attributes = source.getAttributes();
        for (int i = 0; i < attributes.getLength(); i++) {
          Node attr = attributes.item(i);
          attr.setNodeValue(PropertyParser.parse(attr.getNodeValue(), variablesContext));
        }
      }
      NodeList children = source.getChildNodes();
      for (int i = 0; i < children.getLength(); i++) {
        applyIncludes(children.item(i), variablesContext, included);
      }
    } else if (included && (source.getNodeType() == Node.TEXT_NODE || source.getNodeType() == Node.CDATA_SECTION_NODE)
        && !variablesContext.isEmpty()) {
      // replace variables in text node
      source.setNodeValue(PropertyParser.parse(source.getNodeValue(), variablesContext));
    }
  }
   //找SQL的片段
  private Node findSqlFragment(String refid, Properties variables) {
    refid = PropertyParser.parse(refid, variables);
    refid = builderAssistant.applyCurrentNamespace(refid, true);
    try {
      XNode nodeToInclude = configuration.getSqlFragments().get(refid);
      return nodeToInclude.getNode().cloneNode(true);
    } catch (IllegalArgumentException e) {
      throw new IncompleteElementException("Could not find SQL statement to include with refid '" + refid + "'", e);
    }
  }
  //获取节点的属性
  private String getStringAttribute(Node node, String name) {
    return node.getAttributes().getNamedItem(name).getNodeValue();
  }

  /**
   * 读取占位符和他们的值从包含的节点定义中
   * Read placeholders and their values from include node definition.
   * @param node Include node instance
   * @param inheritedVariablesContext Current context used for replace variables in new variables values
   * @return variables context from include instance (no inherited values)
   */
  private Properties getVariablesContext(Node node, Properties inheritedVariablesContext) {
    Map<String, String> declaredProperties = null;
    NodeList children = node.getChildNodes();
    for (int i = 0; i < children.getLength(); i++) {
      Node n = children.item(i);
      if (n.getNodeType() == Node.ELEMENT_NODE) {
        String name = getStringAttribute(n, "name");
        // Replace variables inside
        String value = PropertyParser.parse(getStringAttribute(n, "value"), inheritedVariablesContext);
        if (declaredProperties == null) {
          declaredProperties = new HashMap<>();
        }
        if (declaredProperties.put(name, value) != null) {
          throw new BuilderException("Variable " + name + " defined twice in the same include definition");
        }
      }
    }
    if (declaredProperties == null) {
      return inheritedVariablesContext;
    } else {
      Properties newProperties = new Properties();
      newProperties.putAll(inheritedVariablesContext);
      newProperties.putAll(declaredProperties);
      return newProperties;
    }
  }
}
```

#### 4.1.3.3、XMLMapperBuilder接口构建器

```java
/**
 * XML的mapper接口构建器
 * @author Clinton Begin
 * @author Kazuki Shimizu
 */
public class XMLMapperBuilder extends BaseBuilder {
  //解析器
  private final XPathParser parser;
  //接口助手
  private final MapperBuilderAssistant builderAssistant;
  //sql节点片段
  private final Map<String, XNode> sqlFragments;
  //资源
  private final String resource;
  //不建议使用，未来废弃
  @Deprecated
  public XMLMapperBuilder(Reader reader, Configuration configuration, String resource, Map<String, XNode> sqlFragments, String namespace) {
    this(reader, configuration, resource, sqlFragments);
    this.builderAssistant.setCurrentNamespace(namespace);
  }
  //不建议使用，未来废弃
  @Deprecated
  public XMLMapperBuilder(Reader reader, Configuration configuration, String resource, Map<String, XNode> sqlFragments) {
    this(new XPathParser(reader, true, configuration.getVariables(), new XMLMapperEntityResolver()),
        configuration, resource, sqlFragments);
  }
  //构造函数
  public XMLMapperBuilder(InputStream inputStream, Configuration configuration, String resource, Map<String, XNode> sqlFragments, String namespace) {
    this(inputStream, configuration, resource, sqlFragments);
    this.builderAssistant.setCurrentNamespace(namespace);
  }
  //构造函数
  public XMLMapperBuilder(InputStream inputStream, Configuration configuration, String resource, Map<String, XNode> sqlFragments) {
    this(new XPathParser(inputStream, true, configuration.getVariables(), new XMLMapperEntityResolver()),
        configuration, resource, sqlFragments);
  }
   //构造函数
  private XMLMapperBuilder(XPathParser parser, Configuration configuration, String resource, Map<String, XNode> sqlFragments) {
    super(configuration);
    this.builderAssistant = new MapperBuilderAssistant(configuration, resource);
    this.parser = parser;
    this.sqlFragments = sqlFragments;
    this.resource = resource;
  }
  //解析mapper
  public void parse() {
    if (!configuration.isResourceLoaded(resource)) {
      configurationElement(parser.evalNode("/mapper"));
      configuration.addLoadedResource(resource);
      //将mapper和命名空间绑定
      bindMapperForNamespace();
    }
    //解析等待的结果映射
    parsePendingResultMaps();
    //解析等待的缓存引用
    parsePendingCacheRefs();
    //解析等待的会话
    parsePendingStatements();
  }
  //获取sql片段根据引用ID
  public XNode getSqlFragment(String refid) {
    return sqlFragments.get(refid);
  }
  //配置全局配置
  private void configurationElement(XNode context) {
    try {
      String namespace = context.getStringAttribute("namespace");
      if (namespace == null || namespace.equals("")) {
        throw new BuilderException("Mapper's namespace cannot be empty");
      }
      //绑定当前的命名空间
      builderAssistant.setCurrentNamespace(namespace);
      //缓存引用
      cacheRefElement(context.evalNode("cache-ref"));
      //缓存
      cacheElement(context.evalNode("cache"));
      //参数map
      parameterMapElement(context.evalNodes("/mapper/parameterMap"));
      //结果map
      resultMapElements(context.evalNodes("/mapper/resultMap"));
      //sql
      sqlElement(context.evalNodes("/mapper/sql"));
      //会话语句
      buildStatementFromContext(context.evalNodes("select|insert|update|delete"));
    } catch (Exception e) {
      throw new BuilderException("Error parsing Mapper XML. The XML location is '" + resource + "'. Cause: " + e, e);
    }
  }
  //构建会话从上下文
  private void buildStatementFromContext(List<XNode> list) {
    if (configuration.getDatabaseId() != null) {
      buildStatementFromContext(list, configuration.getDatabaseId());
    }
    buildStatementFromContext(list, null);
  }
  //构建会话从上下文
  private void buildStatementFromContext(List<XNode> list, String requiredDatabaseId) {
    for (XNode context : list) {
      final XMLStatementBuilder statementParser = new XMLStatementBuilder(configuration, builderAssistant, context, requiredDatabaseId);
      try {
        statementParser.parseStatementNode();
      } catch (IncompleteElementException e) {
        configuration.addIncompleteStatement(statementParser);
      }
    }
  }
  //解析等待的结果映射
  private void parsePendingResultMaps() {
    Collection<ResultMapResolver> incompleteResultMaps = configuration.getIncompleteResultMaps();
    synchronized (incompleteResultMaps) {
      Iterator<ResultMapResolver> iter = incompleteResultMaps.iterator();
      while (iter.hasNext()) {
        try {
          iter.next().resolve();
          iter.remove();
        } catch (IncompleteElementException e) {
          // ResultMap is still missing a resource...
        }
      }
    }
  }
  //解析等待的缓存引用
  private void parsePendingCacheRefs() {
    Collection<CacheRefResolver> incompleteCacheRefs = configuration.getIncompleteCacheRefs();
    synchronized (incompleteCacheRefs) {
      Iterator<CacheRefResolver> iter = incompleteCacheRefs.iterator();
      while (iter.hasNext()) {
        try {
          iter.next().resolveCacheRef();
          iter.remove();
        } catch (IncompleteElementException e) {
          // Cache ref is still missing a resource...
        }
      }
    }
  }
  //解析等待的会话
  private void parsePendingStatements() {
    Collection<XMLStatementBuilder> incompleteStatements = configuration.getIncompleteStatements();
    synchronized (incompleteStatements) {
      Iterator<XMLStatementBuilder> iter = incompleteStatements.iterator();
      while (iter.hasNext()) {
        try {
          iter.next().parseStatementNode();
          iter.remove();
        } catch (IncompleteElementException e) {
          // Statement is still missing a resource...
        }
      }
    }
  }
  //缓存引用元素
  private void cacheRefElement(XNode context) {
    if (context != null) {
      configuration.addCacheRef(builderAssistant.getCurrentNamespace(), context.getStringAttribute("namespace"));
      CacheRefResolver cacheRefResolver = new CacheRefResolver(builderAssistant, context.getStringAttribute("namespace"));
      try {
        cacheRefResolver.resolveCacheRef();
      } catch (IncompleteElementException e) {
        configuration.addIncompleteCacheRef(cacheRefResolver);
      }
    }
  }
  //缓存元素
  private void cacheElement(XNode context) {
    if (context != null) {
      String type = context.getStringAttribute("type", "PERPETUAL");
      Class<? extends Cache> typeClass = typeAliasRegistry.resolveAlias(type);
      String eviction = context.getStringAttribute("eviction", "LRU");
      Class<? extends Cache> evictionClass = typeAliasRegistry.resolveAlias(eviction);
      Long flushInterval = context.getLongAttribute("flushInterval");
      Integer size = context.getIntAttribute("size");
      boolean readWrite = !context.getBooleanAttribute("readOnly", false);
      boolean blocking = context.getBooleanAttribute("blocking", false);
      Properties props = context.getChildrenAsProperties();
      builderAssistant.useNewCache(typeClass, evictionClass, flushInterval, size, readWrite, blocking, props);
    }
  }
  //参数映射元素
  private void parameterMapElement(List<XNode> list) {
    for (XNode parameterMapNode : list) {
      String id = parameterMapNode.getStringAttribute("id");
      String type = parameterMapNode.getStringAttribute("type");
      Class<?> parameterClass = resolveClass(type);
      List<XNode> parameterNodes = parameterMapNode.evalNodes("parameter");
      List<ParameterMapping> parameterMappings = new ArrayList<>();
      for (XNode parameterNode : parameterNodes) {
        String property = parameterNode.getStringAttribute("property");
        String javaType = parameterNode.getStringAttribute("javaType");
        String jdbcType = parameterNode.getStringAttribute("jdbcType");
        String resultMap = parameterNode.getStringAttribute("resultMap");
        String mode = parameterNode.getStringAttribute("mode");
        String typeHandler = parameterNode.getStringAttribute("typeHandler");
        Integer numericScale = parameterNode.getIntAttribute("numericScale");
        ParameterMode modeEnum = resolveParameterMode(mode);
        Class<?> javaTypeClass = resolveClass(javaType);
        JdbcType jdbcTypeEnum = resolveJdbcType(jdbcType);
        Class<? extends TypeHandler<?>> typeHandlerClass = resolveClass(typeHandler);
        ParameterMapping parameterMapping = builderAssistant.buildParameterMapping(parameterClass, property, javaTypeClass, jdbcTypeEnum, resultMap, modeEnum, typeHandlerClass, numericScale);
        parameterMappings.add(parameterMapping);
      }
      builderAssistant.addParameterMap(id, parameterClass, parameterMappings);
    }
  }
  //结果映射
  private void resultMapElements(List<XNode> list) throws Exception {
    for (XNode resultMapNode : list) {
      try {
        resultMapElement(resultMapNode);
      } catch (IncompleteElementException e) {
        // ignore, it will be retried
      }
    }
  }
  //结果映射
  private ResultMap resultMapElement(XNode resultMapNode) throws Exception {
    return resultMapElement(resultMapNode, Collections.emptyList(), null);
  }
  //结果映射
  private ResultMap resultMapElement(XNode resultMapNode, List<ResultMapping> additionalResultMappings, Class<?> enclosingType) throws Exception {
    ErrorContext.instance().activity("processing " + resultMapNode.getValueBasedIdentifier());
    String type = resultMapNode.getStringAttribute("type",
        resultMapNode.getStringAttribute("ofType",
            resultMapNode.getStringAttribute("resultType",
                resultMapNode.getStringAttribute("javaType"))));
    Class<?> typeClass = resolveClass(type);
    if (typeClass == null) {
      typeClass = inheritEnclosingType(resultMapNode, enclosingType);
    }
    Discriminator discriminator = null;
    List<ResultMapping> resultMappings = new ArrayList<>();
    resultMappings.addAll(additionalResultMappings);
    List<XNode> resultChildren = resultMapNode.getChildren();
    for (XNode resultChild : resultChildren) {
      if ("constructor".equals(resultChild.getName())) {
        processConstructorElement(resultChild, typeClass, resultMappings);
      } else if ("discriminator".equals(resultChild.getName())) {
        discriminator = processDiscriminatorElement(resultChild, typeClass, resultMappings);
      } else {
        List<ResultFlag> flags = new ArrayList<>();
        if ("id".equals(resultChild.getName())) {
          flags.add(ResultFlag.ID);
        }
        resultMappings.add(buildResultMappingFromContext(resultChild, typeClass, flags));
      }
    }
    String id = resultMapNode.getStringAttribute("id",
            resultMapNode.getValueBasedIdentifier());
    String extend = resultMapNode.getStringAttribute("extends");
    Boolean autoMapping = resultMapNode.getBooleanAttribute("autoMapping");
    ResultMapResolver resultMapResolver = new ResultMapResolver(builderAssistant, id, typeClass, extend, discriminator, resultMappings, autoMapping);
    try {
      return resultMapResolver.resolve();
    } catch (IncompleteElementException  e) {
      configuration.addIncompleteResultMap(resultMapResolver);
      throw e;
    }
  }
  //继承封装类型
  protected Class<?> inheritEnclosingType(XNode resultMapNode, Class<?> enclosingType) {
    if ("association".equals(resultMapNode.getName()) && resultMapNode.getStringAttribute("resultMap") == null) {
      String property = resultMapNode.getStringAttribute("property");
      if (property != null && enclosingType != null) {
        MetaClass metaResultType = MetaClass.forClass(enclosingType, configuration.getReflectorFactory());
        return metaResultType.getSetterType(property);
      }
    } else if ("case".equals(resultMapNode.getName()) && resultMapNode.getStringAttribute("resultMap") == null) {
      return enclosingType;
    }
    return null;
  }
  //执行构造元素
  private void processConstructorElement(XNode resultChild, Class<?> resultType, List<ResultMapping> resultMappings) throws Exception {
    List<XNode> argChildren = resultChild.getChildren();
    for (XNode argChild : argChildren) {
      List<ResultFlag> flags = new ArrayList<>();
      flags.add(ResultFlag.CONSTRUCTOR);
      if ("idArg".equals(argChild.getName())) {
        flags.add(ResultFlag.ID);
      }
      resultMappings.add(buildResultMappingFromContext(argChild, resultType, flags));
    }
  }
  //执行鉴别器元素
  private Discriminator processDiscriminatorElement(XNode context, Class<?> resultType, List<ResultMapping> resultMappings) throws Exception {
    String column = context.getStringAttribute("column");
    String javaType = context.getStringAttribute("javaType");
    String jdbcType = context.getStringAttribute("jdbcType");
    String typeHandler = context.getStringAttribute("typeHandler");
    Class<?> javaTypeClass = resolveClass(javaType);
    Class<? extends TypeHandler<?>> typeHandlerClass = resolveClass(typeHandler);
    JdbcType jdbcTypeEnum = resolveJdbcType(jdbcType);
    Map<String, String> discriminatorMap = new HashMap<>();
    for (XNode caseChild : context.getChildren()) {
      String value = caseChild.getStringAttribute("value");
      String resultMap = caseChild.getStringAttribute("resultMap", processNestedResultMappings(caseChild, resultMappings, resultType));
      discriminatorMap.put(value, resultMap);
    }
    return builderAssistant.buildDiscriminator(resultType, column, javaTypeClass, jdbcTypeEnum, typeHandlerClass, discriminatorMap);
  }
  //sql元素
  private void sqlElement(List<XNode> list) {
    if (configuration.getDatabaseId() != null) {
      sqlElement(list, configuration.getDatabaseId());
    }
    sqlElement(list, null);
  }
  //sql元素
  private void sqlElement(List<XNode> list, String requiredDatabaseId) {
    for (XNode context : list) {
      String databaseId = context.getStringAttribute("databaseId");
      String id = context.getStringAttribute("id");
      id = builderAssistant.applyCurrentNamespace(id, false);
      if (databaseIdMatchesCurrent(id, databaseId, requiredDatabaseId)) {
        sqlFragments.put(id, context);
      }
    }
  }
  //数据库ID匹配当前要求
  private boolean databaseIdMatchesCurrent(String id, String databaseId, String requiredDatabaseId) {
    if (requiredDatabaseId != null) {
      return requiredDatabaseId.equals(databaseId);
    }
    if (databaseId != null) {
      return false;
    }
    if (!this.sqlFragments.containsKey(id)) {
      return true;
    }
    // skip this fragment if there is a previous one with a not null databaseId
    XNode context = this.sqlFragments.get(id);
    return context.getStringAttribute("databaseId") == null;
  } 
  //从上下文构建结果映射
  private ResultMapping buildResultMappingFromContext(XNode context, Class<?> resultType, List<ResultFlag> flags) throws Exception {
    String property;
    if (flags.contains(ResultFlag.CONSTRUCTOR)) {
      property = context.getStringAttribute("name");
    } else {
      property = context.getStringAttribute("property");
    }
    String column = context.getStringAttribute("column");
    String javaType = context.getStringAttribute("javaType");
    String jdbcType = context.getStringAttribute("jdbcType");
    String nestedSelect = context.getStringAttribute("select");
    String nestedResultMap = context.getStringAttribute("resultMap",
        processNestedResultMappings(context, Collections.emptyList(), resultType));
    String notNullColumn = context.getStringAttribute("notNullColumn");
    String columnPrefix = context.getStringAttribute("columnPrefix");
    String typeHandler = context.getStringAttribute("typeHandler");
    String resultSet = context.getStringAttribute("resultSet");
    String foreignColumn = context.getStringAttribute("foreignColumn");
    boolean lazy = "lazy".equals(context.getStringAttribute("fetchType", configuration.isLazyLoadingEnabled() ? "lazy" : "eager"));
    Class<?> javaTypeClass = resolveClass(javaType);
    Class<? extends TypeHandler<?>> typeHandlerClass = resolveClass(typeHandler);
    JdbcType jdbcTypeEnum = resolveJdbcType(jdbcType);
    return builderAssistant.buildResultMapping(resultType, property, column, javaTypeClass, jdbcTypeEnum, nestedSelect, nestedResultMap, notNullColumn, columnPrefix, typeHandlerClass, flags, resultSet, foreignColumn, lazy);
  }
  //执行嵌套结果映射
  private String processNestedResultMappings(XNode context, List<ResultMapping> resultMappings, Class<?> enclosingType) throws Exception {
    if ("association".equals(context.getName())
        || "collection".equals(context.getName())
        || "case".equals(context.getName())) {
      if (context.getStringAttribute("select") == null) {
        validateCollection(context, enclosingType);
        ResultMap resultMap = resultMapElement(context, resultMappings, enclosingType);
        return resultMap.getId();
      }
    }
    return null;
  }
  //校验集合
  protected void validateCollection(XNode context, Class<?> enclosingType) {
    if ("collection".equals(context.getName()) && context.getStringAttribute("resultMap") == null
        && context.getStringAttribute("javaType") == null) {
      MetaClass metaResultType = MetaClass.forClass(enclosingType, configuration.getReflectorFactory());
      String property = context.getStringAttribute("property");
      if (!metaResultType.hasSetter(property)) {
        throw new BuilderException(
          "Ambiguous collection type for property '" + property + "'. You must specify 'javaType' or 'resultMap'.");
      }
    }
  }
  //构建命名空间的mapper
  private void bindMapperForNamespace() {
    String namespace = builderAssistant.getCurrentNamespace();
    if (namespace != null) {
      Class<?> boundType = null;
      try {
        boundType = Resources.classForName(namespace);
      } catch (ClassNotFoundException e) {
        //ignore, bound type is not required
      }
      if (boundType != null) {
        if (!configuration.hasMapper(boundType)) {
          // Spring may not know the real resource name so we set a flag
          // to prevent loading again this resource from the mapper interface
          // look at MapperAnnotationBuilder#loadXmlResource
          configuration.addLoadedResource("namespace:" + namespace);
          configuration.addMapper(boundType);
        }
      }
    }
  }

}
```

#### 4.1.3.4、XMLStatementBuilder会话构建器

```java
/**
 * xml会话构建器
 * @author Clinton Begin
 */
public class XMLStatementBuilder extends BaseBuilder {
   //mapper接口构建器助手
  private final MapperBuilderAssistant builderAssistant;
  //上下文
  private final XNode context;
  //要求的数据库ID
  private final String requiredDatabaseId;
  //构造函数
  public XMLStatementBuilder(Configuration configuration, MapperBuilderAssistant builderAssistant, XNode context) {
    this(configuration, builderAssistant, context, null);
  }
  //构造函数
  public XMLStatementBuilder(Configuration configuration, MapperBuilderAssistant builderAssistant, XNode context, String databaseId) {
    super(configuration);
    this.builderAssistant = builderAssistant;
    this.context = context;
    this.requiredDatabaseId = databaseId;
  }
  //解析会话节点
  public void parseStatementNode() {
    String id = context.getStringAttribute("id");
    String databaseId = context.getStringAttribute("databaseId");

    if (!databaseIdMatchesCurrent(id, databaseId, this.requiredDatabaseId)) {
      return;
    }

    String nodeName = context.getNode().getNodeName();
    SqlCommandType sqlCommandType = SqlCommandType.valueOf(nodeName.toUpperCase(Locale.ENGLISH));
    boolean isSelect = sqlCommandType == SqlCommandType.SELECT;
    boolean flushCache = context.getBooleanAttribute("flushCache", !isSelect);
    boolean useCache = context.getBooleanAttribute("useCache", isSelect);
    boolean resultOrdered = context.getBooleanAttribute("resultOrdered", false);

    // Include Fragments before parsing
    XMLIncludeTransformer includeParser = new XMLIncludeTransformer(configuration, builderAssistant);
    includeParser.applyIncludes(context.getNode());

    String parameterType = context.getStringAttribute("parameterType");
    Class<?> parameterTypeClass = resolveClass(parameterType);

    String lang = context.getStringAttribute("lang");
    LanguageDriver langDriver = getLanguageDriver(lang);

    // Parse selectKey after includes and remove them.
    processSelectKeyNodes(id, parameterTypeClass, langDriver);

    // Parse the SQL (pre: <selectKey> and <include> were parsed and removed)
    KeyGenerator keyGenerator;
    String keyStatementId = id + SelectKeyGenerator.SELECT_KEY_SUFFIX;
    keyStatementId = builderAssistant.applyCurrentNamespace(keyStatementId, true);
    if (configuration.hasKeyGenerator(keyStatementId)) {
      keyGenerator = configuration.getKeyGenerator(keyStatementId);
    } else {
      keyGenerator = context.getBooleanAttribute("useGeneratedKeys",
          configuration.isUseGeneratedKeys() && SqlCommandType.INSERT.equals(sqlCommandType))
          ? Jdbc3KeyGenerator.INSTANCE : NoKeyGenerator.INSTANCE;
    }

    SqlSource sqlSource = langDriver.createSqlSource(configuration, context, parameterTypeClass);
    StatementType statementType = StatementType.valueOf(context.getStringAttribute("statementType", StatementType.PREPARED.toString()));
    Integer fetchSize = context.getIntAttribute("fetchSize");
    Integer timeout = context.getIntAttribute("timeout");
    String parameterMap = context.getStringAttribute("parameterMap");
    String resultType = context.getStringAttribute("resultType");
    Class<?> resultTypeClass = resolveClass(resultType);
    String resultMap = context.getStringAttribute("resultMap");
    String resultSetType = context.getStringAttribute("resultSetType");
    ResultSetType resultSetTypeEnum = resolveResultSetType(resultSetType);
    if (resultSetTypeEnum == null) {
      resultSetTypeEnum = configuration.getDefaultResultSetType();
    }
    String keyProperty = context.getStringAttribute("keyProperty");
    String keyColumn = context.getStringAttribute("keyColumn");
    String resultSets = context.getStringAttribute("resultSets");

    builderAssistant.addMappedStatement(id, sqlSource, statementType, sqlCommandType,
        fetchSize, timeout, parameterMap, parameterTypeClass, resultMap, resultTypeClass,
        resultSetTypeEnum, flushCache, useCache, resultOrdered,
        keyGenerator, keyProperty, keyColumn, databaseId, langDriver, resultSets);
  }
   //执行selectKey节点
  private void processSelectKeyNodes(String id, Class<?> parameterTypeClass, LanguageDriver langDriver) {
    List<XNode> selectKeyNodes = context.evalNodes("selectKey");
    if (configuration.getDatabaseId() != null) {
      parseSelectKeyNodes(id, selectKeyNodes, parameterTypeClass, langDriver, configuration.getDatabaseId());
    }
    parseSelectKeyNodes(id, selectKeyNodes, parameterTypeClass, langDriver, null);
    removeSelectKeyNodes(selectKeyNodes);
  }
   //解析selectKey节点
  private void parseSelectKeyNodes(String parentId, List<XNode> list, Class<?> parameterTypeClass, LanguageDriver langDriver, String skRequiredDatabaseId) {
    for (XNode nodeToHandle : list) {
      String id = parentId + SelectKeyGenerator.SELECT_KEY_SUFFIX;
      String databaseId = nodeToHandle.getStringAttribute("databaseId");
      if (databaseIdMatchesCurrent(id, databaseId, skRequiredDatabaseId)) {
        parseSelectKeyNode(id, nodeToHandle, parameterTypeClass, langDriver, databaseId);
      }
    }
  }
   //解析selectKey节点
  private void parseSelectKeyNode(String id, XNode nodeToHandle, Class<?> parameterTypeClass, LanguageDriver langDriver, String databaseId) {
    String resultType = nodeToHandle.getStringAttribute("resultType");
    Class<?> resultTypeClass = resolveClass(resultType);
    StatementType statementType = StatementType.valueOf(nodeToHandle.getStringAttribute("statementType", StatementType.PREPARED.toString()));
    String keyProperty = nodeToHandle.getStringAttribute("keyProperty");
    String keyColumn = nodeToHandle.getStringAttribute("keyColumn");
    boolean executeBefore = "BEFORE".equals(nodeToHandle.getStringAttribute("order", "AFTER"));

    //defaults
    boolean useCache = false;
    boolean resultOrdered = false;
    KeyGenerator keyGenerator = NoKeyGenerator.INSTANCE;
    Integer fetchSize = null;
    Integer timeout = null;
    boolean flushCache = false;
    String parameterMap = null;
    String resultMap = null;
    ResultSetType resultSetTypeEnum = null;

    SqlSource sqlSource = langDriver.createSqlSource(configuration, nodeToHandle, parameterTypeClass);
    SqlCommandType sqlCommandType = SqlCommandType.SELECT;

    builderAssistant.addMappedStatement(id, sqlSource, statementType, sqlCommandType,
        fetchSize, timeout, parameterMap, parameterTypeClass, resultMap, resultTypeClass,
        resultSetTypeEnum, flushCache, useCache, resultOrdered,
        keyGenerator, keyProperty, keyColumn, databaseId, langDriver, null);

    id = builderAssistant.applyCurrentNamespace(id, false);

    MappedStatement keyStatement = configuration.getMappedStatement(id, false);
    configuration.addKeyGenerator(id, new SelectKeyGenerator(keyStatement, executeBefore));
  }
  //移除selectKey节点
  private void removeSelectKeyNodes(List<XNode> selectKeyNodes) {
    for (XNode nodeToHandle : selectKeyNodes) {
      nodeToHandle.getParent().getNode().removeChild(nodeToHandle.getNode());
    }
  }
  //数据库ID匹配当前ID
  private boolean databaseIdMatchesCurrent(String id, String databaseId, String requiredDatabaseId) {
    if (requiredDatabaseId != null) {
      return requiredDatabaseId.equals(databaseId);
    }
    if (databaseId != null) {
      return false;
    }
    id = builderAssistant.applyCurrentNamespace(id, false);
    if (!this.configuration.hasStatement(id, false)) {
      return true;
    }
    // skip this statement if there is a previous one with a not null databaseId
    MappedStatement previous = this.configuration.getMappedStatement(id, false); // issue #2
    return previous.getDatabaseId() == null;
  }
  //获取语言驱动
  private LanguageDriver getLanguageDriver(String lang) {
    Class<? extends LanguageDriver> langClass = null;
    if (lang != null) {
      langClass = resolveClass(lang);
    }
    return configuration.getLanguageDriver(langClass);
  }

}
```

#### 4.1.3.5、XMLMapperEntityResolver接口实体解析器

```java
/**
 * mybatis DTDs 线下实体解析
 * Offline entity resolver for the MyBatis DTDs.
 *
 * @author Clinton Begin
 * @author Eduardo Macarron
 */
public class XMLMapperEntityResolver implements EntityResolver {
  //ibatis-3-config.dtd
  private static final String IBATIS_CONFIG_SYSTEM = "ibatis-3-config.dtd";
  //ibatis-3-mapper.dtd
  private static final String IBATIS_MAPPER_SYSTEM = "ibatis-3-mapper.dtd";
  //mybatis-3-config.dtd
  private static final String MYBATIS_CONFIG_SYSTEM = "mybatis-3-config.dtd";
  //mybatis-3-mapper.dtd
  private static final String MYBATIS_MAPPER_SYSTEM = "mybatis-3-mapper.dtd";
  //协议
  private static final String MYBATIS_CONFIG_DTD = "org/apache/ibatis/builder/xml/mybatis-3-config.dtd";
  private static final String MYBATIS_MAPPER_DTD = "org/apache/ibatis/builder/xml/mybatis-3-mapper.dtd";

  /**
   * 转换一个公共的DTD成本地的
   * Converts a public DTD into a local one.
   *
   * @param publicId The public id that is what comes after "PUBLIC"
   * @param systemId The system id that is what comes after the public id.
   * @return The InputSource for the DTD
   *
   * @throws org.xml.sax.SAXException If anything goes wrong
   */
  @Override
  public InputSource resolveEntity(String publicId, String systemId) throws SAXException {
    try {
      if (systemId != null) {
        String lowerCaseSystemId = systemId.toLowerCase(Locale.ENGLISH);
        if (lowerCaseSystemId.contains(MYBATIS_CONFIG_SYSTEM) || lowerCaseSystemId.contains(IBATIS_CONFIG_SYSTEM)) {
          return getInputSource(MYBATIS_CONFIG_DTD, publicId, systemId);
        } else if (lowerCaseSystemId.contains(MYBATIS_MAPPER_SYSTEM) || lowerCaseSystemId.contains(IBATIS_MAPPER_SYSTEM)) {
          return getInputSource(MYBATIS_MAPPER_DTD, publicId, systemId);
        }
      }
      return null;
    } catch (Exception e) {
      throw new SAXException(e.toString());
    }
  }
  //获取输入源
  private InputSource getInputSource(String path, String publicId, String systemId) {
    InputSource source = null;
    if (path != null) {
      try {
        InputStream in = Resources.getResourceAsStream(path);
        source = new InputSource(in);
        source.setPublicId(publicId);
        source.setSystemId(systemId);
      } catch (IOException e) {
        // ignore, null is ok
      }
    }
    return source;
  }

}
```

#### 4.1.3.6、mybatis-3-config.dtd

```xml-dtd
<?xml version="1.0" encoding="UTF-8" ?>
<!--

       Copyright 2009-2016 the original author or authors.

       Licensed under the Apache License, Version 2.0 (the "License");
       you may not use this file except in compliance with the License.
       You may obtain a copy of the License at

          http://www.apache.org/licenses/LICENSE-2.0

       Unless required by applicable law or agreed to in writing, software
       distributed under the License is distributed on an "AS IS" BASIS,
       WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
       See the License for the specific language governing permissions and
       limitations under the License.

-->
<!ELEMENT configuration (properties?, settings?, typeAliases?, typeHandlers?, objectFactory?, objectWrapperFactory?, reflectorFactory?, plugins?, environments?, databaseIdProvider?, mappers?)>
<!--全局配置标签（属性，设置，别名，类型处理器，对象工厂，对象包装工厂，反射工厂，插件，环境，数据库ID服务，mapper接口） -->
<!ELEMENT databaseIdProvider (property*)>
<!-- 必添类型-->
<!ATTLIST databaseIdProvider
type CDATA #REQUIRED
>
<!-- 属性资源 -->
<!ELEMENT properties (property*)>
<!ATTLIST properties
resource CDATA #IMPLIED
url CDATA #IMPLIED
>
<!--属性不为空 -->
<!ELEMENT property EMPTY>
<!ATTLIST property
name CDATA #REQUIRED
value CDATA #REQUIRED
>
<!-- 设置不为空 -->
<!ELEMENT settings (setting+)>

<!ELEMENT setting EMPTY>
<!ATTLIST setting
name CDATA #REQUIRED
value CDATA #REQUIRED
>
<!--别名和包名不为空 -->
<!ELEMENT typeAliases (typeAlias*,package*)>

<!ELEMENT typeAlias EMPTY>
<!ATTLIST typeAlias
type CDATA #REQUIRED
alias CDATA #IMPLIED
>
<!--类型处理器 -->
<!ELEMENT typeHandlers (typeHandler*,package*)>

<!ELEMENT typeHandler EMPTY>
<!ATTLIST typeHandler
javaType CDATA #IMPLIED
jdbcType CDATA #IMPLIED
handler CDATA #REQUIRED
>
<!-- 对象工厂 -->
<!ELEMENT objectFactory (property*)>
<!ATTLIST objectFactory
type CDATA #REQUIRED
>
<!--对象包装工厂 -->
<!ELEMENT objectWrapperFactory EMPTY>
<!ATTLIST objectWrapperFactory
type CDATA #REQUIRED
>
<!-- 反射工厂 -->
<!ELEMENT reflectorFactory EMPTY>
<!ATTLIST reflectorFactory
type CDATA #REQUIRED
>
<!--插件-->
<!ELEMENT plugins (plugin+)>

<!ELEMENT plugin (property*)>
<!ATTLIST plugin
interceptor CDATA #REQUIRED
>
<!--环境 -->
<!ELEMENT environments (environment+)>
<!ATTLIST environments
default CDATA #REQUIRED
>

<!ELEMENT environment (transactionManager,dataSource)>
<!ATTLIST environment
id CDATA #REQUIRED
>
<!--事务 -->
<!ELEMENT transactionManager (property*)>
<!ATTLIST transactionManager
type CDATA #REQUIRED
>
<!--数据源 -->
<!ELEMENT dataSource (property*)>
<!ATTLIST dataSource
type CDATA #REQUIRED
>
<!--接口 -->
<!ELEMENT mappers (mapper*,package*)>

<!ELEMENT mapper EMPTY>
<!ATTLIST mapper
resource CDATA #IMPLIED
url CDATA #IMPLIED
class CDATA #IMPLIED
>

<!ELEMENT package EMPTY>
<!ATTLIST package
name CDATA #REQUIRED
>
```

#### 4.1.3.7、mybatis-3-mapper.dtd

```xml-dtd
<?xml version="1.0" encoding="UTF-8" ?>
<!--

       Copyright 2009-2018 the original author or authors.

       Licensed under the Apache License, Version 2.0 (the "License");
       you may not use this file except in compliance with the License.
       You may obtain a copy of the License at

          http://www.apache.org/licenses/LICENSE-2.0

       Unless required by applicable law or agreed to in writing, software
       distributed under the License is distributed on an "AS IS" BASIS,
       WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
       See the License for the specific language governing permissions and
       limitations under the License.

-->
<!ELEMENT mapper (cache-ref | cache | resultMap* | parameterMap* | sql* | insert* | update* | delete* | select* )+>

<!ATTLIST mapper
namespace CDATA #IMPLIED
>

<!ELEMENT cache-ref EMPTY>
<!ATTLIST cache-ref
namespace CDATA #REQUIRED
>

<!ELEMENT cache (property*)>
<!ATTLIST cache
type CDATA #IMPLIED
eviction CDATA #IMPLIED
flushInterval CDATA #IMPLIED
size CDATA #IMPLIED
readOnly CDATA #IMPLIED
blocking CDATA #IMPLIED
>

<!ELEMENT parameterMap (parameter+)?>
<!ATTLIST parameterMap
id CDATA #REQUIRED
type CDATA #REQUIRED
>

<!ELEMENT parameter EMPTY>
<!ATTLIST parameter
property CDATA #REQUIRED
javaType CDATA #IMPLIED
jdbcType CDATA #IMPLIED
mode (IN | OUT | INOUT) #IMPLIED
resultMap CDATA #IMPLIED
scale CDATA #IMPLIED
typeHandler CDATA #IMPLIED
>

<!ELEMENT resultMap (constructor?,id*,result*,association*,collection*, discriminator?)>
<!ATTLIST resultMap
id CDATA #REQUIRED
type CDATA #REQUIRED
extends CDATA #IMPLIED
autoMapping (true|false) #IMPLIED
>

<!ELEMENT constructor (idArg*,arg*)>

<!ELEMENT id EMPTY>
<!ATTLIST id
property CDATA #IMPLIED
javaType CDATA #IMPLIED
column CDATA #IMPLIED
jdbcType CDATA #IMPLIED
typeHandler CDATA #IMPLIED
>

<!ELEMENT result EMPTY>
<!ATTLIST result
property CDATA #IMPLIED
javaType CDATA #IMPLIED
column CDATA #IMPLIED
jdbcType CDATA #IMPLIED
typeHandler CDATA #IMPLIED
>

<!ELEMENT idArg EMPTY>
<!ATTLIST idArg
javaType CDATA #IMPLIED
column CDATA #IMPLIED
jdbcType CDATA #IMPLIED
typeHandler CDATA #IMPLIED
select CDATA #IMPLIED
resultMap CDATA #IMPLIED
name CDATA #IMPLIED
columnPrefix CDATA #IMPLIED
>

<!ELEMENT arg EMPTY>
<!ATTLIST arg
javaType CDATA #IMPLIED
column CDATA #IMPLIED
jdbcType CDATA #IMPLIED
typeHandler CDATA #IMPLIED
select CDATA #IMPLIED
resultMap CDATA #IMPLIED
name CDATA #IMPLIED
columnPrefix CDATA #IMPLIED
>

<!ELEMENT collection (constructor?,id*,result*,association*,collection*, discriminator?)>
<!ATTLIST collection
property CDATA #REQUIRED
column CDATA #IMPLIED
javaType CDATA #IMPLIED
ofType CDATA #IMPLIED
jdbcType CDATA #IMPLIED
select CDATA #IMPLIED
resultMap CDATA #IMPLIED
typeHandler CDATA #IMPLIED
notNullColumn CDATA #IMPLIED
columnPrefix CDATA #IMPLIED
resultSet CDATA #IMPLIED
foreignColumn CDATA #IMPLIED
autoMapping (true|false) #IMPLIED
fetchType (lazy|eager) #IMPLIED
>

<!ELEMENT association (constructor?,id*,result*,association*,collection*, discriminator?)>
<!ATTLIST association
property CDATA #REQUIRED
column CDATA #IMPLIED
javaType CDATA #IMPLIED
jdbcType CDATA #IMPLIED
select CDATA #IMPLIED
resultMap CDATA #IMPLIED
typeHandler CDATA #IMPLIED
notNullColumn CDATA #IMPLIED
columnPrefix CDATA #IMPLIED
resultSet CDATA #IMPLIED
foreignColumn CDATA #IMPLIED
autoMapping (true|false) #IMPLIED
fetchType (lazy|eager) #IMPLIED
>

<!ELEMENT discriminator (case+)>
<!ATTLIST discriminator
column CDATA #IMPLIED
javaType CDATA #REQUIRED
jdbcType CDATA #IMPLIED
typeHandler CDATA #IMPLIED
>

<!ELEMENT case (constructor?,id*,result*,association*,collection*, discriminator?)>
<!ATTLIST case
value CDATA #REQUIRED
resultMap CDATA #IMPLIED
resultType CDATA #IMPLIED
>

<!ELEMENT property EMPTY>
<!ATTLIST property
name CDATA #REQUIRED
value CDATA #REQUIRED
>

<!ELEMENT typeAlias EMPTY>
<!ATTLIST typeAlias
alias CDATA #REQUIRED
type CDATA #REQUIRED
>

<!ELEMENT select (#PCDATA | include | trim | where | set | foreach | choose | if | bind)*>
<!ATTLIST select
id CDATA #REQUIRED
parameterMap CDATA #IMPLIED
parameterType CDATA #IMPLIED
resultMap CDATA #IMPLIED
resultType CDATA #IMPLIED
resultSetType (FORWARD_ONLY | SCROLL_INSENSITIVE | SCROLL_SENSITIVE | DEFAULT) #IMPLIED
statementType (STATEMENT|PREPARED|CALLABLE) #IMPLIED
fetchSize CDATA #IMPLIED
timeout CDATA #IMPLIED
flushCache (true|false) #IMPLIED
useCache (true|false) #IMPLIED
databaseId CDATA #IMPLIED
lang CDATA #IMPLIED
resultOrdered (true|false) #IMPLIED
resultSets CDATA #IMPLIED 
>

<!ELEMENT insert (#PCDATA | selectKey | include | trim | where | set | foreach | choose | if | bind)*>
<!ATTLIST insert
id CDATA #REQUIRED
parameterMap CDATA #IMPLIED
parameterType CDATA #IMPLIED
timeout CDATA #IMPLIED
flushCache (true|false) #IMPLIED
statementType (STATEMENT|PREPARED|CALLABLE) #IMPLIED
keyProperty CDATA #IMPLIED
useGeneratedKeys (true|false) #IMPLIED
keyColumn CDATA #IMPLIED
databaseId CDATA #IMPLIED
lang CDATA #IMPLIED
>

<!ELEMENT selectKey (#PCDATA | include | trim | where | set | foreach | choose | if | bind)*>
<!ATTLIST selectKey
resultType CDATA #IMPLIED
statementType (STATEMENT|PREPARED|CALLABLE) #IMPLIED
keyProperty CDATA #IMPLIED
keyColumn CDATA #IMPLIED
order (BEFORE|AFTER) #IMPLIED
databaseId CDATA #IMPLIED
>

<!ELEMENT update (#PCDATA | selectKey | include | trim | where | set | foreach | choose | if | bind)*>
<!ATTLIST update
id CDATA #REQUIRED
parameterMap CDATA #IMPLIED
parameterType CDATA #IMPLIED
timeout CDATA #IMPLIED
flushCache (true|false) #IMPLIED
statementType (STATEMENT|PREPARED|CALLABLE) #IMPLIED
keyProperty CDATA #IMPLIED
useGeneratedKeys (true|false) #IMPLIED
keyColumn CDATA #IMPLIED
databaseId CDATA #IMPLIED
lang CDATA #IMPLIED
>

<!ELEMENT delete (#PCDATA | include | trim | where | set | foreach | choose | if | bind)*>
<!ATTLIST delete
id CDATA #REQUIRED
parameterMap CDATA #IMPLIED
parameterType CDATA #IMPLIED
timeout CDATA #IMPLIED
flushCache (true|false) #IMPLIED
statementType (STATEMENT|PREPARED|CALLABLE) #IMPLIED
databaseId CDATA #IMPLIED
lang CDATA #IMPLIED
>

<!-- Dynamic -->

<!ELEMENT include (property+)?>
<!ATTLIST include
refid CDATA #REQUIRED
>

<!ELEMENT bind EMPTY>
<!ATTLIST bind
 name CDATA #REQUIRED
 value CDATA #REQUIRED
>

<!ELEMENT sql (#PCDATA | include | trim | where | set | foreach | choose | if | bind)*>
<!ATTLIST sql
id CDATA #REQUIRED
lang CDATA #IMPLIED
databaseId CDATA #IMPLIED
>

<!ELEMENT trim (#PCDATA | include | trim | where | set | foreach | choose | if | bind)*>
<!ATTLIST trim
prefix CDATA #IMPLIED
prefixOverrides CDATA #IMPLIED
suffix CDATA #IMPLIED
suffixOverrides CDATA #IMPLIED
>
<!ELEMENT where (#PCDATA | include | trim | where | set | foreach | choose | if | bind)*>
<!ELEMENT set (#PCDATA | include | trim | where | set | foreach | choose | if | bind)*>

<!ELEMENT foreach (#PCDATA | include | trim | where | set | foreach | choose | if | bind)*>
<!ATTLIST foreach
collection CDATA #REQUIRED
item CDATA #IMPLIED
index CDATA #IMPLIED
open CDATA #IMPLIED
close CDATA #IMPLIED
separator CDATA #IMPLIED
>

<!ELEMENT choose (when* , otherwise?)>
<!ELEMENT when (#PCDATA | include | trim | where | set | foreach | choose | if | bind)*>
<!ATTLIST when
test CDATA #REQUIRED
>
<!ELEMENT otherwise (#PCDATA | include | trim | where | set | foreach | choose | if | bind)*>

<!ELEMENT if (#PCDATA | include | trim | where | set | foreach | choose | if | bind)*>
<!ATTLIST if
test CDATA #REQUIRED
>
```

#### 4.1.3.8、mybatis-config.xsd

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<!--

       Copyright 2009-2017 the original author or authors.

       Licensed under the Apache License, Version 2.0 (the "License");
       you may not use this file except in compliance with the License.
       You may obtain a copy of the License at

          http://www.apache.org/licenses/LICENSE-2.0

       Unless required by applicable law or agreed to in writing, software
       distributed under the License is distributed on an "AS IS" BASIS,
       WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
       See the License for the specific language governing permissions and
       limitations under the License.

-->
<xs:schema
  xmlns="http://mybatis.org/schema/mybatis-config"
  xmlns:xs="http://www.w3.org/2001/XMLSchema"
  targetNamespace="http://mybatis.org/schema/mybatis-config"
  elementFormDefault="qualified">
  <xs:element name="configuration">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" ref="properties"/>
        <xs:element minOccurs="0" ref="settings"/>
        <xs:element minOccurs="0" ref="typeAliases"/>
        <xs:element minOccurs="0" ref="typeHandlers"/>
        <xs:element minOccurs="0" ref="objectFactory"/>
        <xs:element minOccurs="0" ref="objectWrapperFactory"/>
        <xs:element minOccurs="0" ref="reflectorFactory"/>
        <xs:element minOccurs="0" ref="plugins"/>
        <xs:element minOccurs="0" ref="environments"/>
        <xs:element minOccurs="0" ref="databaseIdProvider"/>
        <xs:element minOccurs="0" ref="mappers"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
  <xs:element name="databaseIdProvider">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="property"/>
      </xs:sequence>
      <xs:attribute name="type" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="properties">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="property"/>
      </xs:sequence>
      <xs:attribute name="resource"/>
      <xs:attribute name="url"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="property">
    <xs:complexType>
      <xs:attribute name="name" use="required"/>
      <xs:attribute name="value" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="settings">
    <xs:complexType>
      <xs:sequence>
        <xs:element maxOccurs="unbounded" ref="setting"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
  <xs:element name="setting">
    <xs:complexType>
      <xs:attribute name="name" use="required"/>
      <xs:attribute name="value" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="typeAliases">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="typeAlias"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="package"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
  <xs:element name="typeAlias">
    <xs:complexType>
      <xs:attribute name="type" use="required"/>
      <xs:attribute name="alias"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="typeHandlers">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="typeHandler"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="package"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
  <xs:element name="typeHandler">
    <xs:complexType>
      <xs:attribute name="javaType"/>
      <xs:attribute name="jdbcType"/>
      <xs:attribute name="handler" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="objectFactory">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="property"/>
      </xs:sequence>
      <xs:attribute name="type" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="objectWrapperFactory">
    <xs:complexType>
      <xs:attribute name="type" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="reflectorFactory">
    <xs:complexType>
      <xs:attribute name="type" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="plugins">
    <xs:complexType>
      <xs:sequence>
        <xs:element maxOccurs="unbounded" ref="plugin"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
  <xs:element name="plugin">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="property"/>
      </xs:sequence>
      <xs:attribute name="interceptor" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="environments">
    <xs:complexType>
      <xs:sequence>
        <xs:element maxOccurs="unbounded" ref="environment"/>
      </xs:sequence>
      <xs:attribute name="default" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="environment">
    <xs:complexType>
      <xs:sequence>
        <xs:element ref="transactionManager"/>
        <xs:element ref="dataSource"/>
      </xs:sequence>
      <xs:attribute name="id" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="transactionManager">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="property"/>
      </xs:sequence>
      <xs:attribute name="type" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="dataSource">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="property"/>
      </xs:sequence>
      <xs:attribute name="type" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="mappers">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="mapper"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="package"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
  <xs:element name="mapper">
    <xs:complexType>
      <xs:attribute name="resource"/>
      <xs:attribute name="url"/>
      <xs:attribute name="class"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="package">
    <xs:complexType>
      <xs:attribute name="name" use="required"/>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

#### 4.1.3.9、mybatis-mapper.xsd

```xml-dtd
<?xml version="1.0" encoding="UTF-8"?>
<!--

       Copyright 2009-2018 the original author or authors.

       Licensed under the Apache License, Version 2.0 (the "License");
       you may not use this file except in compliance with the License.
       You may obtain a copy of the License at

          http://www.apache.org/licenses/LICENSE-2.0

       Unless required by applicable law or agreed to in writing, software
       distributed under the License is distributed on an "AS IS" BASIS,
       WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
       See the License for the specific language governing permissions and
       limitations under the License.

-->
<xs:schema
  xmlns="http://mybatis.org/schema/mybatis-mapper"
  xmlns:xs="http://www.w3.org/2001/XMLSchema"
  targetNamespace="http://mybatis.org/schema/mybatis-mapper"
  elementFormDefault="qualified">
  <xs:element name="mapper">
    <xs:complexType>
      <xs:choice maxOccurs="unbounded">
        <xs:element ref="cache-ref"/>
        <xs:element ref="cache"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="resultMap"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="parameterMap"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="sql"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="insert"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="update"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="delete"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="select"/>
      </xs:choice>
      <xs:attribute name="namespace"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="cache-ref">
    <xs:complexType>
      <xs:attribute name="namespace" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="cache">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="property"/>
      </xs:sequence>
      <xs:attribute name="type"/>
      <xs:attribute name="eviction"/>
      <xs:attribute name="flushInterval"/>
      <xs:attribute name="size"/>
      <xs:attribute name="readOnly"/>
      <xs:attribute name="blocking"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="parameterMap">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="parameter"/>
      </xs:sequence>
      <xs:attribute name="id" use="required"/>
      <xs:attribute name="type" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="parameter">
    <xs:complexType>
      <xs:attribute name="property" use="required"/>
      <xs:attribute name="javaType"/>
      <xs:attribute name="jdbcType"/>
      <xs:attribute name="mode">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="IN"/>
            <xs:enumeration value="OUT"/>
            <xs:enumeration value="INOUT"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="resultMap"/>
      <xs:attribute name="scale"/>
      <xs:attribute name="typeHandler"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="resultMap">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" ref="constructor"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="id"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="result"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="association"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="collection"/>
        <xs:element minOccurs="0" ref="discriminator"/>
      </xs:sequence>
      <xs:attribute name="id" use="required"/>
      <xs:attribute name="type" use="required"/>
      <xs:attribute name="extends"/>
      <xs:attribute name="autoMapping">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="true"/>
            <xs:enumeration value="false"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
    </xs:complexType>
  </xs:element>
  <xs:element name="constructor">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="idArg"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="arg"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
  <xs:element name="id">
    <xs:complexType>
      <xs:attribute name="property"/>
      <xs:attribute name="javaType"/>
      <xs:attribute name="column"/>
      <xs:attribute name="jdbcType"/>
      <xs:attribute name="typeHandler"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="result">
    <xs:complexType>
      <xs:attribute name="property"/>
      <xs:attribute name="javaType"/>
      <xs:attribute name="column"/>
      <xs:attribute name="jdbcType"/>
      <xs:attribute name="typeHandler"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="idArg">
    <xs:complexType>
      <xs:attribute name="javaType"/>
      <xs:attribute name="column"/>
      <xs:attribute name="jdbcType"/>
      <xs:attribute name="typeHandler"/>
      <xs:attribute name="select"/>
      <xs:attribute name="resultMap"/>
      <xs:attribute name="name"/>
      <xs:attribute name="columnPrefix"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="arg">
    <xs:complexType>
      <xs:attribute name="javaType"/>
      <xs:attribute name="column"/>
      <xs:attribute name="jdbcType"/>
      <xs:attribute name="typeHandler"/>
      <xs:attribute name="select"/>
      <xs:attribute name="resultMap"/>
      <xs:attribute name="name"/>
      <xs:attribute name="columnPrefix"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="collection">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" ref="constructor"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="id"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="result"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="association"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="collection"/>
        <xs:element minOccurs="0" ref="discriminator"/>
      </xs:sequence>
      <xs:attribute name="property" use="required"/>
      <xs:attribute name="column"/>
      <xs:attribute name="javaType"/>
      <xs:attribute name="ofType"/>
      <xs:attribute name="jdbcType"/>
      <xs:attribute name="select"/>
      <xs:attribute name="resultMap"/>
      <xs:attribute name="typeHandler"/>
      <xs:attribute name="notNullColumn"/>
      <xs:attribute name="columnPrefix"/>
      <xs:attribute name="resultSet"/>
      <xs:attribute name="foreignColumn"/>
      <xs:attribute name="autoMapping">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="true"/>
            <xs:enumeration value="false"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="fetchType">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="lazy"/>
            <xs:enumeration value="eager"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
    </xs:complexType>
  </xs:element>
  <xs:element name="association">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" ref="constructor"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="id"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="result"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="association"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="collection"/>
        <xs:element minOccurs="0" ref="discriminator"/>
      </xs:sequence>
      <xs:attribute name="property" use="required"/>
      <xs:attribute name="column"/>
      <xs:attribute name="javaType"/>
      <xs:attribute name="jdbcType"/>
      <xs:attribute name="select"/>
      <xs:attribute name="resultMap"/>
      <xs:attribute name="typeHandler"/>
      <xs:attribute name="notNullColumn"/>
      <xs:attribute name="columnPrefix"/>
      <xs:attribute name="resultSet"/>
      <xs:attribute name="foreignColumn"/>
      <xs:attribute name="autoMapping">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="true"/>
            <xs:enumeration value="false"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="fetchType">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="lazy"/>
            <xs:enumeration value="eager"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
    </xs:complexType>
  </xs:element>
  <xs:element name="discriminator">
    <xs:complexType>
      <xs:sequence>
        <xs:element maxOccurs="unbounded" ref="case"/>
      </xs:sequence>
      <xs:attribute name="column"/>
      <xs:attribute name="javaType" use="required"/>
      <xs:attribute name="jdbcType"/>
      <xs:attribute name="typeHandler"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="case">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" ref="constructor"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="id"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="result"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="association"/>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="collection"/>
        <xs:element minOccurs="0" ref="discriminator"/>
      </xs:sequence>
      <xs:attribute name="value" use="required"/>
      <xs:attribute name="resultMap"/>
      <xs:attribute name="resultType"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="property">
    <xs:complexType>
      <xs:attribute name="name" use="required"/>
      <xs:attribute name="value" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="typeAlias">
    <xs:complexType>
      <xs:attribute name="alias" use="required"/>
      <xs:attribute name="type" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="select">
    <xs:complexType mixed="true">
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element ref="include"/>
        <xs:element ref="trim"/>
        <xs:element ref="where"/>
        <xs:element ref="set"/>
        <xs:element ref="foreach"/>
        <xs:element ref="choose"/>
        <xs:element ref="if"/>
        <xs:element ref="bind"/>
      </xs:choice>
      <xs:attribute name="id" use="required"/>
      <xs:attribute name="parameterMap"/>
      <xs:attribute name="parameterType"/>
      <xs:attribute name="resultMap"/>
      <xs:attribute name="resultType"/>
      <xs:attribute name="resultSetType">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="FORWARD_ONLY"/>
            <xs:enumeration value="SCROLL_INSENSITIVE"/>
            <xs:enumeration value="SCROLL_SENSITIVE"/>
            <xs:enumeration value="DEFAULT"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="statementType">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="STATEMENT"/>
            <xs:enumeration value="PREPARED"/>
            <xs:enumeration value="CALLABLE"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="fetchSize"/>
      <xs:attribute name="timeout"/>
      <xs:attribute name="flushCache">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="true"/>
            <xs:enumeration value="false"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="useCache">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="true"/>
            <xs:enumeration value="false"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="databaseId"/>
      <xs:attribute name="lang"/>
      <xs:attribute name="resultOrdered">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="true"/>
            <xs:enumeration value="false"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="resultSets"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="insert">
    <xs:complexType mixed="true">
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element ref="selectKey"/>
        <xs:element ref="include"/>
        <xs:element ref="trim"/>
        <xs:element ref="where"/>
        <xs:element ref="set"/>
        <xs:element ref="foreach"/>
        <xs:element ref="choose"/>
        <xs:element ref="if"/>
        <xs:element ref="bind"/>
      </xs:choice>
      <xs:attribute name="id" use="required"/>
      <xs:attribute name="parameterMap"/>
      <xs:attribute name="parameterType"/>
      <xs:attribute name="timeout"/>
      <xs:attribute name="flushCache">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="true"/>
            <xs:enumeration value="false"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="statementType">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="STATEMENT"/>
            <xs:enumeration value="PREPARED"/>
            <xs:enumeration value="CALLABLE"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="keyProperty"/>
      <xs:attribute name="useGeneratedKeys">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="true"/>
            <xs:enumeration value="false"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="keyColumn"/>
      <xs:attribute name="databaseId"/>
      <xs:attribute name="lang"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="selectKey">
    <xs:complexType mixed="true">
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element ref="include"/>
        <xs:element ref="trim"/>
        <xs:element ref="where"/>
        <xs:element ref="set"/>
        <xs:element ref="foreach"/>
        <xs:element ref="choose"/>
        <xs:element ref="if"/>
        <xs:element ref="bind"/>
      </xs:choice>
      <xs:attribute name="resultType"/>
      <xs:attribute name="statementType">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="STATEMENT"/>
            <xs:enumeration value="PREPARED"/>
            <xs:enumeration value="CALLABLE"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="keyProperty"/>
      <xs:attribute name="keyColumn"/>
      <xs:attribute name="order">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="BEFORE"/>
            <xs:enumeration value="AFTER"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="databaseId"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="update">
    <xs:complexType mixed="true">
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element ref="selectKey"/>
        <xs:element ref="include"/>
        <xs:element ref="trim"/>
        <xs:element ref="where"/>
        <xs:element ref="set"/>
        <xs:element ref="foreach"/>
        <xs:element ref="choose"/>
        <xs:element ref="if"/>
        <xs:element ref="bind"/>
      </xs:choice>
      <xs:attribute name="id" use="required"/>
      <xs:attribute name="parameterMap"/>
      <xs:attribute name="parameterType"/>
      <xs:attribute name="timeout"/>
      <xs:attribute name="flushCache">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="true"/>
            <xs:enumeration value="false"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="statementType">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="STATEMENT"/>
            <xs:enumeration value="PREPARED"/>
            <xs:enumeration value="CALLABLE"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="keyProperty"/>
      <xs:attribute name="useGeneratedKeys">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="true"/>
            <xs:enumeration value="false"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="keyColumn"/>
      <xs:attribute name="databaseId"/>
      <xs:attribute name="lang"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="delete">
    <xs:complexType mixed="true">
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element ref="include"/>
        <xs:element ref="trim"/>
        <xs:element ref="where"/>
        <xs:element ref="set"/>
        <xs:element ref="foreach"/>
        <xs:element ref="choose"/>
        <xs:element ref="if"/>
        <xs:element ref="bind"/>
      </xs:choice>
      <xs:attribute name="id" use="required"/>
      <xs:attribute name="parameterMap"/>
      <xs:attribute name="parameterType"/>
      <xs:attribute name="timeout"/>
      <xs:attribute name="flushCache">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="true"/>
            <xs:enumeration value="false"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="statementType">
        <xs:simpleType>
          <xs:restriction base="xs:token">
            <xs:enumeration value="STATEMENT"/>
            <xs:enumeration value="PREPARED"/>
            <xs:enumeration value="CALLABLE"/>
          </xs:restriction>
        </xs:simpleType>
      </xs:attribute>
      <xs:attribute name="databaseId"/>
      <xs:attribute name="lang"/>
    </xs:complexType>
  </xs:element>
  <!-- Dynamic -->
  <xs:element name="include">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="property"/>
      </xs:sequence>
      <xs:attribute name="refid" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="bind">
    <xs:complexType>
      <xs:attribute name="name" use="required"/>
      <xs:attribute name="value" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="sql">
    <xs:complexType mixed="true">
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element ref="include"/>
        <xs:element ref="trim"/>
        <xs:element ref="where"/>
        <xs:element ref="set"/>
        <xs:element ref="foreach"/>
        <xs:element ref="choose"/>
        <xs:element ref="if"/>
        <xs:element ref="bind"/>
      </xs:choice>
      <xs:attribute name="id" use="required"/>
      <xs:attribute name="lang"/>
      <xs:attribute name="databaseId"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="trim">
    <xs:complexType mixed="true">
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element ref="include"/>
        <xs:element ref="trim"/>
        <xs:element ref="where"/>
        <xs:element ref="set"/>
        <xs:element ref="foreach"/>
        <xs:element ref="choose"/>
        <xs:element ref="if"/>
        <xs:element ref="bind"/>
      </xs:choice>
      <xs:attribute name="prefix"/>
      <xs:attribute name="prefixOverrides"/>
      <xs:attribute name="suffix"/>
      <xs:attribute name="suffixOverrides"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="where">
    <xs:complexType mixed="true">
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element ref="include"/>
        <xs:element ref="trim"/>
        <xs:element ref="where"/>
        <xs:element ref="set"/>
        <xs:element ref="foreach"/>
        <xs:element ref="choose"/>
        <xs:element ref="if"/>
        <xs:element ref="bind"/>
      </xs:choice>
    </xs:complexType>
  </xs:element>
  <xs:element name="set">
    <xs:complexType mixed="true">
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element ref="include"/>
        <xs:element ref="trim"/>
        <xs:element ref="where"/>
        <xs:element ref="set"/>
        <xs:element ref="foreach"/>
        <xs:element ref="choose"/>
        <xs:element ref="if"/>
        <xs:element ref="bind"/>
      </xs:choice>
    </xs:complexType>
  </xs:element>
  <xs:element name="foreach">
    <xs:complexType mixed="true">
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element ref="include"/>
        <xs:element ref="trim"/>
        <xs:element ref="where"/>
        <xs:element ref="set"/>
        <xs:element ref="foreach"/>
        <xs:element ref="choose"/>
        <xs:element ref="if"/>
        <xs:element ref="bind"/>
      </xs:choice>
      <xs:attribute name="collection" use="required"/>
      <xs:attribute name="item"/>
      <xs:attribute name="index"/>
      <xs:attribute name="open"/>
      <xs:attribute name="close"/>
      <xs:attribute name="separator"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="choose">
    <xs:complexType>
      <xs:sequence>
        <xs:element minOccurs="0" maxOccurs="unbounded" ref="when"/>
        <xs:element minOccurs="0" ref="otherwise"/>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
  <xs:element name="when">
    <xs:complexType mixed="true">
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element ref="include"/>
        <xs:element ref="trim"/>
        <xs:element ref="where"/>
        <xs:element ref="set"/>
        <xs:element ref="foreach"/>
        <xs:element ref="choose"/>
        <xs:element ref="if"/>
        <xs:element ref="bind"/>
      </xs:choice>
      <xs:attribute name="test" use="required"/>
    </xs:complexType>
  </xs:element>
  <xs:element name="otherwise">
    <xs:complexType mixed="true">
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element ref="include"/>
        <xs:element ref="trim"/>
        <xs:element ref="where"/>
        <xs:element ref="set"/>
        <xs:element ref="foreach"/>
        <xs:element ref="choose"/>
        <xs:element ref="if"/>
        <xs:element ref="bind"/>
      </xs:choice>
    </xs:complexType>
  </xs:element>
  <xs:element name="if">
    <xs:complexType mixed="true">
      <xs:choice minOccurs="0" maxOccurs="unbounded">
        <xs:element ref="include"/>
        <xs:element ref="trim"/>
        <xs:element ref="where"/>
        <xs:element ref="set"/>
        <xs:element ref="foreach"/>
        <xs:element ref="choose"/>
        <xs:element ref="if"/>
        <xs:element ref="bind"/>
      </xs:choice>
      <xs:attribute name="test" use="required"/>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

#### 4.1.3.10、小结

<a href="https://www.w3school.com.cn/dtd/dtd_intro.asp">DTD学习</a>

## 4.2、org.apache.ibatis.cursor  （结果集接口模块）

### 4.2.1、Cursor的UML图概览

![DefaultCursor](/Users/didi/Documents/design/mybatis/chapter_4/Cursor/DefaultCursor.png)

### 4.2.2、Cursor接口源码

#### 4.2.2.1、Cursor接口

该模块的Cursor主要是协助迭代器惰性获取数据项。

- 优点：可以处理数百万个查询对于不适合在内存的数据项。

- 限制：如果希望在resultMaps中使用集合，那么必须使用resultMap的id列对游标SQL查询进行排序（resultOrdered=“true”）。

```java
//游标继承Closeable接口，这个接口只有一个close方法需要实现，一般用于资源需要手动关闭情况，各种io流之类这里是结果集（ResultSet需要关闭，所以继承这个接口）
//继承Iterable接口，游标对结果集（ResultSet）进行遍历功能，需要实现一个迭代器的功能。
public interface Cursor<T> extends Closeable, Iterable<T> {

  /**
   * 如果cursor已经开始从数据库拉去游标项则返回true
   */
  boolean isOpen();

  /**
   *
   * 如果将匹配的查询数据已经全部返回，并消费完毕则返回true
   */
  boolean isConsumed();

  /**
   * 获取当前游标项的下标，第一个游标项下标为0
   * 如果未检索到第一个游标项，则返回-1
   */
  int getCurrentIndex();
}
```

#### 4.2.2.2、记忆加深图

![image-20200222121637645](/Users/didi/Library/Application Support/typora-user-images/image-20200222121637645.png)

### 4.2.3、DefaultCursor实现类源码

 ####  4.2.3.1、 【内部类】游标状态枚举类CursorStatus



<img src="/Users/didi/Library/Application Support/typora-user-images/image-20200222123417157.png" alt="image-20200222123417157" style="zoom: 50%;" />

```java
/**
   * 游标状态枚举值
   */
  private enum CursorStatus {

    /**
     * 新创建的游标，还未开始消费数据集
     *
     */
    CREATED,
    /**
     * 当前在使用的游标，数据集消费已经开始
     */
    OPEN,
    /**
     * 关闭的游标，没有完全消费完毕
     */
    CLOSED,
    /**
     * 完全消费完毕的游标，消费完的游标也是关闭状态
     */
    CONSUMED
  }

```

####  4.2.3.2、【内部类】游标迭代器CursorIterator



<img src="/Users/didi/Library/Application Support/typora-user-images/image-20200222123705082.png" alt="image-20200222123705082" style="zoom:50%;" />

```java
/**
   * 游标迭代器
   */
  protected class CursorIterator implements Iterator<T> {

    /**
     * Holder for the next object to be returned.
     * 下一个要返回的对象
     */
    T object;

    /**
     * Index of objects returned using next(), and as such, visible to users.
     * 使用next()函数返回的值，用户见
     */
    int iteratorIndex = -1;

    /**
     * 重写hasNext函数，判断是否对象包装结果处理器已经拉取成功。，如果不成功，则调用fetchNextUsingRowBound()获取数据，否则返回true。
     * @return
     */
    @Override
    public boolean hasNext() {
      if (!objectWrapperResultHandler.fetched) {
        object = fetchNextUsingRowBound();
      }
      return objectWrapperResultHandler.fetched;
    }

    /**
     * 重写next函数，用hasNext（）函数返回的对象填充next
     * @return
     */
    @Override
    public T next() {
      // Fill next with object fetched from hasNext()
      //用hasNext（）函数返回的对象填充next
      T next = object;
      //如果对象包装类处理器已经拉取成功
      if (!objectWrapperResultHandler.fetched) {
        //未拉取成功则需要重新拉取进行next对象填充
        next = fetchNextUsingRowBound();
      }
      //如果拉取数据成功
      if (objectWrapperResultHandler.fetched) {
        //初始化数据，为下次遍历做准备
        objectWrapperResultHandler.fetched = false;
        object = null;
        iteratorIndex++;
        return next;
      }
      //如果没有拉取成功，则直接抛出异常
      throw new NoSuchElementException();
    }

    /**
     * 重写remove方法，不能从当前游标移除元素
     */
    @Override
    public void remove() {
      throw new UnsupportedOperationException("Cannot remove element from Cursor");
    }
  }
```

####  4.2.3.3、【静态内部类】对象包装结果处理器

![image-20200222123734266](/Users/didi/Library/Application Support/typora-user-images/image-20200222123734266.png)

```java
  /**
   * 对象包装类结果处理器，将结果器上下文数据返回，并标记数据拉取成功。
   * @param <T>
   */
  protected static class ObjectWrapperResultHandler<T> implements ResultHandler<T> {
    //每次拉取数据返回的临时存储对象
    protected T result;
    //判断是否已经拉取数据
    protected boolean fetched;

    @Override
    public void handleResult(ResultContext<? extends T> context) {
      this.result = context.getResultObject();
      context.stop();
      fetched = true;
    }
  }
```

#### 4.2.3.4、【基类】DefaulCursor

![image-20200222123802819](/Users/didi/Library/Application Support/typora-user-images/image-20200222123802819.png)

```java
/**
 * Cursor的默认实现类，非线程安全的类
 */
public class DefaultCursor<T> implements Cursor<T> {

  // ResultSetHandler stuff
  //数据集处理器
  private final DefaultResultSetHandler resultSetHandler;
  //数据集容器
  private final ResultMap resultMap;
  //数据集包装类
  private final ResultSetWrapper rsw;
  //行边界
  private final RowBounds rowBounds;
  //对象包装结果处理器
  protected final ObjectWrapperResultHandler<T> objectWrapperResultHandler = new ObjectWrapperResultHandler<>();
  //游标迭代器
  private final CursorIterator cursorIterator = new CursorIterator();
  //迭代器恢复标志
  private boolean iteratorRetrieved;
  //游标状态
  private CursorStatus status = CursorStatus.CREATED;
  //行边界初始默认值
  private int indexWithRowBound = -1;

  /**
   * 游标状态枚举值
   */
  private enum CursorStatus {
 //省略...参考4.2.3.1游标状态枚举类
  }

  /**
   *   有参构造器
   * @param resultSetHandler
   * @param resultMap
   * @param rsw
   * @param rowBounds
   */
  public DefaultCursor(DefaultResultSetHandler resultSetHandler, ResultMap resultMap, ResultSetWrapper rsw, RowBounds rowBounds) {
    this.resultSetHandler = resultSetHandler;
    this.resultMap = resultMap;
    this.rsw = rsw;
    this.rowBounds = rowBounds;
  }

  @Override
  public boolean isOpen() {
    return status == CursorStatus.OPEN;
  }

  @Override
  public boolean isConsumed() {
    return status == CursorStatus.CONSUMED;
  }

  /**
   * 获取当前游标下标=行边界的出发点+当前游标迭代器的下标
   * @return
   */
  @Override
  public int getCurrentIndex() {
    return rowBounds.getOffset() + cursorIterator.iteratorIndex;
  }

  /**
   * 实现迭代器，同一个时刻只有一个迭代器，如果该游标已经被消费或者关闭则不再继续迭代数据。
   * @return
   */
  @Override
  public Iterator<T> iterator() {
    if (iteratorRetrieved) {
      throw new IllegalStateException("Cannot open more than one iterator on a Cursor");
    }
    if (isClosed()) {
      throw new IllegalStateException("A Cursor is already closed.");
    }
    iteratorRetrieved = true;
    return cursorIterator;
  }

  /**
   * 实现自动关闭父类的close方法，如果已经关闭，则返回，否则将数据集关闭，并将当前游标置为关闭状态
   */
  @Override
  public void close() {
    if (isClosed()) {
      return;
    }

    ResultSet rs = rsw.getResultSet();
    try {
      if (rs != null) {
        rs.close();
      }
    } catch (SQLException e) {
      // ignore
    } finally {
      status = CursorStatus.CLOSED;
    }
  }

  /**
   * 使用行边界来获取下一批数据，直接从数据库获取数据，并
   * @return
   */
  protected T fetchNextUsingRowBound() {
    T result = fetchNextObjectFromDatabase();
    while (objectWrapperResultHandler.fetched && indexWithRowBound < rowBounds.getOffset()) {
      result = fetchNextObjectFromDatabase();
    }
    return result;
  }

  /**
   * 从数据库拉取下一批对象
   * @return
   */
  protected T fetchNextObjectFromDatabase() {
    //如果游标已经关闭了，直接返回空对象
    if (isClosed()) {
      return null;
    }

    try {
      objectWrapperResultHandler.fetched = false;
      status = CursorStatus.OPEN;
      if (!rsw.getResultSet().isClosed()) {
        //结果集处理器处理行数据
        resultSetHandler.handleRowValues(rsw, resultMap, objectWrapperResultHandler, RowBounds.DEFAULT, null);
      }
    } catch (SQLException e) {
      throw new RuntimeException(e);
    }
    //将对象包装器结果集处理器的结果赋值给next
    T next = objectWrapperResultHandler.result;
    //如果拉取数据成功，则行边界下标累计增加
    if (objectWrapperResultHandler.fetched) {
      indexWithRowBound++;
    }
    // No more object or limit reached
    //如果没有拉取到数据或者总条数已经拉取完毕，则游标状态关闭
    if (!objectWrapperResultHandler.fetched || getReadItemsCount() == rowBounds.getOffset() + rowBounds.getLimit()) {
      close();
      status = CursorStatus.CONSUMED;
    }
    //返回对象结果为空
    objectWrapperResultHandler.result = null;

    return next;
  }

  /**
   * 判断当前游标是否已经被关闭，如果游标状态是关闭或者消费完成则返回true，已经关闭
   * @return
   */
  private boolean isClosed() {
    return status == CursorStatus.CLOSED || status == CursorStatus.CONSUMED;
  }

  /**
   * 获取读的数据项条数
   * @return
   */
  private int getReadItemsCount() {
    return indexWithRowBound + 1;
  }

  /**
   * 对象包装类结果处理器，将结果器上下文数据返回，并标记数据拉取成功。
   * @param <T>
   */
  protected static class ObjectWrapperResultHandler<T> implements ResultHandler<T> {
  //省略...参考4.2.3.3对象包装结果处理器
  }

  /**
   * 游标迭代器
   */
  protected class CursorIterator implements Iterator<T> {
  //省略...参考4.2.3.2游标迭代器
  }
}

```

#### 4.2.3.5、记忆加深图

![image-20200222132649579](/Users/didi/Library/Application Support/typora-user-images/image-20200222132649579.png)

### 4.2.4、归纳小结

![image-20200222132413247](/Users/didi/Library/Application Support/typora-user-images/image-20200222132413247.png)

本章主要描述了下org.apache.ibatis.cursor  （结果集接口模块）。核心的文件，接口Cursor和实现类DefaultCursor，主要是方便大数据量的数据进行分页查询，为了处理数据流关闭状态需要继承Closeable接口。

## 4.3、org.apache.ibatis.executor（执行器模块）

<img src="/Users/didi/Library/Application Support/typora-user-images/image-20200310234317441.png" alt="image-20200310234317441" />

### 4.3.1、执行器

#### 4.3.1.1、Executor执行器接口

![image-20200310234434420](/Users/didi/Library/Application Support/typora-user-images/image-20200310234434420.png)

```java
/**
 * 执行器
 * @author Clinton Begin
 */
public interface Executor {
  //没结果处理器
  ResultHandler NO_RESULT_HANDLER = null;
  //更新会话
  int update(MappedStatement ms, Object parameter) throws SQLException;
  //查询带缓存
  <E> List<E> query(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, CacheKey cacheKey, BoundSql boundSql) throws SQLException;
  //查询不带缓存
  <E> List<E> query(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler) throws SQLException;
  //查询游标集
  <E> Cursor<E> queryCursor(MappedStatement ms, Object parameter, RowBounds rowBounds) throws SQLException;
  //刷新会话
  List<BatchResult> flushStatements() throws SQLException;
  //提交事务
  void commit(boolean required) throws SQLException;
  //回滚
  void rollback(boolean required) throws SQLException;
  //创建缓存key
  CacheKey createCacheKey(MappedStatement ms, Object parameterObject, RowBounds rowBounds, BoundSql boundSql);
  //是否会话被缓存
  boolean isCached(MappedStatement ms, CacheKey key);
  //清除本地缓存
  void clearLocalCache();
  //延迟加载
  void deferLoad(MappedStatement ms, MetaObject resultObject, String property, CacheKey key, Class<?> targetType);
  //获取事务
  Transaction getTransaction();
  //关闭，是否强制回滚
  void close(boolean forceRollback);
  //是否关闭
  boolean isClosed();
  //设置执行器包装器
  void setExecutorWrapper(Executor executor);

}
```

#### 4.3.1.2、CachingExecutor缓存执行器

![image-20200311133213337](/Users/didi/Library/Application Support/typora-user-images/image-20200311133213337.png)

```java
/**
 * 缓存执行器
 * @author Clinton Begin
 * @author Eduardo Macarron
 */
public class CachingExecutor implements Executor {
  //代表执行器
  private final Executor delegate;
  //事务缓存管理
  private final TransactionalCacheManager tcm = new TransactionalCacheManager();
  //初始化构造器，执行器就是本身。
  public CachingExecutor(Executor delegate) {
    this.delegate = delegate;
    delegate.setExecutorWrapper(this);
  }
  //获取执行器的事务
  @Override
  public Transaction getTransaction() {
    return delegate.getTransaction();
  }
  //关闭前强制是否回滚或者提交，最后执行器关闭
  @Override
  public void close(boolean forceRollback) {
    try {
      //issues #499, #524 and #573
      if (forceRollback) {
        tcm.rollback();
      } else {
        tcm.commit();
      }
    } finally {
      delegate.close(forceRollback);
    }
  }
  //是否执行器被关闭
  @Override
  public boolean isClosed() {
    return delegate.isClosed();
  }
  //如果有必要刷新缓存，执行器更新会话操作
  @Override
  public int update(MappedStatement ms, Object parameterObject) throws SQLException {
    flushCacheIfRequired(ms);
    return delegate.update(ms, parameterObject);
  }
  //查询，并创建缓存
  @Override
  public <E> List<E> query(MappedStatement ms, Object parameterObject, RowBounds rowBounds, ResultHandler resultHandler) throws SQLException {
    BoundSql boundSql = ms.getBoundSql(parameterObject);
    CacheKey key = createCacheKey(ms, parameterObject, rowBounds, boundSql);
    return query(ms, parameterObject, rowBounds, resultHandler, key, boundSql);
  }
  //查询游标结果集，如果有必要刷新缓存
  @Override
  public <E> Cursor<E> queryCursor(MappedStatement ms, Object parameter, RowBounds rowBounds) throws SQLException {
    flushCacheIfRequired(ms);
    return delegate.queryCursor(ms, parameter, rowBounds);
  }
  //带缓存查询
  @Override
  public <E> List<E> query(MappedStatement ms, Object parameterObject, RowBounds rowBounds, ResultHandler resultHandler, CacheKey key, BoundSql boundSql)
      throws SQLException {
    Cache cache = ms.getCache();
    if (cache != null) {
      flushCacheIfRequired(ms);
      if (ms.isUseCache() && resultHandler == null) {
        ensureNoOutParams(ms, boundSql);
        @SuppressWarnings("unchecked")
        List<E> list = (List<E>) tcm.getObject(cache, key);
        if (list == null) {
          list = delegate.query(ms, parameterObject, rowBounds, resultHandler, key, boundSql);
          tcm.putObject(cache, key, list); // issue #578 and #116
        }
        return list;
      }
    }
    return delegate.query(ms, parameterObject, rowBounds, resultHandler, key, boundSql);
  }
  //刷新会话
  @Override
  public List<BatchResult> flushStatements() throws SQLException {
    return delegate.flushStatements();
  }
  //提交，执行器提交，事务缓存管理提交
  @Override
  public void commit(boolean required) throws SQLException {
    delegate.commit(required);
    tcm.commit();
  }
  //回滚，如果有必要事务缓存管理回滚
  @Override
  public void rollback(boolean required) throws SQLException {
    try {
      delegate.rollback(required);
    } finally {
      if (required) {
        tcm.rollback();
      }
    }
  }
  //确保没有外来参数的缓存 否则不支持，比如字段有自己添加的
  private void ensureNoOutParams(MappedStatement ms, BoundSql boundSql) {
    if (ms.getStatementType() == StatementType.CALLABLE) {
      for (ParameterMapping parameterMapping : boundSql.getParameterMappings()) {
        if (parameterMapping.getMode() != ParameterMode.IN) {
          throw new ExecutorException("Caching stored procedures with OUT params is not supported.  Please configure useCache=false in " + ms.getId() + " statement.");
        }
      }
    }
  }
  //创建缓存key
  @Override
  public CacheKey createCacheKey(MappedStatement ms, Object parameterObject, RowBounds rowBounds, BoundSql boundSql) {
    return delegate.createCacheKey(ms, parameterObject, rowBounds, boundSql);
  }
  //是否缓存
  @Override
  public boolean isCached(MappedStatement ms, CacheKey key) {
    return delegate.isCached(ms, key);
  }
  //延迟加载
  @Override
  public void deferLoad(MappedStatement ms, MetaObject resultObject, String property, CacheKey key, Class<?> targetType) {
    delegate.deferLoad(ms, resultObject, property, key, targetType);
  }
  //清除本地缓存
  @Override
  public void clearLocalCache() {
    delegate.clearLocalCache();
  }
  //如果有必要 刷新缓存 事务缓存管理清空缓存
  private void flushCacheIfRequired(MappedStatement ms) {
    Cache cache = ms.getCache();
    if (cache != null && ms.isFlushCacheRequired()) {
      tcm.clear(cache);
    }
  }
  //设置执行器包装器，这个方法应该从来不会被调用
  @Override
  public void setExecutorWrapper(Executor executor) {
    throw new UnsupportedOperationException("This method should not be called");
  }

}
```

#### 4.3.1.3、ResultExtractor结果提取器

```java
/**
 * 
 * 结果提取器
 * @author Andrew Gustafson
 */
public class ResultExtractor {
  //全局配置对象
  private final Configuration configuration;
  //对象工厂
  private final ObjectFactory objectFactory;
  //结果提取器构造器
  public ResultExtractor(Configuration configuration, ObjectFactory objectFactory) {
    this.configuration = configuration;
    this.objectFactory = objectFactory;
  }
  //从List集合中提取对象
  public Object extractObjectFromList(List<Object> list, Class<?> targetType) {
    Object value = null;
    //判断目标类型是否是本身
    if (targetType != null && targetType.isAssignableFrom(list.getClass())) {
      value = list;
      //判断目标是不是集合
    } else if (targetType != null && objectFactory.isCollection(targetType)) {
      value = objectFactory.create(targetType);
      MetaObject metaObject = configuration.newMetaObject(value);
      metaObject.addAll(list);
      //判断目标是不是数组
    } else if (targetType != null && targetType.isArray()) {
      Class<?> arrayComponentType = targetType.getComponentType();
      //数组反射类构造实例
      Object array = Array.newInstance(arrayComponentType, list.size());
      if (arrayComponentType.isPrimitive()) {
        for (int i = 0; i < list.size(); i++) {
          Array.set(array, i, list.get(i));
        }
        value = array;
      } else {
        value = list.toArray((Object[])array);
      }
      //只有一个对象返回，否则抛出异常
    } else {
      if (list != null && list.size() > 1) {
        throw new ExecutorException("Statement returned more than one row, where no more than one was expected.");
      } else if (list != null && list.size() == 1) {
        value = list.get(0);
      }
    }
    return value;
  }
}
```

#### 4.3.1.4、BaseExecutor基础执行器

```java
/**
 * 基础执行器
 * @author Clinton Begin
 */
public abstract class BaseExecutor implements Executor {
  //日志
  private static final Log log = LogFactory.getLog(BaseExecutor.class);
  //事务
  protected Transaction transaction;
  //执行器包装器
  protected Executor wrapper;
  //并发队列，延迟加载
  protected ConcurrentLinkedQueue<DeferredLoad> deferredLoads;
  //永久的本地缓存
  protected PerpetualCache localCache;
  //永久的本地外放参数缓存
  protected PerpetualCache localOutputParameterCache;
  //配置对象--全局很重要
  protected Configuration configuration;
  //查询栈
  protected int queryStack;
  //是否关闭
  private boolean closed;
  //根据configuration和事务来初始化构造器
  protected BaseExecutor(Configuration configuration, Transaction transaction) {
    this.transaction = transaction;
    this.deferredLoads = new ConcurrentLinkedQueue<>();
    this.localCache = new PerpetualCache("LocalCache");
    this.localOutputParameterCache = new PerpetualCache("LocalOutputParameterCache");
    this.closed = false;
    this.configuration = configuration;
    this.wrapper = this;
  }
  //获取事务，如果关闭了则直接抛出异常 执行器已经被关闭
  @Override
  public Transaction getTransaction() {
    if (closed) {
      throw new ExecutorException("Executor was closed.");
    }
    return transaction;
  }
 //关闭 是否回滚
  @Override
  public void close(boolean forceRollback) {
    try {
      try {
        rollback(forceRollback);
      } finally {
        if (transaction != null) {
          transaction.close();
        }
      }
    } catch (SQLException e) {
      // Ignore.  There's nothing that can be done at this point.
      log.warn("Unexpected exception on closing transaction.  Cause: " + e);
    } finally {
      transaction = null;
      deferredLoads = null;
      localCache = null;
      localOutputParameterCache = null;
      closed = true;
    }
  }
  //是否被关闭
  @Override
  public boolean isClosed() {
    return closed;
  }
  //更新会话操作
  @Override
  public int update(MappedStatement ms, Object parameter) throws SQLException {
    //错误上下文实例获取会话ID
    ErrorContext.instance().resource(ms.getResource()).activity("executing an update").object(ms.getId());
    //如果关闭抛出异常
    if (closed) {
      throw new ExecutorException("Executor was closed.");
    }
    //清除本地缓存
    clearLocalCache();
    //更新会话操作
    return doUpdate(ms, parameter);
  }
  //刷新会话操作 不回滚
  @Override
  public List<BatchResult> flushStatements() throws SQLException {
    return flushStatements(false);
  }
  //刷新会话操作
  public List<BatchResult> flushStatements(boolean isRollBack) throws SQLException {
    if (closed) {
      throw new ExecutorException("Executor was closed.");
    }
    //操作刷新会话
    return doFlushStatements(isRollBack);
  }
  //查询会话
  @Override
  public <E> List<E> query(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler) throws SQLException {
    BoundSql boundSql = ms.getBoundSql(parameter);
    CacheKey key = createCacheKey(ms, parameter, rowBounds, boundSql);
    return query(ms, parameter, rowBounds, resultHandler, key, boundSql);
  }
  //带缓存查询会话
  @SuppressWarnings("unchecked")
  @Override
  public <E> List<E> query(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, CacheKey key, BoundSql boundSql) throws SQLException {
    //错误上下文增加活动记录
    ErrorContext.instance().resource(ms.getResource()).activity("executing a query").object(ms.getId());
    //是否关闭检查
    if (closed) {
      throw new ExecutorException("Executor was closed.");
    }
    //查询栈=0并且要求被刷新缓存的会话操作
    if (queryStack == 0 && ms.isFlushCacheRequired()) {
      //清除本地缓存
      clearLocalCache();
    }
    //查询栈+1
    List<E> list;
    try {
      queryStack++;
      //判断结果处理器是否为空，如果为空从本地缓存获取否则赋值空
      list = resultHandler == null ? (List<E>) localCache.getObject(key) : null;
      if (list != null) {
        //有结果处理器则直接处理缓存外来的参数
        handleLocallyCachedOutputParameters(ms, key, parameter, boundSql);
      } else {
        //没有结果处理器则直接从数据库查
        list = queryFromDatabase(ms, parameter, rowBounds, resultHandler, key, boundSql);
      }
    } finally {
      //查询栈-1维护初始0
      queryStack--;
    }
    //如果查询栈=0则延迟加载队列 进行延迟循环加载
    if (queryStack == 0) {
      for (DeferredLoad deferredLoad : deferredLoads) {
        deferredLoad.load();
      }
      //延迟队列清空
      // issue #601
      deferredLoads.clear();
      //如果配置的本地缓存范围是会话层
      if (configuration.getLocalCacheScope() == LocalCacheScope.STATEMENT) {
        //清除本地缓存
        // issue #482
        clearLocalCache();
      }
    }
    return list;
  }
  //查询游标集
  @Override
  public <E> Cursor<E> queryCursor(MappedStatement ms, Object parameter, RowBounds rowBounds) throws SQLException {
    BoundSql boundSql = ms.getBoundSql(parameter);
    return doQueryCursor(ms, parameter, rowBounds, boundSql);
  }
  //延迟加载
  @Override
  public void deferLoad(MappedStatement ms, MetaObject resultObject, String property, CacheKey key, Class<?> targetType) {
    if (closed) {
      throw new ExecutorException("Executor was closed.");
    }
    DeferredLoad deferredLoad = new DeferredLoad(resultObject, property, key, localCache, configuration, targetType);
    if (deferredLoad.canLoad()) {
      deferredLoad.load();
    } else {
      deferredLoads.add(new DeferredLoad(resultObject, property, key, localCache, configuration, targetType));
    }
  }
  //创建缓存key
  @Override
  public CacheKey createCacheKey(MappedStatement ms, Object parameterObject, RowBounds rowBounds, BoundSql boundSql) {
    if (closed) {
      throw new ExecutorException("Executor was closed.");
    }
    CacheKey cacheKey = new CacheKey();
    cacheKey.update(ms.getId());
    cacheKey.update(rowBounds.getOffset());
    cacheKey.update(rowBounds.getLimit());
    cacheKey.update(boundSql.getSql());
    List<ParameterMapping> parameterMappings = boundSql.getParameterMappings();
    //之前我们看到的类型处理器注册 在type包下的 可以通过会话的配置获取
    TypeHandlerRegistry typeHandlerRegistry = ms.getConfiguration().getTypeHandlerRegistry();
    // mimic DefaultParameterHandler logic
    for (ParameterMapping parameterMapping : parameterMappings) {
      if (parameterMapping.getMode() != ParameterMode.OUT) {
        Object value;
        //参数映射获取属性
        String propertyName = parameterMapping.getProperty();
        if (boundSql.hasAdditionalParameter(propertyName)) {
          value = boundSql.getAdditionalParameter(propertyName);
        } else if (parameterObject == null) {
          value = null;
        } else if (typeHandlerRegistry.hasTypeHandler(parameterObject.getClass())) {
          value = parameterObject;
        } else {
          MetaObject metaObject = configuration.newMetaObject(parameterObject);
          value = metaObject.getValue(propertyName);
        }
        cacheKey.update(value);
      }
    }
    //如果configuration获取环境不为空，则缓存更新环境ID
    if (configuration.getEnvironment() != null) {
      // issue #176
      cacheKey.update(configuration.getEnvironment().getId());
    }
    return cacheKey;
  }
  //是否被缓存会话
  @Override
  public boolean isCached(MappedStatement ms, CacheKey key) {
    return localCache.getObject(key) != null;
  }
  //提交如果有必要的话，清除本地缓存，刷新会话，事务提交
  @Override
  public void commit(boolean required) throws SQLException {
    if (closed) {
      throw new ExecutorException("Cannot commit, transaction is already closed");
    }
    clearLocalCache();
    flushStatements();
    if (required) {
      transaction.commit();
    }
  }
  //回滚，如果有必要回滚事务
  @Override
  public void rollback(boolean required) throws SQLException {
    if (!closed) {
      try {
        clearLocalCache();
        flushStatements(true);
      } finally {
        if (required) {
          transaction.rollback();
        }
      }
    }
  }
  //清除本地缓存
  @Override
  public void clearLocalCache() {
    if (!closed) {
      localCache.clear();
      localOutputParameterCache.clear();
    }
  }
  //约定子类实现的操作更新接口
  protected abstract int doUpdate(MappedStatement ms, Object parameter)
      throws SQLException;
  //约定子类操作的刷新会话接口
  protected abstract List<BatchResult> doFlushStatements(boolean isRollback)
      throws SQLException;
  //约定子类操作的查询接口
  protected abstract <E> List<E> doQuery(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, BoundSql boundSql)
      throws SQLException;
  //约定子类操作的查询结果集接口
  protected abstract <E> Cursor<E> doQueryCursor(MappedStatement ms, Object parameter, RowBounds rowBounds, BoundSql boundSql)
      throws SQLException;
  //关闭会话
  protected void closeStatement(Statement statement) {
    if (statement != null) {
      try {
        statement.close();
      } catch (SQLException e) {
        // ignore
      }
    }
  }

  /**
   * 如果数据库操作访问异常，这个方法被调用报事务异常超时
   * Apply a transaction timeout.
   * @param statement a current statement
   * @throws SQLException if a database access error occurs, this method is called on a closed <code>Statement</code>
   * @since 3.4.0
   * @see StatementUtil#applyTransactionTimeout(Statement, Integer, Integer)
   */
  protected void applyTransactionTimeout(Statement statement) throws SQLException {
    StatementUtil.applyTransactionTimeout(statement, statement.getQueryTimeout(), transaction.getTimeout());
  }
 //处理本地缓存输出参数
  private void handleLocallyCachedOutputParameters(MappedStatement ms, CacheKey key, Object parameter, BoundSql boundSql) {
    if (ms.getStatementType() == StatementType.CALLABLE) {
      final Object cachedParameter = localOutputParameterCache.getObject(key);
      if (cachedParameter != null && parameter != null) {
        final MetaObject metaCachedParameter = configuration.newMetaObject(cachedParameter);
        final MetaObject metaParameter = configuration.newMetaObject(parameter);
        for (ParameterMapping parameterMapping : boundSql.getParameterMappings()) {
          if (parameterMapping.getMode() != ParameterMode.IN) {
            final String parameterName = parameterMapping.getProperty();
            final Object cachedValue = metaCachedParameter.getValue(parameterName);
            metaParameter.setValue(parameterName, cachedValue);
          }
        }
      }
    }
  }
  //从数据库查询
  private <E> List<E> queryFromDatabase(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, CacheKey key, BoundSql boundSql) throws SQLException {
    List<E> list;
    //本地缓存存储
    localCache.putObject(key, EXECUTION_PLACEHOLDER);
    try {
      //执行查询
      list = doQuery(ms, parameter, rowBounds, resultHandler, boundSql);
    } finally {
      //本地缓存移除
      localCache.removeObject(key);
    }
    //本地缓存存储新的
    localCache.putObject(key, list);
    //如果会话类型是回调，则本地外放参数缓存存放
    if (ms.getStatementType() == StatementType.CALLABLE) {
      localOutputParameterCache.putObject(key, parameter);
    }
    return list;
  }
  //获取事务链接
  protected Connection getConnection(Log statementLog) throws SQLException {
    Connection connection = transaction.getConnection();
    if (statementLog.isDebugEnabled()) {
      return ConnectionLogger.newInstance(connection, statementLog, queryStack);
    } else {
      return connection;
    }
  }
  //设置执行器包装器
  @Override
  public void setExecutorWrapper(Executor wrapper) {
    this.wrapper = wrapper;
  }
  //延迟加载类
  private static class DeferredLoad {
    //元对象
    private final MetaObject resultObject;
    //属性
    private final String property;
    //目标类型
    private final Class<?> targetType;
    //缓存key
    private final CacheKey key;
    //本地缓存
    private final PerpetualCache localCache;
    //对象工厂
    private final ObjectFactory objectFactory;
    //结果提取器
    private final ResultExtractor resultExtractor;

    // issue #781
    //延迟加载构造器
    public DeferredLoad(MetaObject resultObject,
                        String property,
                        CacheKey key,
                        PerpetualCache localCache,
                        Configuration configuration,
                        Class<?> targetType) {
      this.resultObject = resultObject;
      this.property = property;
      this.key = key;
      this.localCache = localCache;
      this.objectFactory = configuration.getObjectFactory();
      this.resultExtractor = new ResultExtractor(configuration, objectFactory);
      this.targetType = targetType;
    }
    //是否可以被加载
    public boolean canLoad() {
      return localCache.getObject(key) != null && localCache.getObject(key) != EXECUTION_PLACEHOLDER;
    }
    //加载
    public void load() {
      @SuppressWarnings("unchecked")
      // we suppose we get back a List
      List<Object> list = (List<Object>) localCache.getObject(key);
      Object value = resultExtractor.extractObjectFromList(list, targetType);
      resultObject.setValue(property, value);
    }

  }

}
```

#### 4.3.1.5、SimpleExecutor简单执行器

```java
/**
 * 简单执行器
 * @author Clinton Begin
 */
public class SimpleExecutor extends BaseExecutor {
  //构造执行器
  public SimpleExecutor(Configuration configuration, Transaction transaction) {
    super(configuration, transaction);
  }
  //执行会话操作
  @Override
  public int doUpdate(MappedStatement ms, Object parameter) throws SQLException {
    Statement stmt = null;
    try {
      //会话获取全局配置
      Configuration configuration = ms.getConfiguration();
      //配置创建会话处理器
      StatementHandler handler = configuration.newStatementHandler(this, ms, parameter, RowBounds.DEFAULT, null, null);
      //预会话
      stmt = prepareStatement(handler, ms.getStatementLog());
      //处理会话
      return handler.update(stmt);
    } finally {
      //关闭会话
      closeStatement(stmt);
    }
  }
   //执行查询
  @Override
  public <E> List<E> doQuery(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, BoundSql boundSql) throws SQLException {
    Statement stmt = null;
    try {
      Configuration configuration = ms.getConfiguration();
      StatementHandler handler = configuration.newStatementHandler(wrapper, ms, parameter, rowBounds, resultHandler, boundSql);
      stmt = prepareStatement(handler, ms.getStatementLog());
      return handler.query(stmt, resultHandler);
    } finally {
      closeStatement(stmt);
    }
  }
  //执行查询游标集
  @Override
  protected <E> Cursor<E> doQueryCursor(MappedStatement ms, Object parameter, RowBounds rowBounds, BoundSql boundSql) throws SQLException {
    Configuration configuration = ms.getConfiguration();
    StatementHandler handler = configuration.newStatementHandler(wrapper, ms, parameter, rowBounds, null, boundSql);
    Statement stmt = prepareStatement(handler, ms.getStatementLog());
    Cursor<E> cursor = handler.queryCursor(stmt);
    stmt.closeOnCompletion();
    return cursor;
  }
  //刷新会话
  @Override
  public List<BatchResult> doFlushStatements(boolean isRollback) {
    return Collections.emptyList();
  }
  //预准备会话
  private Statement prepareStatement(StatementHandler handler, Log statementLog) throws SQLException {
    Statement stmt;
    Connection connection = getConnection(statementLog);
    stmt = handler.prepare(connection, transaction.getTimeout());
    handler.parameterize(stmt);
    return stmt;
  }

}
```

#### 4.3.1.6、ReuseExecutor重使用执行器

```java
/**
 * 重用执行器
 * @author Clinton Begin
 */
public class ReuseExecutor extends BaseExecutor {
  //会话容器
  private final Map<String, Statement> statementMap = new HashMap<>();
  //重用执行器构造函数
  public ReuseExecutor(Configuration configuration, Transaction transaction) {
    super(configuration, transaction);
  }
  //执行会话更新操作
  @Override
  public int doUpdate(MappedStatement ms, Object parameter) throws SQLException {
    Configuration configuration = ms.getConfiguration();
    StatementHandler handler = configuration.newStatementHandler(this, ms, parameter, RowBounds.DEFAULT, null, null);
    Statement stmt = prepareStatement(handler, ms.getStatementLog());
    return handler.update(stmt);
  }
  //执行查询操作
  @Override
  public <E> List<E> doQuery(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, BoundSql boundSql) throws SQLException {
    Configuration configuration = ms.getConfiguration();
    StatementHandler handler = configuration.newStatementHandler(wrapper, ms, parameter, rowBounds, resultHandler, boundSql);
    Statement stmt = prepareStatement(handler, ms.getStatementLog());
    return handler.query(stmt, resultHandler);
  }
  //执行查询游标集
  @Override
  protected <E> Cursor<E> doQueryCursor(MappedStatement ms, Object parameter, RowBounds rowBounds, BoundSql boundSql) throws SQLException {
    Configuration configuration = ms.getConfiguration();
    StatementHandler handler = configuration.newStatementHandler(wrapper, ms, parameter, rowBounds, null, boundSql);
    Statement stmt = prepareStatement(handler, ms.getStatementLog());
    return handler.queryCursor(stmt);
  }
  //做刷新会话操作
  @Override
  public List<BatchResult> doFlushStatements(boolean isRollback) {
    //关闭会话容器的所有会话
    for (Statement stmt : statementMap.values()) {
      closeStatement(stmt);
    }
    //清空会话容器
    statementMap.clear();
    //返回空集合
    return Collections.emptyList();
  }
  //预会话构造
  private Statement prepareStatement(StatementHandler handler, Log statementLog) throws SQLException {
    Statement stmt;
    //获取边界SQL
    BoundSql boundSql = handler.getBoundSql();
    String sql = boundSql.getSql();
    //判断是否有处理该SQL的会话
    if (hasStatementFor(sql)) {
      //获取会话
      stmt = getStatement(sql);
      //增加事务超时时间
      applyTransactionTimeout(stmt);
    } else {
      //获取会话日志链接
      Connection connection = getConnection(statementLog);
      //准备会话
      stmt = handler.prepare(connection, transaction.getTimeout());
      //本地存储会话
      putStatement(sql, stmt);
    }
    //处理器参数化 会话
    handler.parameterize(stmt);
    return stmt;
  }
  //sql是否有对应的会话
  private boolean hasStatementFor(String sql) {
    try {
      return statementMap.keySet().contains(sql) && !statementMap.get(sql).getConnection().isClosed();
    } catch (SQLException e) {
      return false;
    }
  }
  //会话容器获取会话
  private Statement getStatement(String s) {
    return statementMap.get(s);
  }
  //存放会话到会话容器
  private void putStatement(String sql, Statement stmt) {
    statementMap.put(sql, stmt);
  }

}
```

#### 4.3.1.7、BatchExecutor批量执行器

```java
/**
 * 批量执行器
 * @author Jeff Butler
 */
public class BatchExecutor extends BaseExecutor {
  //批量更新返回值
  public static final int BATCH_UPDATE_RETURN_VALUE = Integer.MIN_VALUE + 1002;
  //会话列表
  private final List<Statement> statementList = new ArrayList<>();
  //批量结果列表
  private final List<BatchResult> batchResultList = new ArrayList<>();
  //当前SQL
  private String currentSql;
  //当前会话
  private MappedStatement currentStatement;
  //批量执行器构造
  public BatchExecutor(Configuration configuration, Transaction transaction) {
    super(configuration, transaction);
  }
  //执行会话操作
  @Override
  public int doUpdate(MappedStatement ms, Object parameterObject) throws SQLException {
    //全局配置对象
    final Configuration configuration = ms.getConfiguration();
    //获取会话处理器
    final StatementHandler handler = configuration.newStatementHandler(this, ms, parameterObject, RowBounds.DEFAULT, null, null);
    //边界SQL
    final BoundSql boundSql = handler.getBoundSql();
    //获取SQL
    final String sql = boundSql.getSql();
    final Statement stmt;
    //如果当前SQL 并且会话是当前会话
    if (sql.equals(currentSql) && ms.equals(currentStatement)) {
      //缩减会话列表集合
      int last = statementList.size() - 1;
      //获取最新的会话
      stmt = statementList.get(last);
      //增加事务会话超时时间
      applyTransactionTimeout(stmt);
      //处理器将会话参数化
      handler.parameterize(stmt);//fix Issues 322
      BatchResult batchResult = batchResultList.get(last);
      //批量结果添加参数对象
      batchResult.addParameterObject(parameterObject);
    } else {
      //获取会话日志链接
      Connection connection = getConnection(ms.getStatementLog());
      //处理会话
      stmt = handler.prepare(connection, transaction.getTimeout());
      //处理会话参数
      handler.parameterize(stmt);    //fix Issues 322
      currentSql = sql;
      currentStatement = ms;
      statementList.add(stmt);
      batchResultList.add(new BatchResult(ms, sql, parameterObject));
    }
    //处理器处理会话
    handler.batch(stmt);
    return BATCH_UPDATE_RETURN_VALUE;
  }
  //执行查询
  @Override
  public <E> List<E> doQuery(MappedStatement ms, Object parameterObject, RowBounds rowBounds, ResultHandler resultHandler, BoundSql boundSql)
      throws SQLException {
    Statement stmt = null;
    try {
      //刷新会话
      flushStatements();
      //获取全局配置
      Configuration configuration = ms.getConfiguration();
      //构造会话处理器
      StatementHandler handler = configuration.newStatementHandler(wrapper, ms, parameterObject, rowBounds, resultHandler, boundSql);
      //创建会话并执行会话查询
      Connection connection = getConnection(ms.getStatementLog());
      stmt = handler.prepare(connection, transaction.getTimeout());
      handler.parameterize(stmt);
      return handler.query(stmt, resultHandler);
    } finally {
      //关闭会话
      closeStatement(stmt);
    }
  }
  //执行查询游标集
  @Override
  protected <E> Cursor<E> doQueryCursor(MappedStatement ms, Object parameter, RowBounds rowBounds, BoundSql boundSql) throws SQLException {
    //刷新会话
    flushStatements();
    //获取全局配置
    Configuration configuration = ms.getConfiguration();
    //构造会话处理器
    StatementHandler handler = configuration.newStatementHandler(wrapper, ms, parameter, rowBounds, null, boundSql);
    //获取链接
    Connection connection = getConnection(ms.getStatementLog());
    //处理器解析链接获取会话
    Statement stmt = handler.prepare(connection, transaction.getTimeout());
    //处理器执行游标集的查询
    handler.parameterize(stmt);
    Cursor<E> cursor = handler.queryCursor(stmt);
    stmt.closeOnCompletion();
    return cursor;
  }
  //刷新会话
  @Override
  public List<BatchResult> doFlushStatements(boolean isRollback) throws SQLException {
    try {
      //判断是否回滚，回滚返回空结果
      List<BatchResult> results = new ArrayList<>();
      if (isRollback) {
        return Collections.emptyList();
      }
      //遍历会话集合
      for (int i = 0, n = statementList.size(); i < n; i++) {
        //获取会话
        Statement stmt = statementList.get(i);
        //声明会话超时时间
        applyTransactionTimeout(stmt);
        //批量结果获取当前的结果
        BatchResult batchResult = batchResultList.get(i);
        try {
          //设置更新行数
          batchResult.setUpdateCounts(stmt.executeBatch());
          //获取映射的会话
          MappedStatement ms = batchResult.getMappedStatement();
          //获取参数对象
          List<Object> parameterObjects = batchResult.getParameterObjects();
          //主键生成
          KeyGenerator keyGenerator = ms.getKeyGenerator();
           //有key生成
          if (Jdbc3KeyGenerator.class.equals(keyGenerator.getClass())) {
            Jdbc3KeyGenerator jdbc3KeyGenerator = (Jdbc3KeyGenerator) keyGenerator;
            jdbc3KeyGenerator.processBatch(ms, stmt, parameterObjects);
            //咩有key生成
          } else if (!NoKeyGenerator.class.equals(keyGenerator.getClass())) { //issue #141
            for (Object parameter : parameterObjects) {
              keyGenerator.processAfter(this, ms, stmt, parameter);
            }
          }
          // Close statement to close cursor #1109
          //关闭会话为了关闭游标集
          closeStatement(stmt);
        } catch (BatchUpdateException e) {
          StringBuilder message = new StringBuilder();
          message.append(batchResult.getMappedStatement().getId())
              .append(" (batch index #")
              .append(i + 1)
              .append(")")
              .append(" failed.");
          if (i > 0) {
            message.append(" ")
                .append(i)
                .append(" prior sub executor(s) completed successfully, but will be rolled back.");
          }
          throw new BatchExecutorException(message.toString(), e, results, batchResult);
        }
        //添加结果集
        results.add(batchResult);
      }
      //返回结果
      return results;
    } finally {
      //遍历所有会话并关闭会话和清空所有信息
      for (Statement stmt : statementList) {
        closeStatement(stmt);
      }
      currentSql = null;
      statementList.clear();
      batchResultList.clear();
    }
  }

}
```

#### 4.3.1.8、ErrorContext错误上下文



```java
/**
 * 错误上下文
 * @author Clinton Begin
 */
public class ErrorContext {
  //行分割符号
  private static final String LINE_SEPARATOR = System.getProperty("line.separator","\n");
  //线程本地变量
  private static final ThreadLocal<ErrorContext> LOCAL = new ThreadLocal<>();
  //已经存储的错误上下文
  private ErrorContext stored;
  //资源
  private String resource;
  //活动
  private String activity;
  //对象
  private String object;
  //消息
  private String message;
  //sql
  private String sql;
  //抛出异常
  private Throwable cause;
  //防止外部实例化
  private ErrorContext() {
  }
  //错误上下文实例话
  public static ErrorContext instance() {
    //从线程本地变量获取，保证每个线程不一样
    ErrorContext context = LOCAL.get();
    if (context == null) {
      context = new ErrorContext();
      LOCAL.set(context);
    }
    return context;
  }
  //存储本地的错误上下文
  public ErrorContext store() {
    ErrorContext newContext = new ErrorContext();
    newContext.stored = this;
    LOCAL.set(newContext);
    return LOCAL.get();
  }
  //重新调用
  public ErrorContext recall() {
    if (stored != null) {
      LOCAL.set(stored);
      stored = null;
    }
    return LOCAL.get();
  }
  //设置资源
  public ErrorContext resource(String resource) {
    this.resource = resource;
    return this;
  }
 //设置活动
  public ErrorContext activity(String activity) {
    this.activity = activity;
    return this;
  }
  //设置对象
  public ErrorContext object(String object) {
    this.object = object;
    return this;
  }
  //设置信息
  public ErrorContext message(String message) {
    this.message = message;
    return this;
  }
  //设置sql
  public ErrorContext sql(String sql) {
    this.sql = sql;
    return this;
  }
  //设置Throwable
  public ErrorContext cause(Throwable cause) {
    this.cause = cause;
    return this;
  }
  //重置错误上下文
  public ErrorContext reset() {
    resource = null;
    activity = null;
    object = null;
    message = null;
    sql = null;
    cause = null;
    LOCAL.remove();
    return this;
  }
  //实现错误日志编排
  @Override
  public String toString() {
    StringBuilder description = new StringBuilder();

    // message
    if (this.message != null) {
      description.append(LINE_SEPARATOR);
      description.append("### ");
      description.append(this.message);
    }

    // resource
    if (resource != null) {
      description.append(LINE_SEPARATOR);
      description.append("### The error may exist in ");
      description.append(resource);
    }

    // object
    if (object != null) {
      description.append(LINE_SEPARATOR);
      description.append("### The error may involve ");
      description.append(object);
    }

    // activity
    if (activity != null) {
      description.append(LINE_SEPARATOR);
      description.append("### The error occurred while ");
      description.append(activity);
    }

    // sql
    if (sql != null) {
      description.append(LINE_SEPARATOR);
      description.append("### SQL: ");
      description.append(sql.replace('\n', ' ').replace('\r', ' ').replace('\t', ' ').trim());
    }

    // cause
    if (cause != null) {
      description.append(LINE_SEPARATOR);
      description.append("### Cause: ");
      description.append(cause.toString());
    }

    return description.toString();
  }

}
```

#### 4.3.1.9、ExecutionPlaceholder执行占位符

```java
/**
 *执行占位符
 * @author Clinton Begin
 */
public enum ExecutionPlaceholder {
  EXECUTION_PLACEHOLDER
}

```

#### 4.3.1.10、BatchResult批量结果

```java
/**
 * 批量结果
 * @author Jeff Butler
 */
public class BatchResult {
  //会话映射
  private final MappedStatement mappedStatement;
  //sql
  private final String sql;
  //参数对象集合
  private final List<Object> parameterObjects;
  //操作行数
  private int[] updateCounts;
  //构造器
  public BatchResult(MappedStatement mappedStatement, String sql) {
    super();
    this.mappedStatement = mappedStatement;
    this.sql = sql;
    this.parameterObjects = new ArrayList<>();
  }
 //构造器
  public BatchResult(MappedStatement mappedStatement, String sql, Object parameterObject) {
    this(mappedStatement, sql);
    addParameterObject(parameterObject);
  }
  //获取映射会话
  public MappedStatement getMappedStatement() {
    return mappedStatement;
  }
  //获取sql
  public String getSql() {
    return sql;
  }
  //获取参数对象
  @Deprecated
  public Object getParameterObject() {
    return parameterObjects.get(0);
  }
  //获取参数对象
  public List<Object> getParameterObjects() {
    return parameterObjects;
  }
  //获取更新行数
  public int[] getUpdateCounts() {
    return updateCounts;
  }
  //设置更行行数
  public void setUpdateCounts(int[] updateCounts) {
    this.updateCounts = updateCounts;
  }
  //添加参数对象
  public void addParameterObject(Object parameterObject) {
    this.parameterObjects.add(parameterObject);
  }

}
```

#### 4.3.1.11、ExecutorException执行器异常

```java
/**
 * 执行器异常依然是继承了运行期异常
 * @author Clinton Begin
 */
public class ExecutorException extends PersistenceException {

  private static final long serialVersionUID = 4060977051977364820L;
  //空构造函数
  public ExecutorException() {
    super();
  }
  //传递信息构造函数
  public ExecutorException(String message) {
    super(message);
  }
  //传递信息和throwable构造函数
  public ExecutorException(String message, Throwable cause) {
    super(message, cause);
  }
  //传递throwable构造函数
  public ExecutorException(Throwable cause) {
    super(cause);
  }

}
```

#### 4.3.1.12、BatchExecutorException批量执行异常

```java
/**
 * 在任何嵌套批量执行期间，如果 <code>java.sql.BatchUpdateException</code>被捕获，则异常抛出
 * <code>java.sql.BatchUpdateException</code>是根造成的，所以出现该异常一定是没有执行成功。
 * This exception is thrown if a <code>java.sql.BatchUpdateException</code> is caught
 * during the execution of any nested batch.  The exception contains the
 * java.sql.BatchUpdateException that is the root cause, as well as
 * the results from any prior nested batch that executed successfully.
 *
 * @author Jeff Butler
 */
public class BatchExecutorException extends ExecutorException {
  //成功批量结果
  private static final long serialVersionUID = 154049229650533990L;
  private final List<BatchResult> successfulBatchResults;
  //批量更新异常
  private final BatchUpdateException batchUpdateException;
  //批量结果
  private final BatchResult batchResult;
  //构造器
  public BatchExecutorException(String message,
                                BatchUpdateException cause,
                                List<BatchResult> successfulBatchResults,
                                BatchResult batchResult) {
    super(message + " Cause: " + cause, cause);
    this.batchUpdateException = cause;
    this.successfulBatchResults = successfulBatchResults;
    this.batchResult = batchResult;
  }

  /**
   *返回嵌套执行失败的异常，异常包含行数用来帮助定位造成失败的问题。
   * Returns the BatchUpdateException that caused the nested executor
   * to fail.  That exception contains an array of row counts
   * that can be used to determine exactly which statement of the
   * executor caused the failure (or failures).
   *
   * @return the root BatchUpdateException
   */
  public BatchUpdateException getBatchUpdateException() {
    return batchUpdateException;
  }

  /**
   * 返回批量结果，在失败执行前，在每一个成功的子执行器是有一个对象的
   * Returns a list of BatchResult objects.  There will be one entry
   * in the list for each successful sub-executor executed before the failing
   * executor.
   *
   * @return the previously successful executor results (may be an empty list
   *         if no executor has executed successfully)
   */
  public List<BatchResult> getSuccessfulBatchResults() {
    return successfulBatchResults;
  }

  /**
   * 返回造成失败的sql会话
   * Returns the SQL statement that caused the failure
   * (not the parameterArray).
   *
   * @return the failing SQL string
   */
  public String getFailingSqlStatement() {
    return batchResult.getSql();
  }

  /**
   * 返回造成失败的会话ID
   * Returns the statement id of the statement that caused the failure.
   *
   * @return the statement id
   */
  public String getFailingStatementId() {
    return batchResult.getMappedStatement().getId();
  }
}
```

### 4.3.2、keygen包

![image-20200311193809359](/Users/didi/Library/Application Support/typora-user-images/image-20200311193809359.png)

#### 4.3.2.1、KeyGenerator主键生成接口

```java
/**
 * 主键生成接口
 * @author Clinton Begin
 */
public interface KeyGenerator {
  //执行前生成
  void processBefore(Executor executor, MappedStatement ms, Statement stmt, Object parameter);
  //执行后生成
  void processAfter(Executor executor, MappedStatement ms, Statement stmt, Object parameter);

}
```

#### 4.3.2.2、Jdbc3KeyGenerator主键生成策略(todo 第二遍)

```java
/**
 * jdbc链接主键生成
 * @author Clinton Begin
 * @author Kazuki Shimizu
 */
public class Jdbc3KeyGenerator implements KeyGenerator {
  //参数名字
  private static final String SECOND_GENERIC_PARAM_NAME = ParamNameResolver.GENERIC_NAME_PREFIX + "2";

  /**
   * 3.4.3版本以后 一个共享的实例
   * A shared instance.
   *
   * @since 3.4.3
   */
  public static final Jdbc3KeyGenerator INSTANCE = new Jdbc3KeyGenerator();
  //生成太多key的错误提示
  private static final String MSG_TOO_MANY_KEYS = "Too many keys are generated. There are only %d target objects. "
      + "You either specified a wrong 'keyProperty' or encountered a driver bug like #1523.";
  //在执行器前执行，什么也不做
  @Override
  public void processBefore(Executor executor, MappedStatement ms, Statement stmt, Object parameter) {
    // do nothing
  }
  //在执行器后执行，执行会话
  @Override
  public void processAfter(Executor executor, MappedStatement ms, Statement stmt, Object parameter) {
    processBatch(ms, stmt, parameter);
  }
  //执行
  public void processBatch(MappedStatement ms, Statement stmt, Object parameter) {
    //映射会话获取key属性
    final String[] keyProperties = ms.getKeyProperties();
    if (keyProperties == null || keyProperties.length == 0) {
      return;
    }
    //会话获取主键
    try (ResultSet rs = stmt.getGeneratedKeys()) {
      //结果集获取元数据
      final ResultSetMetaData rsmd = rs.getMetaData();
      //获取全局配置
      final Configuration configuration = ms.getConfiguration();
      if (rsmd.getColumnCount() < keyProperties.length) {
        // Error?
      } else {
        //分配key主键
        assignKeys(configuration, rs, rsmd, keyProperties, parameter);
      }
    } catch (Exception e) {
      throw new ExecutorException("Error getting generated key or setting result to parameter object. Cause: " + e, e);
    }
  }
  //分配key主键
  @SuppressWarnings("unchecked")
  private void assignKeys(Configuration configuration, ResultSet rs, ResultSetMetaData rsmd, String[] keyProperties,
      Object parameter) throws SQLException {
    //使用@Param多个参数或者单个参数
    if (parameter instanceof ParamMap || parameter instanceof StrictMap) {
      // Multi-param or single param with @Param
      assignKeysToParamMap(configuration, rs, rsmd, keyProperties, (Map<String, ?>) parameter);
    } else if (parameter instanceof ArrayList && !((ArrayList<?>) parameter).isEmpty()
        && ((ArrayList<?>) parameter).get(0) instanceof ParamMap) {
      //批量操作 @Param多个参数或者单个参数
      // Multi-param or single param with @Param in batch operation
      assignKeysToParamMapList(configuration, rs, rsmd, keyProperties, ((ArrayList<ParamMap<?>>) parameter));
    } else {
      //没有@Param的单个参数
      // Single param without @Param
      assignKeysToParam(configuration, rs, rsmd, keyProperties, parameter);
    }
  }
  //分配主键给参数
  private void assignKeysToParam(Configuration configuration, ResultSet rs, ResultSetMetaData rsmd,
      String[] keyProperties, Object parameter) throws SQLException {
    //集合化参数
    Collection<?> params = collectionize(parameter);
    if (params.isEmpty()) {
      return;
    }
    //分配集合
    List<KeyAssigner> assignerList = new ArrayList<>();
    for (int i = 0; i < keyProperties.length; i++) {
      //添加分配的主键集合
      assignerList.add(new KeyAssigner(configuration, rsmd, i + 1, null, keyProperties[i]));
    }
    //遍历
    Iterator<?> iterator = params.iterator();
    while (rs.next()) {
      if (!iterator.hasNext()) {
        throw new ExecutorException(String.format(MSG_TOO_MANY_KEYS, params.size()));
      }
      //给每个会话绑定
      Object param = iterator.next();
      assignerList.forEach(x -> x.assign(rs, param));
    }
  }
  //分配主键给参数map集合
  private void assignKeysToParamMapList(Configuration configuration, ResultSet rs, ResultSetMetaData rsmd,
      String[] keyProperties, ArrayList<ParamMap<?>> paramMapList) throws SQLException {
    Iterator<ParamMap<?>> iterator = paramMapList.iterator();
    List<KeyAssigner> assignerList = new ArrayList<>();
    long counter = 0;
    //遍历结果信息
    while (rs.next()) {
      if (!iterator.hasNext()) {
        throw new ExecutorException(String.format(MSG_TOO_MANY_KEYS, counter));
      }
      //如果分配集合为空，则给每个对象属性赋值
      ParamMap<?> paramMap = iterator.next();
      if (assignerList.isEmpty()) {
        for (int i = 0; i < keyProperties.length; i++) {
          assignerList
              .add(getAssignerForParamMap(configuration, rsmd, i + 1, paramMap, keyProperties[i], keyProperties, false)
                  .getValue());
        }
      }
      assignerList.forEach(x -> x.assign(rs, paramMap));
      counter++;
    }
  }
  //分配主键给参数Map
  private void assignKeysToParamMap(Configuration configuration, ResultSet rs, ResultSetMetaData rsmd,
      String[] keyProperties, Map<String, ?> paramMap) throws SQLException {
    if (paramMap.isEmpty()) {
      return;
    }
    //遍历map的key属性
    Map<String, Entry<Iterator<?>, List<KeyAssigner>>> assignerMap = new HashMap<>();
    for (int i = 0; i < keyProperties.length; i++) {
      Entry<String, KeyAssigner> entry = getAssignerForParamMap(configuration, rsmd, i + 1, paramMap, keyProperties[i],
          keyProperties, true);
      Entry<Iterator<?>, List<KeyAssigner>> iteratorPair = assignerMap.computeIfAbsent(entry.getKey(),
          k -> entry(collectionize(paramMap.get(k)).iterator(), new ArrayList<>()));
      iteratorPair.getValue().add(entry.getValue());
    }
    //遍历返回结果赋值
    long counter = 0;
    while (rs.next()) {
      for (Entry<Iterator<?>, List<KeyAssigner>> pair : assignerMap.values()) {
        if (!pair.getKey().hasNext()) {
          throw new ExecutorException(String.format(MSG_TOO_MANY_KEYS, counter));
        }
        Object param = pair.getKey().next();
        pair.getValue().forEach(x -> x.assign(rs, param));
      }
      counter++;
    }
  }
  //从参数Map获取分配
  private Entry<String, KeyAssigner> getAssignerForParamMap(Configuration config, ResultSetMetaData rsmd,
      int columnPosition, Map<String, ?> paramMap, String keyProperty, String[] keyProperties, boolean omitParamName) {
    Set<String> keySet = paramMap.keySet();
    //如果唯一的参数这种方式使用{@code @Param("param2")}，一定是被param2.x这样引用
    // A caveat : if the only parameter has {@code @Param("param2")} on it,
    // it must be referenced with param name e.g. 'param2.x'.
    //主键集合不包括参数名字
    boolean singleParam = !keySet.contains(SECOND_GENERIC_PARAM_NAME);
    //属性获取第一个点
    int firstDot = keyProperty.indexOf('.');
    //如果是没有
    if (firstDot == -1) {
      //如果是单个参数，直接分配
      if (singleParam) {
        return getAssignerForSingleParam(config, rsmd, columnPosition, paramMap, keyProperty, omitParamName);
      }
      throw new ExecutorException("Could not determine which parameter to assign generated keys to. "
          + "Note that when there are multiple parameters, 'keyProperty' must include the parameter name (e.g. 'param.id'). "
          + "Specified key properties are " + ArrayUtil.toString(keyProperties) + " and available parameters are "
          + keySet);
    }
    //从0到第一个点截断，取后面的参数名字
    String paramName = keyProperty.substring(0, firstDot);
    //判断key集合是否包含参数名字
    if (keySet.contains(paramName)) {
      String argParamName = omitParamName ? null : paramName;
      String argKeyProperty = keyProperty.substring(firstDot + 1);
      return entry(paramName, new KeyAssigner(config, rsmd, columnPosition, argParamName, argKeyProperty));
    } else if (singleParam) {
      //单个参数的分配
      return getAssignerForSingleParam(config, rsmd, columnPosition, paramMap, keyProperty, omitParamName);
    } else {
      throw new ExecutorException("Could not find parameter '" + paramName + "'. "
          + "Note that when there are multiple parameters, 'keyProperty' must include the parameter name (e.g. 'param.id'). "
          + "Specified key properties are " + ArrayUtil.toString(keyProperties) + " and available parameters are "
          + keySet);
    }
  }
  //从单个参数获取分配
  private Entry<String, KeyAssigner> getAssignerForSingleParam(Configuration config, ResultSetMetaData rsmd,
      int columnPosition, Map<String, ?> paramMap, String keyProperty, boolean omitParamName) {
    // Assume 'keyProperty' to be a property of the single param.
    //假设这个属性的参数有'keyProperty'
    String singleParamName = nameOfSingleParam(paramMap);
    String argParamName = omitParamName ? null : singleParamName;
    //封装该实体类
    return entry(singleParamName, new KeyAssigner(config, rsmd, columnPosition, argParamName, keyProperty));
  }
  //单个参数的名字
  private static String nameOfSingleParam(Map<String, ?> paramMap) {
    // There is virtually one parameter, so any key works.
    return paramMap.keySet().iterator().next();
  }
  //集合化参数
  private static Collection<?> collectionize(Object param) {
    if (param instanceof Collection) {
      return (Collection<?>) param;
    } else if (param instanceof Object[]) {
      return Arrays.asList((Object[]) param);
    } else {
      return Arrays.asList(param);
    }
  }
  //在Java9中取代了Map.entry(key, value)的方式
  private static <K, V> Entry<K, V> entry(K key, V value) {
    // Replace this with Map.entry(key, value) in Java 9.
    return new AbstractMap.SimpleImmutableEntry<>(key, value);
  }
 //主键分配类
  private class KeyAssigner {
    //全局配置
    private final Configuration configuration;
    //结果元数据集
    private final ResultSetMetaData rsmd;
    //类型处理注册器
    private final TypeHandlerRegistry typeHandlerRegistry;
    //列位置
    private final int columnPosition;
    //参数名字
    private final String paramName;
    //属性名字
    private final String propertyName;
    //类型处理器
    private TypeHandler<?> typeHandler;
    //构造函数
    protected KeyAssigner(Configuration configuration, ResultSetMetaData rsmd, int columnPosition, String paramName,
        String propertyName) {
      super();
      this.configuration = configuration;
      this.rsmd = rsmd;
      this.typeHandlerRegistry = configuration.getTypeHandlerRegistry();
      this.columnPosition = columnPosition;
      this.paramName = paramName;
      this.propertyName = propertyName;
    }
    //分配结果集和参数
    protected void assign(ResultSet rs, Object param) {
      if (paramName != null) {
        // If paramName is set, param is ParamMap
        param = ((ParamMap<?>) param).get(paramName);
      }
      //获取元对象
      MetaObject metaParam = configuration.newMetaObject(param);
      try {
        //如果类型处理器为空
        if (typeHandler == null) {
          //元参数判断是否有set方法
          if (metaParam.hasSetter(propertyName)) {
            //元参数获取set类型
            Class<?> propertyType = metaParam.getSetterType(propertyName);
            //类型处理器注册器获取对应的类型处理器
            typeHandler = typeHandlerRegistry.getTypeHandler(propertyType,
                JdbcType.forCode(rsmd.getColumnType(columnPosition)));
          } else {
            throw new ExecutorException("No setter found for the keyProperty '" + propertyName + "' in '"
                + metaParam.getOriginalObject().getClass().getName() + "'.");
          }
        }
        if (typeHandler == null) {
          // Error?
        } else {
          //类型处理器处理结果返回值赋值给属性
          Object value = typeHandler.getResult(rs, columnPosition);
          metaParam.setValue(propertyName, value);
        }
      } catch (SQLException e) {
        throw new ExecutorException("Error getting generated key or setting result to parameter object. Cause: " + e,
            e);
      }
    }
  }
}
```

#### 4.3.2.3、NoKeyGenerator没有主键生成

```java
/**
 * 咩有主键生成
 * @author Clinton Begin
 * @author Kazuki Shimizu
 */
public class NoKeyGenerator implements KeyGenerator {

  /**
   * 共享实例
   * A shared instance.
   * @since 3.4.3
   */
  public static final NoKeyGenerator INSTANCE = new NoKeyGenerator();
  //执行前，什么也不做
  @Override
  public void processBefore(Executor executor, MappedStatement ms, Statement stmt, Object parameter) {
    // Do Nothing
  }
  //执行后什么也不做
  @Override
  public void processAfter(Executor executor, MappedStatement ms, Statement stmt, Object parameter) {
    // Do Nothing
  }

}
```

#### 4.3.2.4、SelectKeyGenerator查询Key生成策略(todo 第二遍)

```java
/**
 * 查询主键生成
 * @author Clinton Begin
 * @author Jeff Butler
 */
public class SelectKeyGenerator implements KeyGenerator {
  //查询key前缀
  public static final String SELECT_KEY_SUFFIX = "!selectKey";
  //是否在执行前
  private final boolean executeBefore;
  //主键会话
  private final MappedStatement keyStatement;
  //构造器函数
  public SelectKeyGenerator(MappedStatement keyStatement, boolean executeBefore) {
    this.executeBefore = executeBefore;
    this.keyStatement = keyStatement;
  }
  //在执行前生成
  @Override
  public void processBefore(Executor executor, MappedStatement ms, Statement stmt, Object parameter) {
    //是否在执行前执行判断
    if (executeBefore) {
      processGeneratedKeys(executor, ms, parameter);
    }
  }
  //在执行后生成
  @Override
  public void processAfter(Executor executor, MappedStatement ms, Statement stmt, Object parameter) {
    //是否在执行后生成
    if (!executeBefore) {
      processGeneratedKeys(executor, ms, parameter);
    }
  }
  //执行生成主键key
  private void processGeneratedKeys(Executor executor, MappedStatement ms, Object parameter) {
    try {
      //参数不为空，主键会话不为空，主键会话的属性不为空
      if (parameter != null && keyStatement != null && keyStatement.getKeyProperties() != null) {
        //获取会话主键属性
        String[] keyProperties = keyStatement.getKeyProperties();
        //获取全局配置
        final Configuration configuration = ms.getConfiguration();
        //配置获取元对象
        final MetaObject metaParam = configuration.newMetaObject(parameter);
        // Do not close keyExecutor.
        // The transaction will be closed by parent executor.
        // 不要关闭主键执行器--类型选择简单执行器SimPleExecutor
        Executor keyExecutor = configuration.newExecutor(executor.getTransaction(), ExecutorType.SIMPLE);
        //主键执行器执行查询
        List<Object> values = keyExecutor.query(keyStatement, parameter, RowBounds.DEFAULT, Executor.NO_RESULT_HANDLER);
        //没有返回数据
        if (values.size() == 0) {
          throw new ExecutorException("SelectKey returned no data.");
          //返回数据超过一条
        } else if (values.size() > 1) {
          throw new ExecutorException("SelectKey returned more than one value.");
        } else {
          //获取元对象结果
          MetaObject metaResult = configuration.newMetaObject(values.get(0));
          if (keyProperties.length == 1) {
            if (metaResult.hasGetter(keyProperties[0])) {
              //设置元参数结果值
              setValue(metaParam, keyProperties[0], metaResult.getValue(keyProperties[0]));
            } else {
              // no getter for the property - maybe just a single value object
              // so try that
              //咩有get方法的属性，可能只是一个值对象，可以尝试下面这种方式。
              setValue(metaParam, keyProperties[0], values.get(0));
            }
          } else {
            //处理多个属性
            handleMultipleProperties(keyProperties, metaParam, metaResult);
          }
        }
      }
    } catch (ExecutorException e) {
      throw e;
    } catch (Exception e) {
      throw new ExecutorException("Error selecting key or setting result to parameter object. Cause: " + e, e);
    }
  }
  //处理多个属性
  private void handleMultipleProperties(String[] keyProperties,
      MetaObject metaParam, MetaObject metaResult) {
    //会话获取列字段
    String[] keyColumns = keyStatement.getKeyColumns();
    //如果列字段没有不存在
    if (keyColumns == null || keyColumns.length == 0) {
      // no key columns specified, just use the property names
      //没有key列被指定，使用属性名字
      for (String keyProperty : keyProperties) {
        setValue(metaParam, keyProperty, metaResult.getValue(keyProperty));
      }
    } else {
      if (keyColumns.length != keyProperties.length) {
        throw new ExecutorException("If SelectKey has key columns, the number must match the number of key properties.");
      }
      //否则遍历列字段设置值。
      for (int i = 0; i < keyProperties.length; i++) {
        setValue(metaParam, keyProperties[i], metaResult.getValue(keyColumns[i]));
      }
    }
  }
  //设置值
  private void setValue(MetaObject metaParam, String property, Object value) {
    //如果元参数有set方法设置值
    if (metaParam.hasSetter(property)) {
      metaParam.setValue(property, value);
    } else {
      throw new ExecutorException("No setter found for the keyProperty '" + property + "' in " + metaParam.getOriginalObject().getClass().getName() + ".");
    }
  }
}
```

### 4.3.3、Loader加载器包

#### 4.3.3.1、基础包

![image-20200311203339813](/Users/didi/Library/Application Support/typora-user-images/image-20200311203339813.png)

##### 4.3.3.1.1、ProxyFactory代理工厂

```java
/**
 * 代理工厂
 * @author Eduardo Macarron
 */
public interface ProxyFactory {
  //接口设置默认的设置属性，子实现类 可以有选择实现 不实现则走默认
  default void setProperties(Properties properties) {
    // NOP
  }
  //创建代理
  Object createProxy(Object target, ResultLoaderMap lazyLoader, Configuration configuration, ObjectFactory objectFactory, List<Class<?>> constructorArgTypes, List<Object> constructorArgs);

}
```

##### 4.3.3.1.2、AbstractEnhanceDeserializationProxy

![image-20200311203954962](/Users/didi/Library/Application Support/typora-user-images/image-20200311203954962.png)

```java
/**
 * 抽象增强反序列化代理
 * @author Clinton Begin
 */
public abstract class AbstractEnhancedDeserializationProxy {
  //最后的方法
  protected static final String FINALIZE_METHOD = "finalize";
  //写取代方法
  protected static final String WRITE_REPLACE_METHOD = "writeReplace";
  //类型
  private final Class<?> type;
  //未加载属性
  private final Map<String, ResultLoaderMap.LoadPair> unloadedProperties;
  //对象工厂
  private final ObjectFactory objectFactory;
  //构造参数类型
  private final List<Class<?>> constructorArgTypes;
  //构造参数
  private final List<Object> constructorArgs;
  //重新加载属性锁
  private final Object reloadingPropertyLock;
  //重新加载属性
  private boolean reloadingProperty;
  //构造函数
  protected AbstractEnhancedDeserializationProxy(Class<?> type, Map<String, ResultLoaderMap.LoadPair> unloadedProperties,
          ObjectFactory objectFactory, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
    this.type = type;
    this.unloadedProperties = unloadedProperties;
    this.objectFactory = objectFactory;
    this.constructorArgTypes = constructorArgTypes;
    this.constructorArgs = constructorArgs;
    this.reloadingPropertyLock = new Object();
    this.reloadingProperty = false;
  }
  //执行
  public final Object invoke(Object enhanced, Method method, Object[] args) throws Throwable {
    //获取方法名称
    final String methodName = method.getName();
    try {
      //判断是否是写取代方法
      if (WRITE_REPLACE_METHOD.equals(methodName)) {
        final Object original;
        //对象工厂创建该类型的对象
        if (constructorArgTypes.isEmpty()) {
          original = objectFactory.create(type);
        } else {
          //创建构造类型和构造参数的对象
          original = objectFactory.create(type, constructorArgTypes, constructorArgs);
        }
        //属性拷贝器拷贝
        PropertyCopier.copyBeanProperties(type, enhanced, original);
        //返回新的序列化状态持有器
        return this.newSerialStateHolder(original, unloadedProperties, objectFactory, constructorArgTypes, constructorArgs);
      } else {
        //是否重新加载属性锁 ，加锁
        synchronized (this.reloadingPropertyLock) {
          //属性名根据方法解析属性，属性获取属性key
          if (!FINALIZE_METHOD.equals(methodName) && PropertyNamer.isProperty(methodName) && !reloadingProperty) {
            final String property = PropertyNamer.methodToProperty(methodName);
            final String propertyKey = property.toUpperCase(Locale.ENGLISH);
            //未加载属性包含该属性
            if (unloadedProperties.containsKey(propertyKey)) {
              //从未加载属性移除该属性
              final ResultLoaderMap.LoadPair loadPair = unloadedProperties.remove(propertyKey);
              if (loadPair != null) {
                try {
                  //重新加载属性
                  reloadingProperty = true;
                  loadPair.load(enhanced);
                } finally {
                  //不需要重新加载属性
                  reloadingProperty = false;
                }
              } else {
                //我不确定如果这种场景会确实发生，或者仅仅测试中。我们有一个未读属性，但是没有加载到它。
                /* I'm not sure if this case can really happen or is just in tests -
                 * we have an unread property but no loadPair to load it. */
                throw new ExecutorException("An attempt has been made to read a not loaded lazy property '"
                        + property + "' of a disconnected object");
              }
            }
          }

          return enhanced;
        }
      }
    } catch (Throwable t) {
      throw ExceptionUtil.unwrapThrowable(t);
    }
  }
 //抽象方法需要初始化序列化状态持有器
  protected abstract AbstractSerialStateHolder newSerialStateHolder(
          Object userBean,
          Map<String, ResultLoaderMap.LoadPair> unloadedProperties,
          ObjectFactory objectFactory,
          List<Class<?>> constructorArgTypes,
          List<Object> constructorArgs);

}
```

##### 4.3.3.1.3、WriteReplaceInterface写入取代接口

```java
/**
 * 写入取代接口
 * @author Eduardo Macarron
 */
public interface WriteReplaceInterface {

  Object writeReplace() throws ObjectStreamException;

}
```

##### 4.3.3.1.4、AbstractSerialStateHolder抽象序列化状态持有器

![image-20200311205421333](/Users/didi/Library/Application Support/typora-user-images/image-20200311205421333.png)

```java
/**
 * 抽象序列化状态持有器
 * @author Eduardo Macarron
 * @author Franta Mejta
 */
public abstract class AbstractSerialStateHolder implements Externalizable {

  private static final long serialVersionUID = 8940388717901644661L;
  //对象输出流
  private static final ThreadLocal<ObjectOutputStream> stream = new ThreadLocal<>();
  //用户bean字节
  private byte[] userBeanBytes = new byte[0];
  //用户bean
  private Object userBean;
  //未加载属性
  private Map<String, ResultLoaderMap.LoadPair> unloadedProperties;
  //对象工厂
  private ObjectFactory objectFactory;
  //构造器参数类型
  private Class<?>[] constructorArgTypes;
  //构造器参数
  private Object[] constructorArgs;
  //抽象序列化状态空构造函数
  public AbstractSerialStateHolder() {
  }
  //抽象序列化状态构造函数
  public AbstractSerialStateHolder(
          final Object userBean,
          final Map<String, ResultLoaderMap.LoadPair> unloadedProperties,
          final ObjectFactory objectFactory,
          List<Class<?>> constructorArgTypes,
          List<Object> constructorArgs) {
    this.userBean = userBean;
    this.unloadedProperties = new HashMap<>(unloadedProperties);
    this.objectFactory = objectFactory;
    this.constructorArgTypes = constructorArgTypes.toArray(new Class<?>[0]);
    this.constructorArgs = constructorArgs.toArray(new Object[0]);
  }
  //写到外面
  @Override
  public final void writeExternal(final ObjectOutput out) throws IOException {
    boolean firstRound = false;
    //字节数组输出流
    final ByteArrayOutputStream baos = new ByteArrayOutputStream();
    //获取流信息
    ObjectOutputStream os = stream.get();
    //如果流为空，则设置流信息
    if (os == null) {
      os = new ObjectOutputStream(baos);
      firstRound = true;
      stream.set(os);
    }
    //输出流设置内容
    os.writeObject(this.userBean);
    os.writeObject(this.unloadedProperties);
    os.writeObject(this.objectFactory);
    os.writeObject(this.constructorArgTypes);
    os.writeObject(this.constructorArgs);

    final byte[] bytes = baos.toByteArray();
    out.writeObject(bytes);
    //如果第一次 流移除
    if (firstRound) {
      stream.remove();
    }
  }
  //读取外部输入流
  @Override
  public final void readExternal(final ObjectInput in) throws IOException, ClassNotFoundException {
    final Object data = in.readObject();
    //设置用户bean信息
    if (data.getClass().isArray()) {
      this.userBeanBytes = (byte[]) data;
    } else {
      this.userBean = data;
    }
  }
  //读取解析
  @SuppressWarnings("unchecked")
  protected final Object readResolve() throws ObjectStreamException {
    //第二次运行
    /* Second run */
    if (this.userBean != null && this.userBeanBytes.length == 0) {
      return this.userBean;
    }
    //第一次运行
    /* First run */
    try (ObjectInputStream in = new LookAheadObjectInputStream(new ByteArrayInputStream(this.userBeanBytes))) {
      this.userBean = in.readObject();
      this.unloadedProperties = (Map<String, ResultLoaderMap.LoadPair>) in.readObject();
      this.objectFactory = (ObjectFactory) in.readObject();
      this.constructorArgTypes = (Class<?>[]) in.readObject();
      this.constructorArgs = (Object[]) in.readObject();
    } catch (final IOException ex) {
      throw (ObjectStreamException) new StreamCorruptedException().initCause(ex);
    } catch (final ClassNotFoundException ex) {
      throw (ObjectStreamException) new InvalidClassException(ex.getLocalizedMessage()).initCause(ex);
    }

    final Map<String, ResultLoaderMap.LoadPair> arrayProps = new HashMap<>(this.unloadedProperties);
    final List<Class<?>> arrayTypes = Arrays.asList(this.constructorArgTypes);
    final List<Object> arrayValues = Arrays.asList(this.constructorArgs);
    //创建反序列化代理
    return this.createDeserializationProxy(userBean, arrayProps, objectFactory, arrayTypes, arrayValues);
  }
  //创建反序列化代理
  protected abstract Object createDeserializationProxy(Object target, Map<String, ResultLoaderMap.LoadPair> unloadedProperties, ObjectFactory objectFactory,
          List<Class<?>> constructorArgTypes, List<Object> constructorArgs);
  //规划输入流
  private static class LookAheadObjectInputStream extends ObjectInputStream {
    //黑名单
    private static final List<String> blacklist = Arrays.asList(
        "org.apache.commons.beanutils.BeanComparator",
        "org.apache.commons.collections.functors.InvokerTransformer",
        "org.apache.commons.collections.functors.InstantiateTransformer",
        "org.apache.commons.collections4.functors.InvokerTransformer",
        "org.apache.commons.collections4.functors.InstantiateTransformer",
        "org.codehaus.groovy.runtime.ConvertedClosure",
        "org.codehaus.groovy.runtime.MethodClosure",
        "org.springframework.beans.factory.ObjectFactory",
        "org.springframework.transaction.jta.JtaTransactionManager",
        "com.sun.org.apache.xalan.internal.xsltc.trax.TemplatesImpl");
    //构造函数
    public LookAheadObjectInputStream(InputStream in) throws IOException {
      super(in);
    }
    //解析类，对于黑名单的类则抛出异常
    @Override
    protected Class<?> resolveClass(ObjectStreamClass desc) throws IOException, ClassNotFoundException {
      String className = desc.getName();
      if (blacklist.contains(className)) {
        throw new InvalidClassException(className, "Deserialization is not allowed for security reasons. "
            + "It is strongly recommended to configure the deserialization filter provided by JDK. "
            + "See http://openjdk.java.net/jeps/290 for the details.");
      }
      return super.resolveClass(desc);
    }
  }
}
```



##### 4.3.3.1.5、ResultLoader结果加载器

```java
/**
 * 结果集加载器
 * @author Clinton Begin
 */
public class ResultLoader {
  //全局配置
  protected final Configuration configuration;
  //执行器
  protected final Executor executor;
  //映射会话
  protected final MappedStatement mappedStatement;
  //参数对象
  protected final Object parameterObject;
  //目标类型
  protected final Class<?> targetType;
  //对象工厂
  protected final ObjectFactory objectFactory;
  //缓存key
  protected final CacheKey cacheKey;
  //执行SQL
  protected final BoundSql boundSql;
  //结果提取器
  protected final ResultExtractor resultExtractor;
  //创建线程ID
  protected final long creatorThreadId;
  //是否被加载
  protected boolean loaded;
  //结果对象
  protected Object resultObject;
  //结果加载构造函数
  public ResultLoader(Configuration config, Executor executor, MappedStatement mappedStatement, Object parameterObject, Class<?> targetType, CacheKey cacheKey, BoundSql boundSql) {
    this.configuration = config;
    this.executor = executor;
    this.mappedStatement = mappedStatement;
    this.parameterObject = parameterObject;
    this.targetType = targetType;
    this.objectFactory = configuration.getObjectFactory();
    this.cacheKey = cacheKey;
    this.boundSql = boundSql;
    this.resultExtractor = new ResultExtractor(configuration, objectFactory);
    this.creatorThreadId = Thread.currentThread().getId();
  }
  //加载结果
  public Object loadResult() throws SQLException {
    List<Object> list = selectList();
    resultObject = resultExtractor.extractObjectFromList(list, targetType);
    return resultObject;
  }
  //查询集合
  private <E> List<E> selectList() throws SQLException {
    Executor localExecutor = executor;
    if (Thread.currentThread().getId() != this.creatorThreadId || localExecutor.isClosed()) {
      localExecutor = newExecutor();
    }
    try {
      //本地执行器查询
      return localExecutor.query(mappedStatement, parameterObject, RowBounds.DEFAULT, Executor.NO_RESULT_HANDLER, cacheKey, boundSql);
    } finally {
      //如果本地执行与执行器不一致，则回滚事务
      if (localExecutor != executor) {
        localExecutor.close(false);
      }
    }
  }
  //初始化执行器
  private Executor newExecutor() {
    //获取环境配置
    final Environment environment = configuration.getEnvironment();
    if (environment == null) {
      throw new ExecutorException("ResultLoader could not load lazily.  Environment was not configured.");
    }
    //获取数据源
    final DataSource ds = environment.getDataSource();
    if (ds == null) {
      throw new ExecutorException("ResultLoader could not load lazily.  DataSource was not configured.");
    }
    //获取事务工厂
    final TransactionFactory transactionFactory = environment.getTransactionFactory();
    //构造事务
    final Transaction tx = transactionFactory.newTransaction(ds, null, false);
    //全局配置构建执行器 执行本次事务
    return configuration.newExecutor(tx, ExecutorType.SIMPLE);
  }
  //是否结果为空
  public boolean wasNull() {
    return resultObject == null;
  }

}

```

##### 4.3.3.1.6、ResultLoadMap结果加载Map

```java
/**
 * 结果加载Map
 * @author Clinton Begin
 * @author Franta Mejta
 */
public class ResultLoaderMap {
  //加载Map
  private final Map<String, LoadPair> loaderMap = new HashMap<>();
  //添加加载器
  public void addLoader(String property, MetaObject metaResultObject, ResultLoader resultLoader) {
    //获取大写的属性
    String upperFirst = getUppercaseFirstProperty(property);
    if (!upperFirst.equalsIgnoreCase(property) && loaderMap.containsKey(upperFirst)) {
      throw new ExecutorException("Nested lazy loaded result property '" + property
              + "' for query id '" + resultLoader.mappedStatement.getId()
              + " already exists in the result map. The leftmost property of all lazy loaded properties must be unique within a result map.");
    }
    //本地加载map存储该属性
    loaderMap.put(upperFirst, new LoadPair(property, metaResultObject, resultLoader));
  }
  //获取属性集合
  public final Map<String, LoadPair> getProperties() {
    return new HashMap<>(this.loaderMap);
  }
  //获取属性名字
  public Set<String> getPropertyNames() {
    return loaderMap.keySet();
  }
  //本地属性集合大小
  public int size() {
    return loaderMap.size();
  }
  //是否属性被加载过
  public boolean hasLoader(String property) {
    return loaderMap.containsKey(property.toUpperCase(Locale.ENGLISH));
  }
  //加载属性
  public boolean load(String property) throws SQLException {
    //本地属性移除该属性
    LoadPair pair = loaderMap.remove(property.toUpperCase(Locale.ENGLISH));
    if (pair != null) {
      pair.load();
      return true;
    }
    return false;
  }
  //移除属性
  public void remove(String property) {
    loaderMap.remove(property.toUpperCase(Locale.ENGLISH));
  }
  //加载所有方法名称
  public void loadAll() throws SQLException {
    final Set<String> methodNameSet = loaderMap.keySet();
    String[] methodNames = methodNameSet.toArray(new String[methodNameSet.size()]);
    for (String methodName : methodNames) {
      load(methodName);
    }
  }
  //获取大写的属性
  private static String getUppercaseFirstProperty(String property) {
    String[] parts = property.split("\\.");
    return parts[0].toUpperCase(Locale.ENGLISH);
  }

  /**
   * 还没被加载的属性
   * Property which was not loaded yet.
   */
  public static class LoadPair implements Serializable {

    private static final long serialVersionUID = 20130412;
    /**
     * 返回数据链接的工厂名字
     * Name of factory method which returns database connection.
     */
    private static final String FACTORY_METHOD = "getConfiguration";
    /**
     * 检查是否我们序列化
     * Object to check whether we went through serialization..
     */
    private final transient Object serializationCheck = new Object();
    /**
     * 元对象 设置加载属性
     * Meta object which sets loaded properties.
     */
    private transient MetaObject metaResultObject;
    /**
     * 结果集加载器 加载无用的属性
     * Result loader which loads unread properties.
     */
    private transient ResultLoader resultLoader;
    /**
     * 日志
     * Wow, logger.
     */
    private transient Log log;
    /**
     * 我们获取数据链接的工厂
     * Factory class through which we get database connection.
     */
    private Class<?> configurationFactory;
    /**
     * 未读属性的名字
     * Name of the unread property.
     */
    private String property;
    /**
     * 加载属性的SQL会话的ID
     * ID of SQL statement which loads the property.
     */
    private String mappedStatement;
    /**
     * sql会话的参数
     * Parameter of the sql statement.
     */
    private Serializable mappedParameter;
    //私有构造器
    private LoadPair(final String property, MetaObject metaResultObject, ResultLoader resultLoader) {
      this.property = property;
      this.metaResultObject = metaResultObject;
      this.resultLoader = resultLoader;
      //仅当原始对象可以序列化时才保存所需信息
      /* Save required information only if original object can be serialized. */
      if (metaResultObject != null && metaResultObject.getOriginalObject() instanceof Serializable) {
        final Object mappedStatementParameter = resultLoader.parameterObject;
         //参数也许会为空
        /* @todo May the parameter be null? */
        if (mappedStatementParameter instanceof Serializable) {
          this.mappedStatement = resultLoader.mappedStatement.getId();
          this.mappedParameter = (Serializable) mappedStatementParameter;
     
          this.configurationFactory = resultLoader.configuration.getConfigurationFactory();
        } else {
          Log log = this.getLogger();
          if (log.isDebugEnabled()) {
            log.debug("Property [" + this.property + "] of ["
                    + metaResultObject.getOriginalObject().getClass() + "] cannot be loaded "
                    + "after deserialization. Make sure it's loaded before serializing "
                    + "forenamed object.");
          }
        }
      }
    } 
    //加载
    public void load() throws SQLException {
      //这些属性应该不为空，除非加载对被序列化了，在这种场景下，方法不应该被调用
      /* These field should not be null unless the loadpair was serialized.
       * Yet in that case this method should not be called. */
      if (this.metaResultObject == null) {
        throw new IllegalArgumentException("metaResultObject is null");
      }
      if (this.resultLoader == null) {
        throw new IllegalArgumentException("resultLoader is null");
      }

      this.load(null);
    }
   //加载使用的对象
    public void load(final Object userObject) throws SQLException {
      if (this.metaResultObject == null || this.resultLoader == null) {
        if (this.mappedParameter == null) {
          throw new ExecutorException("Property [" + this.property + "] cannot be loaded because "
                  + "required parameter of mapped statement ["
                  + this.mappedStatement + "] is not serializable.");
        }

        final Configuration config = this.getConfiguration();
        final MappedStatement ms = config.getMappedStatement(this.mappedStatement);
        if (ms == null) {
          throw new ExecutorException("Cannot lazy load property [" + this.property
                  + "] of deserialized object [" + userObject.getClass()
                  + "] because configuration does not contain statement ["
                  + this.mappedStatement + "]");
        }

        this.metaResultObject = config.newMetaObject(userObject);
        this.resultLoader = new ResultLoader(config, new ClosedExecutor(), ms, this.mappedParameter,
                metaResultObject.getSetterType(this.property), null, null);
      }

      /* We are using a new executor because we may be (and likely are) on a new thread
       * and executors aren't thread safe. (Is this sufficient?)
       *
       * A better approach would be making executors thread safe. */
      if (this.serializationCheck == null) {
        final ResultLoader old = this.resultLoader;
        this.resultLoader = new ResultLoader(old.configuration, new ClosedExecutor(), old.mappedStatement,
                old.parameterObject, old.targetType, old.cacheKey, old.boundSql);
      }

      this.metaResultObject.setValue(property, this.resultLoader.loadResult());
    }
    //获取全局配置
    private Configuration getConfiguration() {
      if (this.configurationFactory == null) {
        throw new ExecutorException("Cannot get Configuration as configuration factory was not set.");
      }

      Object configurationObject;
      try {
        final Method factoryMethod = this.configurationFactory.getDeclaredMethod(FACTORY_METHOD);
        if (!Modifier.isStatic(factoryMethod.getModifiers())) {
          throw new ExecutorException("Cannot get Configuration as factory method ["
                  + this.configurationFactory + "]#["
                  + FACTORY_METHOD + "] is not static.");
        }

        if (!factoryMethod.isAccessible()) {
          configurationObject = AccessController.doPrivileged((PrivilegedExceptionAction<Object>) () -> {
            try {
              factoryMethod.setAccessible(true);
              return factoryMethod.invoke(null);
            } finally {
              factoryMethod.setAccessible(false);
            }
          });
        } else {
          configurationObject = factoryMethod.invoke(null);
        }
      } catch (final ExecutorException ex) {
        throw ex;
      } catch (final NoSuchMethodException ex) {
        throw new ExecutorException("Cannot get Configuration as factory class ["
                + this.configurationFactory + "] is missing factory method of name ["
                + FACTORY_METHOD + "].", ex);
      } catch (final PrivilegedActionException ex) {
        throw new ExecutorException("Cannot get Configuration as factory method ["
                + this.configurationFactory + "]#["
                + FACTORY_METHOD + "] threw an exception.", ex.getCause());
      } catch (final Exception ex) {
        throw new ExecutorException("Cannot get Configuration as factory method ["
                + this.configurationFactory + "]#["
                + FACTORY_METHOD + "] threw an exception.", ex);
      }

      if (!(configurationObject instanceof Configuration)) {
        throw new ExecutorException("Cannot get Configuration as factory method ["
                + this.configurationFactory + "]#["
                + FACTORY_METHOD + "] didn't return [" + Configuration.class + "] but ["
                + (configurationObject == null ? "null" : configurationObject.getClass()) + "].");
      }

      return Configuration.class.cast(configurationObject);
    }
    //获取日志器
    private Log getLogger() {
      if (this.log == null) {
        this.log = LogFactory.getLog(this.getClass());
      }
      return this.log;
    }
  }
  //被关闭的执行器
  private static final class ClosedExecutor extends BaseExecutor {
    //关闭的执行器
    public ClosedExecutor() {
      super(null, null);
    }
    //是否被关闭
    @Override
    public boolean isClosed() {
      return true;
    }
    //执行更新
    @Override
    protected int doUpdate(MappedStatement ms, Object parameter) throws SQLException {
      throw new UnsupportedOperationException("Not supported.");
    }
    //执行刷新
    @Override
    protected List<BatchResult> doFlushStatements(boolean isRollback) throws SQLException {
      throw new UnsupportedOperationException("Not supported.");
    }
    //执行查询
    @Override
    protected <E> List<E> doQuery(MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, BoundSql boundSql) throws SQLException {
      throw new UnsupportedOperationException("Not supported.");
    }
    //执行查询游标集
    @Override
    protected <E> Cursor<E> doQueryCursor(MappedStatement ms, Object parameter, RowBounds rowBounds, BoundSql boundSql) throws SQLException {
      throw new UnsupportedOperationException("Not supported.");
    }
  }
}
```



#### 4.3.3.2、cglib包

##### 4.3.3.2.1、CglibProxyFactory

```java
/**
 * CGLIB代理工厂
 * @author Clinton Begin
 */
public class CglibProxyFactory implements ProxyFactory {
  //释放结束方法
  private static final String FINALIZE_METHOD = "finalize";
  //写取代方法
  private static final String WRITE_REPLACE_METHOD = "writeReplace";
  //构造器 初始化cglib增强器
  public CglibProxyFactory() {
    try {
      Resources.classForName("net.sf.cglib.proxy.Enhancer");
    } catch (Throwable e) {
      throw new IllegalStateException("Cannot enable lazy loading because CGLIB is not available. Add CGLIB to your classpath.", e);
    }
  }
  //创建代理
  @Override
  public Object createProxy(Object target, ResultLoaderMap lazyLoader, Configuration configuration, ObjectFactory objectFactory, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
    return EnhancedResultObjectProxyImpl.createProxy(target, lazyLoader, configuration, objectFactory, constructorArgTypes, constructorArgs);
  }
  //创建反序列化代理
  public Object createDeserializationProxy(Object target, Map<String, ResultLoaderMap.LoadPair> unloadedProperties, ObjectFactory objectFactory, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
    return EnhancedDeserializationProxyImpl.createProxy(target, unloadedProperties, objectFactory, constructorArgTypes, constructorArgs);
  }
  //创建代理
  static Object crateProxy(Class<?> type, Callback callback, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
    Enhancer enhancer = new Enhancer();
    enhancer.setCallback(callback);
    enhancer.setSuperclass(type);
    try {
      //类型获取声明的方法写取代
      type.getDeclaredMethod(WRITE_REPLACE_METHOD);
      // ObjectOutputStream will call writeReplace of objects returned by writeReplace
      if (LogHolder.log.isDebugEnabled()) {
        LogHolder.log.debug(WRITE_REPLACE_METHOD + " method was found on bean " + type + ", make sure it returns this");
      }
    } catch (NoSuchMethodException e) {
      //设置该接口
      enhancer.setInterfaces(new Class[]{WriteReplaceInterface.class});
    } catch (SecurityException e) {
      // nothing to do here
    }
    Object enhanced;
    if (constructorArgTypes.isEmpty()) {
      //创建增强器
      enhanced = enhancer.create();
    } else {
      //创建有参数构造器
      Class<?>[] typesArray = constructorArgTypes.toArray(new Class[constructorArgTypes.size()]);
      Object[] valuesArray = constructorArgs.toArray(new Object[constructorArgs.size()]);
      enhanced = enhancer.create(typesArray, valuesArray);
    }
    return enhanced;
  }
  //方法拦截器  增强结果对象代理实现类
  private static class EnhancedResultObjectProxyImpl implements MethodInterceptor {
   //类型
    private final Class<?> type;
    //懒加载
    private final ResultLoaderMap lazyLoader;
    //是否正向
    private final boolean aggressive;
    //懒加载触发器方法
    private final Set<String> lazyLoadTriggerMethods;
    //对象工厂
    private final ObjectFactory objectFactory;
    //构造参数类型
    private final List<Class<?>> constructorArgTypes;
    //构造参数
    private final List<Object> constructorArgs;
    //私有构造函数
    private EnhancedResultObjectProxyImpl(Class<?> type, ResultLoaderMap lazyLoader, Configuration configuration, ObjectFactory objectFactory, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
      this.type = type;
      this.lazyLoader = lazyLoader;
      this.aggressive = configuration.isAggressiveLazyLoading();
      this.lazyLoadTriggerMethods = configuration.getLazyLoadTriggerMethods();
      this.objectFactory = objectFactory;
      this.constructorArgTypes = constructorArgTypes;
      this.constructorArgs = constructorArgs;
    }
    //创建代理
    public static Object createProxy(Object target, ResultLoaderMap lazyLoader, Configuration configuration, ObjectFactory objectFactory, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
      final Class<?> type = target.getClass();
      //初始化代理
      EnhancedResultObjectProxyImpl callback = new EnhancedResultObjectProxyImpl(type, lazyLoader, configuration, objectFactory, constructorArgTypes, constructorArgs);
      Object enhanced = crateProxy(type, callback, constructorArgTypes, constructorArgs);
      //属性拷贝
      PropertyCopier.copyBeanProperties(type, target, enhanced);
      //返回增强代理
      return enhanced;
    }
    //拦截方法
    @Override
    public Object intercept(Object enhanced, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
      final String methodName = method.getName();
      try {
        //是否懒加载
        synchronized (lazyLoader) {
          //写接口方法
          if (WRITE_REPLACE_METHOD.equals(methodName)) {
            Object original;
            //创建原对象工厂
            if (constructorArgTypes.isEmpty()) {
              original = objectFactory.create(type);
            } else {
              original = objectFactory.create(type, constructorArgTypes, constructorArgs);
            }
            //属性拷贝器
            PropertyCopier.copyBeanProperties(type, enhanced, original);
            //如果懒加载多于1个，则重新初始化持有器
            if (lazyLoader.size() > 0) {
              return new CglibSerialStateHolder(original, lazyLoader.getProperties(), objectFactory, constructorArgTypes, constructorArgs);
            } else {
              return original;
            }
          } else {
            //根据是否需要释放选择加载或者移除
            if (lazyLoader.size() > 0 && !FINALIZE_METHOD.equals(methodName)) {
              if (aggressive || lazyLoadTriggerMethods.contains(methodName)) {
                lazyLoader.loadAll();
              } else if (PropertyNamer.isSetter(methodName)) {
                final String property = PropertyNamer.methodToProperty(methodName);
                lazyLoader.remove(property);
              } else if (PropertyNamer.isGetter(methodName)) {
                final String property = PropertyNamer.methodToProperty(methodName);
                if (lazyLoader.hasLoader(property)) {
                  lazyLoader.load(property);
                }
              }
            }
          }
        }
        //方法代理执行方法
        return methodProxy.invokeSuper(enhanced, args);
      } catch (Throwable t) {
        throw ExceptionUtil.unwrapThrowable(t);
      }
    }
  }
   //静态内部类  增强反序列化代理实现类 并实现了方法拦截器
  private static class EnhancedDeserializationProxyImpl extends AbstractEnhancedDeserializationProxy implements MethodInterceptor {
    //私有构造函数
    private EnhancedDeserializationProxyImpl(Class<?> type, Map<String, ResultLoaderMap.LoadPair> unloadedProperties, ObjectFactory objectFactory,
            List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
      super(type, unloadedProperties, objectFactory, constructorArgTypes, constructorArgs);
    }
     //创建代理
    public static Object createProxy(Object target, Map<String, ResultLoaderMap.LoadPair> unloadedProperties, ObjectFactory objectFactory,
            List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
      final Class<?> type = target.getClass();
      EnhancedDeserializationProxyImpl callback = new EnhancedDeserializationProxyImpl(type, unloadedProperties, objectFactory, constructorArgTypes, constructorArgs);
      Object enhanced = crateProxy(type, callback, constructorArgTypes, constructorArgs);
      PropertyCopier.copyBeanProperties(type, target, enhanced);
      return enhanced;
    }
    //拦截
    @Override
    public Object intercept(Object enhanced, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
      final Object o = super.invoke(enhanced, method, args);
      return o instanceof AbstractSerialStateHolder ? o : methodProxy.invokeSuper(o, args);
    }
    //初始化
    @Override
    protected AbstractSerialStateHolder newSerialStateHolder(Object userBean, Map<String, ResultLoaderMap.LoadPair> unloadedProperties, ObjectFactory objectFactory,
            List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
      return new CglibSerialStateHolder(userBean, unloadedProperties, objectFactory, constructorArgTypes, constructorArgs);
    }
  }
  //日志持有器 内部类
  private static class LogHolder {
    private static final Log log = LogFactory.getLog(CglibProxyFactory.class);
  }

}
```

##### 4.3.3.2.2、CglibSerialStateHolder

```java
/**
 * Cglib序列化状态持有器
 * @author Eduardo Macarron
 */
class CglibSerialStateHolder extends AbstractSerialStateHolder {

  private static final long serialVersionUID = 8940388717901644661L;
  //空构造函数
  public CglibSerialStateHolder() {
  }
  //有参构造器
  public CglibSerialStateHolder(
          final Object userBean,
          final Map<String, ResultLoaderMap.LoadPair> unloadedProperties,
          final ObjectFactory objectFactory,
          List<Class<?>> constructorArgTypes,
          List<Object> constructorArgs) {
    super(userBean, unloadedProperties, objectFactory, constructorArgTypes, constructorArgs);
  }
  //创建反序列化代理
  @Override
  protected Object createDeserializationProxy(Object target, Map<String, ResultLoaderMap.LoadPair> unloadedProperties, ObjectFactory objectFactory,
          List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
    return new CglibProxyFactory().createDeserializationProxy(target, unloadedProperties, objectFactory, constructorArgTypes, constructorArgs);
  }
}
```

#### 4.3.3.3、javassist包

##### 4.3.3.3.1、JavassistSerialStateHolder

```java
/**
 *java字节码的AOP
 * @author Eduardo Macarron
 */
class JavassistSerialStateHolder extends AbstractSerialStateHolder {

  private static final long serialVersionUID = 8940388717901644661L;
  //空构造函数
  public JavassistSerialStateHolder() {
  }
  //有参数构造函数
  public JavassistSerialStateHolder(
          final Object userBean,
          final Map<String, ResultLoaderMap.LoadPair> unloadedProperties,
          final ObjectFactory objectFactory,
          List<Class<?>> constructorArgTypes,
          List<Object> constructorArgs) {
    super(userBean, unloadedProperties, objectFactory, constructorArgTypes, constructorArgs);
  }
  //创建反序列化代理
  @Override
  protected Object createDeserializationProxy(Object target, Map<String, ResultLoaderMap.LoadPair> unloadedProperties, ObjectFactory objectFactory,
          List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
    return new JavassistProxyFactory().createDeserializationProxy(target, unloadedProperties, objectFactory, constructorArgTypes, constructorArgs);
  }
}
```

##### 4.3.3.3.2、JavassistProxyFactory

```java
/**
 * Java字节码AOP
 * @author Eduardo Macarron
 */
public class JavassistProxyFactory implements org.apache.ibatis.executor.loader.ProxyFactory {
  //释放的方法
  private static final String FINALIZE_METHOD = "finalize";
  //写取代的方法
  private static final String WRITE_REPLACE_METHOD = "writeReplace";
  //构造函数加载
  public JavassistProxyFactory() {
    try {
      Resources.classForName("javassist.util.proxy.ProxyFactory");
    } catch (Throwable e) {
      throw new IllegalStateException("Cannot enable lazy loading because Javassist is not available. Add Javassist to your classpath.", e);
    }
  }
  //创建代理
  @Override
  public Object createProxy(Object target, ResultLoaderMap lazyLoader, Configuration configuration, ObjectFactory objectFactory, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
    return EnhancedResultObjectProxyImpl.createProxy(target, lazyLoader, configuration, objectFactory, constructorArgTypes, constructorArgs);
  }
  //创建反序列化代理
  public Object createDeserializationProxy(Object target, Map<String, ResultLoaderMap.LoadPair> unloadedProperties, ObjectFactory objectFactory, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
    return EnhancedDeserializationProxyImpl.createProxy(target, unloadedProperties, objectFactory, constructorArgTypes, constructorArgs);
  }
  //创建代理
  static Object crateProxy(Class<?> type, MethodHandler callback, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {

    ProxyFactory enhancer = new ProxyFactory();
    enhancer.setSuperclass(type);

    try {
      //获取声明的方法
      type.getDeclaredMethod(WRITE_REPLACE_METHOD);
      // ObjectOutputStream will call writeReplace of objects returned by writeReplace
      if (LogHolder.log.isDebugEnabled()) {
        LogHolder.log.debug(WRITE_REPLACE_METHOD + " method was found on bean " + type + ", make sure it returns this");
      }
    } catch (NoSuchMethodException e) {
      enhancer.setInterfaces(new Class[]{WriteReplaceInterface.class});
    } catch (SecurityException e) {
      // nothing to do here
    }

    Object enhanced;
    Class<?>[] typesArray = constructorArgTypes.toArray(new Class[constructorArgTypes.size()]);
    Object[] valuesArray = constructorArgs.toArray(new Object[constructorArgs.size()]);
    try {
      //创建增强器
      enhanced = enhancer.create(typesArray, valuesArray);
    } catch (Exception e) {
      throw new ExecutorException("Error creating lazy proxy.  Cause: " + e, e);
    }
    //设置处理器
    ((Proxy) enhanced).setHandler(callback);
    return enhanced;
  }
  //增强内部类结果对象代理实现类
  private static class EnhancedResultObjectProxyImpl implements MethodHandler {
    //类型
    private final Class<?> type;
    //懒加载结果
    private final ResultLoaderMap lazyLoader;
    //正向
    private final boolean aggressive;
    //懒加载触发器方法
    private final Set<String> lazyLoadTriggerMethods;
    //对象工厂
    private final ObjectFactory objectFactory;
    //构造器参数类型
    private final List<Class<?>> constructorArgTypes;
    //构造器参数
    private final List<Object> constructorArgs;
    //私有构造函数
    private EnhancedResultObjectProxyImpl(Class<?> type, ResultLoaderMap lazyLoader, Configuration configuration, ObjectFactory objectFactory, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
      this.type = type;
      this.lazyLoader = lazyLoader;
      this.aggressive = configuration.isAggressiveLazyLoading();
      this.lazyLoadTriggerMethods = configuration.getLazyLoadTriggerMethods();
      this.objectFactory = objectFactory;
      this.constructorArgTypes = constructorArgTypes;
      this.constructorArgs = constructorArgs;
    }
    //创建代理
    public static Object createProxy(Object target, ResultLoaderMap lazyLoader, Configuration configuration, ObjectFactory objectFactory, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
      final Class<?> type = target.getClass();
      EnhancedResultObjectProxyImpl callback = new EnhancedResultObjectProxyImpl(type, lazyLoader, configuration, objectFactory, constructorArgTypes, constructorArgs);
      Object enhanced = crateProxy(type, callback, constructorArgTypes, constructorArgs);
      PropertyCopier.copyBeanProperties(type, target, enhanced);
      return enhanced;
    }
    //方法执行
    @Override
    public Object invoke(Object enhanced, Method method, Method methodProxy, Object[] args) throws Throwable {
      final String methodName = method.getName();
      try {
        synchronized (lazyLoader) {
          if (WRITE_REPLACE_METHOD.equals(methodName)) {
            Object original;
            if (constructorArgTypes.isEmpty()) {
              original = objectFactory.create(type);
            } else {
              original = objectFactory.create(type, constructorArgTypes, constructorArgs);
            }
            PropertyCopier.copyBeanProperties(type, enhanced, original);
            if (lazyLoader.size() > 0) {
              return new JavassistSerialStateHolder(original, lazyLoader.getProperties(), objectFactory, constructorArgTypes, constructorArgs);
            } else {
              return original;
            }
          } else {
            if (lazyLoader.size() > 0 && !FINALIZE_METHOD.equals(methodName)) {
              if (aggressive || lazyLoadTriggerMethods.contains(methodName)) {
                lazyLoader.loadAll();
              } else if (PropertyNamer.isSetter(methodName)) {
                final String property = PropertyNamer.methodToProperty(methodName);
                lazyLoader.remove(property);
              } else if (PropertyNamer.isGetter(methodName)) {
                final String property = PropertyNamer.methodToProperty(methodName);
                if (lazyLoader.hasLoader(property)) {
                  lazyLoader.load(property);
                }
              }
            }
          }
        }
        return methodProxy.invoke(enhanced, args);
      } catch (Throwable t) {
        throw ExceptionUtil.unwrapThrowable(t);
      }
    }
  }
  //增强反序列化代理实现类
  private static class EnhancedDeserializationProxyImpl extends AbstractEnhancedDeserializationProxy implements MethodHandler {

    private EnhancedDeserializationProxyImpl(Class<?> type, Map<String, ResultLoaderMap.LoadPair> unloadedProperties, ObjectFactory objectFactory,
            List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
      super(type, unloadedProperties, objectFactory, constructorArgTypes, constructorArgs);
    }

    public static Object createProxy(Object target, Map<String, ResultLoaderMap.LoadPair> unloadedProperties, ObjectFactory objectFactory,
            List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
      final Class<?> type = target.getClass();
      EnhancedDeserializationProxyImpl callback = new EnhancedDeserializationProxyImpl(type, unloadedProperties, objectFactory, constructorArgTypes, constructorArgs);
      Object enhanced = crateProxy(type, callback, constructorArgTypes, constructorArgs);
      PropertyCopier.copyBeanProperties(type, target, enhanced);
      return enhanced;
    }

    @Override
    public Object invoke(Object enhanced, Method method, Method methodProxy, Object[] args) throws Throwable {
      final Object o = super.invoke(enhanced, method, args);
      return o instanceof AbstractSerialStateHolder ? o : methodProxy.invoke(o, args);
    }

    @Override
    protected AbstractSerialStateHolder newSerialStateHolder(Object userBean, Map<String, ResultLoaderMap.LoadPair> unloadedProperties, ObjectFactory objectFactory,
            List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
      return new JavassistSerialStateHolder(userBean, unloadedProperties, objectFactory, constructorArgTypes, constructorArgs);
    }
  }

  private static class LogHolder {
    private static final Log log = LogFactory.getLog(JavassistProxyFactory.class);
  }

}
```

#### 4.3.3.4、小结（todo）

* Javassist需要补齐知识，序列化和反序列化也需要补习，cglib也需要补齐

### 4.3.4、parameter参数包

#### 4.3.4.1、ParameterHandler参数处理器

![image-20200311220106077](/Users/didi/Library/Application Support/typora-user-images/image-20200311220106077.png)

```java
/**
 * {@code PreparedStatement}代码的参数集合处理器
 *
 * A parameter handler sets the parameters of the {@code PreparedStatement}.
 *
 * @author Clinton Begin
 */
public interface ParameterHandler {
  //获取参数对象
  Object getParameterObject();
  //设置参数
  void setParameters(PreparedStatement ps)
      throws SQLException;

}
```

### 4.3.5、result结果包

#### 4.3.5.1、DefaultMapResultHandler默认映射结果处理器

```java
/**
 * 默认Map结果处理器
 * @author Clinton Begin
 */
public class DefaultMapResultHandler<K, V> implements ResultHandler<V> {
  //映射结果
  private final Map<K, V> mappedResults;
  //映射key
  private final String mapKey;
  //对象工厂
  private final ObjectFactory objectFactory;
  //对象包装器工厂
  private final ObjectWrapperFactory objectWrapperFactory;
  //反射工厂
  private final ReflectorFactory reflectorFactory;
  //有参数构造函数
  @SuppressWarnings("unchecked")
  public DefaultMapResultHandler(String mapKey, ObjectFactory objectFactory, ObjectWrapperFactory objectWrapperFactory, ReflectorFactory reflectorFactory) {
    this.objectFactory = objectFactory;
    this.objectWrapperFactory = objectWrapperFactory;
    this.reflectorFactory = reflectorFactory;
    this.mappedResults = objectFactory.create(Map.class);
    this.mapKey = mapKey;
  }
  //处理结果
  @Override
  public void handleResult(ResultContext<? extends V> context) {
    final V value = context.getResultObject();
    final MetaObject mo = MetaObject.forObject(value, objectFactory, objectWrapperFactory, reflectorFactory);
    // TODO is that assignment always true?
    final K key = (K) mo.getValue(mapKey);
    mappedResults.put(key, value);
  }
  //获取映射结果
  public Map<K, V> getMappedResults() {
    return mappedResults;
  }
}

```

#### 4.3.5.2、DefaultResultContext默认结果上下文

```java
/**
 * 默认结果上下文
 * @author Clinton Begin
 */
public class DefaultResultContext<T> implements ResultContext<T> {
  //结果对象
  private T resultObject;
  //结果数量
  private int resultCount;
  //是否被停止
  private boolean stopped;
   //默认结果上下文
  public DefaultResultContext() {
    resultObject = null;
    resultCount = 0;
    stopped = false;
  }
  //获取结果对象
  @Override
  public T getResultObject() {
    return resultObject;
  }
  //获取结果数量
  @Override
  public int getResultCount() {
    return resultCount;
  }
  //是否被停止
  @Override
  public boolean isStopped() {
    return stopped;
  }
  //下一个结果对象
  public void nextResultObject(T resultObject) {
    resultCount++;
    this.resultObject = resultObject;
  }
  //停止
  @Override
  public void stop() {
    this.stopped = true;
  }

}
```

#### 4.3.5.3、DefaultResultHandler默认结果处理器

```java
/**
 * 默认结果处理器
 * @author Clinton Begin
 */
public class DefaultResultHandler implements ResultHandler<Object> {
  //集合
  private final List<Object> list;
  //空构造函数
  public DefaultResultHandler() {
    list = new ArrayList<>();
  }
  //有参构造函数
  @SuppressWarnings("unchecked")
  public DefaultResultHandler(ObjectFactory objectFactory) {
    list = objectFactory.create(List.class);
  }
  //处理上下文结果
  @Override
  public void handleResult(ResultContext<?> context) {
    list.add(context.getResultObject());
  }
   //获取结果集
  public List<Object> getResultList() {
    return list;
  }

}
```

#### 4.3.5.4、ResultMapException结果集映射异常

```java
/**
 * 结果集映射异常
 * @author Ryan Lamore
 */
public class ResultMapException extends PersistenceException {
  private static final long serialVersionUID = 3270932060569707623L;
  //空构造函数
  public ResultMapException() {
  }
  //传递信息构造函数
  public ResultMapException(String message) {
    super(message);
  }
  //传递信息和throwable构造函数
  public ResultMapException(String message, Throwable cause) {
    super(message, cause);
  }
  //结果映射异常
  public ResultMapException(Throwable cause) {
    super(cause);
  }
}
```

### 4.3.6、resultse t结果集

![image-20200311221459158](/Users/didi/Library/Application Support/typora-user-images/image-20200311221459158.png)

#### 4.3.6.1、ResultSetHandler结果集处理器

```java
/**
 * 结果集处理器
 * @author Clinton Begin
 */
public interface ResultSetHandler {
  //处理会话
  <E> List<E> handleResultSets(Statement stmt) throws SQLException;
  //处理游标会话
  <E> Cursor<E> handleCursorResultSets(Statement stmt) throws SQLException;
  //处理输出参数
  void handleOutputParameters(CallableStatement cs) throws SQLException;

}
```

#### 4.3.6.2、DefaultResultSetHandler默认结果集处理器(todo)

```java
/**
 * 默认结果集处理器
 * @author Clinton Begin
 * @author Eduardo Macarron
 * @author Iwao AVE!
 * @author Kazuki Shimizu
 */
public class DefaultResultSetHandler implements ResultSetHandler {
  //延迟对象
  private static final Object DEFERRED = new Object();
  //执行器
  private final Executor executor;
  //全局配置
  private final Configuration configuration;
  //映射会话
  private final MappedStatement mappedStatement;
  //行数据
  private final RowBounds rowBounds;
  //参数处理器
  private final ParameterHandler parameterHandler;
  //结果处理器
  private final ResultHandler<?> resultHandler;
  //执行SQL
  private final BoundSql boundSql;
  //类型处理器注册器
  private final TypeHandlerRegistry typeHandlerRegistry;
  //对象工厂
  private final ObjectFactory objectFactory;
  //反射工厂
  private final ReflectorFactory reflectorFactory;
  //嵌套结果映射
  // nested resultmaps
  private final Map<CacheKey, Object> nestedResultObjects = new HashMap<>();
  //原型对象
  private final Map<String, Object> ancestorObjects = new HashMap<>();
  //先前行值
  private Object previousRowValue;
  //多个结果集
  // multiple resultsets
  private final Map<String, ResultMapping> nextResultMaps = new HashMap<>();
  //等待的关系
  private final Map<CacheKey, List<PendingRelation>> pendingRelations = new HashMap<>();
   //自动映射的缓存
  // Cached Automappings
  private final Map<String, List<UnMappedColumnAutoMapping>> autoMappingsCache = new HashMap<>();
  //暂时标记 使用构造器映射（使用属性减少内存使用）
  // temporary marking flag that indicate using constructor mapping (use field to reduce memory usage)
  private boolean useConstructorMappings;
  //等待关系
  private static class PendingRelation {
    public MetaObject metaObject;
    public ResultMapping propertyMapping;
  }
  //未映射的列自动映射  内部类
  private static class UnMappedColumnAutoMapping {
    private final String column;
    private final String property;
    private final TypeHandler<?> typeHandler;
    private final boolean primitive;

    public UnMappedColumnAutoMapping(String column, String property, TypeHandler<?> typeHandler, boolean primitive) {
      this.column = column;
      this.property = property;
      this.typeHandler = typeHandler;
      this.primitive = primitive;
    }
  }
  //有参构造函数
  public DefaultResultSetHandler(Executor executor, MappedStatement mappedStatement, ParameterHandler parameterHandler, ResultHandler<?> resultHandler, BoundSql boundSql,
                                 RowBounds rowBounds) {
    this.executor = executor;
    this.configuration = mappedStatement.getConfiguration();
    this.mappedStatement = mappedStatement;
    this.rowBounds = rowBounds;
    this.parameterHandler = parameterHandler;
    this.boundSql = boundSql;
    this.typeHandlerRegistry = configuration.getTypeHandlerRegistry();
    this.objectFactory = configuration.getObjectFactory();
    this.reflectorFactory = configuration.getReflectorFactory();
    this.resultHandler = resultHandler;
  }

  //处理输出参数
  // HANDLE OUTPUT PARAMETER
  //

  @Override
  public void handleOutputParameters(CallableStatement cs) throws SQLException {
    final Object parameterObject = parameterHandler.getParameterObject();
    final MetaObject metaParam = configuration.newMetaObject(parameterObject);
    final List<ParameterMapping> parameterMappings = boundSql.getParameterMappings();
    for (int i = 0; i < parameterMappings.size(); i++) {
      final ParameterMapping parameterMapping = parameterMappings.get(i);
      if (parameterMapping.getMode() == ParameterMode.OUT || parameterMapping.getMode() == ParameterMode.INOUT) {
        if (ResultSet.class.equals(parameterMapping.getJavaType())) {
          handleRefCursorOutputParameter((ResultSet) cs.getObject(i + 1), parameterMapping, metaParam);
        } else {
          final TypeHandler<?> typeHandler = parameterMapping.getTypeHandler();
          metaParam.setValue(parameterMapping.getProperty(), typeHandler.getResult(cs, i + 1));
        }
      }
    }
  }
 //处理游标输出参数
  private void handleRefCursorOutputParameter(ResultSet rs, ParameterMapping parameterMapping, MetaObject metaParam) throws SQLException {
    if (rs == null) {
      return;
    }
    try {
      final String resultMapId = parameterMapping.getResultMapId();
      final ResultMap resultMap = configuration.getResultMap(resultMapId);
      final ResultSetWrapper rsw = new ResultSetWrapper(rs, configuration);
      if (this.resultHandler == null) {
        final DefaultResultHandler resultHandler = new DefaultResultHandler(objectFactory);
        handleRowValues(rsw, resultMap, resultHandler, new RowBounds(), null);
        metaParam.setValue(parameterMapping.getProperty(), resultHandler.getResultList());
      } else {
        handleRowValues(rsw, resultMap, resultHandler, new RowBounds(), null);
      }
    } finally {
      // issue #228 (close resultsets)
      closeResultSet(rs);
    }
  }

  //处理结果集
  // HANDLE RESULT SETS
  //
  @Override
  public List<Object> handleResultSets(Statement stmt) throws SQLException {
    ErrorContext.instance().activity("handling results").object(mappedStatement.getId());

    final List<Object> multipleResults = new ArrayList<>();

    int resultSetCount = 0;
    ResultSetWrapper rsw = getFirstResultSet(stmt);

    List<ResultMap> resultMaps = mappedStatement.getResultMaps();
    int resultMapCount = resultMaps.size();
    validateResultMapsCount(rsw, resultMapCount);
    while (rsw != null && resultMapCount > resultSetCount) {
      ResultMap resultMap = resultMaps.get(resultSetCount);
      handleResultSet(rsw, resultMap, multipleResults, null);
      rsw = getNextResultSet(stmt);
      cleanUpAfterHandlingResultSet();
      resultSetCount++;
    }

    String[] resultSets = mappedStatement.getResultSets();
    if (resultSets != null) {
      while (rsw != null && resultSetCount < resultSets.length) {
        ResultMapping parentMapping = nextResultMaps.get(resultSets[resultSetCount]);
        if (parentMapping != null) {
          String nestedResultMapId = parentMapping.getNestedResultMapId();
          ResultMap resultMap = configuration.getResultMap(nestedResultMapId);
          handleResultSet(rsw, resultMap, null, parentMapping);
        }
        rsw = getNextResultSet(stmt);
        cleanUpAfterHandlingResultSet();
        resultSetCount++;
      }
    }

    return collapseSingleResultList(multipleResults);
  }
  //处理游标结果集
  @Override
  public <E> Cursor<E> handleCursorResultSets(Statement stmt) throws SQLException {
    ErrorContext.instance().activity("handling cursor results").object(mappedStatement.getId());

    ResultSetWrapper rsw = getFirstResultSet(stmt);

    List<ResultMap> resultMaps = mappedStatement.getResultMaps();

    int resultMapCount = resultMaps.size();
    validateResultMapsCount(rsw, resultMapCount);
    if (resultMapCount != 1) {
      throw new ExecutorException("Cursor results cannot be mapped to multiple resultMaps");
    }

    ResultMap resultMap = resultMaps.get(0);
    return new DefaultCursor<>(this, resultMap, rsw, rowBounds);
  }
  //获取首个结果集
  private ResultSetWrapper getFirstResultSet(Statement stmt) throws SQLException {
    ResultSet rs = stmt.getResultSet();
    while (rs == null) {
      // move forward to get the first resultset in case the driver
      // doesn't return the resultset as the first result (HSQLDB 2.1)
      if (stmt.getMoreResults()) {
        rs = stmt.getResultSet();
      } else {
        if (stmt.getUpdateCount() == -1) {
          // no more results. Must be no resultset
          break;
        }
      }
    }
    return rs != null ? new ResultSetWrapper(rs, configuration) : null;
  }
  //获取下一个结果集
  private ResultSetWrapper getNextResultSet(Statement stmt) {
    // Making this method tolerant of bad JDBC drivers
    try {
      if (stmt.getConnection().getMetaData().supportsMultipleResultSets()) {
        // Crazy Standard JDBC way of determining if there are more results
        if (!(!stmt.getMoreResults() && stmt.getUpdateCount() == -1)) {
          ResultSet rs = stmt.getResultSet();
          if (rs == null) {
            return getNextResultSet(stmt);
          } else {
            return new ResultSetWrapper(rs, configuration);
          }
        }
      }
    } catch (Exception e) {
      // Intentionally ignored.
    }
    return null;
  }
  //关闭结果集
  private void closeResultSet(ResultSet rs) {
    try {
      if (rs != null) {
        rs.close();
      }
    } catch (SQLException e) {
      // ignore
    }
  }
  //嵌套结果对象清空
  private void cleanUpAfterHandlingResultSet() {
    nestedResultObjects.clear();
  }
   //校验结果映射数量
  private void validateResultMapsCount(ResultSetWrapper rsw, int resultMapCount) {
    if (rsw != null && resultMapCount < 1) {
      throw new ExecutorException("A query was run and no Result Maps were found for the Mapped Statement '" + mappedStatement.getId()
          + "'.  It's likely that neither a Result Type nor a Result Map was specified.");
    }
  }
  //处理结果集
  private void handleResultSet(ResultSetWrapper rsw, ResultMap resultMap, List<Object> multipleResults, ResultMapping parentMapping) throws SQLException {
    try {
      if (parentMapping != null) {
        handleRowValues(rsw, resultMap, null, RowBounds.DEFAULT, parentMapping);
      } else {
        if (resultHandler == null) {
          DefaultResultHandler defaultResultHandler = new DefaultResultHandler(objectFactory);
          handleRowValues(rsw, resultMap, defaultResultHandler, rowBounds, null);
          multipleResults.add(defaultResultHandler.getResultList());
        } else {
          handleRowValues(rsw, resultMap, resultHandler, rowBounds, null);
        }
      }
    } finally {
      // issue #228 (close resultsets)
      closeResultSet(rsw.getResultSet());
    }
  }
  //
  //处理单个结果集
  @SuppressWarnings("unchecked")
  private List<Object> collapseSingleResultList(List<Object> multipleResults) {
    return multipleResults.size() == 1 ? (List<Object>) multipleResults.get(0) : multipleResults;
  }

  // 简单的结果集 处理行数据
  // HANDLE ROWS FOR SIMPLE RESULTMAP
  //

  public void handleRowValues(ResultSetWrapper rsw, ResultMap resultMap, ResultHandler<?> resultHandler, RowBounds rowBounds, ResultMapping parentMapping) throws SQLException {
    if (resultMap.hasNestedResultMaps()) {
      ensureNoRowBounds();
      checkResultHandler();
      handleRowValuesForNestedResultMap(rsw, resultMap, resultHandler, rowBounds, parentMapping);
    } else {
      handleRowValuesForSimpleResultMap(rsw, resultMap, resultHandler, rowBounds, parentMapping);
    }
  }
  //确保没行数据
  private void ensureNoRowBounds() {
    if (configuration.isSafeRowBoundsEnabled() && rowBounds != null && (rowBounds.getLimit() < RowBounds.NO_ROW_LIMIT || rowBounds.getOffset() > RowBounds.NO_ROW_OFFSET)) {
      throw new ExecutorException("Mapped Statements with nested result mappings cannot be safely constrained by RowBounds. "
          + "Use safeRowBoundsEnabled=false setting to bypass this check.");
    }
  }
  //检查结果处理器
  protected void checkResultHandler() {
    if (resultHandler != null && configuration.isSafeResultHandlerEnabled() && !mappedStatement.isResultOrdered()) {
      throw new ExecutorException("Mapped Statements with nested result mappings cannot be safely used with a custom ResultHandler. "
          + "Use safeResultHandlerEnabled=false setting to bypass this check "
          + "or ensure your statement returns ordered data and set resultOrdered=true on it.");
    }
  }
  //处理简单结果集的行数据值
  private void handleRowValuesForSimpleResultMap(ResultSetWrapper rsw, ResultMap resultMap, ResultHandler<?> resultHandler, RowBounds rowBounds, ResultMapping parentMapping)
      throws SQLException {
    DefaultResultContext<Object> resultContext = new DefaultResultContext<>();
    ResultSet resultSet = rsw.getResultSet();
    skipRows(resultSet, rowBounds);
    while (shouldProcessMoreRows(resultContext, rowBounds) && !resultSet.isClosed() && resultSet.next()) {
      ResultMap discriminatedResultMap = resolveDiscriminatedResultMap(resultSet, resultMap, null);
      Object rowValue = getRowValue(rsw, discriminatedResultMap, null);
      storeObject(resultHandler, resultContext, rowValue, parentMapping, resultSet);
    }
  }
  //存储对象
  private void storeObject(ResultHandler<?> resultHandler, DefaultResultContext<Object> resultContext, Object rowValue, ResultMapping parentMapping, ResultSet rs) throws SQLException {
    if (parentMapping != null) {
      linkToParents(rs, parentMapping, rowValue);
    } else {
      callResultHandler(resultHandler, resultContext, rowValue);
    }
  }
 //调用结果处理器
  @SuppressWarnings("unchecked" /* because ResultHandler<?> is always ResultHandler<Object>*/)
  private void callResultHandler(ResultHandler<?> resultHandler, DefaultResultContext<Object> resultContext, Object rowValue) {
    resultContext.nextResultObject(rowValue);
    ((ResultHandler<Object>) resultHandler).handleResult(resultContext);
  }
  //应该执行更多行
  private boolean shouldProcessMoreRows(ResultContext<?> context, RowBounds rowBounds) {
    return !context.isStopped() && context.getResultCount() < rowBounds.getLimit();
  }
  //跳过行数据
  private void skipRows(ResultSet rs, RowBounds rowBounds) throws SQLException {
    if (rs.getType() != ResultSet.TYPE_FORWARD_ONLY) {
      if (rowBounds.getOffset() != RowBounds.NO_ROW_OFFSET) {
        rs.absolute(rowBounds.getOffset());
      }
    } else {
      for (int i = 0; i < rowBounds.getOffset(); i++) {
        if (!rs.next()) {
          break;
        }
      }
    }
  }

  // 获取简单结果集的行数据值
  // GET VALUE FROM ROW FOR SIMPLE RESULT MAP
  //

  private Object getRowValue(ResultSetWrapper rsw, ResultMap resultMap, String columnPrefix) throws SQLException {
    final ResultLoaderMap lazyLoader = new ResultLoaderMap();
    Object rowValue = createResultObject(rsw, resultMap, lazyLoader, columnPrefix);
    if (rowValue != null && !hasTypeHandlerForResultObject(rsw, resultMap.getType())) {
      final MetaObject metaObject = configuration.newMetaObject(rowValue);
      boolean foundValues = this.useConstructorMappings;
      if (shouldApplyAutomaticMappings(resultMap, false)) {
        foundValues = applyAutomaticMappings(rsw, resultMap, metaObject, columnPrefix) || foundValues;
      }
      foundValues = applyPropertyMappings(rsw, resultMap, metaObject, lazyLoader, columnPrefix) || foundValues;
      foundValues = lazyLoader.size() > 0 || foundValues;
      rowValue = foundValues || configuration.isReturnInstanceForEmptyRow() ? rowValue : null;
    }
    return rowValue;
  }
  //应该自动映射
  private boolean shouldApplyAutomaticMappings(ResultMap resultMap, boolean isNested) {
    if (resultMap.getAutoMapping() != null) {
      return resultMap.getAutoMapping();
    } else {
      if (isNested) {
        return AutoMappingBehavior.FULL == configuration.getAutoMappingBehavior();
      } else {
        return AutoMappingBehavior.NONE != configuration.getAutoMappingBehavior();
      }
    }
  }

  // 属性映射
  // PROPERTY MAPPINGS
  //

  private boolean applyPropertyMappings(ResultSetWrapper rsw, ResultMap resultMap, MetaObject metaObject, ResultLoaderMap lazyLoader, String columnPrefix)
      throws SQLException {
    final List<String> mappedColumnNames = rsw.getMappedColumnNames(resultMap, columnPrefix);
    boolean foundValues = false;
    final List<ResultMapping> propertyMappings = resultMap.getPropertyResultMappings();
    for (ResultMapping propertyMapping : propertyMappings) {
      String column = prependPrefix(propertyMapping.getColumn(), columnPrefix);
      if (propertyMapping.getNestedResultMapId() != null) {
        // the user added a column attribute to a nested result map, ignore it
        column = null;
      }
      if (propertyMapping.isCompositeResult()
          || (column != null && mappedColumnNames.contains(column.toUpperCase(Locale.ENGLISH)))
          || propertyMapping.getResultSet() != null) {
        Object value = getPropertyMappingValue(rsw.getResultSet(), metaObject, propertyMapping, lazyLoader, columnPrefix);
        // issue #541 make property optional
        final String property = propertyMapping.getProperty();
        if (property == null) {
          continue;
        } else if (value == DEFERRED) {
          foundValues = true;
          continue;
        }
        if (value != null) {
          foundValues = true;
        }
        if (value != null || (configuration.isCallSettersOnNulls() && !metaObject.getSetterType(property).isPrimitive())) {
          // gcode issue #377, call setter on nulls (value is not 'found')
          metaObject.setValue(property, value);
        }
      }
    }
    return foundValues;
  }
  // 获取属性映射的值
  private Object getPropertyMappingValue(ResultSet rs, MetaObject metaResultObject, ResultMapping propertyMapping, ResultLoaderMap lazyLoader, String columnPrefix)
      throws SQLException {
    if (propertyMapping.getNestedQueryId() != null) {
      return getNestedQueryMappingValue(rs, metaResultObject, propertyMapping, lazyLoader, columnPrefix);
    } else if (propertyMapping.getResultSet() != null) {
      addPendingChildRelation(rs, metaResultObject, propertyMapping);   // TODO is that OK?
      return DEFERRED;
    } else {
      final TypeHandler<?> typeHandler = propertyMapping.getTypeHandler();
      final String column = prependPrefix(propertyMapping.getColumn(), columnPrefix);
      return typeHandler.getResult(rs, column);
    }
  }
  // 创建自动映射
  private List<UnMappedColumnAutoMapping> createAutomaticMappings(ResultSetWrapper rsw, ResultMap resultMap, MetaObject metaObject, String columnPrefix) throws SQLException {
    final String mapKey = resultMap.getId() + ":" + columnPrefix;
    List<UnMappedColumnAutoMapping> autoMapping = autoMappingsCache.get(mapKey);
    if (autoMapping == null) {
      autoMapping = new ArrayList<>();
      final List<String> unmappedColumnNames = rsw.getUnmappedColumnNames(resultMap, columnPrefix);
      for (String columnName : unmappedColumnNames) {
        String propertyName = columnName;
        if (columnPrefix != null && !columnPrefix.isEmpty()) {
          // When columnPrefix is specified,
          // ignore columns without the prefix.
          if (columnName.toUpperCase(Locale.ENGLISH).startsWith(columnPrefix)) {
            propertyName = columnName.substring(columnPrefix.length());
          } else {
            continue;
          }
        }
        final String property = metaObject.findProperty(propertyName, configuration.isMapUnderscoreToCamelCase());
        if (property != null && metaObject.hasSetter(property)) {
          if (resultMap.getMappedProperties().contains(property)) {
            continue;
          }
          final Class<?> propertyType = metaObject.getSetterType(property);
          if (typeHandlerRegistry.hasTypeHandler(propertyType, rsw.getJdbcType(columnName))) {
            final TypeHandler<?> typeHandler = rsw.getTypeHandler(propertyType, columnName);
            autoMapping.add(new UnMappedColumnAutoMapping(columnName, property, typeHandler, propertyType.isPrimitive()));
          } else {
            configuration.getAutoMappingUnknownColumnBehavior()
                .doAction(mappedStatement, columnName, property, propertyType);
          }
        } else {
          configuration.getAutoMappingUnknownColumnBehavior()
              .doAction(mappedStatement, columnName, (property != null) ? property : propertyName, null);
        }
      }
      autoMappingsCache.put(mapKey, autoMapping);
    }
    return autoMapping;
  }
  //是否应用自动映射
  private boolean applyAutomaticMappings(ResultSetWrapper rsw, ResultMap resultMap, MetaObject metaObject, String columnPrefix) throws SQLException {
    List<UnMappedColumnAutoMapping> autoMapping = createAutomaticMappings(rsw, resultMap, metaObject, columnPrefix);
    boolean foundValues = false;
    if (!autoMapping.isEmpty()) {
      for (UnMappedColumnAutoMapping mapping : autoMapping) {
        final Object value = mapping.typeHandler.getResult(rsw.getResultSet(), mapping.column);
        if (value != null) {
          foundValues = true;
        }
        if (value != null || (configuration.isCallSettersOnNulls() && !mapping.primitive)) {
          // gcode issue #377, call setter on nulls (value is not 'found')
          metaObject.setValue(mapping.property, value);
        }
      }
    }
    return foundValues;
  }
  //多个结果集
  // MULTIPLE RESULT SETS

  private void linkToParents(ResultSet rs, ResultMapping parentMapping, Object rowValue) throws SQLException {
    CacheKey parentKey = createKeyForMultipleResults(rs, parentMapping, parentMapping.getColumn(), parentMapping.getForeignColumn());
    List<PendingRelation> parents = pendingRelations.get(parentKey);
    if (parents != null) {
      for (PendingRelation parent : parents) {
        if (parent != null && rowValue != null) {
          linkObjects(parent.metaObject, parent.propertyMapping, rowValue);
        }
      }
    }
  }
  //添加等待子关系
  private void addPendingChildRelation(ResultSet rs, MetaObject metaResultObject, ResultMapping parentMapping) throws SQLException {
    CacheKey cacheKey = createKeyForMultipleResults(rs, parentMapping, parentMapping.getColumn(), parentMapping.getColumn());
    PendingRelation deferLoad = new PendingRelation();
    deferLoad.metaObject = metaResultObject;
    deferLoad.propertyMapping = parentMapping;
    List<PendingRelation> relations = pendingRelations.computeIfAbsent(cacheKey, k -> new ArrayList<>());
    // issue #255
    relations.add(deferLoad);
    ResultMapping previous = nextResultMaps.get(parentMapping.getResultSet());
    if (previous == null) {
      nextResultMaps.put(parentMapping.getResultSet(), parentMapping);
    } else {
      if (!previous.equals(parentMapping)) {
        throw new ExecutorException("Two different properties are mapped to the same resultSet");
      }
    }
  }
  //创建多个结果集的key
  private CacheKey createKeyForMultipleResults(ResultSet rs, ResultMapping resultMapping, String names, String columns) throws SQLException {
    CacheKey cacheKey = new CacheKey();
    cacheKey.update(resultMapping);
    if (columns != null && names != null) {
      String[] columnsArray = columns.split(",");
      String[] namesArray = names.split(",");
      for (int i = 0; i < columnsArray.length; i++) {
        Object value = rs.getString(columnsArray[i]);
        if (value != null) {
          cacheKey.update(namesArray[i]);
          cacheKey.update(value);
        }
      }
    }
    return cacheKey;
  }

  //  创建结果集
  // INSTANTIATION & CONSTRUCTOR MAPPING
  //

  private Object createResultObject(ResultSetWrapper rsw, ResultMap resultMap, ResultLoaderMap lazyLoader, String columnPrefix) throws SQLException {
    this.useConstructorMappings = false; // reset previous mapping result
    final List<Class<?>> constructorArgTypes = new ArrayList<>();
    final List<Object> constructorArgs = new ArrayList<>();
    Object resultObject = createResultObject(rsw, resultMap, constructorArgTypes, constructorArgs, columnPrefix);
    if (resultObject != null && !hasTypeHandlerForResultObject(rsw, resultMap.getType())) {
      final List<ResultMapping> propertyMappings = resultMap.getPropertyResultMappings();
      for (ResultMapping propertyMapping : propertyMappings) {
        // issue gcode #109 && issue #149
        if (propertyMapping.getNestedQueryId() != null && propertyMapping.isLazy()) {
          resultObject = configuration.getProxyFactory().createProxy(resultObject, lazyLoader, configuration, objectFactory, constructorArgTypes, constructorArgs);
          break;
        }
      }
    }
    this.useConstructorMappings = resultObject != null && !constructorArgTypes.isEmpty(); // set current mapping result
    return resultObject;
  }
   //创建结果对象
  private Object createResultObject(ResultSetWrapper rsw, ResultMap resultMap, List<Class<?>> constructorArgTypes, List<Object> constructorArgs, String columnPrefix)
      throws SQLException {
    final Class<?> resultType = resultMap.getType();
    final MetaClass metaType = MetaClass.forClass(resultType, reflectorFactory);
    final List<ResultMapping> constructorMappings = resultMap.getConstructorResultMappings();
    if (hasTypeHandlerForResultObject(rsw, resultType)) {
      return createPrimitiveResultObject(rsw, resultMap, columnPrefix);
    } else if (!constructorMappings.isEmpty()) {
      return createParameterizedResultObject(rsw, resultType, constructorMappings, constructorArgTypes, constructorArgs, columnPrefix);
    } else if (resultType.isInterface() || metaType.hasDefaultConstructor()) {
      return objectFactory.create(resultType);
    } else if (shouldApplyAutomaticMappings(resultMap, false)) {
      return createByConstructorSignature(rsw, resultType, constructorArgTypes, constructorArgs);
    }
    throw new ExecutorException("Do not know how to create an instance of " + resultType);
  }
   //创建参数化的结果对象
  Object createParameterizedResultObject(ResultSetWrapper rsw, Class<?> resultType, List<ResultMapping> constructorMappings,
                                         List<Class<?>> constructorArgTypes, List<Object> constructorArgs, String columnPrefix) {
    boolean foundValues = false;
    for (ResultMapping constructorMapping : constructorMappings) {
      final Class<?> parameterType = constructorMapping.getJavaType();
      final String column = constructorMapping.getColumn();
      final Object value;
      try {
        if (constructorMapping.getNestedQueryId() != null) {
          value = getNestedQueryConstructorValue(rsw.getResultSet(), constructorMapping, columnPrefix);
        } else if (constructorMapping.getNestedResultMapId() != null) {
          final ResultMap resultMap = configuration.getResultMap(constructorMapping.getNestedResultMapId());
          value = getRowValue(rsw, resultMap, getColumnPrefix(columnPrefix, constructorMapping));
        } else {
          final TypeHandler<?> typeHandler = constructorMapping.getTypeHandler();
          value = typeHandler.getResult(rsw.getResultSet(), prependPrefix(column, columnPrefix));
        }
      } catch (ResultMapException | SQLException e) {
        throw new ExecutorException("Could not process result for mapping: " + constructorMapping, e);
      }
      constructorArgTypes.add(parameterType);
      constructorArgs.add(value);
      foundValues = value != null || foundValues;
    }
    return foundValues ? objectFactory.create(resultType, constructorArgTypes, constructorArgs) : null;
  }
  //创建根据构造特征的对象
  private Object createByConstructorSignature(ResultSetWrapper rsw, Class<?> resultType, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) throws SQLException {
    final Constructor<?>[] constructors = resultType.getDeclaredConstructors();
    final Constructor<?> defaultConstructor = findDefaultConstructor(constructors);
    if (defaultConstructor != null) {
      return createUsingConstructor(rsw, resultType, constructorArgTypes, constructorArgs, defaultConstructor);
    } else {
      for (Constructor<?> constructor : constructors) {
        if (allowedConstructorUsingTypeHandlers(constructor, rsw.getJdbcTypes())) {
          return createUsingConstructor(rsw, resultType, constructorArgTypes, constructorArgs, constructor);
        }
      }
    }
    throw new ExecutorException("No constructor found in " + resultType.getName() + " matching " + rsw.getClassNames());
  }
  //使用构造器创建对象
  private Object createUsingConstructor(ResultSetWrapper rsw, Class<?> resultType, List<Class<?>> constructorArgTypes, List<Object> constructorArgs, Constructor<?> constructor) throws SQLException {
    boolean foundValues = false;
    for (int i = 0; i < constructor.getParameterTypes().length; i++) {
      Class<?> parameterType = constructor.getParameterTypes()[i];
      String columnName = rsw.getColumnNames().get(i);
      TypeHandler<?> typeHandler = rsw.getTypeHandler(parameterType, columnName);
      Object value = typeHandler.getResult(rsw.getResultSet(), columnName);
      constructorArgTypes.add(parameterType);
      constructorArgs.add(value);
      foundValues = value != null || foundValues;
    }
    return foundValues ? objectFactory.create(resultType, constructorArgTypes, constructorArgs) : null;
  }
  //找到默认的构造器
  private Constructor<?> findDefaultConstructor(final Constructor<?>[] constructors) {
    if (constructors.length == 1) {
      return constructors[0];
    }

    for (final Constructor<?> constructor : constructors) {
      if (constructor.isAnnotationPresent(AutomapConstructor.class)) {
        return constructor;
      }
    }
    return null;
  }
  //允许使用类型处理器的构造器
  private boolean allowedConstructorUsingTypeHandlers(final Constructor<?> constructor, final List<JdbcType> jdbcTypes) {
    final Class<?>[] parameterTypes = constructor.getParameterTypes();
    if (parameterTypes.length != jdbcTypes.size()) {
      return false;
    }
    for (int i = 0; i < parameterTypes.length; i++) {
      if (!typeHandlerRegistry.hasTypeHandler(parameterTypes[i], jdbcTypes.get(i))) {
        return false;
      }
    }
    return true;
  }
  //创建主要的结果对象
  private Object createPrimitiveResultObject(ResultSetWrapper rsw, ResultMap resultMap, String columnPrefix) throws SQLException {
    final Class<?> resultType = resultMap.getType();
    final String columnName;
    if (!resultMap.getResultMappings().isEmpty()) {
      final List<ResultMapping> resultMappingList = resultMap.getResultMappings();
      final ResultMapping mapping = resultMappingList.get(0);
      columnName = prependPrefix(mapping.getColumn(), columnPrefix);
    } else {
      columnName = rsw.getColumnNames().get(0);
    }
    final TypeHandler<?> typeHandler = rsw.getTypeHandler(resultType, columnName);
    return typeHandler.getResult(rsw.getResultSet(), columnName);
  }

  //  嵌套查询
  // NESTED QUERY
  //

  private Object getNestedQueryConstructorValue(ResultSet rs, ResultMapping constructorMapping, String columnPrefix) throws SQLException {
    final String nestedQueryId = constructorMapping.getNestedQueryId();
    final MappedStatement nestedQuery = configuration.getMappedStatement(nestedQueryId);
    final Class<?> nestedQueryParameterType = nestedQuery.getParameterMap().getType();
    final Object nestedQueryParameterObject = prepareParameterForNestedQuery(rs, constructorMapping, nestedQueryParameterType, columnPrefix);
    Object value = null;
    if (nestedQueryParameterObject != null) {
      final BoundSql nestedBoundSql = nestedQuery.getBoundSql(nestedQueryParameterObject);
      final CacheKey key = executor.createCacheKey(nestedQuery, nestedQueryParameterObject, RowBounds.DEFAULT, nestedBoundSql);
      final Class<?> targetType = constructorMapping.getJavaType();
      final ResultLoader resultLoader = new ResultLoader(configuration, executor, nestedQuery, nestedQueryParameterObject, targetType, key, nestedBoundSql);
      value = resultLoader.loadResult();
    }
    return value;
  }
  // 获取嵌套查询映射值
  private Object getNestedQueryMappingValue(ResultSet rs, MetaObject metaResultObject, ResultMapping propertyMapping, ResultLoaderMap lazyLoader, String columnPrefix)
      throws SQLException {
    final String nestedQueryId = propertyMapping.getNestedQueryId();
    final String property = propertyMapping.getProperty();
    final MappedStatement nestedQuery = configuration.getMappedStatement(nestedQueryId);
    final Class<?> nestedQueryParameterType = nestedQuery.getParameterMap().getType();
    final Object nestedQueryParameterObject = prepareParameterForNestedQuery(rs, propertyMapping, nestedQueryParameterType, columnPrefix);
    Object value = null;
    if (nestedQueryParameterObject != null) {
      final BoundSql nestedBoundSql = nestedQuery.getBoundSql(nestedQueryParameterObject);
      final CacheKey key = executor.createCacheKey(nestedQuery, nestedQueryParameterObject, RowBounds.DEFAULT, nestedBoundSql);
      final Class<?> targetType = propertyMapping.getJavaType();
      if (executor.isCached(nestedQuery, key)) {
        executor.deferLoad(nestedQuery, metaResultObject, property, key, targetType);
        value = DEFERRED;
      } else {
        final ResultLoader resultLoader = new ResultLoader(configuration, executor, nestedQuery, nestedQueryParameterObject, targetType, key, nestedBoundSql);
        if (propertyMapping.isLazy()) {
          lazyLoader.addLoader(property, metaResultObject, resultLoader);
          value = DEFERRED;
        } else {
          value = resultLoader.loadResult();
        }
      }
    }
    return value;
  }
   //准备参数 为嵌套查询
  private Object prepareParameterForNestedQuery(ResultSet rs, ResultMapping resultMapping, Class<?> parameterType, String columnPrefix) throws SQLException {
    if (resultMapping.isCompositeResult()) {
      return prepareCompositeKeyParameter(rs, resultMapping, parameterType, columnPrefix);
    } else {
      return prepareSimpleKeyParameter(rs, resultMapping, parameterType, columnPrefix);
    }
  }
  //准备单个key参数
  private Object prepareSimpleKeyParameter(ResultSet rs, ResultMapping resultMapping, Class<?> parameterType, String columnPrefix) throws SQLException {
    final TypeHandler<?> typeHandler;
    if (typeHandlerRegistry.hasTypeHandler(parameterType)) {
      typeHandler = typeHandlerRegistry.getTypeHandler(parameterType);
    } else {
      typeHandler = typeHandlerRegistry.getUnknownTypeHandler();
    }
    return typeHandler.getResult(rs, prependPrefix(resultMapping.getColumn(), columnPrefix));
  }
  //准备混合key参数
  private Object prepareCompositeKeyParameter(ResultSet rs, ResultMapping resultMapping, Class<?> parameterType, String columnPrefix) throws SQLException {
    final Object parameterObject = instantiateParameterObject(parameterType);
    final MetaObject metaObject = configuration.newMetaObject(parameterObject);
    boolean foundValues = false;
    for (ResultMapping innerResultMapping : resultMapping.getComposites()) {
      final Class<?> propType = metaObject.getSetterType(innerResultMapping.getProperty());
      final TypeHandler<?> typeHandler = typeHandlerRegistry.getTypeHandler(propType);
      final Object propValue = typeHandler.getResult(rs, prependPrefix(innerResultMapping.getColumn(), columnPrefix));
      // issue #353 & #560 do not execute nested query if key is null
      if (propValue != null) {
        metaObject.setValue(innerResultMapping.getProperty(), propValue);
        foundValues = true;
      }
    }
    return foundValues ? parameterObject : null;
  }
  //实例话参数对象
  private Object instantiateParameterObject(Class<?> parameterType) {
    if (parameterType == null) {
      return new HashMap<>();
    } else if (ParamMap.class.equals(parameterType)) {
      return new HashMap<>(); // issue #649
    } else {
      return objectFactory.create(parameterType);
    }
  }

  //解决区别结果集
  // DISCRIMINATOR
  //

  public ResultMap resolveDiscriminatedResultMap(ResultSet rs, ResultMap resultMap, String columnPrefix) throws SQLException {
    Set<String> pastDiscriminators = new HashSet<>();
    Discriminator discriminator = resultMap.getDiscriminator();
    while (discriminator != null) {
      final Object value = getDiscriminatorValue(rs, discriminator, columnPrefix);
      final String discriminatedMapId = discriminator.getMapIdFor(String.valueOf(value));
      if (configuration.hasResultMap(discriminatedMapId)) {
        resultMap = configuration.getResultMap(discriminatedMapId);
        Discriminator lastDiscriminator = discriminator;
        discriminator = resultMap.getDiscriminator();
        if (discriminator == lastDiscriminator || !pastDiscriminators.add(discriminatedMapId)) {
          break;
        }
      } else {
        break;
      }
    }
    return resultMap;
  }
  //获取区别值
  private Object getDiscriminatorValue(ResultSet rs, Discriminator discriminator, String columnPrefix) throws SQLException {
    final ResultMapping resultMapping = discriminator.getResultMapping();
    final TypeHandler<?> typeHandler = resultMapping.getTypeHandler();
    return typeHandler.getResult(rs, prependPrefix(resultMapping.getColumn(), columnPrefix));
  }
  //封装前缀
  private String prependPrefix(String columnName, String prefix) {
    if (columnName == null || columnName.length() == 0 || prefix == null || prefix.length() == 0) {
      return columnName;
    }
    return prefix + columnName;
  }

  //  处理嵌套结果映射
  // HANDLE NESTED RESULT MAPS
  //

  private void handleRowValuesForNestedResultMap(ResultSetWrapper rsw, ResultMap resultMap, ResultHandler<?> resultHandler, RowBounds rowBounds, ResultMapping parentMapping) throws SQLException {
    final DefaultResultContext<Object> resultContext = new DefaultResultContext<>();
    ResultSet resultSet = rsw.getResultSet();
    skipRows(resultSet, rowBounds);
    Object rowValue = previousRowValue;
    while (shouldProcessMoreRows(resultContext, rowBounds) && !resultSet.isClosed() && resultSet.next()) {
      final ResultMap discriminatedResultMap = resolveDiscriminatedResultMap(resultSet, resultMap, null);
      final CacheKey rowKey = createRowKey(discriminatedResultMap, rsw, null);
      Object partialObject = nestedResultObjects.get(rowKey);
      // issue #577 && #542
      if (mappedStatement.isResultOrdered()) {
        if (partialObject == null && rowValue != null) {
          nestedResultObjects.clear();
          storeObject(resultHandler, resultContext, rowValue, parentMapping, resultSet);
        }
        rowValue = getRowValue(rsw, discriminatedResultMap, rowKey, null, partialObject);
      } else {
        rowValue = getRowValue(rsw, discriminatedResultMap, rowKey, null, partialObject);
        if (partialObject == null) {
          storeObject(resultHandler, resultContext, rowValue, parentMapping, resultSet);
        }
      }
    }
    if (rowValue != null && mappedStatement.isResultOrdered() && shouldProcessMoreRows(resultContext, rowBounds)) {
      storeObject(resultHandler, resultContext, rowValue, parentMapping, resultSet);
      previousRowValue = null;
    } else if (rowValue != null) {
      previousRowValue = rowValue;
    }
  }

  //  从嵌套结果映射中获取值
  // GET VALUE FROM ROW FOR NESTED RESULT MAP
  //

  private Object getRowValue(ResultSetWrapper rsw, ResultMap resultMap, CacheKey combinedKey, String columnPrefix, Object partialObject) throws SQLException {
    final String resultMapId = resultMap.getId();
    Object rowValue = partialObject;
    if (rowValue != null) {
      final MetaObject metaObject = configuration.newMetaObject(rowValue);
      putAncestor(rowValue, resultMapId);
      applyNestedResultMappings(rsw, resultMap, metaObject, columnPrefix, combinedKey, false);
      ancestorObjects.remove(resultMapId);
    } else {
      final ResultLoaderMap lazyLoader = new ResultLoaderMap();
      rowValue = createResultObject(rsw, resultMap, lazyLoader, columnPrefix);
      if (rowValue != null && !hasTypeHandlerForResultObject(rsw, resultMap.getType())) {
        final MetaObject metaObject = configuration.newMetaObject(rowValue);
        boolean foundValues = this.useConstructorMappings;
        if (shouldApplyAutomaticMappings(resultMap, true)) {
          foundValues = applyAutomaticMappings(rsw, resultMap, metaObject, columnPrefix) || foundValues;
        }
        foundValues = applyPropertyMappings(rsw, resultMap, metaObject, lazyLoader, columnPrefix) || foundValues;
        putAncestor(rowValue, resultMapId);
        foundValues = applyNestedResultMappings(rsw, resultMap, metaObject, columnPrefix, combinedKey, true) || foundValues;
        ancestorObjects.remove(resultMapId);
        foundValues = lazyLoader.size() > 0 || foundValues;
        rowValue = foundValues || configuration.isReturnInstanceForEmptyRow() ? rowValue : null;
      }
      if (combinedKey != CacheKey.NULL_CACHE_KEY) {
        nestedResultObjects.put(combinedKey, rowValue);
      }
    }
    return rowValue;
  }

  private void putAncestor(Object resultObject, String resultMapId) {
    ancestorObjects.put(resultMapId, resultObject);
  }

  //  嵌套结果集
  // NESTED RESULT MAP (JOIN MAPPING)
  //

  private boolean applyNestedResultMappings(ResultSetWrapper rsw, ResultMap resultMap, MetaObject metaObject, String parentPrefix, CacheKey parentRowKey, boolean newObject) {
    boolean foundValues = false;
    for (ResultMapping resultMapping : resultMap.getPropertyResultMappings()) {
      final String nestedResultMapId = resultMapping.getNestedResultMapId();
      if (nestedResultMapId != null && resultMapping.getResultSet() == null) {
        try {
          final String columnPrefix = getColumnPrefix(parentPrefix, resultMapping);
          final ResultMap nestedResultMap = getNestedResultMap(rsw.getResultSet(), nestedResultMapId, columnPrefix);
          if (resultMapping.getColumnPrefix() == null) {
            // try to fill circular reference only when columnPrefix
            // is not specified for the nested result map (issue #215)
            Object ancestorObject = ancestorObjects.get(nestedResultMapId);
            if (ancestorObject != null) {
              if (newObject) {
                linkObjects(metaObject, resultMapping, ancestorObject); // issue #385
              }
              continue;
            }
          }
          final CacheKey rowKey = createRowKey(nestedResultMap, rsw, columnPrefix);
          final CacheKey combinedKey = combineKeys(rowKey, parentRowKey);
          Object rowValue = nestedResultObjects.get(combinedKey);
          boolean knownValue = rowValue != null;
          instantiateCollectionPropertyIfAppropriate(resultMapping, metaObject); // mandatory
          if (anyNotNullColumnHasValue(resultMapping, columnPrefix, rsw)) {
            rowValue = getRowValue(rsw, nestedResultMap, combinedKey, columnPrefix, rowValue);
            if (rowValue != null && !knownValue) {
              linkObjects(metaObject, resultMapping, rowValue);
              foundValues = true;
            }
          }
        } catch (SQLException e) {
          throw new ExecutorException("Error getting nested result map values for '" + resultMapping.getProperty() + "'.  Cause: " + e, e);
        }
      }
    }
    return foundValues;
  }
  //获取列前缀
  private String getColumnPrefix(String parentPrefix, ResultMapping resultMapping) {
    final StringBuilder columnPrefixBuilder = new StringBuilder();
    if (parentPrefix != null) {
      columnPrefixBuilder.append(parentPrefix);
    }
    if (resultMapping.getColumnPrefix() != null) {
      columnPrefixBuilder.append(resultMapping.getColumnPrefix());
    }
    return columnPrefixBuilder.length() == 0 ? null : columnPrefixBuilder.toString().toUpperCase(Locale.ENGLISH);
  }
  //应用非空字段是否有值
  private boolean anyNotNullColumnHasValue(ResultMapping resultMapping, String columnPrefix, ResultSetWrapper rsw) throws SQLException {
    Set<String> notNullColumns = resultMapping.getNotNullColumns();
    if (notNullColumns != null && !notNullColumns.isEmpty()) {
      ResultSet rs = rsw.getResultSet();
      for (String column : notNullColumns) {
        rs.getObject(prependPrefix(column, columnPrefix));
        if (!rs.wasNull()) {
          return true;
        }
      }
      return false;
    } else if (columnPrefix != null) {
      for (String columnName : rsw.getColumnNames()) {
        if (columnName.toUpperCase().startsWith(columnPrefix.toUpperCase())) {
          return true;
        }
      }
      return false;
    }
    return true;
  }
  //获取嵌套结果映射
  private ResultMap getNestedResultMap(ResultSet rs, String nestedResultMapId, String columnPrefix) throws SQLException {
    ResultMap nestedResultMap = configuration.getResultMap(nestedResultMapId);
    return resolveDiscriminatedResultMap(rs, nestedResultMap, columnPrefix);
  }

  //  唯一的结果key
  // UNIQUE RESULT KEY
  //

  private CacheKey createRowKey(ResultMap resultMap, ResultSetWrapper rsw, String columnPrefix) throws SQLException {
    final CacheKey cacheKey = new CacheKey();
    cacheKey.update(resultMap.getId());
    List<ResultMapping> resultMappings = getResultMappingsForRowKey(resultMap);
    if (resultMappings.isEmpty()) {
      if (Map.class.isAssignableFrom(resultMap.getType())) {
        createRowKeyForMap(rsw, cacheKey);
      } else {
        createRowKeyForUnmappedProperties(resultMap, rsw, cacheKey, columnPrefix);
      }
    } else {
      createRowKeyForMappedProperties(resultMap, rsw, cacheKey, resultMappings, columnPrefix);
    }
    if (cacheKey.getUpdateCount() < 2) {
      return CacheKey.NULL_CACHE_KEY;
    }
    return cacheKey;
  }
  //合起来key
  private CacheKey combineKeys(CacheKey rowKey, CacheKey parentRowKey) {
    if (rowKey.getUpdateCount() > 1 && parentRowKey.getUpdateCount() > 1) {
      CacheKey combinedKey;
      try {
        combinedKey = rowKey.clone();
      } catch (CloneNotSupportedException e) {
        throw new ExecutorException("Error cloning cache key.  Cause: " + e, e);
      }
      combinedKey.update(parentRowKey);
      return combinedKey;
    }
    return CacheKey.NULL_CACHE_KEY;
  }
  //获取行key的结果映射
  private List<ResultMapping> getResultMappingsForRowKey(ResultMap resultMap) {
    List<ResultMapping> resultMappings = resultMap.getIdResultMappings();
    if (resultMappings.isEmpty()) {
      resultMappings = resultMap.getPropertyResultMappings();
    }
    return resultMappings;
  }
  //创建行key对于映射的属性
  private void createRowKeyForMappedProperties(ResultMap resultMap, ResultSetWrapper rsw, CacheKey cacheKey, List<ResultMapping> resultMappings, String columnPrefix) throws SQLException {
    for (ResultMapping resultMapping : resultMappings) {
      if (resultMapping.getNestedResultMapId() != null && resultMapping.getResultSet() == null) {
        // Issue #392
        final ResultMap nestedResultMap = configuration.getResultMap(resultMapping.getNestedResultMapId());
        createRowKeyForMappedProperties(nestedResultMap, rsw, cacheKey, nestedResultMap.getConstructorResultMappings(),
            prependPrefix(resultMapping.getColumnPrefix(), columnPrefix));
      } else if (resultMapping.getNestedQueryId() == null) {
        final String column = prependPrefix(resultMapping.getColumn(), columnPrefix);
        final TypeHandler<?> th = resultMapping.getTypeHandler();
        List<String> mappedColumnNames = rsw.getMappedColumnNames(resultMap, columnPrefix);
        // Issue #114
        if (column != null && mappedColumnNames.contains(column.toUpperCase(Locale.ENGLISH))) {
          final Object value = th.getResult(rsw.getResultSet(), column);
          if (value != null || configuration.isReturnInstanceForEmptyRow()) {
            cacheKey.update(column);
            cacheKey.update(value);
          }
        }
      }
    }
  }
  //创建行key对应未映射的属性
  private void createRowKeyForUnmappedProperties(ResultMap resultMap, ResultSetWrapper rsw, CacheKey cacheKey, String columnPrefix) throws SQLException {
    final MetaClass metaType = MetaClass.forClass(resultMap.getType(), reflectorFactory);
    List<String> unmappedColumnNames = rsw.getUnmappedColumnNames(resultMap, columnPrefix);
    for (String column : unmappedColumnNames) {
      String property = column;
      if (columnPrefix != null && !columnPrefix.isEmpty()) {
        // When columnPrefix is specified, ignore columns without the prefix.
        if (column.toUpperCase(Locale.ENGLISH).startsWith(columnPrefix)) {
          property = column.substring(columnPrefix.length());
        } else {
          continue;
        }
      }
      if (metaType.findProperty(property, configuration.isMapUnderscoreToCamelCase()) != null) {
        String value = rsw.getResultSet().getString(column);
        if (value != null) {
          cacheKey.update(column);
          cacheKey.update(value);
        }
      }
    }
  } 
  //创建行key
  private void createRowKeyForMap(ResultSetWrapper rsw, CacheKey cacheKey) throws SQLException {
    List<String> columnNames = rsw.getColumnNames();
    for (String columnName : columnNames) {
      final String value = rsw.getResultSet().getString(columnName);
      if (value != null) {
        cacheKey.update(columnName);
        cacheKey.update(value);
      }
    }
  }
  //链接对象
  private void linkObjects(MetaObject metaObject, ResultMapping resultMapping, Object rowValue) {
    final Object collectionProperty = instantiateCollectionPropertyIfAppropriate(resultMapping, metaObject);
    if (collectionProperty != null) {
      final MetaObject targetMetaObject = configuration.newMetaObject(collectionProperty);
      targetMetaObject.add(rowValue);
    } else {
      metaObject.setValue(resultMapping.getProperty(), rowValue);
    }
  }
  //实例话集合属性如果合适的
  private Object instantiateCollectionPropertyIfAppropriate(ResultMapping resultMapping, MetaObject metaObject) {
    final String propertyName = resultMapping.getProperty();
    Object propertyValue = metaObject.getValue(propertyName);
    if (propertyValue == null) {
      Class<?> type = resultMapping.getJavaType();
      if (type == null) {
        type = metaObject.getSetterType(propertyName);
      }
      try {
        if (objectFactory.isCollection(type)) {
          propertyValue = objectFactory.create(type);
          metaObject.setValue(propertyName, propertyValue);
          return propertyValue;
        }
      } catch (Exception e) {
        throw new ExecutorException("Error instantiating collection property for result '" + resultMapping.getProperty() + "'.  Cause: " + e, e);
      }
    } else if (objectFactory.isCollection(propertyValue.getClass())) {
      return propertyValue;
    }
    return null;
  }
  //结果对象的类型处理器是否存在
  private boolean hasTypeHandlerForResultObject(ResultSetWrapper rsw, Class<?> resultType) {
    if (rsw.getColumnNames().size() == 1) {
      return typeHandlerRegistry.hasTypeHandler(resultType, rsw.getJdbcType(rsw.getColumnNames().get(0)));
    }
    return typeHandlerRegistry.hasTypeHandler(resultType);
  }

}
```

#### 4.3.6.3、ResultSetWrapper结果集包装器

```java
/**
 * 结果集包装器
 * @author Iwao AVE!
 */
public class ResultSetWrapper {
  //结果集
  private final ResultSet resultSet;
  //类型处理器注册器
  private final TypeHandlerRegistry typeHandlerRegistry;
  //列字段
  private final List<String> columnNames = new ArrayList<>();
  //类名称
  private final List<String> classNames = new ArrayList<>();
  //jdbc类型
  private final List<JdbcType> jdbcTypes = new ArrayList<>();
  //类型处理器集合
  private final Map<String, Map<Class<?>, TypeHandler<?>>> typeHandlerMap = new HashMap<>();
  //映射字段集合
  private final Map<String, List<String>> mappedColumnNamesMap = new HashMap<>();
  //未映射字段集合
  private final Map<String, List<String>> unMappedColumnNamesMap = new HashMap<>();
  //构造函数
  public ResultSetWrapper(ResultSet rs, Configuration configuration) throws SQLException {
    super();
    this.typeHandlerRegistry = configuration.getTypeHandlerRegistry();
    this.resultSet = rs;
    final ResultSetMetaData metaData = rs.getMetaData();
    final int columnCount = metaData.getColumnCount();
    for (int i = 1; i <= columnCount; i++) {
      columnNames.add(configuration.isUseColumnLabel() ? metaData.getColumnLabel(i) : metaData.getColumnName(i));
      jdbcTypes.add(JdbcType.forCode(metaData.getColumnType(i)));
      classNames.add(metaData.getColumnClassName(i));
    }
  }
  //获取结果集
  public ResultSet getResultSet() {
    return resultSet;
  }
  //获取列名
  public List<String> getColumnNames() {
    return this.columnNames;
  }
  //获取类名
  public List<String> getClassNames() {
    return Collections.unmodifiableList(classNames);
  }
  //获取jdbc类型
  public List<JdbcType> getJdbcTypes() {
    return jdbcTypes;
  }
  //获取jdbc类型根据某个列
  public JdbcType getJdbcType(String columnName) {
    for (int i = 0 ; i < columnNames.size(); i++) {
      if (columnNames.get(i).equalsIgnoreCase(columnName)) {
        return jdbcTypes.get(i);
      }
    }
    return null;
  }

  /**
   * 获取类型处理器使用， 当读取结果集。通过从属性类型检索获取类型处理器注册器，如果没找到，则从jdbc类型获取
   *
   * @param propertyType
   * @param columnName
   * @return
   */
  public TypeHandler<?> getTypeHandler(Class<?> propertyType, String columnName) {
    TypeHandler<?> handler = null;
    Map<Class<?>, TypeHandler<?>> columnHandlers = typeHandlerMap.get(columnName);
    if (columnHandlers == null) {
      columnHandlers = new HashMap<>();
      typeHandlerMap.put(columnName, columnHandlers);
    } else {
      handler = columnHandlers.get(propertyType);
    }
    if (handler == null) {
      JdbcType jdbcType = getJdbcType(columnName);
      handler = typeHandlerRegistry.getTypeHandler(propertyType, jdbcType);
      // Replicate logic of UnknownTypeHandler#resolveTypeHandler
      // See issue #59 comment 10
      if (handler == null || handler instanceof UnknownTypeHandler) {
        final int index = columnNames.indexOf(columnName);
        final Class<?> javaType = resolveClass(classNames.get(index));
        if (javaType != null && jdbcType != null) {
          handler = typeHandlerRegistry.getTypeHandler(javaType, jdbcType);
        } else if (javaType != null) {
          handler = typeHandlerRegistry.getTypeHandler(javaType);
        } else if (jdbcType != null) {
          handler = typeHandlerRegistry.getTypeHandler(jdbcType);
        }
      }
      if (handler == null || handler instanceof UnknownTypeHandler) {
        handler = new ObjectTypeHandler();
      }
      columnHandlers.put(propertyType, handler);
    }
    return handler;
  }
  //解析类
  private Class<?> resolveClass(String className) {
    try {
      // #699 className could be null
      if (className != null) {
        return Resources.classForName(className);
      }
    } catch (ClassNotFoundException e) {
      // ignore
    }
    return null;
  }
  //加载映射和未映射的列名字
  private void loadMappedAndUnmappedColumnNames(ResultMap resultMap, String columnPrefix) throws SQLException {
    List<String> mappedColumnNames = new ArrayList<>();
    List<String> unmappedColumnNames = new ArrayList<>();
    final String upperColumnPrefix = columnPrefix == null ? null : columnPrefix.toUpperCase(Locale.ENGLISH);
    final Set<String> mappedColumns = prependPrefixes(resultMap.getMappedColumns(), upperColumnPrefix);
    for (String columnName : columnNames) {
      final String upperColumnName = columnName.toUpperCase(Locale.ENGLISH);
      if (mappedColumns.contains(upperColumnName)) {
        mappedColumnNames.add(upperColumnName);
      } else {
        unmappedColumnNames.add(columnName);
      }
    }
    mappedColumnNamesMap.put(getMapKey(resultMap, columnPrefix), mappedColumnNames);
    unMappedColumnNamesMap.put(getMapKey(resultMap, columnPrefix), unmappedColumnNames);
  }
  //获取映射的列名字
  public List<String> getMappedColumnNames(ResultMap resultMap, String columnPrefix) throws SQLException {
    List<String> mappedColumnNames = mappedColumnNamesMap.get(getMapKey(resultMap, columnPrefix));
    if (mappedColumnNames == null) {
      loadMappedAndUnmappedColumnNames(resultMap, columnPrefix);
      mappedColumnNames = mappedColumnNamesMap.get(getMapKey(resultMap, columnPrefix));
    }
    return mappedColumnNames;
  }
  //获取未映射的列名字
  public List<String> getUnmappedColumnNames(ResultMap resultMap, String columnPrefix) throws SQLException {
    List<String> unMappedColumnNames = unMappedColumnNamesMap.get(getMapKey(resultMap, columnPrefix));
    if (unMappedColumnNames == null) {
      loadMappedAndUnmappedColumnNames(resultMap, columnPrefix);
      unMappedColumnNames = unMappedColumnNamesMap.get(getMapKey(resultMap, columnPrefix));
    }
    return unMappedColumnNames;
  }
  //获取映射的key
  private String getMapKey(ResultMap resultMap, String columnPrefix) {
    return resultMap.getId() + ":" + columnPrefix;
  }
  //预置前缀
  private Set<String> prependPrefixes(Set<String> columnNames, String prefix) {
    if (columnNames == null || columnNames.isEmpty() || prefix == null || prefix.length() == 0) {
      return columnNames;
    }
    final Set<String> prefixed = new HashSet<>();
    for (String columnName : columnNames) {
      prefixed.add(prefix + columnName);
    }
    return prefixed;
  }

}
```

### 4.3.7、statement会话

![image-20200311224833543](/Users/didi/Library/Application Support/typora-user-images/image-20200311224833543.png)

#### 4.3.7.1、StatementHandler会话处理器

![image-20200311224921130](/Users/didi/Library/Application Support/typora-user-images/image-20200311224921130.png)

```java
/**
 * 会话处理器
 * @author Clinton Begin
 */
public interface StatementHandler {
  //预会话
  Statement prepare(Connection connection, Integer transactionTimeout)
      throws SQLException;
  //参数化
  void parameterize(Statement statement)
      throws SQLException;
  //批量执行
  void batch(Statement statement)
      throws SQLException;
  //更新
  int update(Statement statement)
      throws SQLException;
  //查询
  <E> List<E> query(Statement statement, ResultHandler resultHandler)
      throws SQLException;
  //查询结果集
  <E> Cursor<E> queryCursor(Statement statement)
      throws SQLException;
  //获取执行SQL
  BoundSql getBoundSql();
  //参数处理器
  ParameterHandler getParameterHandler();

}
```

#### 4.3.7.2、RoutingStatementHandler路由会话处理器

![image-20200311225315521](/Users/didi/Library/Application Support/typora-user-images/image-20200311225315521.png)

```java
/**
 * 路由会话处理器
 * @author Clinton Begin
 */
public class RoutingStatementHandler implements StatementHandler {
  //会话处理器
  private final StatementHandler delegate;

  public RoutingStatementHandler(Executor executor, MappedStatement ms, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, BoundSql boundSql) {

    switch (ms.getStatementType()) {
      //会话操作
      case STATEMENT:
        delegate = new SimpleStatementHandler(executor, ms, parameter, rowBounds, resultHandler, boundSql);
        break;
        //预会话操作
      case PREPARED:
        delegate = new PreparedStatementHandler(executor, ms, parameter, rowBounds, resultHandler, boundSql);
        break;
      case CALLABLE:
        //回调会话操作
        delegate = new CallableStatementHandler(executor, ms, parameter, rowBounds, resultHandler, boundSql);
        break;
      default:
        throw new ExecutorException("Unknown statement type: " + ms.getStatementType());
    }

  }
  //准备
  @Override
  public Statement prepare(Connection connection, Integer transactionTimeout) throws SQLException {
    return delegate.prepare(connection, transactionTimeout);
  }
  //参数化
  @Override
  public void parameterize(Statement statement) throws SQLException {
    delegate.parameterize(statement);
  }
  //执行
  @Override
  public void batch(Statement statement) throws SQLException {
    delegate.batch(statement);
  }
  //更新
  @Override
  public int update(Statement statement) throws SQLException {
    return delegate.update(statement);
  }
  //查询
  @Override
  public <E> List<E> query(Statement statement, ResultHandler resultHandler) throws SQLException {
    return delegate.query(statement, resultHandler);
  }
  //查询游标
  @Override
  public <E> Cursor<E> queryCursor(Statement statement) throws SQLException {
    return delegate.queryCursor(statement);
  }
  //执行sql
  @Override
  public BoundSql getBoundSql() {
    return delegate.getBoundSql();
  }
  //获取参数处理器
  @Override
  public ParameterHandler getParameterHandler() {
    return delegate.getParameterHandler();
  }
}
```

#### 4.3.7.3、BaseStatementHandler基础会话处理器

![image-20200311225333718](/Users/didi/Library/Application Support/typora-user-images/image-20200311225333718.png)

```java
/**
 * //基础会话处理器
 * @author Clinton Begin
 */
public abstract class BaseStatementHandler implements StatementHandler {
  //全局配置
  protected final Configuration configuration;
  //对象工厂
  protected final ObjectFactory objectFactory;
  //类型处理器注册器
  protected final TypeHandlerRegistry typeHandlerRegistry;
  //结果集处理器
  protected final ResultSetHandler resultSetHandler;
  //参数处理器
  protected final ParameterHandler parameterHandler;
  //执行器
  protected final Executor executor;
  //映射会话
  protected final MappedStatement mappedStatement;
  //行数据
  protected final RowBounds rowBounds;
  //执行sql
  protected BoundSql boundSql;
  //基础会话处理器构造函数
  protected BaseStatementHandler(Executor executor, MappedStatement mappedStatement, Object parameterObject, RowBounds rowBounds, ResultHandler resultHandler, BoundSql boundSql) {
    this.configuration = mappedStatement.getConfiguration();
    this.executor = executor;
    this.mappedStatement = mappedStatement;
    this.rowBounds = rowBounds;

    this.typeHandlerRegistry = configuration.getTypeHandlerRegistry();
    this.objectFactory = configuration.getObjectFactory();

    if (boundSql == null) { // issue #435, get the key before calculating the statement
      generateKeys(parameterObject);
      boundSql = mappedStatement.getBoundSql(parameterObject);
    }

    this.boundSql = boundSql;

    this.parameterHandler = configuration.newParameterHandler(mappedStatement, parameterObject, boundSql);
    this.resultSetHandler = configuration.newResultSetHandler(executor, mappedStatement, rowBounds, parameterHandler, resultHandler, boundSql);
  }
  //获取执行SQL
  @Override
  public BoundSql getBoundSql() {
    return boundSql;
  }
  //获取参数处理器
  @Override
  public ParameterHandler getParameterHandler() {
    return parameterHandler;
  }
  //准备会话
  @Override
  public Statement prepare(Connection connection, Integer transactionTimeout) throws SQLException {
    ErrorContext.instance().sql(boundSql.getSql());
    Statement statement = null;
    try {
      statement = instantiateStatement(connection);
      setStatementTimeout(statement, transactionTimeout);
      setFetchSize(statement);
      return statement;
    } catch (SQLException e) {
      closeStatement(statement);
      throw e;
    } catch (Exception e) {
      closeStatement(statement);
      throw new ExecutorException("Error preparing statement.  Cause: " + e, e);
    }
  }
  //实例话会话
  protected abstract Statement instantiateStatement(Connection connection) throws SQLException;
  //设置会话超时
  protected void setStatementTimeout(Statement stmt, Integer transactionTimeout) throws SQLException {
    Integer queryTimeout = null;
    if (mappedStatement.getTimeout() != null) {
      queryTimeout = mappedStatement.getTimeout();
    } else if (configuration.getDefaultStatementTimeout() != null) {
      queryTimeout = configuration.getDefaultStatementTimeout();
    }
    if (queryTimeout != null) {
      stmt.setQueryTimeout(queryTimeout);
    }
    StatementUtil.applyTransactionTimeout(stmt, queryTimeout, transactionTimeout);
  }
  //设置拉取数量
  protected void setFetchSize(Statement stmt) throws SQLException {
    Integer fetchSize = mappedStatement.getFetchSize();
    if (fetchSize != null) {
      stmt.setFetchSize(fetchSize);
      return;
    }
    Integer defaultFetchSize = configuration.getDefaultFetchSize();
    if (defaultFetchSize != null) {
      stmt.setFetchSize(defaultFetchSize);
    }
  }
  //关闭会话
  protected void closeStatement(Statement statement) {
    try {
      if (statement != null) {
        statement.close();
      }
    } catch (SQLException e) {
      //ignore
    }
  }
  //生成主键
  protected void generateKeys(Object parameter) {
    KeyGenerator keyGenerator = mappedStatement.getKeyGenerator();
    ErrorContext.instance().store();
    keyGenerator.processBefore(executor, mappedStatement, null, parameter);
    ErrorContext.instance().recall();
  }

}
```



#### 4.3.7.4、CallableStatementHandler回调会话处理器

![image-20200311225351256](/Users/didi/Library/Application Support/typora-user-images/image-20200311225351256.png)

```java
/**
 * 回调会话处理器
 * @author Clinton Begin
 */
public class CallableStatementHandler extends BaseStatementHandler {
  //有参数构造器
  public CallableStatementHandler(Executor executor, MappedStatement mappedStatement, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, BoundSql boundSql) {
    super(executor, mappedStatement, parameter, rowBounds, resultHandler, boundSql);
  }
  //更新操作
  @Override
  public int update(Statement statement) throws SQLException {
    CallableStatement cs = (CallableStatement) statement;
    cs.execute();
    int rows = cs.getUpdateCount();
    Object parameterObject = boundSql.getParameterObject();
    KeyGenerator keyGenerator = mappedStatement.getKeyGenerator();
    keyGenerator.processAfter(executor, mappedStatement, cs, parameterObject);
    resultSetHandler.handleOutputParameters(cs);
    return rows;
  }
  //执行批量操作
  @Override
  public void batch(Statement statement) throws SQLException {
    CallableStatement cs = (CallableStatement) statement;
    cs.addBatch();
  }
  //查询
  @Override
  public <E> List<E> query(Statement statement, ResultHandler resultHandler) throws SQLException {
    CallableStatement cs = (CallableStatement) statement;
    cs.execute();
    List<E> resultList = resultSetHandler.handleResultSets(cs);
    resultSetHandler.handleOutputParameters(cs);
    return resultList;
  }
  //查询游标集
  @Override
  public <E> Cursor<E> queryCursor(Statement statement) throws SQLException {
    CallableStatement cs = (CallableStatement) statement;
    cs.execute();
    Cursor<E> resultList = resultSetHandler.handleCursorResultSets(cs);
    resultSetHandler.handleOutputParameters(cs);
    return resultList;
  }
  //实例话会话
  @Override
  protected Statement instantiateStatement(Connection connection) throws SQLException {
    String sql = boundSql.getSql();
    if (mappedStatement.getResultSetType() == ResultSetType.DEFAULT) {
      return connection.prepareCall(sql);
    } else {
      return connection.prepareCall(sql, mappedStatement.getResultSetType().getValue(), ResultSet.CONCUR_READ_ONLY);
    }
  }
  //参数化会话
  @Override
  public void parameterize(Statement statement) throws SQLException {
    registerOutputParameters((CallableStatement) statement);
    parameterHandler.setParameters((CallableStatement) statement);
  }
  //注册输出参数
  private void registerOutputParameters(CallableStatement cs) throws SQLException {
    List<ParameterMapping> parameterMappings = boundSql.getParameterMappings();
    for (int i = 0, n = parameterMappings.size(); i < n; i++) {
      ParameterMapping parameterMapping = parameterMappings.get(i);
      if (parameterMapping.getMode() == ParameterMode.OUT || parameterMapping.getMode() == ParameterMode.INOUT) {
        if (null == parameterMapping.getJdbcType()) {
          throw new ExecutorException("The JDBC Type must be specified for output parameter.  Parameter: " + parameterMapping.getProperty());
        } else {
          if (parameterMapping.getNumericScale() != null && (parameterMapping.getJdbcType() == JdbcType.NUMERIC || parameterMapping.getJdbcType() == JdbcType.DECIMAL)) {
            cs.registerOutParameter(i + 1, parameterMapping.getJdbcType().TYPE_CODE, parameterMapping.getNumericScale());
          } else {
            if (parameterMapping.getJdbcTypeName() == null) {
              cs.registerOutParameter(i + 1, parameterMapping.getJdbcType().TYPE_CODE);
            } else {
              cs.registerOutParameter(i + 1, parameterMapping.getJdbcType().TYPE_CODE, parameterMapping.getJdbcTypeName());
            }
          }
        }
      }
    }
  }

}
```

#### 4.3.7.5、PreparedStatementHandler预会话处理器

![image-20200311225407648](/Users/didi/Library/Application Support/typora-user-images/image-20200311225407648.png)

```java
/**
 * 预会话处理器
 * @author Clinton Begin
 */
public class PreparedStatementHandler extends BaseStatementHandler {
  //构造函数
  public PreparedStatementHandler(Executor executor, MappedStatement mappedStatement, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, BoundSql boundSql) {
    super(executor, mappedStatement, parameter, rowBounds, resultHandler, boundSql);
  }
  //更新
  @Override
  public int update(Statement statement) throws SQLException {
    PreparedStatement ps = (PreparedStatement) statement;
    ps.execute();
    int rows = ps.getUpdateCount();
    Object parameterObject = boundSql.getParameterObject();
    KeyGenerator keyGenerator = mappedStatement.getKeyGenerator();
    keyGenerator.processAfter(executor, mappedStatement, ps, parameterObject);
    return rows;
  }
  //批量执行
  @Override
  public void batch(Statement statement) throws SQLException {
    PreparedStatement ps = (PreparedStatement) statement;
    ps.addBatch();
  } 
  //查询
  @Override
  public <E> List<E> query(Statement statement, ResultHandler resultHandler) throws SQLException {
    PreparedStatement ps = (PreparedStatement) statement;
    ps.execute();
    return resultSetHandler.handleResultSets(ps);
  }
  //查询游标集
  @Override
  public <E> Cursor<E> queryCursor(Statement statement) throws SQLException {
    PreparedStatement ps = (PreparedStatement) statement;
    ps.execute();
    return resultSetHandler.handleCursorResultSets(ps);
  }
  //实例话会话
  @Override
  protected Statement instantiateStatement(Connection connection) throws SQLException {
    String sql = boundSql.getSql();
    if (mappedStatement.getKeyGenerator() instanceof Jdbc3KeyGenerator) {
      String[] keyColumnNames = mappedStatement.getKeyColumns();
      if (keyColumnNames == null) {
        return connection.prepareStatement(sql, PreparedStatement.RETURN_GENERATED_KEYS);
      } else {
        return connection.prepareStatement(sql, keyColumnNames);
      }
    } else if (mappedStatement.getResultSetType() == ResultSetType.DEFAULT) {
      return connection.prepareStatement(sql);
    } else {
      return connection.prepareStatement(sql, mappedStatement.getResultSetType().getValue(), ResultSet.CONCUR_READ_ONLY);
    }
  }
  //参数化会话
  @Override
  public void parameterize(Statement statement) throws SQLException {
    parameterHandler.setParameters((PreparedStatement) statement);
  }

}
```



#### 4.3.7.6、SimpleStatementHandler简单会话处理器

![image-20200311225421471](/Users/didi/Library/Application Support/typora-user-images/image-20200311225421471.png)

```java
/**
 * 简单会话操作
 * @author Clinton Begin
 */
public class SimpleStatementHandler extends BaseStatementHandler {
  //构造函数
  public SimpleStatementHandler(Executor executor, MappedStatement mappedStatement, Object parameter, RowBounds rowBounds, ResultHandler resultHandler, BoundSql boundSql) {
    super(executor, mappedStatement, parameter, rowBounds, resultHandler, boundSql);
  }
  //更新操作
  @Override
  public int update(Statement statement) throws SQLException {
    String sql = boundSql.getSql();
    Object parameterObject = boundSql.getParameterObject();
    KeyGenerator keyGenerator = mappedStatement.getKeyGenerator();
    int rows;
    if (keyGenerator instanceof Jdbc3KeyGenerator) {
      statement.execute(sql, Statement.RETURN_GENERATED_KEYS);
      rows = statement.getUpdateCount();
      keyGenerator.processAfter(executor, mappedStatement, statement, parameterObject);
    } else if (keyGenerator instanceof SelectKeyGenerator) {
      statement.execute(sql);
      rows = statement.getUpdateCount();
      keyGenerator.processAfter(executor, mappedStatement, statement, parameterObject);
    } else {
      statement.execute(sql);
      rows = statement.getUpdateCount();
    }
    return rows;
  }
  //执行
  @Override
  public void batch(Statement statement) throws SQLException {
    String sql = boundSql.getSql();
    statement.addBatch(sql);
  }
   //查询
  @Override
  public <E> List<E> query(Statement statement, ResultHandler resultHandler) throws SQLException {
    String sql = boundSql.getSql();
    statement.execute(sql);
    return resultSetHandler.handleResultSets(statement);
  }
  //查询游标集
  @Override
  public <E> Cursor<E> queryCursor(Statement statement) throws SQLException {
    String sql = boundSql.getSql();
    statement.execute(sql);
    return resultSetHandler.handleCursorResultSets(statement);
  }
   //实例话会话
  @Override
  protected Statement instantiateStatement(Connection connection) throws SQLException {
    if (mappedStatement.getResultSetType() == ResultSetType.DEFAULT) {
      return connection.createStatement();
    } else {
      return connection.createStatement(mappedStatement.getResultSetType().getValue(), ResultSet.CONCUR_READ_ONLY);
    }
  }
  //参数化会话
  @Override
  public void parameterize(Statement statement) {
    // N/A
  }

}

```



#### 4.3.7.7、StatementUtil会话工具类

```java
/**
 *  {@link java.sql.Statement}工具类 3。4。0版本以后
 * Utility for {@link java.sql.Statement}.
 *
 * @since 3.4.0
 * @author Kazuki Shimizu
 */
public class StatementUtil {
  
  private StatementUtil() {
    // NOP
  }

  /**
   * 应用事务超时
   * Apply a transaction timeout.
   * <p>
   * Update a query timeout to apply a transaction timeout.
   * </p>
   * @param statement a target statement
   * @param queryTimeout a query timeout
   * @param transactionTimeout a transaction timeout
   * @throws SQLException if a database access error occurs, this method is called on a closed <code>Statement</code>
   */
  public static void applyTransactionTimeout(Statement statement, Integer queryTimeout, Integer transactionTimeout) throws SQLException {
    if (transactionTimeout == null) {
      return;
    }
    if (queryTimeout == null || queryTimeout == 0 || transactionTimeout < queryTimeout) {
      statement.setQueryTimeout(transactionTimeout);
    }
  }

}
```

#### 4.3.7.8、小结（todo 总结三种不同处理器实现区别）

## 4.4、org.apache.ibatis.mapping映射包

### 4.4.1、SqlSource

```java
/**
 * 从XML或者注解读取表达的映射会话，它创建将从用户接收的输入参数传递给数据库的SQL
 *
 * @author Clinton Begin
 */
public interface SqlSource {
  //根据参数对象获取绑定的SQL
  BoundSql getBoundSql(Object parameterObject);

}
```

### 4.4.2、DatabaseIdProvider数据库ID服务

```java
/**
 * 应该返回指定类型数据库的ID，对于每种数据库类型的构建不同类型查询是否用的。
 * 此机制支持多个供应商或版本
 *
 * @author Eduardo Macarron
 */
public interface DatabaseIdProvider {

  default void setProperties(Properties p) {
    // NOP
  }
  //获取数据库ID
  String getDatabaseId(DataSource dataSource) throws SQLException;
}
```

### 4.4.3、VendorDatabaseIdProvider供应商数据库ID提供服务类

```java
/**
 * 供应商数据库ID提供服务类
 * Vendor DatabaseId provider.
 *返回数据库产品名字作为数据库ID，如果这个用户提供了属性，将用它翻译数据库产品名字，比如key="Microsoft SQL Server", value="ms" ，返回ms
 * 也可能返回null,如果没有数据库产品名字或者指定的属性没有翻译被找到。
 * It returns database product name as a databaseId.
 * If the user provides a properties it uses it to translate database product name
 * key="Microsoft SQL Server", value="ms" will return "ms".
 * It can return null, if no database product name or
 * a properties was specified and no translation was found.
 *
 * @author Eduardo Macarron
 */
public class VendorDatabaseIdProvider implements DatabaseIdProvider {
  //属性对象
  private Properties properties;
  //根据数据源获取数据库名字
  @Override
  public String getDatabaseId(DataSource dataSource) {
    if (dataSource == null) {
      throw new NullPointerException("dataSource cannot be null");
    }
    try {
      return getDatabaseName(dataSource);
    } catch (Exception e) {
      LogHolder.log.error("Could not get a databaseId from dataSource", e);
    }
    return null;
  }
  //设置属性对象
  @Override
  public void setProperties(Properties p) {
    this.properties = p;
  }
  //获取数据库名字
  private String getDatabaseName(DataSource dataSource) throws SQLException {
    String productName = getDatabaseProductName(dataSource);
    //如果指定属性里有，则遍历返回映射的value值
    if (this.properties != null) {
      for (Map.Entry<Object, Object> property : properties.entrySet()) {
        if (productName.contains((String) property.getKey())) {
          return (String) property.getValue();
        }
      }
      // no match, return null
      return null;
    }
    return productName;
  }
  //获取数据源对应的产品名称
  private String getDatabaseProductName(DataSource dataSource) throws SQLException {
    try (Connection con = dataSource.getConnection()) {
      DatabaseMetaData metaData = con.getMetaData();
      return metaData.getDatabaseProductName();
    }

  }
  //获取日志持有器
  private static class LogHolder {
    private static final Log log = LogFactory.getLog(VendorDatabaseIdProvider.class);
  }

}

```

### 4.4.4、FetchType拉取类型

```java
/**
 * 拉取类型
 * @author Eduardo Macarron
 */
public enum FetchType {
  //延迟，渴望，默认
  LAZY, EAGER, DEFAULT
}
```

### 4.4.5、parameterMode参数风格

```java
/**
 * 参数风格
 * @author Clinton Begin
 */
public enum ParameterMode {
  //输入，输出，进出
  IN, OUT, INOUT
}

```

### 4.4.6、ResultFlag结果标示

```java
/**
 * 结果标示
 * @author Clinton Begin
 */
public enum ResultFlag {
  //ID，构造器
  ID, CONSTRUCTOR
}
```

### 4.4.7、SqlCommandType命令类型

```java
/**
 * sql命令类型
 * @author Clinton Begin
 */
public enum SqlCommandType {
  //未知，插入，更新，删除，查询，刷新
  UNKNOWN, INSERT, UPDATE, DELETE, SELECT, FLUSH
}

```

### 4.4.8、StatementType会话类型

```java
/**
 * 会话类型
 * @author Clinton Begin
 */
public enum StatementType {
  //会话，预会话，回调
  STATEMENT, PREPARED, CALLABLE
}
```

### 4.4.9、BoundSql绑定SQL

```java
/**
 * 在已经执行一些动态内容之后，从{@link SqlSource} 获取实际的SQL。
 * SQL可能有占位符，并且参数映射有序 对于每个参数另外的信息（至少是从输入的对象读取值）
 * 可能也有另外的参数通过动态语言创建 比如循环或者绑定
 * An actual SQL String got from an {@link SqlSource} after having processed any dynamic content.
 * The SQL may have SQL placeholders "?" and an list (ordered) of an parameter mappings
 * with the additional information for each parameter (at least the property name of the input object to read
 * the value from).
 * <p>
 * Can also have additional parameters that are created by the dynamic language (for loops, bind...).
 *
 * @author Clinton Begin
 */
public class BoundSql {
  //sql
  private final String sql;
  //参数映射
  private final List<ParameterMapping> parameterMappings;
  //参数对象
  private final Object parameterObject;
  //附加参数
  private final Map<String, Object> additionalParameters;
  //元参数
  private final MetaObject metaParameters;
  //绑定SQL构造器
  public BoundSql(Configuration configuration, String sql, List<ParameterMapping> parameterMappings, Object parameterObject) {
    this.sql = sql;
    this.parameterMappings = parameterMappings;
    this.parameterObject = parameterObject;
    this.additionalParameters = new HashMap<>();
    this.metaParameters = configuration.newMetaObject(additionalParameters);
  }
  //获取SQL
  public String getSql() {
    return sql;
  }
 //获取参数映射
  public List<ParameterMapping> getParameterMappings() {
    return parameterMappings;
  }
  //获取参数对象
  public Object getParameterObject() {
    return parameterObject;
  }
  //是否有附加参数
  public boolean hasAdditionalParameter(String name) {
    String paramName = new PropertyTokenizer(name).getName();
    return additionalParameters.containsKey(paramName);
  }
  //设置附加参数--存放元参数信息
  public void setAdditionalParameter(String name, Object value) {
    metaParameters.setValue(name, value);
  }
  //获取附加参数
  public Object getAdditionalParameter(String name) {
    return metaParameters.getValue(name);
  }
}
```

### 4.4.10、CacheBuilder缓存构建器

```java
/**
 * 缓存构建器
 * @author Clinton Begin
 */
public class CacheBuilder {
  //id
  private final String id;
  //实现
  private Class<? extends Cache> implementation;
  //装饰集合
  private final List<Class<? extends Cache>> decorators;
  //大小
  private Integer size;
  //清除间隔
  private Long clearInterval;
  //是否读写
  private boolean readWrite;
  //属性对象
  private Properties properties;
  //阻塞
  private boolean blocking;
  //缓存构建器构造函数
  public CacheBuilder(String id) {
    this.id = id;
    this.decorators = new ArrayList<>();
  }
  //缓存构建器 缓存实现类
  public CacheBuilder implementation(Class<? extends Cache> implementation) {
    this.implementation = implementation;
    return this;
  } 
  //添加装饰缓存
  public CacheBuilder addDecorator(Class<? extends Cache> decorator) {
    if (decorator != null) {
      this.decorators.add(decorator);
    }
    return this;
  }
  //缓存构建器大小
  public CacheBuilder size(Integer size) {
    this.size = size;
    return this;
  }
  //清除间隔
  public CacheBuilder clearInterval(Long clearInterval) {
    this.clearInterval = clearInterval;
    return this;
  }
  //是否读写操作
  public CacheBuilder readWrite(boolean readWrite) {
    this.readWrite = readWrite;
    return this;
  }
  //是否阻塞
  public CacheBuilder blocking(boolean blocking) {
    this.blocking = blocking;
    return this;
  }
  //属性对象
  public CacheBuilder properties(Properties properties) {
    this.properties = properties;
    return this;
  }
  //构建
  public Cache build() {
    //设置默认的实现类
    setDefaultImplementations();
    //初始化缓存实例
    Cache cache = newBaseCacheInstance(implementation, id);
    //设置缓存属性
    setCacheProperties(cache);
    //没有应用装饰到定制的缓存的修复
    // issue #352, do not apply decorators to custom caches
    if (PerpetualCache.class.equals(cache.getClass())) {
      for (Class<? extends Cache> decorator : decorators) {
        cache = newCacheDecoratorInstance(decorator, cache);
        setCacheProperties(cache);
      }
      //设置标准的缓存装饰
      cache = setStandardDecorators(cache);
      //如果是日志缓存则改为日志缓存
    } else if (!LoggingCache.class.isAssignableFrom(cache.getClass())) {
      cache = new LoggingCache(cache);
    }
    return cache;
  }
  //设置默认的缓存实现策略--LruCache
  private void setDefaultImplementations() {
    if (implementation == null) {
      implementation = PerpetualCache.class;
      if (decorators.isEmpty()) {
        decorators.add(LruCache.class);
      }
    }
  }
  //设置标准的缓存
  private Cache setStandardDecorators(Cache cache) {
    try {
      //系统元对象获取缓存元对象
      MetaObject metaCache = SystemMetaObject.forObject(cache);
      //设置大小
      if (size != null && metaCache.hasSetter("size")) {
        metaCache.setValue("size", size);
      }
      //设置清除间隔
      if (clearInterval != null) {
        cache = new ScheduledCache(cache);
        ((ScheduledCache) cache).setClearInterval(clearInterval);
      }
      //是否读写 进行序列化缓存
      if (readWrite) {
        cache = new SerializedCache(cache);
      }
      //日志缓存
      cache = new LoggingCache(cache);
      //同步缓存
      cache = new SynchronizedCache(cache);
      //是否阻塞缓存
      if (blocking) {
        cache = new BlockingCache(cache);
      }
      return cache;
    } catch (Exception e) {
      throw new CacheException("Error building standard cache decorators.  Cause: " + e, e);
    }
  }
  //设置缓存属性
  private void setCacheProperties(Cache cache) {
    if (properties != null) {
      MetaObject metaCache = SystemMetaObject.forObject(cache);
      for (Map.Entry<Object, Object> entry : properties.entrySet()) {
        String name = (String) entry.getKey();
        String value = (String) entry.getValue();
        if (metaCache.hasSetter(name)) {
          Class<?> type = metaCache.getSetterType(name);
          if (String.class == type) {
            metaCache.setValue(name, value);
          } else if (int.class == type
              || Integer.class == type) {
            metaCache.setValue(name, Integer.valueOf(value));
          } else if (long.class == type
              || Long.class == type) {
            metaCache.setValue(name, Long.valueOf(value));
          } else if (short.class == type
              || Short.class == type) {
            metaCache.setValue(name, Short.valueOf(value));
          } else if (byte.class == type
              || Byte.class == type) {
            metaCache.setValue(name, Byte.valueOf(value));
          } else if (float.class == type
              || Float.class == type) {
            metaCache.setValue(name, Float.valueOf(value));
          } else if (boolean.class == type
              || Boolean.class == type) {
            metaCache.setValue(name, Boolean.valueOf(value));
          } else if (double.class == type
              || Double.class == type) {
            metaCache.setValue(name, Double.valueOf(value));
          } else {
            throw new CacheException("Unsupported property type for cache: '" + name + "' of type " + type);
          }
        }
      }
    }
    if (InitializingObject.class.isAssignableFrom(cache.getClass())) {
      try {
        ((InitializingObject) cache).initialize();
      } catch (Exception e) {
        throw new CacheException("Failed cache initialization for '"
          + cache.getId() + "' on '" + cache.getClass().getName() + "'", e);
      }
    }
  }
  //初始化基本缓存实例
  private Cache newBaseCacheInstance(Class<? extends Cache> cacheClass, String id) {
    Constructor<? extends Cache> cacheConstructor = getBaseCacheConstructor(cacheClass);
    try {
      return cacheConstructor.newInstance(id);
    } catch (Exception e) {
      throw new CacheException("Could not instantiate cache implementation (" + cacheClass + "). Cause: " + e, e);
    }
  }
  //获取基本缓存实例
  private Constructor<? extends Cache> getBaseCacheConstructor(Class<? extends Cache> cacheClass) {
    try {
      return cacheClass.getConstructor(String.class);
    } catch (Exception e) {
      throw new CacheException("Invalid base cache implementation (" + cacheClass + ").  "
        + "Base cache implementations must have a constructor that takes a String id as a parameter.  Cause: " + e, e);
    }
  }
  //初始化带有装饰的缓存
  private Cache newCacheDecoratorInstance(Class<? extends Cache> cacheClass, Cache base) {
    Constructor<? extends Cache> cacheConstructor = getCacheDecoratorConstructor(cacheClass);
    try {
      return cacheConstructor.newInstance(base);
    } catch (Exception e) {
      throw new CacheException("Could not instantiate cache decorator (" + cacheClass + "). Cause: " + e, e);
    }
  }
  //获取带有装饰的缓存
  private Constructor<? extends Cache> getCacheDecoratorConstructor(Class<? extends Cache> cacheClass) {
    try {
      return cacheClass.getConstructor(Cache.class);
    } catch (Exception e) {
      throw new CacheException("Invalid cache decorator (" + cacheClass + ").  "
        + "Cache decorators must have a constructor that takes a Cache instance as a parameter.  Cause: " + e, e);
    }
  }
}
```

### 4.4.11、Discriminator鉴别器

```java
/**
 * 鉴别器
 * @author Clinton Begin
 */
public class Discriminator {
  //结果映射
  private ResultMapping resultMapping;
  //鉴别器映射
  private Map<String, String> discriminatorMap;
  //空构造器
  Discriminator() {
  }
  //内部构建器
  public static class Builder {
    //鉴别器
    private Discriminator discriminator = new Discriminator();
     //构建鉴别器
    public Builder(Configuration configuration, ResultMapping resultMapping, Map<String, String> discriminatorMap) {
      discriminator.resultMapping = resultMapping;
      discriminator.discriminatorMap = discriminatorMap;
    }
    //并设置鉴别器映射为不可修改的map
    public Discriminator build() {
      assert discriminator.resultMapping != null;
      assert discriminator.discriminatorMap != null;
      assert !discriminator.discriminatorMap.isEmpty();
      //lock down map
      discriminator.discriminatorMap = Collections.unmodifiableMap(discriminator.discriminatorMap);
      return discriminator;
    }
  }
  //获取结果映射
  public ResultMapping getResultMapping() {
    return resultMapping;
  }
  //获取鉴别器映射
  public Map<String, String> getDiscriminatorMap() {
    return discriminatorMap;
  }
  //根据key获取鉴别器的value
  public String getMapIdFor(String s) {
    return discriminatorMap.get(s);
  }

}

```

### 4.4.12、MappedStatement映射会话

```java
/**
 * 映射会话
 * @author Clinton Begin
 */
public final class MappedStatement {
  //资源
  private String resource;
  //全局配置
  private Configuration configuration;
   //id
  private String id;
  //拉取数量
  private Integer fetchSize;
  //超时
  private Integer timeout;
  //会话类型
  private StatementType statementType;
  //结果集类型
  private ResultSetType resultSetType;
  //sql源
  private SqlSource sqlSource;
  //缓存
  private Cache cache;
  //参数映射
  private ParameterMap parameterMap;
  //结果集合
  private List<ResultMap> resultMaps;
  //是否有必要刷新缓存
  private boolean flushCacheRequired;
  //是否使用缓存
  private boolean useCache;
  //结果是否有序
  private boolean resultOrdered;
  //sql命令类型
  private SqlCommandType sqlCommandType;
  //是否生成主键
  private KeyGenerator keyGenerator;
  //key属性集合
  private String[] keyProperties;
  //key列集合
  private String[] keyColumns;
  //是否有嵌套结果集合
  private boolean hasNestedResultMaps;
  //数据库ID
  private String databaseId;
  //会话日志
  private Log statementLog;
  //语言驱动
  private LanguageDriver lang;
  //结果集
  private String[] resultSets;
  //映射会话
  MappedStatement() {
    // constructor disabled
  }
  //内部构建类
  public static class Builder {
    //映射会话
    private MappedStatement mappedStatement = new MappedStatement();
    //构建器
    public Builder(Configuration configuration, String id, SqlSource sqlSource, SqlCommandType sqlCommandType) {
      mappedStatement.configuration = configuration;
      mappedStatement.id = id;
      mappedStatement.sqlSource = sqlSource;
      mappedStatement.statementType = StatementType.PREPARED;
      mappedStatement.resultSetType = ResultSetType.DEFAULT;
      mappedStatement.parameterMap = new ParameterMap.Builder(configuration, "defaultParameterMap", null, new ArrayList<>()).build();
      mappedStatement.resultMaps = new ArrayList<>();
      mappedStatement.sqlCommandType = sqlCommandType;
      //Jdbc3KeyGenerator是生成key的主要实现
      mappedStatement.keyGenerator = configuration.isUseGeneratedKeys() && SqlCommandType.INSERT.equals(sqlCommandType) ? Jdbc3KeyGenerator.INSTANCE : NoKeyGenerator.INSTANCE;
      String logId = id;
      if (configuration.getLogPrefix() != null) {
        logId = configuration.getLogPrefix() + id;
      }
      mappedStatement.statementLog = LogFactory.getLog(logId);
      //获取默认的脚本语言实例
      mappedStatement.lang = configuration.getDefaultScriptingLanguageInstance();
    }
    //映射资源
    public Builder resource(String resource) {
      mappedStatement.resource = resource;
      return this;
    }
    //映射ID
    public String id() {
      return mappedStatement.id;
    }
    //参数映射
    public Builder parameterMap(ParameterMap parameterMap) {
      mappedStatement.parameterMap = parameterMap;
      return this;
    }
    //结果集
    public Builder resultMaps(List<ResultMap> resultMaps) {
      mappedStatement.resultMaps = resultMaps;
      for (ResultMap resultMap : resultMaps) {
        mappedStatement.hasNestedResultMaps = mappedStatement.hasNestedResultMaps || resultMap.hasNestedResultMaps();
      }
      return this;
    }
    //拉取数量
    public Builder fetchSize(Integer fetchSize) {
      mappedStatement.fetchSize = fetchSize;
      return this;
    }
    //超时时间
    public Builder timeout(Integer timeout) {
      mappedStatement.timeout = timeout;
      return this;
    }
    //会话类型
    public Builder statementType(StatementType statementType) {
      mappedStatement.statementType = statementType;
      return this;
    }
    //结果集类型
    public Builder resultSetType(ResultSetType resultSetType) {
      mappedStatement.resultSetType = resultSetType == null ? ResultSetType.DEFAULT : resultSetType;
      return this;
    }
   //缓存
    public Builder cache(Cache cache) {
      mappedStatement.cache = cache;
      return this;
    }
    //是否有必要刷新缓存
    public Builder flushCacheRequired(boolean flushCacheRequired) {
      mappedStatement.flushCacheRequired = flushCacheRequired;
      return this;
    }
   //是否使用缓存
    public Builder useCache(boolean useCache) {
      mappedStatement.useCache = useCache;
      return this;
    }
    //是否结果有序
    public Builder resultOrdered(boolean resultOrdered) {
      mappedStatement.resultOrdered = resultOrdered;
      return this;
    }
    //是否生成主键
    public Builder keyGenerator(KeyGenerator keyGenerator) {
      mappedStatement.keyGenerator = keyGenerator;
      return this;
    }
    //key属性
    public Builder keyProperty(String keyProperty) {
      mappedStatement.keyProperties = delimitedStringToArray(keyProperty);
      return this;
    }
    //key列
    public Builder keyColumn(String keyColumn) {
      mappedStatement.keyColumns = delimitedStringToArray(keyColumn);
      return this;
    }
    //数据库ID
    public Builder databaseId(String databaseId) {
      mappedStatement.databaseId = databaseId;
      return this;
    }
    //语言驱动
    public Builder lang(LanguageDriver driver) {
      mappedStatement.lang = driver;
      return this;
    }
    //结果集
    public Builder resultSets(String resultSet) {
      mappedStatement.resultSets = delimitedStringToArray(resultSet);
      return this;
    }

    /**
     * 不再建议使用
     * @deprecated Use {@link #resultSets}
     */
    @Deprecated
    public Builder resulSets(String resultSet) {
      mappedStatement.resultSets = delimitedStringToArray(resultSet);
      return this;
    }
    //构建返回会话
    public MappedStatement build() {
      assert mappedStatement.configuration != null;
      assert mappedStatement.id != null;
      assert mappedStatement.sqlSource != null;
      assert mappedStatement.lang != null;
      mappedStatement.resultMaps = Collections.unmodifiableList(mappedStatement.resultMaps);
      return mappedStatement;
    }
  }
  //属性的get方法，没有set防止外部破坏内部构建
  public KeyGenerator getKeyGenerator() {
    return keyGenerator;
  }

  public SqlCommandType getSqlCommandType() {
    return sqlCommandType;
  }

  public String getResource() {
    return resource;
  }

  public Configuration getConfiguration() {
    return configuration;
  }

  public String getId() {
    return id;
  }

  public boolean hasNestedResultMaps() {
    return hasNestedResultMaps;
  }

  public Integer getFetchSize() {
    return fetchSize;
  }

  public Integer getTimeout() {
    return timeout;
  }

  public StatementType getStatementType() {
    return statementType;
  }

  public ResultSetType getResultSetType() {
    return resultSetType;
  }

  public SqlSource getSqlSource() {
    return sqlSource;
  }

  public ParameterMap getParameterMap() {
    return parameterMap;
  }

  public List<ResultMap> getResultMaps() {
    return resultMaps;
  }

  public Cache getCache() {
    return cache;
  }

  public boolean isFlushCacheRequired() {
    return flushCacheRequired;
  }

  public boolean isUseCache() {
    return useCache;
  }

  public boolean isResultOrdered() {
    return resultOrdered;
  }

  public String getDatabaseId() {
    return databaseId;
  }

  public String[] getKeyProperties() {
    return keyProperties;
  }

  public String[] getKeyColumns() {
    return keyColumns;
  }

  public Log getStatementLog() {
    return statementLog;
  }

  public LanguageDriver getLang() {
    return lang;
  }

  public String[] getResultSets() {
    return resultSets;
  }

  /**
   * 不建议使用
   * @deprecated Use {@link #getResultSets()}
   */
  @Deprecated
  public String[] getResulSets() {
    return resultSets;
  }

  public BoundSql getBoundSql(Object parameterObject) {
    BoundSql boundSql = sqlSource.getBoundSql(parameterObject);
    List<ParameterMapping> parameterMappings = boundSql.getParameterMappings();
    if (parameterMappings == null || parameterMappings.isEmpty()) {
      boundSql = new BoundSql(configuration, boundSql.getSql(), parameterMap.getParameterMappings(), parameterObject);
    }

    // check for nested result maps in parameter mappings (issue #30)
    for (ParameterMapping pm : boundSql.getParameterMappings()) {
      String rmId = pm.getResultMapId();
      if (rmId != null) {
        ResultMap rm = configuration.getResultMap(rmId);
        if (rm != null) {
          hasNestedResultMaps |= rm.hasNestedResultMaps();
        }
      }
    }

    return boundSql;
  }
  //根据逗号分割返回数组
  private static String[] delimitedStringToArray(String in) {
    if (in == null || in.trim().length() == 0) {
      return null;
    } else {
      return in.split(",");
    }
  }

}

```

### 4.4.13、ParameterMapping参数映射

```java
/**
 * 参数映射
 * @author Clinton Begin
 */
public class ParameterMapping {
  //全局配置
  private Configuration configuration;
  //属性
  private String property;
  //参数风格
  private ParameterMode mode;
  //Java类型
  private Class<?> javaType = Object.class;
  //jdbc类型
  private JdbcType jdbcType;
  //数值级别
  private Integer numericScale;
  //类型处理器
  private TypeHandler<?> typeHandler;
  //结果集映射ID
  private String resultMapId;
  //jdbc类型名称
  private String jdbcTypeName;
  //正则表达
  private String expression;

  private ParameterMapping() {
  }
  //内部构建器
  public static class Builder {
    //参数映射
    private ParameterMapping parameterMapping = new ParameterMapping();
    //构建构造函数
    public Builder(Configuration configuration, String property, TypeHandler<?> typeHandler) {
      parameterMapping.configuration = configuration;
      parameterMapping.property = property;
      parameterMapping.typeHandler = typeHandler;
      parameterMapping.mode = ParameterMode.IN;
    }
    //构建构造函数
    public Builder(Configuration configuration, String property, Class<?> javaType) {
      parameterMapping.configuration = configuration;
      parameterMapping.property = property;
      parameterMapping.javaType = javaType;
      parameterMapping.mode = ParameterMode.IN;
    }
    //参数风格
    public Builder mode(ParameterMode mode) {
      parameterMapping.mode = mode;
      return this;
    }
    //Java类型
    public Builder javaType(Class<?> javaType) {
      parameterMapping.javaType = javaType;
      return this;
    }
    //jdbc类型
    public Builder jdbcType(JdbcType jdbcType) {
      parameterMapping.jdbcType = jdbcType;
      return this;
    } 
    //数值级别
    public Builder numericScale(Integer numericScale) {
      parameterMapping.numericScale = numericScale;
      return this;
    }
    //结果映射ID
    public Builder resultMapId(String resultMapId) {
      parameterMapping.resultMapId = resultMapId;
      return this;
    }
    //类型处理器
    public Builder typeHandler(TypeHandler<?> typeHandler) {
      parameterMapping.typeHandler = typeHandler;
      return this;
    }
    //jdbc类型名字
    public Builder jdbcTypeName(String jdbcTypeName) {
      parameterMapping.jdbcTypeName = jdbcTypeName;
      return this;
    }
    //表达
    public Builder expression(String expression) {
      parameterMapping.expression = expression;
      return this;
    }
   //构建
    public ParameterMapping build() {
      //解析类型处理器
      resolveTypeHandler();
      //校验
      validate();
      return parameterMapping;
    }
    //校验
    private void validate() {
      //结果集是否是jdbc类型
      if (ResultSet.class.equals(parameterMapping.javaType)) {
        if (parameterMapping.resultMapId == null) {
          throw new IllegalStateException("Missing resultmap in property '"
              + parameterMapping.property + "'.  "
              + "Parameters of type java.sql.ResultSet require a resultmap.");
        }
      } else {
        if (parameterMapping.typeHandler == null) {
          throw new IllegalStateException("Type handler was null on parameter mapping for property '"
            + parameterMapping.property + "'. It was either not specified and/or could not be found for the javaType ("
            + parameterMapping.javaType.getName() + ") : jdbcType (" + parameterMapping.jdbcType + ") combination.");
        }
      }
    }
    //解析类型处理器
    private void resolveTypeHandler() {
      if (parameterMapping.typeHandler == null && parameterMapping.javaType != null) {
        Configuration configuration = parameterMapping.configuration;
        TypeHandlerRegistry typeHandlerRegistry = configuration.getTypeHandlerRegistry();
        parameterMapping.typeHandler = typeHandlerRegistry.getTypeHandler(parameterMapping.javaType, parameterMapping.jdbcType);
      }
    }

  }
  //获取属性
  public String getProperty() {
    return property;
  }

  /**
   * 回调会话的处理输出
   * Used for handling output of callable statements.
   * @return
   */
  public ParameterMode getMode() {
    return mode;
  }

  /**
   * 回调会话的处理输出
   * Used for handling output of callable statements.
   * @return
   */
  public Class<?> getJavaType() {
    return javaType;
  }

  /**
   * 未知类型处理器以防没有没有属性类型对应的处理器
   * Used in the UnknownTypeHandler in case there is no handler for the property type.
   * @return
   */
  public JdbcType getJdbcType() {
    return jdbcType;
  }

  /**
   * 用于回调会话的处理输出
   * Used for handling output of callable statements.
   * @return
   */
  public Integer getNumericScale() {
    return numericScale;
  }

  /**
   * 用于给预会话设置参数
   * Used when setting parameters to the PreparedStatement.
   * @return
   */
  public TypeHandler<?> getTypeHandler() {
    return typeHandler;
  }

  /**
   * 回调会话的处理输出
   * Used for handling output of callable statements.
   * @return
   */
  public String getResultMapId() {
    return resultMapId;
  }

  /**
   * 回调会话的处理输出
   * Used for handling output of callable statements.
   * @return
   */
  public String getJdbcTypeName() {
    return jdbcTypeName;
  }

  /**
   * 没有使用
   * Not used
   * @return
   */
  public String getExpression() {
    return expression;
  }

  @Override
  public String toString() {
    final StringBuilder sb = new StringBuilder("ParameterMapping{");
    //sb.append("configuration=").append(configuration); // configuration doesn't have a useful .toString()
    sb.append("property='").append(property).append('\'');
    sb.append(", mode=").append(mode);
    sb.append(", javaType=").append(javaType);
    sb.append(", jdbcType=").append(jdbcType);
    sb.append(", numericScale=").append(numericScale);
    //sb.append(", typeHandler=").append(typeHandler); // typeHandler also doesn't have a useful .toString()
    sb.append(", resultMapId='").append(resultMapId).append('\'');
    sb.append(", jdbcTypeName='").append(jdbcTypeName).append('\'');
    sb.append(", expression='").append(expression).append('\'');
    sb.append('}');
    return sb.toString();
  }
}

```

### 4.4.14、ResultMapping

```java
/**
 * 结果映射
 * @author Clinton Begin
 */
public class ResultMapping {
  //全局配置
  private Configuration configuration;
  //属性
  private String property;
  //列
  private String column;
  //Java类型
  private Class<?> javaType;
  //jdbc类型
  private JdbcType jdbcType;
  //类型处理器
  private TypeHandler<?> typeHandler;
  //嵌套结果映射ID
  private String nestedResultMapId;
  //嵌套查询ID
  private String nestedQueryId;
  //非空列
  private Set<String> notNullColumns;
  //列前缀
  private String columnPrefix;
  //结果标记
  private List<ResultFlag> flags;
  //合成的结果集
  private List<ResultMapping> composites;
  //结果集
  private String resultSet;
  //外键
  private String foreignColumn;
  //是否懒加载
  private boolean lazy;

  ResultMapping() {
  }
  //内部类构建器
  public static class Builder {
    //结果映射
    private ResultMapping resultMapping = new ResultMapping();
    //构建器构造函数
    public Builder(Configuration configuration, String property, String column, TypeHandler<?> typeHandler) {
      this(configuration, property);
      resultMapping.column = column;
      resultMapping.typeHandler = typeHandler;
    }
    //构建器构造函数
    public Builder(Configuration configuration, String property, String column, Class<?> javaType) {
      this(configuration, property);
      resultMapping.column = column;
      resultMapping.javaType = javaType;
    }
    //构建器构造函数
    public Builder(Configuration configuration, String property) {
      resultMapping.configuration = configuration;
      resultMapping.property = property;
      resultMapping.flags = new ArrayList<>();
      resultMapping.composites = new ArrayList<>();
      resultMapping.lazy = configuration.isLazyLoadingEnabled();
    }
    //Java类型
    public Builder javaType(Class<?> javaType) {
      resultMapping.javaType = javaType;
      return this;
    }
    //jdbc类型
    public Builder jdbcType(JdbcType jdbcType) {
      resultMapping.jdbcType = jdbcType;
      return this;
    }
   //嵌套结果映射ID
    public Builder nestedResultMapId(String nestedResultMapId) {
      resultMapping.nestedResultMapId = nestedResultMapId;
      return this;
    }
    //嵌套查询ID
    public Builder nestedQueryId(String nestedQueryId) {
      resultMapping.nestedQueryId = nestedQueryId;
      return this;
    }
    //结果集
    public Builder resultSet(String resultSet) {
      resultMapping.resultSet = resultSet;
      return this;
    }
   //外键
    public Builder foreignColumn(String foreignColumn) {
      resultMapping.foreignColumn = foreignColumn;
      return this;
    }
   //非空列
    public Builder notNullColumns(Set<String> notNullColumns) {
      resultMapping.notNullColumns = notNullColumns;
      return this;
    }
   //列前缀
    public Builder columnPrefix(String columnPrefix) {
      resultMapping.columnPrefix = columnPrefix;
      return this;
    }
   //标记
    public Builder flags(List<ResultFlag> flags) {
      resultMapping.flags = flags;
      return this;
    }
   //类型处理器
    public Builder typeHandler(TypeHandler<?> typeHandler) {
      resultMapping.typeHandler = typeHandler;
      return this;
    }
   //混合的结果集
    public Builder composites(List<ResultMapping> composites) {
      resultMapping.composites = composites;
      return this;
    }
   //是否懒加载
    public Builder lazy(boolean lazy) {
      resultMapping.lazy = lazy;
      return this;
    }
   //构建器构建
    public ResultMapping build() {
      // lock down collections
      resultMapping.flags = Collections.unmodifiableList(resultMapping.flags);
      resultMapping.composites = Collections.unmodifiableList(resultMapping.composites);
      //解析类型处理器
      resolveTypeHandler();
      //校验
      validate();
      return resultMapping;
    }
 //解决了一些bug校验
    private void validate() {
      //不能同时定义嵌套查询ID和嵌套结果ID
      // Issue #697: cannot define both nestedQueryId and nestedResultMapId
      if (resultMapping.nestedQueryId != null && resultMapping.nestedResultMapId != null) {
        throw new IllegalStateException("Cannot define both nestedQueryId and nestedResultMapId in property " + resultMapping.property);
      }
      //没有类型处理器的映射
      // Issue #5: there should be no mappings without typehandler
      if (resultMapping.nestedQueryId == null && resultMapping.nestedResultMapId == null && resultMapping.typeHandler == null) {
        throw new IllegalStateException("No typehandler found for property " + resultMapping.property);
      }
      //列仅在嵌套的resultmaps中是可选的，但在其余的resultmaps中不是
      // Issue #4 and GH #39: column is optional only in nested resultmaps but not in the rest
      if (resultMapping.nestedResultMapId == null && resultMapping.column == null && resultMapping.composites.isEmpty()) {
        throw new IllegalStateException("Mapping is missing column attribute for property " + resultMapping.property);
      }
      if (resultMapping.getResultSet() != null) {
        int numColumns = 0;
        if (resultMapping.column != null) {
          numColumns = resultMapping.column.split(",").length;
        }
        int numForeignColumns = 0;
        if (resultMapping.foreignColumn != null) {
          numForeignColumns = resultMapping.foreignColumn.split(",").length;
        }
        if (numColumns != numForeignColumns) {
          throw new IllegalStateException("There should be the same number of columns and foreignColumns in property " + resultMapping.property);
        }
      }
    }
    //解析类型处理器
    private void resolveTypeHandler() {
      if (resultMapping.typeHandler == null && resultMapping.javaType != null) {
        Configuration configuration = resultMapping.configuration;
        TypeHandlerRegistry typeHandlerRegistry = configuration.getTypeHandlerRegistry();
        resultMapping.typeHandler = typeHandlerRegistry.getTypeHandler(resultMapping.javaType, resultMapping.jdbcType);
      }
    }
   //列字段构建
    public Builder column(String column) {
      resultMapping.column = column;
      return this;
    }
  } 
  //只提供查询get方法

  public String getProperty() {
    return property;
  }

  public String getColumn() {
    return column;
  }

  public Class<?> getJavaType() {
    return javaType;
  }

  public JdbcType getJdbcType() {
    return jdbcType;
  }

  public TypeHandler<?> getTypeHandler() {
    return typeHandler;
  }

  public String getNestedResultMapId() {
    return nestedResultMapId;
  }

  public String getNestedQueryId() {
    return nestedQueryId;
  }

  public Set<String> getNotNullColumns() {
    return notNullColumns;
  }

  public String getColumnPrefix() {
    return columnPrefix;
  }

  public List<ResultFlag> getFlags() {
    return flags;
  }

  public List<ResultMapping> getComposites() {
    return composites;
  }

  public boolean isCompositeResult() {
    return this.composites != null && !this.composites.isEmpty();
  }

  public String getResultSet() {
    return this.resultSet;
  }

  public String getForeignColumn() {
    return foreignColumn;
  }

  public void setForeignColumn(String foreignColumn) {
    this.foreignColumn = foreignColumn;
  }

  public boolean isLazy() {
    return lazy;
  }

  public void setLazy(boolean lazy) {
    this.lazy = lazy;
  }

  @Override
  public boolean equals(Object o) {
    if (this == o) {
      return true;
    }
    if (o == null || getClass() != o.getClass()) {
      return false;
    }

    ResultMapping that = (ResultMapping) o;

    return property != null && property.equals(that.property);
  }

  @Override
  public int hashCode() {
    if (property != null) {
      return property.hashCode();
    } else if (column != null) {
      return column.hashCode();
    } else {
      return 0;
    }
  }

  @Override
  public String toString() {
    final StringBuilder sb = new StringBuilder("ResultMapping{");
    //sb.append("configuration=").append(configuration); // configuration doesn't have a useful .toString()
    sb.append("property='").append(property).append('\'');
    sb.append(", column='").append(column).append('\'');
    sb.append(", javaType=").append(javaType);
    sb.append(", jdbcType=").append(jdbcType);
    //sb.append(", typeHandler=").append(typeHandler); // typeHandler also doesn't have a useful .toString()
    sb.append(", nestedResultMapId='").append(nestedResultMapId).append('\'');
    sb.append(", nestedQueryId='").append(nestedQueryId).append('\'');
    sb.append(", notNullColumns=").append(notNullColumns);
    sb.append(", columnPrefix='").append(columnPrefix).append('\'');
    sb.append(", flags=").append(flags);
    sb.append(", composites=").append(composites);
    sb.append(", resultSet='").append(resultSet).append('\'');
    sb.append(", foreignColumn='").append(foreignColumn).append('\'');
    sb.append(", lazy=").append(lazy);
    sb.append('}');
    return sb.toString();
  }

}

```

### 4.4.15、ResultMap

```java
/**
 * 结果集
 * @author Clinton Begin
 */
public class ResultMap {
  //全局配置
  private Configuration configuration;
  //id
  private String id;
  //类型
  private Class<?> type;
  //结果映射
  private List<ResultMapping> resultMappings;
  //id结果映射
  private List<ResultMapping> idResultMappings;
  //构造器结果映射
  private List<ResultMapping> constructorResultMappings;
  //属性结果映射
  private List<ResultMapping> propertyResultMappings;
  //被映射的列
  private Set<String> mappedColumns;
  //被映射的属性
  private Set<String> mappedProperties;
  //鉴别器
  private Discriminator discriminator;
  //是否有嵌套结果集
  private boolean hasNestedResultMaps;
  //是否有嵌套查询
  private boolean hasNestedQueries;
  //是否自动映射
  private Boolean autoMapping;

  private ResultMap() {
  }
  //内部构建器
  public static class Builder {
    //日志
    private static final Log log = LogFactory.getLog(Builder.class);
   //结果映射
    private ResultMap resultMap = new ResultMap();
   //构造函数
    public Builder(Configuration configuration, String id, Class<?> type, List<ResultMapping> resultMappings) {
      this(configuration, id, type, resultMappings, null);
    }
    //构造函数
    public Builder(Configuration configuration, String id, Class<?> type, List<ResultMapping> resultMappings, Boolean autoMapping) {
      resultMap.configuration = configuration;
      resultMap.id = id;
      resultMap.type = type;
      resultMap.resultMappings = resultMappings;
      resultMap.autoMapping = autoMapping;
    }
    //鉴别器
    public Builder discriminator(Discriminator discriminator) {
      resultMap.discriminator = discriminator;
      return this;
    }
   //返回结果类型
    public Class<?> type() {
      return resultMap.type;
    }
   //构建方法
    public ResultMap build() {
      if (resultMap.id == null) {
        throw new IllegalArgumentException("ResultMaps must have an id");
      }
      resultMap.mappedColumns = new HashSet<>();
      resultMap.mappedProperties = new HashSet<>();
      resultMap.idResultMappings = new ArrayList<>();
      resultMap.constructorResultMappings = new ArrayList<>();
      resultMap.propertyResultMappings = new ArrayList<>();
      final List<String> constructorArgNames = new ArrayList<>();
      //处理结果的映射
      for (ResultMapping resultMapping : resultMap.resultMappings) {
        resultMap.hasNestedQueries = resultMap.hasNestedQueries || resultMapping.getNestedQueryId() != null;
        resultMap.hasNestedResultMaps = resultMap.hasNestedResultMaps || (resultMapping.getNestedResultMapId() != null && resultMapping.getResultSet() == null);
        final String column = resultMapping.getColumn();
        if (column != null) {
          resultMap.mappedColumns.add(column.toUpperCase(Locale.ENGLISH));
        } else if (resultMapping.isCompositeResult()) {
          for (ResultMapping compositeResultMapping : resultMapping.getComposites()) {
            final String compositeColumn = compositeResultMapping.getColumn();
            if (compositeColumn != null) {
              resultMap.mappedColumns.add(compositeColumn.toUpperCase(Locale.ENGLISH));
            }
          }
        }
        final String property = resultMapping.getProperty();
        if (property != null) {
          resultMap.mappedProperties.add(property);
        }
        if (resultMapping.getFlags().contains(ResultFlag.CONSTRUCTOR)) {
          resultMap.constructorResultMappings.add(resultMapping);
          if (resultMapping.getProperty() != null) {
            constructorArgNames.add(resultMapping.getProperty());
          }
        } else {
          resultMap.propertyResultMappings.add(resultMapping);
        }
        if (resultMapping.getFlags().contains(ResultFlag.ID)) {
          resultMap.idResultMappings.add(resultMapping);
        }
      }
      //id结果映射
      if (resultMap.idResultMappings.isEmpty()) {
        resultMap.idResultMappings.addAll(resultMap.resultMappings);
      }
      if (!constructorArgNames.isEmpty()) {
        final List<String> actualArgNames = argNamesOfMatchingConstructor(constructorArgNames);
        if (actualArgNames == null) {
          throw new BuilderException("Error in result map '" + resultMap.id
              + "'. Failed to find a constructor in '"
              + resultMap.getType().getName() + "' by arg names " + constructorArgNames
              + ". There might be more info in debug log.");
        }
        resultMap.constructorResultMappings.sort((o1, o2) -> {
          int paramIdx1 = actualArgNames.indexOf(o1.getProperty());
          int paramIdx2 = actualArgNames.indexOf(o2.getProperty());
          return paramIdx1 - paramIdx2;
        });
      }
      //所有的结果可读设置
      // lock down collections
      resultMap.resultMappings = Collections.unmodifiableList(resultMap.resultMappings);
      resultMap.idResultMappings = Collections.unmodifiableList(resultMap.idResultMappings);
      resultMap.constructorResultMappings = Collections.unmodifiableList(resultMap.constructorResultMappings);
      resultMap.propertyResultMappings = Collections.unmodifiableList(resultMap.propertyResultMappings);
      resultMap.mappedColumns = Collections.unmodifiableSet(resultMap.mappedColumns);
      return resultMap;
    }
   //参数名字匹配构造器
    private List<String> argNamesOfMatchingConstructor(List<String> constructorArgNames) {
      Constructor<?>[] constructors = resultMap.type.getDeclaredConstructors();
      for (Constructor<?> constructor : constructors) {
        Class<?>[] paramTypes = constructor.getParameterTypes();
        if (constructorArgNames.size() == paramTypes.length) {
          List<String> paramNames = getArgNames(constructor);
          if (constructorArgNames.containsAll(paramNames)
              && argTypesMatch(constructorArgNames, paramTypes, paramNames)) {
            return paramNames;
          }
        }
      }
      return null;
    }
   //参数类型匹配
    private boolean argTypesMatch(final List<String> constructorArgNames,
        Class<?>[] paramTypes, List<String> paramNames) {
      for (int i = 0; i < constructorArgNames.size(); i++) {
        Class<?> actualType = paramTypes[paramNames.indexOf(constructorArgNames.get(i))];
        Class<?> specifiedType = resultMap.constructorResultMappings.get(i).getJavaType();
        if (!actualType.equals(specifiedType)) {
          if (log.isDebugEnabled()) {
            log.debug("While building result map '" + resultMap.id
                + "', found a constructor with arg names " + constructorArgNames
                + ", but the type of '" + constructorArgNames.get(i)
                + "' did not match. Specified: [" + specifiedType.getName() + "] Declared: ["
                + actualType.getName() + "]");
          }
          return false;
        }
      }
      return true;
    }
   //获取参数名字
    private List<String> getArgNames(Constructor<?> constructor) {
      List<String> paramNames = new ArrayList<>();
      List<String> actualParamNames = null;
      final Annotation[][] paramAnnotations = constructor.getParameterAnnotations();
      int paramCount = paramAnnotations.length;
      for (int paramIndex = 0; paramIndex < paramCount; paramIndex++) {
        String name = null;
        for (Annotation annotation : paramAnnotations[paramIndex]) {
          if (annotation instanceof Param) {
            name = ((Param) annotation).value();
            break;
          }
        }
        if (name == null && resultMap.configuration.isUseActualParamName()) {
          if (actualParamNames == null) {
            actualParamNames = ParamNameUtil.getParamNames(constructor);
          }
          if (actualParamNames.size() > paramIndex) {
            name = actualParamNames.get(paramIndex);
          }
        }
        paramNames.add(name != null ? name : "arg" + paramIndex);
      }
      return paramNames;
    }
  }
  //获取只查询的get方法
  public String getId() {
    return id;
  }

  public boolean hasNestedResultMaps() {
    return hasNestedResultMaps;
  }

  public boolean hasNestedQueries() {
    return hasNestedQueries;
  }

  public Class<?> getType() {
    return type;
  }

  public List<ResultMapping> getResultMappings() {
    return resultMappings;
  }

  public List<ResultMapping> getConstructorResultMappings() {
    return constructorResultMappings;
  }

  public List<ResultMapping> getPropertyResultMappings() {
    return propertyResultMappings;
  }

  public List<ResultMapping> getIdResultMappings() {
    return idResultMappings;
  }

  public Set<String> getMappedColumns() {
    return mappedColumns;
  }

  public Set<String> getMappedProperties() {
    return mappedProperties;
  }

  public Discriminator getDiscriminator() {
    return discriminator;
  }

  public void forceNestedResultMaps() {
    hasNestedResultMaps = true;
  }

  public Boolean getAutoMapping() {
    return autoMapping;
  }

}

```

### 4.4.16、Enviroment

```java
/**
 * 全局环境
 * @author Clinton Begin
 */
public final class Environment {
  //id
  private final String id;
  //事务工厂
  private final TransactionFactory transactionFactory;
  //数据源
  private final DataSource dataSource;
  //环境构造函数
  public Environment(String id, TransactionFactory transactionFactory, DataSource dataSource) {
    if (id == null) {
      throw new IllegalArgumentException("Parameter 'id' must not be null");
    }
    if (transactionFactory == null) {
      throw new IllegalArgumentException("Parameter 'transactionFactory' must not be null");
    }
    this.id = id;
    if (dataSource == null) {
      throw new IllegalArgumentException("Parameter 'dataSource' must not be null");
    }
    this.transactionFactory = transactionFactory;
    this.dataSource = dataSource;
  }
  //内部构建器
  public static class Builder {
    //id
    private final String id;
    //事务工厂
    private TransactionFactory transactionFactory;
    //数据源
    private DataSource dataSource;

    public Builder(String id) {
      this.id = id;
    }

    public Builder transactionFactory(TransactionFactory transactionFactory) {
      this.transactionFactory = transactionFactory;
      return this;
    }

    public Builder dataSource(DataSource dataSource) {
      this.dataSource = dataSource;
      return this;
    }

    public String id() {
      return this.id;
    }
    //构建器构建环境

    public Environment build() {
      return new Environment(this.id, this.transactionFactory, this.dataSource);
    }

  }
  //get方法
  public String getId() {
    return this.id;
  }

  public TransactionFactory getTransactionFactory() {
    return this.transactionFactory;
  }

  public DataSource getDataSource() {
    return this.dataSource;
  }

}
```



### 4.4.17、小结

* 建造器设计模式，其实在mybatis框架中很多场景用，目的实际上是尽量暴露出的是查询，而不是修改。所以框架内不建造器模式很大范围使用。
* 框架内部喜欢拆分不同职能类，然后有个聚合类来对外统一提供职能。



## 4.5、org.apache.ibatis.plugin插件模块

### 4.5.1、PluginException插件异常

```java
/**
 * 插件异常
 * @author Clinton Begin
 */
public class PluginException extends PersistenceException {

  private static final long serialVersionUID = 8548771664564998595L;
  //空构造函数
  public PluginException() {
    super();
  }
  //传递信息的构造函数
  public PluginException(String message) {
    super(message);
  }
  //传递信息和throwable的构造函数
  public PluginException(String message, Throwable cause) {
    super(message, cause);
  }
  //传递throwable的构造函数
  public PluginException(Throwable cause) {
    super(cause);
  }
}
```

### 4.5.2、Intercepts拦截注解

```java
/**
 * 指定拦截目标方法的注解
 * The annotation that specify target methods to intercept.
 *
 * <b>How to use:</b>
 * <pre>
 * &#064;Intercepts({&#064;Signature(
 *   type= Executor.class,
 *   method = "update",
 *   args = {MappedStatement.class ,Object.class})})
 * public class ExamplePlugin implements Interceptor {
 *   &#064;Override
 *   public Object intercept(Invocation invocation) throws Throwable {
 *     // implement pre-processing if needed
 *     Object returnObject = invocation.proceed();
 *     // implement post-processing if needed
 *     return returnObject;
 *   }
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Intercepts {
  /**
   * 返回拦截方法的签名
   * Returns method signatures to intercept.
   *
   * @return method signatures
   */
  Signature[] value();
}
```

### 4.5.3、Signature签名

```java
/**
 * 方法签名的注解
 * The annotation that indicate the method signature.
 *
 * @see Intercepts
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({})
public @interface Signature {
  /**
   * 返回Java类型
   * Returns the java type.
   *
   * @return the java type
   */
  Class<?> type();

  /**
   * 返回方法的名字
   * Returns the method name.
   *
   * @return the method name
   */
  String method();

  /**
   * 返回方法参数的Java类型
   * Returns java types for method argument.
   * @return java types for method argument
   */
  Class<?>[] args();
}
```

### 4.5.4、Invocation执行方法

```java
/**
 * 执行类
 * @author Clinton Begin
 */
public class Invocation {
  //目标类
  private final Object target;
  //方法
  private final Method method;
  //参数
  private final Object[] args;
  //执行构造函数
  public Invocation(Object target, Method method, Object[] args) {
    this.target = target;
    this.method = method;
    this.args = args;
  }
  //获取目标类
  public Object getTarget() {
    return target;
  }
  //获取方法
  public Method getMethod() {
    return method;
  }
  //获取参数
  public Object[] getArgs() {
    return args;
  }
  //执行目标类和参数
  public Object proceed() throws InvocationTargetException, IllegalAccessException {
    return method.invoke(target, args);
  }

}
```

### 4.5.5、Plugin插件

```java
/**
 * 执行处理器
 * @author Clinton Begin
 */
public class Plugin implements InvocationHandler {
  //目标类
  private final Object target;
  //拦截器
  private final Interceptor interceptor;
  //方法的签名集合
  private final Map<Class<?>, Set<Method>> signatureMap;
  //插件构造函数
  private Plugin(Object target, Interceptor interceptor, Map<Class<?>, Set<Method>> signatureMap) {
    this.target = target;
    this.interceptor = interceptor;
    this.signatureMap = signatureMap;
  }
  //包装目标类和拦截器
  public static Object wrap(Object target, Interceptor interceptor) {
    //获取拦截器的签名集合
    Map<Class<?>, Set<Method>> signatureMap = getSignatureMap(interceptor);
    Class<?> type = target.getClass();
    //获取所有的该类型的接口
    Class<?>[] interfaces = getAllInterfaces(type, signatureMap);
    if (interfaces.length > 0) {
      //代理执行插件和接口
      return Proxy.newProxyInstance(
          type.getClassLoader(),
          interfaces,
          new Plugin(target, interceptor, signatureMap));
    }
    return target;
  }
  //执行代理方法
  @Override
  public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    try {
      //拦截器拦截该方法
      Set<Method> methods = signatureMap.get(method.getDeclaringClass());
      if (methods != null && methods.contains(method)) {
        return interceptor.intercept(new Invocation(target, method, args));
      }
      //方法执行
      return method.invoke(target, args);
    } catch (Exception e) {
      throw ExceptionUtil.unwrapThrowable(e);
    }
  }
  //获取签名的拦截器集合
  private static Map<Class<?>, Set<Method>> getSignatureMap(Interceptor interceptor) {
    //注解拦截器
    Intercepts interceptsAnnotation = interceptor.getClass().getAnnotation(Intercepts.class);
    // issue #251
    if (interceptsAnnotation == null) {
      throw new PluginException("No @Intercepts annotation was found in interceptor " + interceptor.getClass().getName());
    }
    //签名数组
    Signature[] sigs = interceptsAnnotation.value();
    Map<Class<?>, Set<Method>> signatureMap = new HashMap<>();
    //编列签名
    for (Signature sig : sigs) {
      Set<Method> methods = signatureMap.computeIfAbsent(sig.type(), k -> new HashSet<>());
      try {
        Method method = sig.type().getMethod(sig.method(), sig.args());
        methods.add(method);
      } catch (NoSuchMethodException e) {
        throw new PluginException("Could not find method on " + sig.type() + " named " + sig.method() + ". Cause: " + e, e);
      }
    }
    return signatureMap;
  }
  //获取所有接口类
  private static Class<?>[] getAllInterfaces(Class<?> type, Map<Class<?>, Set<Method>> signatureMap) {
    Set<Class<?>> interfaces = new HashSet<>();
    while (type != null) {
      for (Class<?> c : type.getInterfaces()) {
        if (signatureMap.containsKey(c)) {
          interfaces.add(c);
        }
      }
      type = type.getSuperclass();
    }
    return interfaces.toArray(new Class<?>[interfaces.size()]);
  }

}
```

### 4.5.6、Interceptor拦截器

```java
/**
 * 拦截器
 * @author Clinton Begin
 */
public interface Interceptor {
  //拦截执行方法
  Object intercept(Invocation invocation) throws Throwable;
  //插件包装目标对象
  default Object plugin(Object target) {
    return Plugin.wrap(target, this);
  }
  //设置属性
  default void setProperties(Properties properties) {
    // NOP
  }

}
```

### 4.5.7、InterceptorChain拦截器链

```java
/**
 * 拦截器链
 * @author Clinton Begin
 */
public class InterceptorChain {
  //拦截器链
  private final List<Interceptor> interceptors = new ArrayList<>();
  //所有的拦截器
  public Object pluginAll(Object target) {
    for (Interceptor interceptor : interceptors) {
      target = interceptor.plugin(target);
    }
    return target;
  }
 //添加拦截器
  public void addInterceptor(Interceptor interceptor) {
    interceptors.add(interceptor);
  }
  //获取只读拦截器
  public List<Interceptor> getInterceptors() {
    return Collections.unmodifiableList(interceptors);
  }

}
```

### 4.5.8、小结

* 注解的使用，拦截器的包装，拦截器链的使用，对外可扩展

## 4.6、org.apache.ibatis.scripting脚本语言模块

### 4.6.1、基础模块

#### 4.6.1.1、LanguageDriver语言驱动

```java
/**
 * 语言驱动
 */
public interface LanguageDriver {

  /**
   * 创建参数处理器将实际参数传递给jdbc会话
   */
  ParameterHandler createParameterHandler(MappedStatement mappedStatement, Object parameterObject, BoundSql boundSql);

  /**
   * 从mapper 的xml文件 创建sqlSource持有会话。在启动的时候会被调用，当这个映射的会话从类或者xml文件读取的时候。
   */
  SqlSource createSqlSource(Configuration configuration, XNode script, Class<?> parameterType);

  /**
   * 从注解创建sqlSource持有会话。在启动的时候被调用，当这个映射的会话从类或者xml读取的时候。
   */
  SqlSource createSqlSource(Configuration configuration, String script, Class<?> parameterType);

}
```

#### 4.6.1.2、LanguageDriverRegistry

```java
/**
 * 语言驱动注册类
 * @author Frank D. Martinez [mnesarco]
 */
public class LanguageDriverRegistry {
  //语言驱动集合
  private final Map<Class<? extends LanguageDriver>, LanguageDriver> LANGUAGE_DRIVER_MAP = new HashMap<>();
  //默认的驱动类
  private Class<? extends LanguageDriver> defaultDriverClass;
  //注册语言驱动类
  public void register(Class<? extends LanguageDriver> cls) {
    if (cls == null) {
      throw new IllegalArgumentException("null is not a valid Language Driver");
    }
    LANGUAGE_DRIVER_MAP.computeIfAbsent(cls, k -> {
      try {
        return k.getDeclaredConstructor().newInstance();
      } catch (Exception ex) {
        throw new ScriptingException("Failed to load language driver for " + cls.getName(), ex);
      }
    });
  }
  //注册语言驱动实例
  public void register(LanguageDriver instance) {
    if (instance == null) {
      throw new IllegalArgumentException("null is not a valid Language Driver");
    }
    Class<? extends LanguageDriver> cls = instance.getClass();
    if (!LANGUAGE_DRIVER_MAP.containsKey(cls)) {
      LANGUAGE_DRIVER_MAP.put(cls, instance);
    }
  }
  //获取语言驱动实例
  public LanguageDriver getDriver(Class<? extends LanguageDriver> cls) {
    return LANGUAGE_DRIVER_MAP.get(cls);
  }
  //获取默认驱动类
  public LanguageDriver getDefaultDriver() {
    return getDriver(getDefaultDriverClass());
  }
  //获取默认的驱动类
  public Class<? extends LanguageDriver> getDefaultDriverClass() {
    return defaultDriverClass;
  }
  //设置默认的驱动类
  public void setDefaultDriverClass(Class<? extends LanguageDriver> defaultDriverClass) {
    register(defaultDriverClass);
    this.defaultDriverClass = defaultDriverClass;
  }

}
```

#### 4.6.1.3、ScriptingException

```java
/**
 * 脚本异常
 * @author Frank D. Martinez [mnesarco]
 */
public class ScriptingException extends PersistenceException {

  private static final long serialVersionUID = 7642570221267566591L;
  //空构造函数
  public ScriptingException() {
    super();
  }
  //传递信息的构造函数
  public ScriptingException(String message) {
    super(message);
  }
  //传递信息和throwable的构造函数
  public ScriptingException(String message, Throwable cause) {
    super(message, cause);
  }
  //传递throwable的构造函数
  public ScriptingException(Throwable cause) {
    super(cause);
  }

}
```

### 4.6.2、defaults

#### 4.6.2.1、RawLanguageDriver(不建议用)

```java
/**
 * 从3.2.4版本起，默认的xml语言是能区分静态会话并且创建原生{@link RawSqlSource}.。因此除非你想要确保任何原因下都没有动态标记，否则没必要使用原生的。
 * @since 3.2.0
 * @author Eduardo Macarron
 */
public class RawLanguageDriver extends XMLLanguageDriver {
  //创建sqlSource
  @Override
  public SqlSource createSqlSource(Configuration configuration, XNode script, Class<?> parameterType) {
    SqlSource source = super.createSqlSource(configuration, script, parameterType);
    checkIsNotDynamic(source);
    return source;
  }
  //创建sqlSource
  @Override
  public SqlSource createSqlSource(Configuration configuration, String script, Class<?> parameterType) {
    SqlSource source = super.createSqlSource(configuration, script, parameterType);
    checkIsNotDynamic(source);
    return source;
  }
 //检查是否是动态内容，当使用原生语言的时候动态内容不允许
  private void checkIsNotDynamic(SqlSource source) {
    if (!RawSqlSource.class.equals(source.getClass())) {
      throw new BuilderException("Dynamic content is not allowed when using RAW language");
    }
  }

}
```

#### 4.6.2.1、RawSqlSource

```java
/**
 * 静态的sqlSource，在启动的时候映射文件被加载，所以它比动态sqlSource快
 *
 * @since 3.2.0
 * @author Eduardo Macarron
 */
public class RawSqlSource implements SqlSource {
  //sqlSource
  private final SqlSource sqlSource;
  //构造函数
  public RawSqlSource(Configuration configuration, SqlNode rootSqlNode, Class<?> parameterType) {
    this(configuration, getSql(configuration, rootSqlNode), parameterType);
  }
  //构造函数
  public RawSqlSource(Configuration configuration, String sql, Class<?> parameterType) {
    SqlSourceBuilder sqlSourceParser = new SqlSourceBuilder(configuration);
    Class<?> clazz = parameterType == null ? Object.class : parameterType;
    sqlSource = sqlSourceParser.parse(sql, clazz, new HashMap<>());
  }
  //获取sql从配置文件
  private static String getSql(Configuration configuration, SqlNode rootSqlNode) {
    DynamicContext context = new DynamicContext(configuration, null);
    rootSqlNode.apply(context);
    return context.getSql();
  }
  //获取参数对象的绑定sql
  @Override
  public BoundSql getBoundSql(Object parameterObject) {
    return sqlSource.getBoundSql(parameterObject);
  }

}
```

#### 4.6.2.3、DefaultParameterHandler

```java
/**
 * 默认的参数处理器
 * @author Clinton Begin
 * @author Eduardo Macarron
 */
public class DefaultParameterHandler implements ParameterHandler {
  //类型处理器的注册器
  private final TypeHandlerRegistry typeHandlerRegistry;
  //映射会话
  private final MappedStatement mappedStatement;
  //参数对象
  private final Object parameterObject;
  //绑定sql
  private final BoundSql boundSql;
  //全局配置
  private final Configuration configuration;
  //构造函数
  public DefaultParameterHandler(MappedStatement mappedStatement, Object parameterObject, BoundSql boundSql) {
    this.mappedStatement = mappedStatement;
    this.configuration = mappedStatement.getConfiguration();
    this.typeHandlerRegistry = mappedStatement.getConfiguration().getTypeHandlerRegistry();
    this.parameterObject = parameterObject;
    this.boundSql = boundSql;
  }
  //获取参数对象
  @Override
  public Object getParameterObject() {
    return parameterObject;
  }
  //设置预会话参数
  @Override
  public void setParameters(PreparedStatement ps) {
    //错误上下文设置会话的参数记录
    ErrorContext.instance().activity("setting parameters").object(mappedStatement.getParameterMap().getId());
    //绑定的sql获取参数映射
    List<ParameterMapping> parameterMappings = boundSql.getParameterMappings();
    //参数映射不为空
    if (parameterMappings != null) {
      for (int i = 0; i < parameterMappings.size(); i++) {
        ParameterMapping parameterMapping = parameterMappings.get(i);
        //参数映射的参数风格不是出去
        if (parameterMapping.getMode() != ParameterMode.OUT) {
          Object value;
          String propertyName = parameterMapping.getProperty();
          //判断绑定sql是否有附加的参数
          if (boundSql.hasAdditionalParameter(propertyName)) { // issue #448 ask first for additional params
            value = boundSql.getAdditionalParameter(propertyName);
          } else if (parameterObject == null) {
            value = null;
            //类型处理器注册器是否有参数对象的类型处理器
          } else if (typeHandlerRegistry.hasTypeHandler(parameterObject.getClass())) {
            value = parameterObject;
          } else {
            //获取属性的元对象
            MetaObject metaObject = configuration.newMetaObject(parameterObject);
            value = metaObject.getValue(propertyName);
          }
          //类型处理器
          TypeHandler typeHandler = parameterMapping.getTypeHandler();
          JdbcType jdbcType = parameterMapping.getJdbcType();
          if (value == null && jdbcType == null) {
            jdbcType = configuration.getJdbcTypeForNull();
          }
          //类型处理器给参数会话赋值
          try {
            typeHandler.setParameter(ps, i + 1, value, jdbcType);
          } catch (TypeException | SQLException e) {
            throw new TypeException("Could not set parameters for mapping: " + parameterMapping + ". Cause: " + e, e);
          }
        }
      }
    }
  }

}
```

### 4.6.3、xmltags

![image-20200315094645687](/Users/didi/Library/Application Support/typora-user-images/image-20200315094645687.png)

#### 4.6.3.1、SqlNode

```java
/**
 * sql节点接口
 * @author Clinton Begin
 */
public interface SqlNode {
  //是否应用动态上下文
  boolean apply(DynamicContext context);
}
```

#### 4.6.3.2、TrimSqlNode

```java
/**
 *修正Sql节点
 * @author Clinton Begin
 */
public class TrimSqlNode implements SqlNode {
  //sql节点
  private final SqlNode contents;
  //前缀
  private final String prefix;
  //后缀
  private final String suffix;
  //前缀被重写
  private final List<String> prefixesToOverride;
  //后缀被重写
  private final List<String> suffixesToOverride;
  //全局配置
  private final Configuration configuration;
  //构造函数
  public TrimSqlNode(Configuration configuration, SqlNode contents, String prefix, String prefixesToOverride, String suffix, String suffixesToOverride) {
    this(configuration, contents, prefix, parseOverrides(prefixesToOverride), suffix, parseOverrides(suffixesToOverride));
  }
  //构造函数
  protected TrimSqlNode(Configuration configuration, SqlNode contents, String prefix, List<String> prefixesToOverride, String suffix, List<String> suffixesToOverride) {
    this.contents = contents;
    this.prefix = prefix;
    this.prefixesToOverride = prefixesToOverride;
    this.suffix = suffix;
    this.suffixesToOverride = suffixesToOverride;
    this.configuration = configuration;
  }
  //是否应用动态上下文
  @Override
  public boolean apply(DynamicContext context) {
    FilteredDynamicContext filteredDynamicContext = new FilteredDynamicContext(context);
    boolean result = contents.apply(filteredDynamicContext);
    filteredDynamicContext.applyAll();
    return result;
  }
  //解析重写
  private static List<String> parseOverrides(String overrides) {
    if (overrides != null) {
      final StringTokenizer parser = new StringTokenizer(overrides, "|", false);
      final List<String> list = new ArrayList<>(parser.countTokens());
      while (parser.hasMoreTokens()) {
        list.add(parser.nextToken().toUpperCase(Locale.ENGLISH));
      }
      return list;
    }
    return Collections.emptyList();
  }
//过滤后的动态上下文
  private class FilteredDynamicContext extends DynamicContext {
    //动态上下文
    private DynamicContext delegate;
    //是否是被应用前缀
    private boolean prefixApplied;
    //是否被应用后缀
    private boolean suffixApplied;
    //sql的缓存
    private StringBuilder sqlBuffer;
    //过滤后的动态上下文
    public FilteredDynamicContext(DynamicContext delegate) {
      super(configuration, null);
      this.delegate = delegate;
      this.prefixApplied = false;
      this.suffixApplied = false;
      this.sqlBuffer = new StringBuilder();
    }
    //应用所有
    public void applyAll() {
      sqlBuffer = new StringBuilder(sqlBuffer.toString().trim());
      String trimmedUppercaseSql = sqlBuffer.toString().toUpperCase(Locale.ENGLISH);
      if (trimmedUppercaseSql.length() > 0) {
        applyPrefix(sqlBuffer, trimmedUppercaseSql);
        applySuffix(sqlBuffer, trimmedUppercaseSql);
      }
      delegate.appendSql(sqlBuffer.toString());
    }
    //获取绑定
    @Override
    public Map<String, Object> getBindings() {
      return delegate.getBindings();
    }
    //绑定名字和值对应
    @Override
    public void bind(String name, Object value) {
      delegate.bind(name, value);
    }
    //获取唯一的数值
    @Override
    public int getUniqueNumber() {
      return delegate.getUniqueNumber();
    }
   //追加SQL
    @Override
    public void appendSql(String sql) {
      sqlBuffer.append(sql);
    }
    //获取SQL
    @Override
    public String getSql() {
      return delegate.getSql();
    }
   //应用前缀
    private void applyPrefix(StringBuilder sql, String trimmedUppercaseSql) {
      if (!prefixApplied) {
        prefixApplied = true;
        if (prefixesToOverride != null) {
          for (String toRemove : prefixesToOverride) {
            if (trimmedUppercaseSql.startsWith(toRemove)) {
              sql.delete(0, toRemove.trim().length());
              break;
            }
          }
        }
        if (prefix != null) {
          sql.insert(0, " ");
          sql.insert(0, prefix);
        }
      }
    }
   //应用后缀
    private void applySuffix(StringBuilder sql, String trimmedUppercaseSql) {
      if (!suffixApplied) {
        suffixApplied = true;
        if (suffixesToOverride != null) {
          for (String toRemove : suffixesToOverride) {
            if (trimmedUppercaseSql.endsWith(toRemove) || trimmedUppercaseSql.endsWith(toRemove.trim())) {
              int start = sql.length() - toRemove.trim().length();
              int end = sql.length();
              sql.delete(start, end);
              break;
            }
          }
        }
        if (suffix != null) {
          sql.append(" ");
          sql.append(suffix);
        }
      }
    }

  }

}
```

#### 4.6.3.3、WhereSqlNode

```java
/**
 * where的SQL节点
 * @author Clinton Begin
 */
public class WhereSqlNode extends TrimSqlNode {
  //前缀集合
  private static List<String> prefixList = Arrays.asList("AND ","OR ","AND\n", "OR\n", "AND\r", "OR\r", "AND\t", "OR\t");
  //全局配置，处理SQL节点内容
  public WhereSqlNode(Configuration configuration, SqlNode contents) {
    super(configuration, contents, "WHERE", prefixList, null, null);
  }

}
```

#### 4.6.3.4、SetSqlNode

```java
/**
 * set的SQL节点
 * @author Clinton Begin
 */
public class SetSqlNode extends TrimSqlNode {
 //根据逗号分割集合
  private static final List<String> COMMA = Collections.singletonList(",");
 //全局配置 设置sql节点
  public SetSqlNode(Configuration configuration,SqlNode contents) {
    super(configuration, contents, "SET", COMMA, null, COMMA);
  }

}
```

#### 4.6.3.5、ChooseSqlNode

```java
/**
 * If的sql节点
 * @author Clinton Begin
 */
public class ChooseSqlNode implements SqlNode {
  //sql节点
  private final SqlNode defaultSqlNode;
  //If的sql节点
  private final List<SqlNode> ifSqlNodes;
  //选择sql节点
  public ChooseSqlNode(List<SqlNode> ifSqlNodes, SqlNode defaultSqlNode) {
    this.ifSqlNodes = ifSqlNodes;
    this.defaultSqlNode = defaultSqlNode;
  }
  //是否应用在动态上下文中
  @Override
  public boolean apply(DynamicContext context) {
    for (SqlNode sqlNode : ifSqlNodes) {
      if (sqlNode.apply(context)) {
        return true;
      }
    }
    if (defaultSqlNode != null) {
      defaultSqlNode.apply(context);
      return true;
    }
    return false;
  }
}
```

#### 4.6.3.6、ForEachSqlNode

```java
/**
 * foreach的SQL节点
 * @author Clinton Begin
 */
public class ForEachSqlNode implements SqlNode {
  public static final String ITEM_PREFIX = "__frch_";
  //正则校验
  private final ExpressionEvaluator evaluator;
  //集合正则
  private final String collectionExpression;
  //内容
  private final SqlNode contents;
  //（
  private final String open;
  // ）
  private final String close;
  //分割符
  private final String separator;
  //项
  private final String item;
  //索引
  private final String index;
  //全局配置
  private final Configuration configuration;
  //构造函数
  public ForEachSqlNode(Configuration configuration, SqlNode contents, String collectionExpression, String index, String item, String open, String close, String separator) {
    this.evaluator = new ExpressionEvaluator();
    this.collectionExpression = collectionExpression;
    this.contents = contents;
    this.open = open;
    this.close = close;
    this.separator = separator;
    this.index = index;
    this.item = item;
    this.configuration = configuration;
  }
  //应用动态上下文
  @Override
  public boolean apply(DynamicContext context) {
    Map<String, Object> bindings = context.getBindings();
    //校验集合正则
    final Iterable<?> iterable = evaluator.evaluateIterable(collectionExpression, bindings);
    if (!iterable.iterator().hasNext()) {
      return true;
    }
    boolean first = true;
    //应用 （
    applyOpen(context);
    int i = 0;
    //遍历foreach循环
    for (Object o : iterable) {
      DynamicContext oldContext = context;
      if (first || separator == null) {
        context = new PrefixedContext(context, "");
      } else {
        context = new PrefixedContext(context, separator);
      }
      int uniqueNumber = context.getUniqueNumber();
      // Issue #709
      if (o instanceof Map.Entry) {
        @SuppressWarnings("unchecked")
        Map.Entry<Object, Object> mapEntry = (Map.Entry<Object, Object>) o;
        applyIndex(context, mapEntry.getKey(), uniqueNumber);
        applyItem(context, mapEntry.getValue(), uniqueNumber);
      } else {
        applyIndex(context, i, uniqueNumber);
        applyItem(context, o, uniqueNumber);
      }
      contents.apply(new FilteredDynamicContext(configuration, context, index, item, uniqueNumber));
      if (first) {
        first = !((PrefixedContext) context).isPrefixApplied();
      }
      context = oldContext;
      i++;
    }
    //应用 ）
    applyClose(context);
    //移除item
    context.getBindings().remove(item);
    //移除索引
    context.getBindings().remove(index);
    return true;
  }
  //应用索引
  private void applyIndex(DynamicContext context, Object o, int i) {
    if (index != null) {
      context.bind(index, o);
      context.bind(itemizeItem(index, i), o);
    }
  }
  //应用item
  private void applyItem(DynamicContext context, Object o, int i) {
    if (item != null) {
      context.bind(item, o);
      context.bind(itemizeItem(item, i), o);
    }
  }
   //上下文追加（
  private void applyOpen(DynamicContext context) {
    if (open != null) {
      context.appendSql(open);
    }
  }
  //上下文追加 ）
  private void applyClose(DynamicContext context) {
    if (close != null) {
      context.appendSql(close);
    }
  }
  //item
  private static String itemizeItem(String item, int i) {
    return ITEM_PREFIX + item + "_" + i;
  }
  //被过滤的动态上下文
  private static class FilteredDynamicContext extends DynamicContext {
    //动态上下文
    private final DynamicContext delegate;
    //索引
    private final int index;
    //item索引
    private final String itemIndex;
    //item
    private final String item;
    //构造函数
    public FilteredDynamicContext(Configuration configuration,DynamicContext delegate, String itemIndex, String item, int i) {
      super(configuration, null);
      this.delegate = delegate;
      this.index = i;
      this.itemIndex = itemIndex;
      this.item = item;
    }
   //获取绑定
    @Override
    public Map<String, Object> getBindings() {
      return delegate.getBindings();
    }
    //绑定
    @Override
    public void bind(String name, Object value) {
      delegate.bind(name, value);
    }
   //获取sql
    @Override
    public String getSql() {
      return delegate.getSql();
    }
    //追加sql
    @Override
    public void appendSql(String sql) {
      GenericTokenParser parser = new GenericTokenParser("#{", "}", content -> {
        String newContent = content.replaceFirst("^\\s*" + item + "(?![^.,:\\s])", itemizeItem(item, index));
        if (itemIndex != null && newContent.equals(content)) {
          newContent = content.replaceFirst("^\\s*" + itemIndex + "(?![^.,:\\s])", itemizeItem(itemIndex, index));
        }
        return "#{" + newContent + "}";
      });

      delegate.appendSql(parser.parse(sql));
    }
   //获取唯一的值
    @Override
    public int getUniqueNumber() {
      return delegate.getUniqueNumber();
    }

  }

  //前缀上下文
  private class PrefixedContext extends DynamicContext {
    //动态上下文
    private final DynamicContext delegate;
    //前缀
    private final String prefix;
    //前缀是否被应用
    private boolean prefixApplied;
   //前缀上下文
    public PrefixedContext(DynamicContext delegate, String prefix) {
      super(configuration, null);
      this.delegate = delegate;
      this.prefix = prefix;
      this.prefixApplied = false;
    }
    //是否前缀被应用
    public boolean isPrefixApplied() {
      return prefixApplied;
    }
    //获取绑定
    @Override
    public Map<String, Object> getBindings() {
      return delegate.getBindings();
    }
    //绑定名字和value
    @Override
    public void bind(String name, Object value) {
      delegate.bind(name, value);
    }
    //追加sql
    @Override
    public void appendSql(String sql) {
      if (!prefixApplied && sql != null && sql.trim().length() > 0) {
        delegate.appendSql(prefix);
        prefixApplied = true;
      }
      delegate.appendSql(sql);
    }
    //获取sql
    @Override
    public String getSql() {
      return delegate.getSql();
    }
    //获取唯一的数值
    @Override
    public int getUniqueNumber() {
      return delegate.getUniqueNumber();
    }
  }

}
```

#### 4.6.3.7、IfSqlNode

```java
/**
 * ifSQL节点
 * @author Clinton Begin
 */
public class IfSqlNode implements SqlNode {
  //正则校验
  private final ExpressionEvaluator evaluator;
  //test节点
  private final String test;
  //内容
  private final SqlNode contents;
   //构造函数
  public IfSqlNode(SqlNode contents, String test) {
    this.test = test;
    this.contents = contents;
    this.evaluator = new ExpressionEvaluator();
  }
  //应用校验是否条件通过
  @Override
  public boolean apply(DynamicContext context) {
    if (evaluator.evaluateBoolean(test, context.getBindings())) {
      contents.apply(context);
      return true;
    }
    return false;
  }

}
```

#### 4.6.3.8、TextSqlNode

```java
/**
 * 文件sql节点
 * @author Clinton Begin
 */
public class TextSqlNode implements SqlNode {
  //文本
  private final String text;
  //注入过滤器
  private final Pattern injectionFilter;
  //构造函数
  public TextSqlNode(String text) {
    this(text, null);
  }
  //构造函数
  public TextSqlNode(String text, Pattern injectionFilter) {
    this.text = text;
    this.injectionFilter = injectionFilter;
  }
  //是否是动态的
  public boolean isDynamic() {
    DynamicCheckerTokenParser checker = new DynamicCheckerTokenParser();
    GenericTokenParser parser = createParser(checker);
    parser.parse(text);
    return checker.isDynamic();
  }
  //应用动态上下文
  @Override
  public boolean apply(DynamicContext context) {
    GenericTokenParser parser = createParser(new BindingTokenParser(context, injectionFilter));
    context.appendSql(parser.parse(text));
    return true;
  }
  //解决$注入的问题
  private GenericTokenParser createParser(TokenHandler handler) {
    return new GenericTokenParser("${", "}", handler);
  }
  //静态内部类绑定token解析
  private static class BindingTokenParser implements TokenHandler {
    //动态上下文
    private DynamicContext context;
    //注入过滤器
    private Pattern injectionFilter;

    public BindingTokenParser(DynamicContext context, Pattern injectionFilter) {
      this.context = context;
      this.injectionFilter = injectionFilter;
    }
   //处理token
    @Override
    public String handleToken(String content) {
      Object parameter = context.getBindings().get("_parameter");
      if (parameter == null) {
        context.getBindings().put("value", null);
      } else if (SimpleTypeRegistry.isSimpleType(parameter.getClass())) {
        context.getBindings().put("value", parameter);
      }
      Object value = OgnlCache.getValue(content, context.getBindings());
      String srtValue = value == null ? "" : String.valueOf(value); // issue #274 return "" instead of "null"
      checkInjection(srtValue);
      return srtValue;
    }
    //检查注入
    private void checkInjection(String value) {
      if (injectionFilter != null && !injectionFilter.matcher(value).matches()) {
        throw new ScriptingException("Invalid input. Please conform to regex" + injectionFilter.pattern());
      }
    }
  }
  //动态检查token解析
  private static class DynamicCheckerTokenParser implements TokenHandler {
   //是否是动态的
    private boolean isDynamic;

    public DynamicCheckerTokenParser() {
      // Prevent Synthetic Access
    }
    //是否是动态的
    public boolean isDynamic() {
      return isDynamic;
    }
    //处理token
    @Override
    public String handleToken(String content) {
      this.isDynamic = true;
      return null;
    }
  }

}
```

#### 4.6.3.9、VarDeclSqlNode

```java
/**
 *Var声明的sql节点
 * @author Frank D. Martinez [mnesarco]
 */
public class VarDeclSqlNode implements SqlNode {
  //名字
  private final String name;
  //表达式
  private final String expression;
  //构造函数
  public VarDeclSqlNode(String var, String exp) {
    name = var;
    expression = exp;
  }
  //是否应用动态上下文
  @Override
  public boolean apply(DynamicContext context) {
    final Object value = OgnlCache.getValue(expression, context.getBindings());
    context.bind(name, value);
    return true;
  }

}
```

#### 4.6.3.10、StaticTextSqlNode

```java
/**
 * 静态文本sql节点
 * @author Clinton Begin
 */
public class StaticTextSqlNode implements SqlNode {
  //文本
  private final String text;
  //构造函数
  public StaticTextSqlNode(String text) {
    this.text = text;
  }
  //是否应用动态上下文
  @Override
  public boolean apply(DynamicContext context) {
    context.appendSql(text);
    return true;
  }

}
```

#### 4.6.3.11、MixedSqlNode

```java
/**
 * 混合的SQL节点
 * @author Clinton Begin
 */
public class MixedSqlNode implements SqlNode {
  //内容
  private final List<SqlNode> contents;
  //构造函数
  public MixedSqlNode(List<SqlNode> contents) {
    this.contents = contents;
  }
  //循环遍历应用节点的上下文追加
  @Override
  public boolean apply(DynamicContext context) {
    contents.forEach(node -> node.apply(context));
    return true;
  }
}
```

#### 4.6.3.12、DynamicContext

```java
/**
 * 动态上下文
 * @author Clinton Begin
 */
public class DynamicContext {
  //参数key
  public static final String PARAMETER_OBJECT_KEY = "_parameter";
  //数据库id key
  public static final String DATABASE_ID_KEY = "_databaseId";
  //Ognl运行器设置属性可访问
  static {
    OgnlRuntime.setPropertyAccessor(ContextMap.class, new ContextAccessor());
  }
  //绑定的上下文
  private final ContextMap bindings;
  //数组拼接器
  private final StringJoiner sqlBuilder = new StringJoiner(" ");
  //唯一的数值
  private int uniqueNumber = 0;
  //构造函数
  public DynamicContext(Configuration configuration, Object parameterObject) {
    if (parameterObject != null && !(parameterObject instanceof Map)) {
      MetaObject metaObject = configuration.newMetaObject(parameterObject);
      boolean existsTypeHandler = configuration.getTypeHandlerRegistry().hasTypeHandler(parameterObject.getClass());
      bindings = new ContextMap(metaObject, existsTypeHandler);
    } else {
      bindings = new ContextMap(null, false);
    }
    bindings.put(PARAMETER_OBJECT_KEY, parameterObject);
    bindings.put(DATABASE_ID_KEY, configuration.getDatabaseId());
  }
  //获取绑定的集合
  public Map<String, Object> getBindings() {
    return bindings;
  }
  //绑定名字和值
  public void bind(String name, Object value) {
    bindings.put(name, value);
  }
//追加SQL
  public void appendSql(String sql) {
    sqlBuilder.add(sql);
  }
  //获取SQL
  public String getSql() {
    return sqlBuilder.toString().trim();
  }
  //获取唯一的数值
  public int getUniqueNumber() {
    return uniqueNumber++;
  }
  //上下文集合
  static class ContextMap extends HashMap<String, Object> {
    private static final long serialVersionUID = 2977601501966151582L;
    //参数元对象
    private final MetaObject parameterMetaObject;
    //回滚参数对象
    private final boolean fallbackParameterObject;
   //构造函数
    public ContextMap(MetaObject parameterMetaObject, boolean fallbackParameterObject) {
      this.parameterMetaObject = parameterMetaObject;
      this.fallbackParameterObject = fallbackParameterObject;
    }
    //获取key对应的参数值
    @Override
    public Object get(Object key) {
      String strKey = (String) key;
      if (super.containsKey(strKey)) {
        return super.get(strKey);
      }

      if (parameterMetaObject == null) {
        return null;
      }

      if (fallbackParameterObject && !parameterMetaObject.hasGetter(strKey)) {
        return parameterMetaObject.getOriginalObject();
      } else {
        // issue #61 do not modify the context when reading
        return parameterMetaObject.getValue(strKey);
      }
    }
  }
  //上下文访问
  static class ContextAccessor implements PropertyAccessor {
    //从上下文获取属性对象
    @Override
    public Object getProperty(Map context, Object target, Object name) {
      Map map = (Map) target;

      Object result = map.get(name);
      if (map.containsKey(name) || result != null) {
        return result;
      }

      Object parameterObject = map.get(PARAMETER_OBJECT_KEY);
      if (parameterObject instanceof Map) {
        return ((Map)parameterObject).get(name);
      }

      return null;
    }
   //设置属性
    @Override
    public void setProperty(Map context, Object target, Object name, Object value) {
      Map<Object, Object> map = (Map<Object, Object>) target;
      map.put(name, value);
    }
   //获取源访问
    @Override
    public String getSourceAccessor(OgnlContext arg0, Object arg1, Object arg2) {
      return null;
    }
    //获取源set方法
    @Override
    public String getSourceSetter(OgnlContext arg0, Object arg1, Object arg2) {
      return null;
    }
  }
}
```

#### 4.6.3.13、DynamicSqlSource

```java
/**
 * 动态SQL源
 * @author Clinton Begin
 */
public class DynamicSqlSource implements SqlSource {
  //全局配置
  private final Configuration configuration;
  //root节点
  private final SqlNode rootSqlNode;
  //动态sql源
  public DynamicSqlSource(Configuration configuration, SqlNode rootSqlNode) {
    this.configuration = configuration;
    this.rootSqlNode = rootSqlNode;
  }
   //获取绑定的SQL
  @Override
  public BoundSql getBoundSql(Object parameterObject) {
    DynamicContext context = new DynamicContext(configuration, parameterObject);
    rootSqlNode.apply(context);
    SqlSourceBuilder sqlSourceParser = new SqlSourceBuilder(configuration);
    Class<?> parameterType = parameterObject == null ? Object.class : parameterObject.getClass();
    SqlSource sqlSource = sqlSourceParser.parse(context.getSql(), parameterType, context.getBindings());
    BoundSql boundSql = sqlSource.getBoundSql(parameterObject);
    context.getBindings().forEach(boundSql::setAdditionalParameter);
    return boundSql;
  }

}
```

#### 4.6.3.14、ExpressionEvaluator

```java
/**
 * 正则校验器
 * @author Clinton Begin
 */
public class ExpressionEvaluator {
  //校验是否一致
  public boolean evaluateBoolean(String expression, Object parameterObject) {
    Object value = OgnlCache.getValue(expression, parameterObject);
    if (value instanceof Boolean) {
      return (Boolean) value;
    }
    if (value instanceof Number) {
      return new BigDecimal(String.valueOf(value)).compareTo(BigDecimal.ZERO) != 0;
    }
    return value != null;
  }
  //校验是否是迭代器
  public Iterable<?> evaluateIterable(String expression, Object parameterObject) {
    Object value = OgnlCache.getValue(expression, parameterObject);
    if (value == null) {
      throw new BuilderException("The expression '" + expression + "' evaluated to a null value.");
    }
    if (value instanceof Iterable) {
      return (Iterable<?>) value;
    }
    if (value.getClass().isArray()) {
      // the array may be primitive, so Arrays.asList() may throw
      // a ClassCastException (issue 209).  Do the work manually
      // Curse primitives! :) (JGB)
      int size = Array.getLength(value);
      List<Object> answer = new ArrayList<>();
      for (int i = 0; i < size; i++) {
        Object o = Array.get(value, i);
        answer.add(o);
      }
      return answer;
    }
    if (value instanceof Map) {
      return ((Map) value).entrySet();
    }
    throw new BuilderException("Error evaluating expression '" + expression + "'.  Return value (" + value + ") was not iterable.");
  }

}
```

#### 4.6.3.15、OgnlMemberAccess

```java
/**
 * MemberAccess类基于<a href=
 *  * 'https://github.com/jkuhnert/ognl/blob/OGNL_3_2_1/src/java/ognl/DefaultMemberAccess.java'>DefaultMemberAccess</a>.
 * The {@link MemberAccess} class that based on <a href=
 * 'https://github.com/jkuhnert/ognl/blob/OGNL_3_2_1/src/java/ognl/DefaultMemberAccess.java'>DefaultMemberAccess</a>.
 *
 * @author Kazuki Shimizu
 * @since 3.5.0
 *
 * @see <a href=
 *      'https://github.com/jkuhnert/ognl/blob/OGNL_3_2_1/src/java/ognl/DefaultMemberAccess.java'>DefaultMemberAccess</a>
 * @see <a href='https://github.com/jkuhnert/ognl/issues/47'>#47 of ognl</a>
 */
class OgnlMemberAccess implements MemberAccess {
   //是否可控制访问
  private final boolean canControlMemberAccessible;
   //构造函数
  OgnlMemberAccess() {
    this.canControlMemberAccessible = Reflector.canControlMemberAccessible();
  }
  //设置属性是否可访问
  @Override
  public Object setup(Map context, Object target, Member member, String propertyName) {
    Object result = null;
    if (isAccessible(context, target, member, propertyName)) {
      AccessibleObject accessible = (AccessibleObject) member;
      if (!accessible.isAccessible()) {
        result = Boolean.FALSE;
        accessible.setAccessible(true);
      }
    }
    return result;
  }
  //重新设置
  @Override
  public void restore(Map context, Object target, Member member, String propertyName,
      Object state) {
    if (state != null) {
      ((AccessibleObject) member).setAccessible((Boolean) state);
    }
  }
  //是否可访问
  @Override
  public boolean isAccessible(Map context, Object target, Member member, String propertyName) {
    return canControlMemberAccessible;
  }

}
```

#### 4.6.3.16、OgnlClassResolver

```java
/**
 * 定制的 {@code ClassResolver} 行为比较像{@code DefaultClassResolver}。但是使用{@code Resources}工具类找寻目标类而不是{@code Class#forName(String)}.
 *
 * @author Daniel Guggi
 *
 * @see <a href='https://github.com/mybatis/mybatis-3/issues/161'>Issue 161</a>
 */
public class OgnlClassResolver extends DefaultClassResolver {

  @Override
  protected Class toClassForName(String className) throws ClassNotFoundException {
    return Resources.classForName(className);
  }

}
```

#### 4.6.3.17、XMLLanguageDriver

```java
/**XML语言驱动器
 * @author Eduardo Macarron
 */
public class XMLLanguageDriver implements LanguageDriver {
  //创建参数处理器
  @Override
  public ParameterHandler createParameterHandler(MappedStatement mappedStatement, Object parameterObject, BoundSql boundSql) {
    return new DefaultParameterHandler(mappedStatement, parameterObject, boundSql);
  }
  //创建SQL源
  @Override
  public SqlSource createSqlSource(Configuration configuration, XNode script, Class<?> parameterType) {
    XMLScriptBuilder builder = new XMLScriptBuilder(configuration, script, parameterType);
    return builder.parseScriptNode();
  }
  //创建SQL源
  @Override
  public SqlSource createSqlSource(Configuration configuration, String script, Class<?> parameterType) {
    // issue #3
    if (script.startsWith("<script>")) {
      XPathParser parser = new XPathParser(script, false, configuration.getVariables(), new XMLMapperEntityResolver());
      return createSqlSource(configuration, parser.evalNode("/script"), parameterType);
    } else {
      // issue #127
      script = PropertyParser.parse(script, configuration.getVariables());
      TextSqlNode textSqlNode = new TextSqlNode(script);
      if (textSqlNode.isDynamic()) {
        return new DynamicSqlSource(configuration, textSqlNode);
      } else {
        return new RawSqlSource(configuration, script, parameterType);
      }
    }
  }

}
```

#### 4.6.3.18、XMLScriptBuilder

```java
/**
 * XML脚本构建器
 * @author Clinton Begin
 */
public class XMLScriptBuilder extends BaseBuilder {
  //上下文
  private final XNode context;
  //是否动态
  private boolean isDynamic;
  //参数类型
  private final Class<?> parameterType;
  //节点处理器集合
  private final Map<String, NodeHandler> nodeHandlerMap = new HashMap<>();

  public XMLScriptBuilder(Configuration configuration, XNode context) {
    this(configuration, context, null);
  }
  //构造函数
  public XMLScriptBuilder(Configuration configuration, XNode context, Class<?> parameterType) {
    super(configuration);
    this.context = context;
    this.parameterType = parameterType;
    initNodeHandlerMap();
  }

  //初始化节点集合
  private void initNodeHandlerMap() {
    nodeHandlerMap.put("trim", new TrimHandler());
    nodeHandlerMap.put("where", new WhereHandler());
    nodeHandlerMap.put("set", new SetHandler());
    nodeHandlerMap.put("foreach", new ForEachHandler());
    nodeHandlerMap.put("if", new IfHandler());
    nodeHandlerMap.put("choose", new ChooseHandler());
    nodeHandlerMap.put("when", new IfHandler());
    nodeHandlerMap.put("otherwise", new OtherwiseHandler());
    nodeHandlerMap.put("bind", new BindHandler());
  }
  //解析脚本节点
  public SqlSource parseScriptNode() {
    MixedSqlNode rootSqlNode = parseDynamicTags(context);
    SqlSource sqlSource;
    if (isDynamic) {
      sqlSource = new DynamicSqlSource(configuration, rootSqlNode);
    } else {
      sqlSource = new RawSqlSource(configuration, rootSqlNode, parameterType);
    }
    return sqlSource;
  }
  //解析动态标签
  protected MixedSqlNode parseDynamicTags(XNode node) {
    List<SqlNode> contents = new ArrayList<>();
    NodeList children = node.getNode().getChildNodes();
    for (int i = 0; i < children.getLength(); i++) {
      XNode child = node.newXNode(children.item(i));
      if (child.getNode().getNodeType() == Node.CDATA_SECTION_NODE || child.getNode().getNodeType() == Node.TEXT_NODE) {
        String data = child.getStringBody("");
        TextSqlNode textSqlNode = new TextSqlNode(data);
        if (textSqlNode.isDynamic()) {
          contents.add(textSqlNode);
          isDynamic = true;
        } else {
          contents.add(new StaticTextSqlNode(data));
        }
      } else if (child.getNode().getNodeType() == Node.ELEMENT_NODE) { // issue #628
        String nodeName = child.getNode().getNodeName();
        NodeHandler handler = nodeHandlerMap.get(nodeName);
        if (handler == null) {
          throw new BuilderException("Unknown element <" + nodeName + "> in SQL statement.");
        }
        handler.handleNode(child, contents);
        isDynamic = true;
      }
    }
    return new MixedSqlNode(contents);
  }
  //节点处理器
  private interface NodeHandler {
    void handleNode(XNode nodeToHandle, List<SqlNode> targetContents);
  }
  //绑定处理器
  private class BindHandler implements NodeHandler {
    public BindHandler() {
      // Prevent Synthetic Access
    }

    @Override
    public void handleNode(XNode nodeToHandle, List<SqlNode> targetContents) {
      final String name = nodeToHandle.getStringAttribute("name");
      final String expression = nodeToHandle.getStringAttribute("value");
      final VarDeclSqlNode node = new VarDeclSqlNode(name, expression);
      targetContents.add(node);
    }
  }
  //修整处理器
  private class TrimHandler implements NodeHandler {
    public TrimHandler() {
      // Prevent Synthetic Access
    }

    @Override
    public void handleNode(XNode nodeToHandle, List<SqlNode> targetContents) {
      MixedSqlNode mixedSqlNode = parseDynamicTags(nodeToHandle);
      String prefix = nodeToHandle.getStringAttribute("prefix");
      String prefixOverrides = nodeToHandle.getStringAttribute("prefixOverrides");
      String suffix = nodeToHandle.getStringAttribute("suffix");
      String suffixOverrides = nodeToHandle.getStringAttribute("suffixOverrides");
      TrimSqlNode trim = new TrimSqlNode(configuration, mixedSqlNode, prefix, prefixOverrides, suffix, suffixOverrides);
      targetContents.add(trim);
    }
  }
  //where处理器
  private class WhereHandler implements NodeHandler {
    public WhereHandler() {
      // Prevent Synthetic Access
    }

    @Override
    public void handleNode(XNode nodeToHandle, List<SqlNode> targetContents) {
      MixedSqlNode mixedSqlNode = parseDynamicTags(nodeToHandle);
      WhereSqlNode where = new WhereSqlNode(configuration, mixedSqlNode);
      targetContents.add(where);
    }
  }
  //set的处理器
  private class SetHandler implements NodeHandler {
    public SetHandler() {
      // Prevent Synthetic Access
    }

    @Override
    public void handleNode(XNode nodeToHandle, List<SqlNode> targetContents) {
      MixedSqlNode mixedSqlNode = parseDynamicTags(nodeToHandle);
      SetSqlNode set = new SetSqlNode(configuration, mixedSqlNode);
      targetContents.add(set);
    }
  }
  //foreache处理器
  private class ForEachHandler implements NodeHandler {
    public ForEachHandler() {
      // Prevent Synthetic Access
    }

    @Override
    public void handleNode(XNode nodeToHandle, List<SqlNode> targetContents) {
      MixedSqlNode mixedSqlNode = parseDynamicTags(nodeToHandle);
      String collection = nodeToHandle.getStringAttribute("collection");
      String item = nodeToHandle.getStringAttribute("item");
      String index = nodeToHandle.getStringAttribute("index");
      String open = nodeToHandle.getStringAttribute("open");
      String close = nodeToHandle.getStringAttribute("close");
      String separator = nodeToHandle.getStringAttribute("separator");
      ForEachSqlNode forEachSqlNode = new ForEachSqlNode(configuration, mixedSqlNode, collection, index, item, open, close, separator);
      targetContents.add(forEachSqlNode);
    }
  }
  //if处理器
  private class IfHandler implements NodeHandler {
    public IfHandler() {
      // Prevent Synthetic Access
    }

    @Override
    public void handleNode(XNode nodeToHandle, List<SqlNode> targetContents) {
      MixedSqlNode mixedSqlNode = parseDynamicTags(nodeToHandle);
      String test = nodeToHandle.getStringAttribute("test");
      IfSqlNode ifSqlNode = new IfSqlNode(mixedSqlNode, test);
      targetContents.add(ifSqlNode);
    }
  }
  //Otherwise处理器
  private class OtherwiseHandler implements NodeHandler {
    public OtherwiseHandler() {
      // Prevent Synthetic Access
    }

    @Override
    public void handleNode(XNode nodeToHandle, List<SqlNode> targetContents) {
      MixedSqlNode mixedSqlNode = parseDynamicTags(nodeToHandle);
      targetContents.add(mixedSqlNode);
    }
  }
  //choose处理器
  private class ChooseHandler implements NodeHandler {
    public ChooseHandler() {
      // Prevent Synthetic Access
    }

    @Override
    public void handleNode(XNode nodeToHandle, List<SqlNode> targetContents) {
      List<SqlNode> whenSqlNodes = new ArrayList<>();
      List<SqlNode> otherwiseSqlNodes = new ArrayList<>();
      handleWhenOtherwiseNodes(nodeToHandle, whenSqlNodes, otherwiseSqlNodes);
      SqlNode defaultSqlNode = getDefaultSqlNode(otherwiseSqlNodes);
      ChooseSqlNode chooseSqlNode = new ChooseSqlNode(whenSqlNodes, defaultSqlNode);
      targetContents.add(chooseSqlNode);
    }
  //处理 When   Otherwise 节点
    private void handleWhenOtherwiseNodes(XNode chooseSqlNode, List<SqlNode> ifSqlNodes, List<SqlNode> defaultSqlNodes) {
      List<XNode> children = chooseSqlNode.getChildren();
      for (XNode child : children) {
        String nodeName = child.getNode().getNodeName();
        NodeHandler handler = nodeHandlerMap.get(nodeName);
        if (handler instanceof IfHandler) {
          handler.handleNode(child, ifSqlNodes);
        } else if (handler instanceof OtherwiseHandler) {
          handler.handleNode(child, defaultSqlNodes);
        }
      }
    }
   //获取默认的sql节点
    private SqlNode getDefaultSqlNode(List<SqlNode> defaultSqlNodes) {
      SqlNode defaultSqlNode = null;
      if (defaultSqlNodes.size() == 1) {
        defaultSqlNode = defaultSqlNodes.get(0);
      } else if (defaultSqlNodes.size() > 1) {
        throw new BuilderException("Too many default (otherwise) elements in choose statement.");
      }
      return defaultSqlNode;
    }
  }

}
```

### 4.6.4、小结

* 每个包下有一个呈上启下的类，用于承接业务内容

# 第5章、基础模块层

## 5.1、org.apache.ibatis.annotations注解模块

### 5.1.1、方法类型

#### 5.1.1.1、Delete删除

```java
/**
 * 指定SQL的删除记录注解
 * The annotation that specify an SQL for deleting record(s).
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *   &#064;Delete("DELETE FROM users WHERE id = #{id}")
 *   boolean deleteById(int id);
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Delete {
  /**
   * 返回删除记录的SQL
   * Returns an SQL for deleting record(s).
   *
   * @return an SQL for deleting record(s)
   */
  String[] value();
}
```

#### 5.1.1.2、DeleteProvider删除服务方法

```java
/**
 * 指定一个提供删除记录的SQL方法的注解
 * The annotation that specify a method that provide an SQL for deleting record(s).
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *
 *   &#064;DeleteProvider(type = SqlProvider.class, method = "deleteById")
 *   boolean deleteById(int id);
 *
 *   public static class SqlProvider {
 *     public static String deleteById() {
 *       return "DELETE FROM users WHERE id = #{id}";
 *     }
 *   }
 *
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface DeleteProvider {

  /**
   * 指定实现SQL服务方法的类型
   * Specify a type that implements an SQL provider method.
   *
   * @return a type that implements an SQL provider method
   * @since 3.5.2
   * @see #type()
   */
  Class<?> value() default void.class;

  /**
   * 指定实现SQL服务方法的类型
   * Specify a type that implements an SQL provider method.
   * <p>
   * This attribute is alias of {@link #value()}.
   * </p>
   * @return a type that implements an SQL provider method
   * @see #value()
   */
  Class<?> type() default void.class;

  /**
   * 指定一个提供SQL的方法
   * Specify a method for providing an SQL.
   *
   * <p>
   * Since 3.5.1, this attribute can omit.
   * If this attribute omit, the MyBatis will call a method that decide by following rules.
   * <ul>
   *   <li>
   *     If class that specified the {@link #type()} attribute implements the {@link org.apache.ibatis.builder.annotation.ProviderMethodResolver},
   *     the MyBatis use a method that returned by it
   *   </li>
   *   <li>
   *     If cannot resolve a method by {@link org.apache.ibatis.builder.annotation.ProviderMethodResolver}(= not implement it or it was returned {@code null}),
   *     the MyBatis will search and use a fallback method that named {@code provideSql} from specified type
   *   </li>
   * </ul>
   *
   * @return a method name of method for providing an SQL
   */
  String method() default "";

}
```

#### 5.1.1.3、Select查询

```java
/**
 * 指定检索记录的SQL注解
 * The annotation that specify an SQL for retrieving record(s).
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *   &#064;Select("SELECT id, name FROM users WHERE id = #{id}")
 *   User selectById(int id);
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Select {
  /**
   * 返回检索记录的SQL
   * Returns an SQL for retrieving record(s).
   *
   * @return an SQL for retrieving record(s)
   */
  String[] value();
}
```

#### 5.1.1.4、SelectProvider查询服务方法

```java
/**
 * 指定一个提供检索记录SQL的方法注解
 * The annotation that specify a method that provide an SQL for retrieving record(s).
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *
 *   &#064;SelectProvider(type = SqlProvider.class, method = "selectById")
 *   User selectById(int id);
 *
 *   public static class SqlProvider {
 *     public static String selectById() {
 *       return "SELECT id, name FROM users WHERE id = #{id}";
 *     }
 *   }
 *
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface SelectProvider {

  /**
   * 指定实现SQL服务方法的类型
   * Specify a type that implements an SQL provider method.
   *
   * @return a type that implements an SQL provider method
   * @since 3.5.2
   * @see #type()
   */
  Class<?> value() default void.class;

  /**
   * 指定实现SQL服务方法的类型
   * Specify a type that implements an SQL provider method.
   * <p>
   * This attribute is alias of {@link #value()}.
   * </p>
   *
   * @return a type that implements an SQL provider method
   * @see #value()
   */
  Class<?> type() default void.class;

  /**
   * 指定提供SQL的方法
   * Specify a method for providing an SQL.
   *
   * <p>
   * Since 3.5.1, this attribute can omit.
   * If this attribute omit, the MyBatis will call a method that decide by following rules.
   * <ul>
   *   <li>
   *     If class that specified the {@link #type()} attribute implements the {@link org.apache.ibatis.builder.annotation.ProviderMethodResolver},
   *     the MyBatis use a method that returned by it
   *   </li>
   *   <li>
   *     If cannot resolve a method by {@link org.apache.ibatis.builder.annotation.ProviderMethodResolver}(= not implement it or it was returned {@code null}),
   *     the MyBatis will search and use a fallback method that named {@code provideSql} from specified type
   *   </li>
   * </ul>
   *
   * @return a method name of method for providing an SQL
   */
  String method() default "";

}
```

#### 5.1.1.5、Insert插入

```java
/**
 * 指定一个插入记录的SQL注解
 * The annotation that specify an SQL for inserting record(s).
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *   &#064;Insert("INSERT INTO users (id, name) VALUES(#{id}, #{name})")
 *   void insert(User user);
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Insert {
  /**
   * 返回插入记录的SQL
   * Returns an SQL for inserting record(s).
   *
   * @return an SQL for inserting record(s)
   */
  String[] value();
}
```

#### 5.1.1.6、InsertProvider插入服务方法

```java
/**
 * 指定提供一个插入记录的SQL方法的注解
 * The annotation that specify a method that provide an SQL for inserting record(s).
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *
 *   &#064;InsertProvider(type = SqlProvider.class, method = "insert")
 *   void insert(User user);
 *
 *   public static class SqlProvider {
 *     public static String insert() {
 *       return "INSERT INTO users (id, name) VALUES(#{id}, #{name})";
 *     }
 *   }
 *
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface InsertProvider {

  /**
   * 指定一个实现SQL提供服务的类型
   * Specify a type that implements an SQL provider method.
   *
   * @return a type that implements an SQL provider method
   * @since 3.5.2
   * @see #type()
   */
  Class<?> value() default void.class;

  /**指定一个实现SQL提供服务的类型
   * Specify a type that implements an SQL provider method.
   * <p>
   * This attribute is alias of {@link #value()}.
   * </p>
   *
   * @return a type that implements an SQL provider method
   * @see #value()
   */
  Class<?> type() default void.class;

  /**
   * 指定一个提供SQL的方法
   * Specify a method for providing an SQL.
   *
   * <p>
   * Since 3.5.1, this attribute can omit.
   * If this attribute omit, the MyBatis will call a method that decide by following rules.
   * <ul>
   *   <li>
   *     If class that specified the {@link #type()} attribute implements the {@link org.apache.ibatis.builder.annotation.ProviderMethodResolver},
   *     the MyBatis use a method that returned by it
   *   </li>
   *   <li>
   *     If cannot resolve a method by {@link org.apache.ibatis.builder.annotation.ProviderMethodResolver}(= not implement it or it was returned {@code null}),
   *     the MyBatis will search and use a fallback method that named {@code provideSql} from specified type
   *   </li>
   * </ul>
   *
   * @return a method name of method for providing an SQL
   */
  String method() default "";

}
```

#### 5.1.1.7、Update更新SQL

```java
/**
 * 指定更新记录的SQL注解
 * The annotation that specify an SQL for updating record(s).
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *   &#064;Update("UPDATE users SET name = #{name} WHERE id = #{id}")
 *   boolean update(User user);
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Update {
  /**
   * 返回更新记录的SQL
   * Returns an SQL for updating record(s).
   *
   * @return an SQL for updating record(s)
   */
  String[] value();
}
```

#### 5.1.1.8、UpdateProvider更新服务方法

```java
/**
 * 指定提供更新记录的方法的SQL注解
 * The annotation that specify a method that provide an SQL for updating record(s).
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *
 *   &#064;UpdateProvider(type = SqlProvider.class, method = "update")
 *   boolean update(User user);
 *
 *   public static class SqlProvider {
 *     public static String update() {
 *       return "UPDATE users SET name = #{name} WHERE id = #{id}";
 *     }
 *   }
 *
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface UpdateProvider {

  /**
   * 指定实现SQL服务方法的类型
   * Specify a type that implements an SQL provider method.
   *
   * @return a type that implements an SQL provider method
   * @since 3.5.2
   * @see #type()
   */
  Class<?> value() default void.class;

  /**
   * 指定实现SQL服务方法的类型
   * Specify a type that implements an SQL provider method.
   * <p>
   * This attribute is alias of {@link #value()}.
   * </p>
   *
   * @return a type that implements an SQL provider method
   * @see #value()
   */
  Class<?> type() default void.class;

  /**
   * 指定服务一个SQL的方法
   * Specify a method for providing an SQL.
   *
   * <p>
   * Since 3.5.1, this attribute can omit.
   * If this attribute omit, the MyBatis will call a method that decide by following rules.
   * <ul>
   *   <li>
   *     If class that specified the {@link #type()} attribute implements the {@link org.apache.ibatis.builder.annotation.ProviderMethodResolver},
   *     the MyBatis use a method that returned by it
   *   </li>
   *   <li>
   *     If cannot resolve a method by {@link org.apache.ibatis.builder.annotation.ProviderMethodResolver}(= not implement it or it was returned {@code null}),
   *     the MyBatis will search and use a fallback method that named {@code provideSql} from specified type
   *   </li>
   * </ul>
   *
   * @return a method name of method for providing an SQL
   */
  String method() default "";

}
```

#### 5.1.1.9、Flush强制刷新

```java
/**
 * 这个注解 执行一个刷新会话代理Mapper接口
 * The maker annotation that invoke a flush statements via Mapper interface.
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *   &#064;Flush
 *   List&lt;BatchResult&gt; flush();
 * }
 * </pre>
 * @since 3.3.0
 * @author Kazuki Shimizu
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Flush {
}
```

### 5.1.2、参数，构造器类型

#### 5.1.2.1、Arg参数注解

```java
/**
 * 这个注解是指定一个映射定义为构造参数
 * The annotation that specify a mapping definition for the constructor argument.
 *
 * @see ConstructorArgs
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@Repeatable(ConstructorArgs.class)
public @interface Arg {

  /**
   * 返回ID列 是或者不是
   * Returns whether id column or not.
   *
   * @return {@code true} if id column; {@code false} if otherwise
   */
  boolean id() default false;

  /**
   * 返回映射这个参数的列名或者列标签
   * Return the column name(or column label) to map to this argument.
   *
   * @return the column name(or column label)
   */
  String column() default "";

  /**
   * 返回这个参数的Java类型
   * Return the java type for this argument.
   *
   * @return the java type
   */
  Class<?> javaType() default void.class;

  /**
   * 返回映射这个参数列的jdbc类型
   * Return the jdbc type for column that map to this argument.
   *
   * @return the jdbc type
   */
  JdbcType jdbcType() default JdbcType.UNDEFINED;

  /**
   * 返回从结果集获取列值对应的类型处理器
   * Returns the {@link TypeHandler} type for retrieving a column value from result set.
   *
   * @return the {@link TypeHandler} type
   */
  Class<? extends TypeHandler> typeHandler() default UnknownTypeHandler.class;

  /**
   * 返回会话ID对于映射这个参数的获取对象
   * Return the statement id for retrieving a object that map to this argument.
   *
   * @return the statement id
   */
  String select() default "";

  /**
   * 返回结果 映射 ID 映射到这个参数的对象
   * Returns the result map id for mapping to a object that map to this argument.
   *
   * @return the result map id
   */
  String resultMap() default "";

  /**
   * 返回正在使用这个映射的 参数名字
   * Returns the parameter name for applying this mapping.
   *
   * @return the parameter name
   * @since 3.4.3
   */
  String name() default "";

  /**
   * 返回当使用 {@link #resultMap()}. 的列前缀
   * Returns the column prefix that use when applying {@link #resultMap()}.
   *
   * @return the column prefix
   * @since 3.5.0
   */
  String columnPrefix() default "";
}
```

#### 5.1.2.2、AutomapConstructor自动映射构造器

```java
/**
 * 显示一个自动映射的构造器注解
 * The marker annotation that indicate a constructor for automatic mapping.
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public class User {
 *
 *   private int id;
 *   private String name;
 *
 *   public User(int id) {
 *     this.id = id;
 *   }
 *
 *   &#064;AutomapConstructor
 *   public User(int id, String name) {
 *     this.id = id;
 *     this.name = name;
 *   }
 *   // ...
 * }
 * </pre>
 * @author Tim Chen
 * @since 3.4.3
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.CONSTRUCTOR})
public @interface AutomapConstructor {
}
```

#### 5.1.2.3、ConstructorArgs构造器的参数注解

```java
/**
 * 构造器的组映射定义注解
 * The annotation that be grouping mapping definitions for constructor.
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *   &#064;ConstructorArgs({
 *     &#064;Arg(column = "id", javaType = int.class, id = true),
 *     &#064;Arg(column = "name", javaType = String.class),
 *     &#064;Arg(javaType = UserEmail.class, select = "selectUserEmailById", column = "id")
 *   })
 *   &#064;Select("SELECT id, name FROM users WHERE id = #{id}")
 *   User selectById(int id);
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface ConstructorArgs {
  /**
   * 返回构造器的映射定义
   * Returns mapping definitions for constructor.
   *
   * @return mapping definitions
   */
  Arg[] value() default {};
}
```

#### 5.1.2.4、Params参数注解

```java
/**
 * 指定参数名字的注解
 * The annotation that specify the parameter name.
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *   &#064;Select("SELECT id, name FROM users WHERE name = #{name}")
 *   User selectById(&#064;Param("name") String value);
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.PARAMETER)
public @interface Param {
  /**
   * 返回参数的名字
   * Returns the parameter name.
   *
   * @return the parameter name
   */
  String value();
}
```

5.1.2.5、Property

```java
/**
 * 注入一个属性值的注解
 * The annotation that inject a property value.
 *
 * @since 3.4.2
 * @author Kazuki Shimizu
 * @see CacheNamespace
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({})
public @interface Property {

  /**
   * 返回属性的名字
   * Returns the property name.
   *
   * @return the property name
   */
  String name();

  /**
   * 返回占位符的属性值
   * Returns the property value or placeholder.
   *
   * @return the property value or placeholder
   */
  String value();
}
```

### 5.1.3、缓存

#### 5.1.3.1、CacheNamespace命名或者mapper接口的缓存

```java
/**
 * 这个注解在命名空间使用缓存 或者mapper接口
 * The annotation that specify to use cache on namespace(e.g. mapper interface).
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * &#064;acheNamespace(implementation = CustomCache.class, properties = {
 *   &#064;Property(name = "host", value = "${mybatis.cache.host}"),
 *   &#064;Property(name = "port", value = "${mybatis.cache.port}"),
 *   &#064;Property(name = "name", value = "usersCache")
 * })
 * public interface UserMapper {
 *   // ...
 * }
 * </pre>
 * @author Clinton Begin
 * @author Kazuki Shimizu
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface CacheNamespace {

  /**
   * 返回缓存实现类型使用
   * Returns the cache implementation type to use.
   *
   * @return the cache implementation type
   */
  Class<? extends Cache> implementation() default PerpetualCache.class;

  /**
   * 返回要使用的缓存逐出实现类型
   * Returns the cache evicting implementation type to use.
   *
   * @return the cache evicting implementation type
   */
  Class<? extends Cache> eviction() default LruCache.class;

  /**
   * 返回刷新间隔
   * Returns the flush interval.
   *
   * @return the flush interval
   */
  long flushInterval() default 0;

  /**
   * 返回缓存大小
   * Return the cache size.
   *
   * @return the cache size
   */
  int size() default 1024;

  /**
   * 返回是否使用读写缓存
   * Returns whether use read/write cache.
   *
   * @return {@code true} if use read/write cache; {@code false} if otherwise
   */
  boolean readWrite() default true;

  /**
   * 返回是否在请求时候是否阻塞缓存
   * Returns whether block the cache at request time or not.
   *
   * @return {@code true} if block the cache; {@code false} if otherwise
   */
  boolean blocking() default false;

  /**
   * 返回实现对象的属性值
   * Returns property values for a implementation object.
   *
   * @return property values
   * @since 3.4.2
   */
  Property[] properties() default {};

}
```

#### 5.1.3.2、CacheNamespaceRef引用缓存

```java
/**
 * 引用缓存的注解
 * The annotation that reference a cache.
 * 如果你使用这个注解，应该通过{@link #value()}或者{@link #name()}属性指定。
 * <p>If you use this annotation, should be specified either {@link #value()} or {@link #name()} attribute.
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * &#064;CacheNamespaceRef(UserMapper.class)
 * public interface AdminUserMapper {
 *   // ...
 * }
 * </pre>
 * @author Clinton Begin
 * @author Kazuki Shimizu
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface CacheNamespaceRef {

  /**
   * 返回引用缓存的命名空间类型（这个命名空间变成指定类型的名字）
   * Returns the namespace type to reference a cache (the namespace name become a FQCN of specified type).
   *
   * @return the namespace type to reference a cache
   */
  Class<?> value() default void.class;

  /**
   * 返回引用缓存的命名空间
   * Returns the namespace name to reference a cache.
   *
   * @return the namespace name
   * @since 3.4.2
   */
  String name() default "";
}
```

### 5.1.4、主键生成策略

#### 5.1.4.1、Options定制行为的注解

```java
/**
 * 指定定制默认行为的的可选项注解
 * The annotation that specify options for customizing default behaviors.
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *   &#064;Option(useGeneratedKeys = true, keyProperty = "id")
 *   &#064;Insert("INSERT INTO users (name) VALUES(#{name})")
 *   boolean insert(User user);
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Options {
  /**
   * 刷新缓存的可选项。默认策略
   * The options for the {@link Options#flushCache()}.
   * The default is {@link FlushCachePolicy#DEFAULT}
   */
  enum FlushCachePolicy {
    //查询会话 默认false，插入，更新，删除 默认是true
    /** <code>false</code> for select statement; <code>true</code> for insert/update/delete statement. */
    DEFAULT,
    //无视会话类型 刷新缓存
    /** Flushes cache regardless of the statement type. */
    TRUE,
    //无视会话类型  不刷新缓存
    /** Does not flush cache regardless of the statement type. */
    FALSE
  }

  /**
   * 如果分配缓存返回是否使用二级缓存
   * Returns whether use the 2nd cache feature if assigned the cache.
   *
   * @return {@code true} if use; {@code false} if otherwise
   */
  boolean useCache() default true;

  /**返回二级缓存刷新策略
   * Returns the 2nd cache flush strategy.
   *
   * @return the 2nd cache flush strategy
   */
  FlushCachePolicy flushCache() default FlushCachePolicy.DEFAULT;

  /**
   * 返回结果集类型
   * Returns the result set type.
   *
   * @return the result set type
   */
  ResultSetType resultSetType() default ResultSetType.DEFAULT;

  /**
   * 返回会话类型
   * Return the statement type.
   *
   * @return the statement type
   */
  StatementType statementType() default StatementType.PREPARED;

  /**
   * 返回拉取数量
   * Returns the fetch size.
   *
   * @return the fetch size
   */
  int fetchSize() default -1;

  /**
   * 返回会话超时时间
   * Returns the statement timeout.
   * @return the statement timeout
   */
  int timeout() default -1;

  /**
   * 返回是否使用生成的key,要求：3.0版本jdbc
   * Returns whether use the generated keys feature supported by JDBC 3.0
   *
   * @return {@code true} if use; {@code false} if otherwise
   */
  boolean useGeneratedKeys() default false;

  /**
   * 返回持有key值的属性名字以逗号分割开
   * Returns property names that holds a key value.
   * <p>
   * If you specify multiple property, please separate using comma(',').
   * </p>
   *
   * @return property names that separate with comma(',')
   */
  String keyProperty() default "";

  /**
   * 返回检索key值的列名字
   * Returns column names that retrieves a key value.
   * <p>
   * If you specify multiple column, please separate using comma(',').
   * </p>
   *
   * @return column names that separate with comma(',')
   */
  String keyColumn() default "";

  /**
   * 返回结果集名字，如果多个 以逗号分开返回
   * Returns result set names.
   * <p>
   * If you specify multiple result set, please separate using comma(',').
   * </p>
   *
   * @return result set names that separate with comma(',')
   */
  String resultSets() default "";
}
```

#### 5.1.4.2、SelectKey检索key的注解

```java
/**
 * 指定检索key值的SQL注解
 * The annotation that specify an SQL for retrieving a key value.
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *   &#064;SelectKey(statement = "SELECT identity('users')", keyProperty = "id", before = true, resultType = int.class)
 *   &#064;Insert("INSERT INTO users (id, name) VALUES(#{id}, #{name})")
 *   boolean insert(User user);
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface SelectKey {
  /**
   * 返回检索key值的SQL
   * Returns an SQL for retrieving a key value.
   *
   * @return an SQL for retrieving a key value
   */
  String[] statement();

  /**
   * 返回持有key值的属性名字
   * Returns property names that holds a key value.
   * <p>
   * If you specify multiple property, please separate using comma(',').
   * </p>
   *
   * @return property names that separate with comma(',')
   */
  String keyProperty();

  /**
   * 返回检索key值的列名字
   * Returns column names that retrieves a key value.
   * <p>
   * If you specify multiple column, please separate using comma(',').
   * </p>
   *
   * @return column names that separate with comma(',')
   */
  String keyColumn() default "";

  /**
   * 返回是否检索一个key值在之前插入或者更新会话前
   * Returns whether retrieves a key value before executing insert/update statement.
   *
   * @return {@code true} if execute before; {@code false} if otherwise
   */
  boolean before();

  /**
   * 返回key值类型
   * Returns the key value type.
   *
   * @return the key value type
   */
  Class<?> resultType();

  /**
   * 返回使用的会话类型
   * Returns the statement type to use.
   *
   * @return the statement type
   */
  StatementType statementType() default StatementType.PREPARED;
}
```

### 5.1.5、条件区分

#### 5.1.5.1、Case条件

```java
/**
 * 这个注解是 类型鉴别器的条件映射定义
 * The annotation that conditional mapping definition for {@link TypeDiscriminator}.
 *
 * @see TypeDiscriminator
 * @see Result
 * @see Arg
 * @see Results
 * @see ConstructorArgs
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({})
public @interface Case {

  /**
   * 返回应用这个映射的条件值
   * Return the condition value to apply this mapping.
   *
   * @return the condition value
   */
  String value();

  /**
   * 返回使用这个映射创建对象的对象类型
   * Return the object type that create a object using this mapping.
   *
   * @return the object type
   */
  Class<?> type();

  /**
   * 返回属性的映射定义
   * Return mapping definitions for property.
   *
   * @return mapping definitions for property
   */
  Result[] results() default {};

  /**
   * 返回构造器的映射定义
   * Return mapping definitions for constructor.
   *
   * @return mapping definitions for constructor
   */
  Arg[] constructArgs() default {};

}
```

#### 5.1.5.2、TypeDiscriminator类型鉴别器

```java
/**
 * 分组条件映射的注解
 * The annotation that be grouping conditional mapping definitions.
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *   &#064;Select("SELECT id, name, type FROM users ORDER BY id")
 *   &#064;TypeDiscriminator(
 *     column = "type",
 *     javaType = String.class,
 *     cases = {
 *       &#064;Case(value = "1", type = PremiumUser.class),
 *       &#064;Case(value = "2", type = GeneralUser.class),
 *       &#064;Case(value = "3", type = TemporaryUser.class)
 *     }
 *   )
 *   List&lt;User&gt; selectAll();
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface TypeDiscriminator {

  /**
   * 返回持有条件值的列字段
   * Returns the column name(column label) that hold conditional value.
   *
   * @return the column name(column label)
   */
  String column();

  /**
   * 返回条件值的Java类型
   * Return the java type for conditional value.
   *
   * @return the java type
   */
  Class<?> javaType() default void.class;

  /**
   * 返回持有条件值的jdbc类型的列
   * Return the jdbc type for column that hold conditional value.
   *
   * @return the jdbc type
   */
  JdbcType jdbcType() default JdbcType.UNDEFINED;

  /**
   * 返回从结果集检索一个列值的类型处理器的类型
   * Returns the {@link TypeHandler} type for retrieving a column value from result set.
   *
   * @return the {@link TypeHandler} type
   */
  Class<? extends TypeHandler> typeHandler() default UnknownTypeHandler.class;

  /**
   * 返回有条件的映射定义
   * Returns conditional mapping definitions.
   *
   * @return conditional mapping definitions
   */
  Case[] cases();
}
```

### 5.1.6、返回结果

#### 5.1.6.1、Many检索集合的嵌套会话

```java
/**
 * 指定嵌套检索集合的会话注解
 * The annotation that specify the nested statement for retrieving collections.
 *
 * @see Result
 * @see Results
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({})
public @interface Many {
  /**
   * 返回检索集合的会话ID
   * Returns the statement id that retrieves collection.
   *
   * @return the statement id
   */
  String select() default "";

  /**
   * 返回嵌套会话的拉取策略
   * Returns the fetch strategy for nested statement.
   *
   * @return the fetch strategy
   */
  FetchType fetchType() default FetchType.DEFAULT;

}
```

#### 5.1.6.2、MapKey返回map

```java
/**
 * 指定属性名字或者列名为map的注解
 * The annotation that specify the property name(or column name) for a key value of {@link java.util.Map}.
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *   &#064;MapKey("id")
 *   &#064;Select("SELECT id, name FROM users WHERE name LIKE #{name} || '%")
 *   Map&lt;Integer, User&gt; selectByStartingWithName(String name);
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface MapKey {
  /**
   * 返回作为键值的属性名字或者列名
   * Returns the property name(or column name) for a key value of {@link java.util.Map}.
   *
   * @return the property name(or column name)
   */
  String value();
}
```

#### 5.1.6.3、One单条数据

```java
/**
 * 指定检索单个对象的嵌套会话的注解
 * The annotation that specify the nested statement for retrieving single object.
 *
 * @see Result
 * @see Results
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({})
public @interface One {
  /**
   * 返回检索单个对象的会话ID
   * Returns the statement id that retrieves single object.
   *
   * @return the statement id
   */
  String select() default "";

  /**
   * 返回嵌套会话的拉取策略
   * Returns the fetch strategy for nested statement.
   *
   * @return the fetch strategy
   */
  FetchType fetchType() default FetchType.DEFAULT;

}
```

#### 5.1.6.4、Result属性的映射

```java
/**
 * 指定属性的映射定义注解
 * The annotation that specify a mapping definition for the property.
 *
 * @see Results
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@Repeatable(Results.class)
public @interface Result {
  /**
   * 返回是否ID列或者不是
   * Returns whether id column or not.
   *
   * @return {@code true} if id column; {@code false} if otherwise
   */
  boolean id() default false;

  /**
   * 返回映射到这个参数的列名
   * Return the column name(or column label) to map to this argument.
   *
   * @return the column name(or column label)
   */
  String column() default "";

  /**
   * 返回使用这个映射的属性名字
   * Returns the property name for applying this mapping.
   *
   * @return the property name
   */
  String property() default "";

  /**
   * 返回参数的Java类型
   * Return the java type for this argument.
   *
   * @return the java type
   */
  Class<?> javaType() default void.class;

  /**
   * 返回映射到这个参数的列的jdbc类型
   * Return the jdbc type for column that map to this argument.
   *
   * @return the jdbc type
   */
  JdbcType jdbcType() default JdbcType.UNDEFINED;

  /**
   * 返回检索一个列值从结果集的类型处理器
   * Returns the {@link TypeHandler} type for retrieving a column value from result set.
   *
   * @return the {@link TypeHandler} type
   */
  Class<? extends TypeHandler> typeHandler() default UnknownTypeHandler.class;

  /**
   * 返回单个关系的映射定义
   * Returns the mapping definition for single relationship.
   *
   * @return the mapping definition for single relationship
   */
  One one() default @One;

  /**
   * 返回集合关系的映射定义
   * Returns the mapping definition for collection relationship.
   *
   * @return the mapping definition for collection relationship
   */
  Many many() default @Many;
}
```

#### 5.1.6.5、ResultMap结果映射集合

```java
/**
 * 指定使用的结果映射名字
 * The annotation that specify result map names to use.
 *
 * <p><br>
 * <b>How to use:</b><br>
 * Mapper interface:
 * <pre>
 * public interface UserMapper {
 *   &#064;Select("SELECT id, name FROM users WHERE id = #{id}")
 *   &#064;ResultMap("userMap")
 *   User selectById(int id);
 *
 *   &#064;Select("SELECT u.id, u.name FROM users u INNER JOIN users_email ue ON u.id = ue.id WHERE ue.email = #{email}")
 *   &#064;ResultMap("userMap")
 *   User selectByEmail(String email);
 * }
 * </pre>
 * Mapper XML:
 * <pre>{@code
 * <mapper namespace="com.example.mapper.UserMapper">
 *   <resultMap id="userMap" type="com.example.model.User">
 *     <id property="id" column="id" />
 *     <result property="name" column="name" />
 *     <association property="email" select="selectUserEmailById" column="id" fetchType="lazy"/>
 *   </resultMap>
 * </mapper>
 * }
 * </pre>
 * @author Jeff Butler
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface ResultMap {
  /**
   * 返回使用的结果映射名字
   * Returns result map names to use.
   *
   * @return result map names
   */
  String[] value();
}
```

#### 5.1.6.6、Results分组属性映射定义

```java
/**
 * 分组属性映射定义的注解
 * The annotation that be grouping mapping definitions for property.
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *   &#064;Results({
 *     &#064;Result(property = "id", column = "id", id = true),
 *     &#064;Result(property = "name", column = "name"),
 *     &#064;Result(property = "email" column = "id", one = @One(select = "selectUserEmailById", fetchType = FetchType.LAZY)),
 *     &#064;Result(property = "telephoneNumbers" column = "id", many = @Many(select = "selectAllUserTelephoneNumberById", fetchType = FetchType.LAZY))
 *   })
 *   &#064;Select("SELECT id, name FROM users WHERE id = #{id}")
 *   User selectById(int id);
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Results {
  /**
   * 返回结果集的ID
   * Returns the id of this result map.
   *
   * @return the id of this result map
   */
  String id() default "";

  /**
   * 返回属性映射定义
   * Returns mapping definitions for property.
   *
   * @return mapping definitions
   */
  Result[] value() default {};
}
```

#### 5.1.6.7、ResultType查询返回的结果类型

```java
/**
 * 当@Select方法使用结果处理器的时候，这个注解可以被使用。那些方法一定有不需要返回的类型，因此这个注解可以用来告诉mybatis每行的对象应该是构建成什么样子
 * This annotation can be used when a @Select method is using a
 * ResultHandler.  Those methods must have void return type, so
 * this annotation can be used to tell MyBatis what kind of object
 * it should build for each row.
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *   &#064;ResultType(User.class)
 *   &#064;Select("SELECT id, name FROM users WHERE name LIKE #{name} || '%' ORDER BY id")
 *   void collectByStartingWithName(String name, ResultHandler&lt;User&gt; handler);
 * }
 * </pre>
 * @since 3.2.0
 * @author Jeff Butler
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface ResultType {
  /**
   * 返回返回类型
   * Returns the return type.
   *
   * @return the return type
   */
  Class<?> value();
}
```

### 5.1.7、接口和驱动

#### 5.1.7.1、Lang语言驱动

```java
/**
 * 指定使用语言驱动的注解
 * The annotation that specify a {@link LanguageDriver} to use.
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * public interface UserMapper {
 *   &#064;Lang(MyXMLLanguageDriver.class)
 *   &#064;Select("SELECT id, name FROM users WHERE id = #{id}")
 *   User selectById(int id);
 * }
 * </pre>
 * @author Clinton Begin
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Lang {
  /**
   * 返回使用的驱动类型的类型
   * Returns the {@link LanguageDriver} implementation type to use.
   *
   * @return the {@link LanguageDriver} implementation type
   */
  Class<? extends LanguageDriver> value();
}
```

#### 5.1.7.2、Mapper接口

```java
/**
 * MyBatis mappers的接口
 * Marker interface for MyBatis mappers.
 *
 * <p><br>
 * <b>How to use:</b>
 * <pre>
 * &#064;Mapper
 * public interface UserMapper {
 *   // ...
 * }
 * </pre>
 * @author Frank David Martínez
 */
@Documented
@Inherited
@Retention(RetentionPolicy.RUNTIME)
@Target({ ElementType.TYPE, ElementType.METHOD, ElementType.FIELD, ElementType.PARAMETER })
public @interface Mapper {
  // Interface Mapper
}
```

## 5.2、org.apache.ibatis.binding绑定模块

### 5.2.1、BindingException

```java
/**
 * 绑定异常
 * @author Clinton Begin
 */
public class BindingException extends PersistenceException {

  private static final long serialVersionUID = 4300802238789381562L;
  //空构造函数
  public BindingException() {
    super();
  }
  //传递信息的构造函数
  public BindingException(String message) {
    super(message);
  }
  //传递信息和throwable的构造函数
  public BindingException(String message, Throwable cause) {
    super(message, cause);
  }
  //传递throwable的构造函数
  public BindingException(Throwable cause) {
    super(cause);
  }
}
```

### 5.2.2、MapperMethod方法

```java
/**
 * Mapper的方法
 * @author Clinton Begin
 * @author Eduardo Macarron
 * @author Lasse Voss
 * @author Kazuki Shimizu
 */
public class MapperMethod {
  //命令
  private final SqlCommand command;
  //方法签名
  private final MethodSignature method;
  //映射的接口和方法和全局配置
  public MapperMethod(Class<?> mapperInterface, Method method, Configuration config) {
    this.command = new SqlCommand(config, mapperInterface, method);
    this.method = new MethodSignature(config, mapperInterface, method);
  }
  //执行sqlSession和参数
  public Object execute(SqlSession sqlSession, Object[] args) {
    Object result;
    switch (command.getType()) {
      //插入
      case INSERT: {
        Object param = method.convertArgsToSqlCommandParam(args);
        result = rowCountResult(sqlSession.insert(command.getName(), param));
        break;
      }
      //更新
      case UPDATE: {
        Object param = method.convertArgsToSqlCommandParam(args);
        result = rowCountResult(sqlSession.update(command.getName(), param));
        break;
      }
      //删除
      case DELETE: {
        Object param = method.convertArgsToSqlCommandParam(args);
        result = rowCountResult(sqlSession.delete(command.getName(), param));
        break;
      }
      //查询
      case SELECT:
        //无返回值
        if (method.returnsVoid() && method.hasResultHandler()) {
          executeWithResultHandler(sqlSession, args);
          result = null;
          //返回多条结果
        } else if (method.returnsMany()) {
          result = executeForMany(sqlSession, args);
          //返回map
        } else if (method.returnsMap()) {
          result = executeForMap(sqlSession, args);
          //返回游标结果集
        } else if (method.returnsCursor()) {
          result = executeForCursor(sqlSession, args);
        } else {
          //返回值类型
          Object param = method.convertArgsToSqlCommandParam(args);
          result = sqlSession.selectOne(command.getName(), param);
          if (method.returnsOptional()
              && (result == null || !method.getReturnType().equals(result.getClass()))) {
            result = Optional.ofNullable(result);
          }
        }
        break;
        //刷新
      case FLUSH:
        result = sqlSession.flushStatements();
        break;
      default:
        throw new BindingException("Unknown execution method for: " + command.getName());
    }
    if (result == null && method.getReturnType().isPrimitive() && !method.returnsVoid()) {
      throw new BindingException("Mapper method '" + command.getName()
          + " attempted to return null from a method with a primitive return type (" + method.getReturnType() + ").");
    }
    return result;
  }
  //行数结果
  private Object rowCountResult(int rowCount) {
    final Object result;
    //无返回值
    if (method.returnsVoid()) {
      result = null;
      //int返回类型
    } else if (Integer.class.equals(method.getReturnType()) || Integer.TYPE.equals(method.getReturnType())) {
      result = rowCount;
      //long返回类型
    } else if (Long.class.equals(method.getReturnType()) || Long.TYPE.equals(method.getReturnType())) {
      result = (long)rowCount;
      //boolean返回类型
    } else if (Boolean.class.equals(method.getReturnType()) || Boolean.TYPE.equals(method.getReturnType())) {
      result = rowCount > 0;
    } else {
      //其他不支持
      throw new BindingException("Mapper method '" + command.getName() + "' has an unsupported return type: " + method.getReturnType());
    }
    return result;
  }
 //携带结果处理器执行
  private void executeWithResultHandler(SqlSession sqlSession, Object[] args) {
    MappedStatement ms = sqlSession.getConfiguration().getMappedStatement(command.getName());
    if (!StatementType.CALLABLE.equals(ms.getStatementType())
        && void.class.equals(ms.getResultMaps().get(0).getType())) {
      throw new BindingException("method " + command.getName()
          + " needs either a @ResultMap annotation, a @ResultType annotation,"
          + " or a resultType attribute in XML so a ResultHandler can be used as a parameter.");
    }
    //转换参数成SQL命令参数
    Object param = method.convertArgsToSqlCommandParam(args);
    //是否有分页查询
    if (method.hasRowBounds()) {
      RowBounds rowBounds = method.extractRowBounds(args);
      sqlSession.select(command.getName(), param, rowBounds, method.extractResultHandler(args));
    } else {
      sqlSession.select(command.getName(), param, method.extractResultHandler(args));
    }
  }
  //执行查询许多条记录
  private <E> Object executeForMany(SqlSession sqlSession, Object[] args) {
    List<E> result;
    //转换成SQL命令行参数
    Object param = method.convertArgsToSqlCommandParam(args);
    //是否分页查询
    if (method.hasRowBounds()) {
      RowBounds rowBounds = method.extractRowBounds(args);
      result = sqlSession.selectList(command.getName(), param, rowBounds);
      //单次查询
    } else {
      result = sqlSession.selectList(command.getName(), param);
    }
    //集合和数组类型的支持
    // issue #510 Collections & arrays support
    if (!method.getReturnType().isAssignableFrom(result.getClass())) {
      if (method.getReturnType().isArray()) {
        return convertToArray(result);
      } else {
        return convertToDeclaredCollection(sqlSession.getConfiguration(), result);
      }
    }
    return result;
  }
  //执行查询分页查询带有游标
  private <T> Cursor<T> executeForCursor(SqlSession sqlSession, Object[] args) {
    Cursor<T> result;
    Object param = method.convertArgsToSqlCommandParam(args);
    //是否分页
    if (method.hasRowBounds()) {
      RowBounds rowBounds = method.extractRowBounds(args);
      result = sqlSession.selectCursor(command.getName(), param, rowBounds);
    } else {
      result = sqlSession.selectCursor(command.getName(), param);
    }
    return result;
  }
  //将方法返回类型转换声明的集合
  private <E> Object convertToDeclaredCollection(Configuration config, List<E> list) {
    Object collection = config.getObjectFactory().create(method.getReturnType());
    MetaObject metaObject = config.newMetaObject(collection);
    metaObject.addAll(list);
    return collection;
  }
  //转成数组类型
  @SuppressWarnings("unchecked")
  private <E> Object convertToArray(List<E> list) {
    Class<?> arrayComponentType = method.getReturnType().getComponentType();
    Object array = Array.newInstance(arrayComponentType, list.size());
    if (arrayComponentType.isPrimitive()) {
      for (int i = 0; i < list.size(); i++) {
        Array.set(array, i, list.get(i));
      }
      return array;
    } else {
      return list.toArray((E[])array);
    }
  }
  //执行返回map类型查询
  private <K, V> Map<K, V> executeForMap(SqlSession sqlSession, Object[] args) {
    Map<K, V> result;
    Object param = method.convertArgsToSqlCommandParam(args);
    if (method.hasRowBounds()) {
      RowBounds rowBounds = method.extractRowBounds(args);
      result = sqlSession.selectMap(command.getName(), param, method.getMapKey(), rowBounds);
    } else {
      result = sqlSession.selectMap(command.getName(), param, method.getMapKey());
    }
    return result;
  }
  //参数映射
  public static class ParamMap<V> extends HashMap<String, V> {

    private static final long serialVersionUID = -2212268410512043556L;
   //获取某个value
    @Override
    public V get(Object key) {
      if (!super.containsKey(key)) {
        throw new BindingException("Parameter '" + key + "' not found. Available parameters are " + keySet());
      }
      return super.get(key);
    }

  }
  //sql命令行
  public static class SqlCommand {
    //名字
    private final String name;
    //类型
    private final SqlCommandType type;
    //构造函数
    public SqlCommand(Configuration configuration, Class<?> mapperInterface, Method method) {
      final String methodName = method.getName();
      final Class<?> declaringClass = method.getDeclaringClass();
      MappedStatement ms = resolveMappedStatement(mapperInterface, methodName, declaringClass,
          configuration);
      //处理映射的会话
      if (ms == null) {
        if (method.getAnnotation(Flush.class) != null) {
          name = null;
          type = SqlCommandType.FLUSH;
        } else {
          throw new BindingException("Invalid bound statement (not found): "
              + mapperInterface.getName() + "." + methodName);
        }
      } else {
        name = ms.getId();
        type = ms.getSqlCommandType();
        if (type == SqlCommandType.UNKNOWN) {
          throw new BindingException("Unknown execution method for: " + name);
        }
      }
    }
    //获取名字
    public String getName() {
      return name;
    }
    //获取类型
    public SqlCommandType getType() {
      return type;
    }
    //解析映射的会话
    private MappedStatement resolveMappedStatement(Class<?> mapperInterface, String methodName,
        Class<?> declaringClass, Configuration configuration) {
      String statementId = mapperInterface.getName() + "." + methodName;
      if (configuration.hasStatement(statementId)) {
        return configuration.getMappedStatement(statementId);
      } else if (mapperInterface.equals(declaringClass)) {
        return null;
      }
      for (Class<?> superInterface : mapperInterface.getInterfaces()) {
        if (declaringClass.isAssignableFrom(superInterface)) {
          MappedStatement ms = resolveMappedStatement(superInterface, methodName,
              declaringClass, configuration);
          if (ms != null) {
            return ms;
          }
        }
      }
      return null;
    }
  }
  //方法签名
  public static class MethodSignature {
   //是否返回许多条数
    private final boolean returnsMany;
    //是否返回map
    private final boolean returnsMap;
    //是否无返回值
    private final boolean returnsVoid;
    //是否返回游标集
    private final boolean returnsCursor;
    //是否返回可选项
    private final boolean returnsOptional;
    //返回类型
    private final Class<?> returnType;
    //映射key
    private final String mapKey;
    //结果处理索引
    private final Integer resultHandlerIndex;
    //行绑定索引
    private final Integer rowBoundsIndex;
    //参数名字解析器
    private final ParamNameResolver paramNameResolver;
    //方法签名构造器
    public MethodSignature(Configuration configuration, Class<?> mapperInterface, Method method) {
      Type resolvedReturnType = TypeParameterResolver.resolveReturnType(method, mapperInterface);
      if (resolvedReturnType instanceof Class<?>) {
        this.returnType = (Class<?>) resolvedReturnType;
      } else if (resolvedReturnType instanceof ParameterizedType) {
        this.returnType = (Class<?>) ((ParameterizedType) resolvedReturnType).getRawType();
      } else {
        this.returnType = method.getReturnType();
      }
      this.returnsVoid = void.class.equals(this.returnType);
      this.returnsMany = configuration.getObjectFactory().isCollection(this.returnType) || this.returnType.isArray();
      this.returnsCursor = Cursor.class.equals(this.returnType);
      this.returnsOptional = Optional.class.equals(this.returnType);
      this.mapKey = getMapKey(method);
      this.returnsMap = this.mapKey != null;
      this.rowBoundsIndex = getUniqueParamIndex(method, RowBounds.class);
      this.resultHandlerIndex = getUniqueParamIndex(method, ResultHandler.class);
      this.paramNameResolver = new ParamNameResolver(configuration, method);
    }
    //转换参数成SQL命令参数
    public Object convertArgsToSqlCommandParam(Object[] args) {
      return paramNameResolver.getNamedParams(args);
    }
    //是否还有行索引
    public boolean hasRowBounds() {
      return rowBoundsIndex != null;
    }
    //提取行数
    public RowBounds extractRowBounds(Object[] args) {
      return hasRowBounds() ? (RowBounds) args[rowBoundsIndex] : null;
    }
    //是否有结果处理器
    public boolean hasResultHandler() {
      return resultHandlerIndex != null;
    }
    //提取结果处理器
    public ResultHandler extractResultHandler(Object[] args) {
      return hasResultHandler() ? (ResultHandler) args[resultHandlerIndex] : null;
    }
    //获取mapKey
    public String getMapKey() {
      return mapKey;
    }
    //获取返回类型
    public Class<?> getReturnType() {
      return returnType;
    }
    //返回所有
    public boolean returnsMany() {
      return returnsMany;
    }
    //返回map
    public boolean returnsMap() {
      return returnsMap;
    }
    //无返回
    public boolean returnsVoid() {
      return returnsVoid;
    }
   //返回游标集
    public boolean returnsCursor() {
      return returnsCursor;
    }

    /**
     * 返回类型是否是 {@code java.util.Optional}.
     * return whether return type is {@code java.util.Optional}.
     * @return return {@code true}, if return type is {@code java.util.Optional}
     * @since 3.5.0
     */
    public boolean returnsOptional() {
      return returnsOptional;
    }
    //返回唯一的参数索引
    private Integer getUniqueParamIndex(Method method, Class<?> paramType) {
      Integer index = null;
      final Class<?>[] argTypes = method.getParameterTypes();
      for (int i = 0; i < argTypes.length; i++) {
        if (paramType.isAssignableFrom(argTypes[i])) {
          if (index == null) {
            index = i;
          } else {
            throw new BindingException(method.getName() + " cannot have multiple " + paramType.getSimpleName() + " parameters");
          }
        }
      }
      return index;
    }
   //根据方法获取map的key
    private String getMapKey(Method method) {
      String mapKey = null;
      if (Map.class.isAssignableFrom(method.getReturnType())) {
        final MapKey mapKeyAnnotation = method.getAnnotation(MapKey.class);
        if (mapKeyAnnotation != null) {
          mapKey = mapKeyAnnotation.value();
        }
      }
      return mapKey;
    }
  }

}
```

### 5.2.3、MapperProxy代理

```java
/**
 * Mapper接口代理--真正的mapper执行类
 * @author Clinton Begin
 * @author Eduardo Macarron
 */
public class MapperProxy<T> implements InvocationHandler, Serializable {

  private static final long serialVersionUID = -4724728412955527868L;
  //允许的方法
  private static final int ALLOWED_MODES = MethodHandles.Lookup.PRIVATE | MethodHandles.Lookup.PROTECTED
      | MethodHandles.Lookup.PACKAGE | MethodHandles.Lookup.PUBLIC;
  //查询构造
  private static final Constructor<Lookup> lookupConstructor;
  //私有方法
  private static final Method privateLookupInMethod;
  //会话
  private final SqlSession sqlSession;
  //mapper接口
  private final Class<T> mapperInterface;
  //方法缓存
  private final Map<Method, MapperMethodInvoker> methodCache;
  //构造函数
  public MapperProxy(SqlSession sqlSession, Class<T> mapperInterface, Map<Method, MapperMethodInvoker> methodCache) {
    this.sqlSession = sqlSession;
    this.mapperInterface = mapperInterface;
    this.methodCache = methodCache;
  }
  //静态代码初始化
  static {
    Method privateLookupIn;
    try {
      //私有方法的访问权限
      privateLookupIn = MethodHandles.class.getMethod("privateLookupIn", Class.class, MethodHandles.Lookup.class);
    } catch (NoSuchMethodException e) {
      privateLookupIn = null;
    }
    privateLookupInMethod = privateLookupIn;

    Constructor<Lookup> lookup = null;
    if (privateLookupInMethod == null) {

      //jdk 1.8支持，直接根据类获取声明的构造函数
      // JDK 1.8
      try {
        lookup = MethodHandles.Lookup.class.getDeclaredConstructor(Class.class, int.class);
        lookup.setAccessible(true);
      } catch (NoSuchMethodException e) {
        throw new IllegalStateException(
            "There is neither 'privateLookupIn(Class, Lookup)' nor 'Lookup(Class, int)' method in java.lang.invoke.MethodHandles.",
            e);
      } catch (Exception e) {
        lookup = null;
      }
    }
    lookupConstructor = lookup;
  }
  //mapper接口的执行
  @Override
  public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    try {
      //object类直接执行
      if (Object.class.equals(method.getDeclaringClass())) {
        return method.invoke(this, args);
        //缓存的执行器执行
      } else {
        return cachedInvoker(proxy, method, args).invoke(proxy, method, args, sqlSession);
      }
    } catch (Throwable t) {
      throw ExceptionUtil.unwrapThrowable(t);
    }
  }
  //缓存的执行器
  private MapperMethodInvoker cachedInvoker(Object proxy, Method method, Object[] args) throws Throwable {
    try {
      return methodCache.computeIfAbsent(method, m -> {
        if (m.isDefault()) {
          try {
            //jdk8和jdk9的默认方法执行
            if (privateLookupInMethod == null) {
              return new DefaultMethodInvoker(getMethodHandleJava8(method));
            } else {
              return new DefaultMethodInvoker(getMethodHandleJava9(method));
            }
          } catch (IllegalAccessException | InstantiationException | InvocationTargetException
              | NoSuchMethodException e) {
            throw new RuntimeException(e);
          }
        } else {
          //返回声明的方法执行器
          return new PlainMethodInvoker(new MapperMethod(mapperInterface, method, sqlSession.getConfiguration()));
        }
      });
    } catch (RuntimeException re) {
      Throwable cause = re.getCause();
      throw cause == null ? re : cause;
    }
  }
 //Java9获取方法处理器
  private MethodHandle getMethodHandleJava9(Method method)
      throws NoSuchMethodException, IllegalAccessException, InvocationTargetException {
    final Class<?> declaringClass = method.getDeclaringClass();
    return ((Lookup) privateLookupInMethod.invoke(null, declaringClass, MethodHandles.lookup())).findSpecial(
        declaringClass, method.getName(), MethodType.methodType(method.getReturnType(), method.getParameterTypes()),
        declaringClass);
  }
 //Java8获取方法处理器
  private MethodHandle getMethodHandleJava8(Method method)
      throws IllegalAccessException, InstantiationException, InvocationTargetException {
    final Class<?> declaringClass = method.getDeclaringClass();
    return lookupConstructor.newInstance(declaringClass, ALLOWED_MODES).unreflectSpecial(method, declaringClass);
  }
  //Mapper方法执行器接口
  interface MapperMethodInvoker {
    Object invoke(Object proxy, Method method, Object[] args, SqlSession sqlSession) throws Throwable;
  }
  //声明的方法执行器
  private static class PlainMethodInvoker implements MapperMethodInvoker {
    private final MapperMethod mapperMethod;

    public PlainMethodInvoker(MapperMethod mapperMethod) {
      super();
      this.mapperMethod = mapperMethod;
    }
    //执行SQLsession
    @Override
    public Object invoke(Object proxy, Method method, Object[] args, SqlSession sqlSession) throws Throwable {
      return mapperMethod.execute(sqlSession, args);
    }
  }
  //默认的方法执行器
  private static class DefaultMethodInvoker implements MapperMethodInvoker {
    private final MethodHandle methodHandle;

    public DefaultMethodInvoker(MethodHandle methodHandle) {
      super();
      this.methodHandle = methodHandle;
    }
   //绑定的代理执行
    @Override
    public Object invoke(Object proxy, Method method, Object[] args, SqlSession sqlSession) throws Throwable {
      return methodHandle.bindTo(proxy).invokeWithArguments(args);
    }
  }
}
```

### 5.2.4、MapperProxyFactory代理工厂

```java
/**
 * Mapper代理工厂
 * @author Lasse Voss
 */
public class MapperProxyFactory<T> {
  //mapper接口
  private final Class<T> mapperInterface;
  //方法缓存
  private final Map<Method, MapperMethodInvoker> methodCache = new ConcurrentHashMap<>();
  //构造函数
  public MapperProxyFactory(Class<T> mapperInterface) {
    this.mapperInterface = mapperInterface;
  }
  //获取mapper接口
  public Class<T> getMapperInterface() {
    return mapperInterface;
  }
  //获取方法缓存
  public Map<Method, MapperMethodInvoker> getMethodCache() {
    return methodCache;
  }
  //初始化代理实例
  @SuppressWarnings("unchecked")
  protected T newInstance(MapperProxy<T> mapperProxy) {
    return (T) Proxy.newProxyInstance(mapperInterface.getClassLoader(), new Class[] { mapperInterface }, mapperProxy);
  }
  //初始化代理类
  public T newInstance(SqlSession sqlSession) {
    final MapperProxy<T> mapperProxy = new MapperProxy<>(sqlSession, mapperInterface, methodCache);
    return newInstance(mapperProxy);
  }

}
```

### 5.2.5、MapperRegistry注册器

```java
/**
 * mapper注册器
 * @author Clinton Begin
 * @author Eduardo Macarron
 * @author Lasse Voss
 */
public class MapperRegistry {
  //全局配置
  private final Configuration config;
  //已知的mapper代理工厂
  private final Map<Class<?>, MapperProxyFactory<?>> knownMappers = new HashMap<>();
  //构造函数
  public MapperRegistry(Configuration config) {
    this.config = config;
  }
  //获取mapper根据类型
  @SuppressWarnings("unchecked")
  public <T> T getMapper(Class<T> type, SqlSession sqlSession) {
    final MapperProxyFactory<T> mapperProxyFactory = (MapperProxyFactory<T>) knownMappers.get(type);
    if (mapperProxyFactory == null) {
      throw new BindingException("Type " + type + " is not known to the MapperRegistry.");
    }
    try {
      return mapperProxyFactory.newInstance(sqlSession);
    } catch (Exception e) {
      throw new BindingException("Error getting mapper instance. Cause: " + e, e);
    }
  }
  //是否有该mapper
  public <T> boolean hasMapper(Class<T> type) {
    return knownMappers.containsKey(type);
  }
  //添加mapper
  public <T> void addMapper(Class<T> type) {
    if (type.isInterface()) {
      if (hasMapper(type)) {
        throw new BindingException("Type " + type + " is already known to the MapperRegistry.");
      }
      boolean loadCompleted = false;
      try {
        knownMappers.put(type, new MapperProxyFactory<>(type));
        // It's important that the type is added before the parser is run
        // otherwise the binding may automatically be attempted by the
        // mapper parser. If the type is already known, it won't try.
        MapperAnnotationBuilder parser = new MapperAnnotationBuilder(config, type);
        parser.parse();
        loadCompleted = true;
      } finally {
        if (!loadCompleted) {
          knownMappers.remove(type);
        }
      }
    }
  }

  /**
   * 获取只读mapper结合
   * @since 3.2.2
   */
  public Collection<Class<?>> getMappers() {
    return Collections.unmodifiableCollection(knownMappers.keySet());
  }

  /**
   * 添加mapper根据包名和超类
   * @since 3.2.2
   */
  public void addMappers(String packageName, Class<?> superType) {
    ResolverUtil<Class<?>> resolverUtil = new ResolverUtil<>();
    resolverUtil.find(new ResolverUtil.IsA(superType), packageName);
    Set<Class<? extends Class<?>>> mapperSet = resolverUtil.getClasses();
    for (Class<?> mapperClass : mapperSet) {
      addMapper(mapperClass);
    }
  }

  /**
   * 添加mapper根据包名
   * @since 3.2.2
   */
  public void addMappers(String packageName) {
    addMappers(packageName, Object.class);
  }

}
```

### 5.2.6、小结

* 本章实际上是将mapper的定义的方法与SqlSession绑定，并存放于全局配置Configuration中。

## 5.3、org.apache.ibatis.cache缓存模块

### 5.3.1、基础

#### 5.3.1.1、CacheException

```java
/**
 * 缓存异常
 * @author Clinton Begin
 */
public class CacheException extends PersistenceException {

  private static final long serialVersionUID = -193202262468464650L;
  //空构造函数
  public CacheException() {
    super();
  }
  //传递信息的构造函数
  public CacheException(String message) {
    super(message);
  }
  //传递信息和throwable的构造函数
  public CacheException(String message, Throwable cause) {
    super(message, cause);
  }
  //传递throwable的构造函数
  public CacheException(Throwable cause) {
    super(cause);
  }

}
```

#### 5.3.1.2、CacheKey缓存ke y

```java
/**
 * 缓存key
 * @author Clinton Begin
 */
public class CacheKey implements Cloneable, Serializable {

  private static final long serialVersionUID = 1146682552656046210L;
  //内部缓存key
  public static final CacheKey NULL_CACHE_KEY = new CacheKey(){
    @Override
    public void update(Object object) {
      throw new CacheException("Not allowed to update a null cache key instance.");
    }
    @Override
    public void updateAll(Object[] objects) {
      throw new CacheException("Not allowed to update a null cache key instance.");
    }
  };
  //默认的数量
  private static final int DEFAULT_MULTIPLIER = 37;
  //默认的hashcode
  private static final int DEFAULT_HASHCODE = 17;
  //乘数
  private final int multiplier;
  //哈希码
  private int hashcode;
  //检查数
  private long checksum;
  //条数
  private int count;
   //Sonarlint将此标记为需要标记为瞬态。如果内容不可序列化，则为true，但这并不总是true，因此不应标记为transient。
  // 8/21/2017 - Sonarlint flags this as needing to be marked transient.  While true if content is not serializable, this is not always true and thus should not be marked transient.
  private List<Object> updateList;
  //构造函数
  public CacheKey() {
    this.hashcode = DEFAULT_HASHCODE;
    this.multiplier = DEFAULT_MULTIPLIER;
    this.count = 0;
    this.updateList = new ArrayList<>();
  }
  //构造函数
  public CacheKey(Object[] objects) {
    this();
    updateAll(objects);
  }
  //获取更新数
  public int getUpdateCount() {
    return updateList.size();
  }
  //更新对象
  public void update(Object object) {
    int baseHashCode = object == null ? 1 : ArrayUtil.hashCode(object);

    count++;
    checksum += baseHashCode;
    baseHashCode *= count;

    hashcode = multiplier * hashcode + baseHashCode;

    updateList.add(object);
  }
  //更新所有
  public void updateAll(Object[] objects) {
    for (Object o : objects) {
      update(o);
    }
  }
  //校验对象是否一致
  @Override
  public boolean equals(Object object) {
    if (this == object) {
      return true;
    }
    if (!(object instanceof CacheKey)) {
      return false;
    }

    final CacheKey cacheKey = (CacheKey) object;

    if (hashcode != cacheKey.hashcode) {
      return false;
    }
    if (checksum != cacheKey.checksum) {
      return false;
    }
    if (count != cacheKey.count) {
      return false;
    }

    for (int i = 0; i < updateList.size(); i++) {
      Object thisObject = updateList.get(i);
      Object thatObject = cacheKey.updateList.get(i);
      if (!ArrayUtil.equals(thisObject, thatObject)) {
        return false;
      }
    }
    return true;
  }
  //hashcode值 默认17
  @Override
  public int hashCode() {
    return hashcode;
  }
  //toString
  @Override
  public String toString() {
    StringJoiner returnValue = new StringJoiner(":");
    returnValue.add(String.valueOf(hashcode));
    returnValue.add(String.valueOf(checksum));
    updateList.stream().map(ArrayUtil::toString).forEach(returnValue::add);
    return returnValue.toString();
  }
  //克隆一个对象
  @Override
  public CacheKey clone() throws CloneNotSupportedException {
    CacheKey clonedCacheKey = (CacheKey) super.clone();
    clonedCacheKey.updateList = new ArrayList<>(updateList);
    return clonedCacheKey;
  }

}
```

#### 5.3.1.3、Cache接口

```java
/**
 * 缓存服务的SPI
 * SPI for cache providers.
 * 为每个命名空间创建一个缓存实例
 * <p>
 * One instance of cache will be created for each namespace.
 *缓存实现必须有一个将缓存id作为字符串参数接收的构造函数
 * <p>
 * The cache implementation must have a constructor that receives the cache id as an String parameter.
 * Mybatis将传递命名空间作为ID给构造函数
 * <p>
 * MyBatis will pass the namespace as id to the constructor.
 *
 * <pre>
 * public MyCache(final String id) {
 *  if (id == null) {
 *    throw new IllegalArgumentException("Cache instances require an ID");
 *  }
 *  this.id = id;
 *  initialize();
 * }
 * </pre>
 *
 * @author Clinton Begin
 */

public interface Cache {

  /**
   * 返回缓存的ID
   * @return The identifier of this cache
   */
  String getId();

  /**
   * 存放对象
   * @param key Can be any object but usually it is a {@link CacheKey}
   * @param value The result of a select.
   */
  void putObject(Object key, Object value);

  /**
   * 获取缓存的对象
   * @param key The key
   * @return The object stored in the cache.
   */
  Object getObject(Object key);

  /**
   * 从3.3.0版本开始i，这个方法仅仅是在回滚在缓存丢失的一些旧值被调用。
   * 可以使先前放进key的一些阻塞缓存释放锁
   * 阻塞的缓存 当值为空 放一把锁，并且发布它 当值再次回来
   * 这种方式，其他线程将这种值作为有效值而不是等待击中数据库的
   * As of 3.3.0 this method is only called during a rollback
   * for any previous value that was missing in the cache.
   * This lets any blocking cache to release the lock that
   * may have previously put on the key.
   * A blocking cache puts a lock when a value is null
   * and releases it when the value is back again.
   * This way other threads will wait for the value to be
   * available instead of hitting the database.
   *
   *
   * @param key The key
   * @return Not used
   */
  Object removeObject(Object key);

  /**
   * 清除这个缓存实例
   * Clears this cache instance.
   */
  void clear();

  /**
   * 可选，这个方法不会被核心调用
   * Optional. This method is not called by the core.
   *
   * @return The number of elements stored in the cache (not its capacity).
   */
  int getSize();

  /**
   * 可选，从3.2.6版本开始，这个方法不会被核心调用
   * Optional. As of 3.2.6 this method is no longer called by the core.
   * <p>
   * Any locking needed by the cache must be provided internally by the cache provider.
   *
   * @return A ReadWriteLock
   */
  default ReadWriteLock getReadWriteLock() {
    return null;
  }

}
```

#### 5.3.1.4、TransactionCacheManager事务缓存管理器

```java
/**
 * 事务缓存管理器
 * @author Clinton Begin
 */
public class TransactionalCacheManager {
  //事务缓存集合
  private final Map<Cache, TransactionalCache> transactionalCaches = new HashMap<>();
  //清理缓存
  public void clear(Cache cache) {
    getTransactionalCache(cache).clear();
  }
  //获取对象
  public Object getObject(Cache cache, CacheKey key) {
    return getTransactionalCache(cache).getObject(key);
  }
  //存放对象
  public void putObject(Cache cache, CacheKey key, Object value) {
    getTransactionalCache(cache).putObject(key, value);
  }
  //提交事务
  public void commit() {
    for (TransactionalCache txCache : transactionalCaches.values()) {
      txCache.commit();
    }
  }
  //回滚事务
  public void rollback() {
    for (TransactionalCache txCache : transactionalCaches.values()) {
      txCache.rollback();
    }
  }
  //获取事务隔离级别
  private TransactionalCache getTransactionalCache(Cache cache) {
    return transactionalCaches.computeIfAbsent(cache, TransactionalCache::new);
  }

}
```

![image-20200315195816932](/Users/didi/Library/Application Support/typora-user-images/image-20200315195816932.png)

### 5.3.2、Impl

#### 5.3.2.1、PerpetualCache持久缓存

```java
/**
 * 持久缓存
 * @author Clinton Begin
 */
public class PerpetualCache implements Cache {
  //id
  private final String id;
  //缓存
  private final Map<Object, Object> cache = new HashMap<>();
  //构造函数
  public PerpetualCache(String id) {
    this.id = id;
  }
  //获取ID
  @Override
  public String getId() {
    return id;
  }
  //获取大小
  @Override
  public int getSize() {
    return cache.size();
  }
  //存放对象
  @Override
  public void putObject(Object key, Object value) {
    cache.put(key, value);
  }
  //获取对象
  @Override
  public Object getObject(Object key) {
    return cache.get(key);
  }
  //移除对象
  @Override
  public Object removeObject(Object key) {
    return cache.remove(key);
  }
  //清空缓存
  @Override
  public void clear() {
    cache.clear();
  }

  @Override
  public boolean equals(Object o) {
    if (getId() == null) {
      throw new CacheException("Cache instances require an ID.");
    }
    if (this == o) {
      return true;
    }
    if (!(o instanceof Cache)) {
      return false;
    }

    Cache otherCache = (Cache) o;
    return getId().equals(otherCache.getId());
  }

  @Override
  public int hashCode() {
    if (getId() == null) {
      throw new CacheException("Cache instances require an ID.");
    }
    return getId().hashCode();
  }

}
```

### 5.3.3、decorators装饰器



#### 5.3.3.1、BlockingCache

```java
/**
 * 简单的阻塞装饰
 * Simple blocking decorator
 *
 * 简单并且效率低下的增强BlockingCache装饰版本。
 * 设置一个锁在缓存key，当元素在缓存没找到的时候。这种方式，其他线程直到这个元素被填充而不是查询数据库
 * Simple and inefficient version of EhCache's BlockingCache decorator.
 * It sets a lock over a cache key when the element is not found in cache.
 * This way, other threads will wait until this element is filled instead of hitting the database.
 *
 * @author Eduardo Macarron
 *
 */
public class BlockingCache implements Cache {
  //超时
  private long timeout;
  //缓存对象
  private final Cache delegate;
  //锁-可重入锁
  private final ConcurrentHashMap<Object, ReentrantLock> locks;
  //构造函数
  public BlockingCache(Cache delegate) {
    this.delegate = delegate;
    this.locks = new ConcurrentHashMap<>();
  }
  //获取ID
  @Override
  public String getId() {
    return delegate.getId();
  }
  //获取大小
  @Override
  public int getSize() {
    return delegate.getSize();
  }
  //存放对象
  @Override
  public void putObject(Object key, Object value) {
    try {
      delegate.putObject(key, value);
    } finally {
      //释放锁
      releaseLock(key);
    }
  }
  //获取对象，对象不为空 释放锁
  @Override
  public Object getObject(Object key) {
    acquireLock(key);
    Object value = delegate.getObject(key);
    if (value != null) {
      releaseLock(key);
    }
    return value;
  }
  //移除对象，释放锁
  @Override
  public Object removeObject(Object key) {
    // despite of its name, this method is called only to release locks
    releaseLock(key);
    return null;
  }
  //清除缓存
  @Override
  public void clear() {
    delegate.clear();
  }
  //获取key的可重入锁
  private ReentrantLock getLockForKey(Object key) {
    return locks.computeIfAbsent(key, k -> new ReentrantLock());
  }
  //获取锁
  private void acquireLock(Object key) {
    Lock lock = getLockForKey(key);
    if (timeout > 0) {
      try {
        boolean acquired = lock.tryLock(timeout, TimeUnit.MILLISECONDS);
        if (!acquired) {
          throw new CacheException("Couldn't get a lock in " + timeout + " for the key " +  key + " at the cache " + delegate.getId());
        }
      } catch (InterruptedException e) {
        throw new CacheException("Got interrupted while trying to acquire lock for key " + key, e);
      }
    } else {
      lock.lock();
    }
  }
  //释放锁
  private void releaseLock(Object key) {
    ReentrantLock lock = locks.get(key);
    if (lock.isHeldByCurrentThread()) {
      lock.unlock();
    }
  }
  //获取超时时间
  public long getTimeout() {
    return timeout;
  }
  //设置超时时间
  public void setTimeout(long timeout) {
    this.timeout = timeout;
  }
}
```

#### 5.3.3.2、FifoCache

```java
/**
 * 先进先出缓存
 * FIFO (first in, first out) cache decorator.
 *
 * @author Clinton Begin
 */
public class FifoCache implements Cache {
  //缓存
  private final Cache delegate;
  //双向队列
  private final Deque<Object> keyList;
  //大小
  private int size;
  //构造函数，大小默认1024
  public FifoCache(Cache delegate) {
    this.delegate = delegate;
    this.keyList = new LinkedList<>();
    this.size = 1024;
  }
  //获取ID
  @Override
  public String getId() {
    return delegate.getId();
  }
  //获取大小
  @Override
  public int getSize() {
    return delegate.getSize();
  }
  //设置大小
  public void setSize(int size) {
    this.size = size;
  }
  //存放对象
  @Override
  public void putObject(Object key, Object value) {
    cycleKeyList(key);
    delegate.putObject(key, value);
  }
  //获取对象
  @Override
  public Object getObject(Object key) {
    return delegate.getObject(key);
  }
  //移除对象
  @Override
  public Object removeObject(Object key) {
    return delegate.removeObject(key);
  }
  //清除缓存
  @Override
  public void clear() {
    delegate.clear();
    keyList.clear();
  }
  //循环回收
  private void cycleKeyList(Object key) {
    keyList.addLast(key);
    if (keyList.size() > size) {
      Object oldestKey = keyList.removeFirst();
      delegate.removeObject(oldestKey);
    }
  }

}
```

#### 5.3.3.3、LoggingCache

```java
/**
 * 日志缓存
 * @author Clinton Begin
 */
public class LoggingCache implements Cache {
  //日志接口
  private final Log log;
  //缓存
  private final Cache delegate;
  //请求数
  protected int requests = 0;
  //命中数
  protected int hits = 0;
  //缓存构造函数
  public LoggingCache(Cache delegate) {
    this.delegate = delegate;
    this.log = LogFactory.getLog(getId());
  }
  //获取ID
  @Override
  public String getId() {
    return delegate.getId();
  }
  //获取大小
  @Override
  public int getSize() {
    return delegate.getSize();
  }
  //存放缓存
  @Override
  public void putObject(Object key, Object object) {
    delegate.putObject(key, object);
  }
  //获取缓存
  @Override
  public Object getObject(Object key) {
    requests++;
    final Object value = delegate.getObject(key);
    //如果值不为空，命中数+1
    if (value != null) {
      hits++;
    }
    if (log.isDebugEnabled()) {
      log.debug("Cache Hit Ratio [" + getId() + "]: " + getHitRatio());
    }
    return value;
  }
  //移除对象
  @Override
  public Object removeObject(Object key) {
    return delegate.removeObject(key);
  }
  //清除缓存
  @Override
  public void clear() {
    delegate.clear();
  }
  //hash值
  @Override
  public int hashCode() {
    return delegate.hashCode();
  }
  //比较对象
  @Override
  public boolean equals(Object obj) {
    return delegate.equals(obj);
  }
  //获取命中比例
  private double getHitRatio() {
    return (double) hits / (double) requests;
  }

}
```

#### 5.3.3.4、LruCache

```java
/**
 * 最近使用缓存装饰
 * Lru (least recently used) cache decorator.
 *
 * @author Clinton Begin
 */
public class LruCache implements Cache {
  //缓存对象
  private final Cache delegate;
  //键值
  private Map<Object, Object> keyMap;
  //老的key对象
  private Object eldestKey;
  //构造函数设置默认1024
  public LruCache(Cache delegate) {
    this.delegate = delegate;
    setSize(1024);
  }
  //获取ID
  @Override
  public String getId() {
    return delegate.getId();
  }
  //获取大小
  @Override
  public int getSize() {
    return delegate.getSize();
  }
  //设置大小
  public void setSize(final int size) {
    keyMap = new LinkedHashMap<Object, Object>(size, .75F, true) {
      private static final long serialVersionUID = 4267176411845948333L;

      @Override
      protected boolean removeEldestEntry(Map.Entry<Object, Object> eldest) {
        boolean tooBig = size() > size;
        if (tooBig) {
          eldestKey = eldest.getKey();
        }
        return tooBig;
      }
    };
  }
  //存放对象，循环移除key
  @Override
  public void putObject(Object key, Object value) {
    delegate.putObject(key, value);
    cycleKeyList(key);
  }
  //获取对象
  @Override
  public Object getObject(Object key) {
    //触发key+1
    keyMap.get(key); //touch
    return delegate.getObject(key);
  }
   //移除对象
  @Override
  public Object removeObject(Object key) {
    return delegate.removeObject(key);
  }
  //清空缓存和key值集合
  @Override
  public void clear() {
    delegate.clear();
    keyMap.clear();
  }
  //循环移除key值
  private void cycleKeyList(Object key) {
    keyMap.put(key, key);
    if (eldestKey != null) {
      delegate.removeObject(eldestKey);
      eldestKey = null;
    }
  }

}
```

#### 5.3.3.5、ScheduledCache

```java
/**
 * 定时缓存
 * @author Clinton Begin
 */
public class ScheduledCache implements Cache {
  //缓存
  private final Cache delegate;
  //清除间隔
  protected long clearInterval;
  //上次清理时间
  protected long lastClear;
  //构造函数
  public ScheduledCache(Cache delegate) {
    this.delegate = delegate;
    this.clearInterval = TimeUnit.HOURS.toMillis(1);
    this.lastClear = System.currentTimeMillis();
  }
  //设置清除间隔时间
  public void setClearInterval(long clearInterval) {
    this.clearInterval = clearInterval;
  }
  //获取id
  @Override
  public String getId() {
    return delegate.getId();
  }
  //获取大小
  @Override
  public int getSize() {
    clearWhenStale();
    return delegate.getSize();
  }
  //存放对象
  @Override
  public void putObject(Object key, Object object) {
    clearWhenStale();
    delegate.putObject(key, object);
  }
  //获取对象
  @Override
  public Object getObject(Object key) {
    return clearWhenStale() ? null : delegate.getObject(key);
  }
  //移除对象
  @Override
  public Object removeObject(Object key) {
    clearWhenStale();
    return delegate.removeObject(key);
  }
  //清空缓存
  @Override
  public void clear() {
    lastClear = System.currentTimeMillis();
    delegate.clear();
  }
  //hashcode
  @Override
  public int hashCode() {
    return delegate.hashCode();
  }
  //比较对象
  @Override
  public boolean equals(Object obj) {
    return delegate.equals(obj);
  }
  //定时清理任务
  private boolean clearWhenStale() {
    if (System.currentTimeMillis() - lastClear > clearInterval) {
      clear();
      return true;
    }
    return false;
  }

}
```

#### 5.3.3.6、SerializedCache

```java
/**
 * 序列化缓存
 * @author Clinton Begin
 */
public class SerializedCache implements Cache {
  ///缓存
  private final Cache delegate;
  //构造函数
  public SerializedCache(Cache delegate) {
    this.delegate = delegate;
  }
  //获取ID
  @Override
  public String getId() {
    return delegate.getId();
  }
  //获取大小
  @Override
  public int getSize() {
    return delegate.getSize();
  }
  //存放对象
  @Override
  public void putObject(Object key, Object object) {
    if (object == null || object instanceof Serializable) {
      delegate.putObject(key, serialize((Serializable) object));
    } else {
      throw new CacheException("SharedCache failed to make a copy of a non-serializable object: " + object);
    }
  }
  //获取对象
  @Override
  public Object getObject(Object key) {
    Object object = delegate.getObject(key);
    return object == null ? null : deserialize((byte[]) object);
  }
  //移除对象
  @Override
  public Object removeObject(Object key) {
    return delegate.removeObject(key);
  }
  //清空缓存
  @Override
  public void clear() {
    delegate.clear();
  }
  //hash值
  @Override
  public int hashCode() {
    return delegate.hashCode();
  }
  //比较对象
  @Override
  public boolean equals(Object obj) {
    return delegate.equals(obj);
  }
  //序列化对象
  private byte[] serialize(Serializable value) {
    try (ByteArrayOutputStream bos = new ByteArrayOutputStream();
         ObjectOutputStream oos = new ObjectOutputStream(bos)) {
      oos.writeObject(value);
      oos.flush();
      return bos.toByteArray();
    } catch (Exception e) {
      throw new CacheException("Error serializing object.  Cause: " + e, e);
    }
  }
  //反序列化
  private Serializable deserialize(byte[] value) {
    Serializable result;
    try (ByteArrayInputStream bis = new ByteArrayInputStream(value);
         ObjectInputStream ois = new CustomObjectInputStream(bis)) {
      result = (Serializable) ois.readObject();
    } catch (Exception e) {
      throw new CacheException("Error deserializing object.  Cause: " + e, e);
    }
    return result;
  }
  //定制对象输入流
  public static class CustomObjectInputStream extends ObjectInputStream {

    public CustomObjectInputStream(InputStream in) throws IOException {
      super(in);
    }
    //移除对象类
    @Override
    protected Class<?> resolveClass(ObjectStreamClass desc) throws ClassNotFoundException {
      return Resources.classForName(desc.getName());
    }

  }

}
```

#### 5.3.3.7、SoftCache

```java
/**
 * 软引用缓存
 * Soft Reference cache decorator
 * Thanks to Dr. Heinz Kabutz for his guidance here.
 *
 * @author Clinton Begin
 */
public class SoftCache implements Cache {
  //强引用的对象避免垃圾回收
  private final Deque<Object> hardLinksToAvoidGarbageCollection;
  //垃圾回收的实体队列
  private final ReferenceQueue<Object> queueOfGarbageCollectedEntries;
  //缓存
  private final Cache delegate;
  //强引用的数量
  private int numberOfHardLinks;
  //构造函数，默认256强引用
  public SoftCache(Cache delegate) {
    this.delegate = delegate;
    this.numberOfHardLinks = 256;
    this.hardLinksToAvoidGarbageCollection = new LinkedList<>();
    this.queueOfGarbageCollectedEntries = new ReferenceQueue<>();
  }
  //获取ID
  @Override
  public String getId() {
    return delegate.getId();
  }
  //获取大小，移除垃圾回收的item
  @Override
  public int getSize() {
    removeGarbageCollectedItems();
    return delegate.getSize();
  }

  //设置大小
  public void setSize(int size) {
    this.numberOfHardLinks = size;
  }
  //存放缓存，移除垃圾回收的item
  @Override
  public void putObject(Object key, Object value) {
    removeGarbageCollectedItems();
    delegate.putObject(key, new SoftEntry(key, value, queueOfGarbageCollectedEntries));
  }
  //获取对象，移除没有引用的
  @Override
  public Object getObject(Object key) {
    Object result = null;
    @SuppressWarnings("unchecked") // assumed delegate cache is totally managed by this cache
    SoftReference<Object> softReference = (SoftReference<Object>) delegate.getObject(key);
    if (softReference != null) {
      result = softReference.get();
      if (result == null) {
        delegate.removeObject(key);
      } else {
        // See #586 (and #335) modifications need more than a read lock
        synchronized (hardLinksToAvoidGarbageCollection) {
          hardLinksToAvoidGarbageCollection.addFirst(result);
          if (hardLinksToAvoidGarbageCollection.size() > numberOfHardLinks) {
            hardLinksToAvoidGarbageCollection.removeLast();
          }
        }
      }
    }
    return result;
  }
  //移除对象，移除垃圾回收的对象
  @Override
  public Object removeObject(Object key) {
    removeGarbageCollectedItems();
    return delegate.removeObject(key);
  }
  //清空缓存
  @Override
  public void clear() {
    synchronized (hardLinksToAvoidGarbageCollection) {
      hardLinksToAvoidGarbageCollection.clear();
    }
    removeGarbageCollectedItems();
    delegate.clear();
  }
  //移除垃圾回收的item
  private void removeGarbageCollectedItems() {
    SoftEntry sv;
    while ((sv = (SoftEntry) queueOfGarbageCollectedEntries.poll()) != null) {
      delegate.removeObject(sv.key);
    }
  }
  //软引用实体对象
  private static class SoftEntry extends SoftReference<Object> {
    private final Object key;

    SoftEntry(Object key, Object value, ReferenceQueue<Object> garbageCollectionQueue) {
      super(value, garbageCollectionQueue);
      this.key = key;
    }
  }

}
```

#### 5.3.3.8、SynchronizedCache

```java
/**
 * 同步缓存
 * @author Clinton Begin
 */
public class SynchronizedCache implements Cache {
  //缓存
  private final Cache delegate;
  //构造函数
  public SynchronizedCache(Cache delegate) {
    this.delegate = delegate;
  }
  //获取ID
  @Override
  public String getId() {
    return delegate.getId();
  }
  //获取大小
  @Override
  public synchronized int getSize() {
    return delegate.getSize();
  }
  //存放对象
  @Override
  public synchronized void putObject(Object key, Object object) {
    delegate.putObject(key, object);
  }
  //获取对象
  @Override
  public synchronized Object getObject(Object key) {
    return delegate.getObject(key);
  }
  //移除对象
  @Override
  public synchronized Object removeObject(Object key) {
    return delegate.removeObject(key);
  }
  //清空缓存
  @Override
  public synchronized void clear() {
    delegate.clear();
  }
  //hash值
  @Override
  public int hashCode() {
    return delegate.hashCode();
  }
  //对象是否一致
  @Override
  public boolean equals(Object obj) {
    return delegate.equals(obj);
  }

}
```

#### 5.3.3.9、TransactionalCache

```java
/**
 * 第二级缓存事务
 * The 2nd level cache transactional buffer.
 * <p>
 *   这个类持有所有缓存对象 被添加到二级缓存的在session期间。实体对象被发送给缓存，当提交被带哦用或者撤销如果这个session被回滚。
 *   阻塞的缓存支持已经被添加的。因此任何的get方法将返回一个缓存错过，将被put跟着执行，依次任何的与key有关系的锁都会被释放。
 *
 * @author Clinton Begin
 * @author Eduardo Macarron
 */
public class TransactionalCache implements Cache {

  private static final Log log = LogFactory.getLog(TransactionalCache.class);
  //缓存
  private final Cache delegate;
  //清空或者提交
  private boolean clearOnCommit;
  //添加提交的实体对象
  private final Map<Object, Object> entriesToAddOnCommit;
  //在缓存被错过的实体
  private final Set<Object> entriesMissedInCache;
  //构造函数
  public TransactionalCache(Cache delegate) {
    this.delegate = delegate;
    this.clearOnCommit = false;
    this.entriesToAddOnCommit = new HashMap<>();
    this.entriesMissedInCache = new HashSet<>();
  }
  //获取ID
  @Override
  public String getId() {
    return delegate.getId();
  }
  //获取大小
  @Override
  public int getSize() {
    return delegate.getSize();
  }
  //获取对象
  @Override
  public Object getObject(Object key) {
    // issue #116
    Object object = delegate.getObject(key);
    //错过的缓存对象被存放起来
    if (object == null) {
      entriesMissedInCache.add(key);
    }
    // issue #146
    if (clearOnCommit) {
      return null;
    } else {
      return object;
    }
  }
  //存放缓存
  @Override
  public void putObject(Object key, Object object) {
    entriesToAddOnCommit.put(key, object);
  }
  //移除缓存
  @Override
  public Object removeObject(Object key) {
    return null;
  }
  //清空
  @Override
  public void clear() {
    clearOnCommit = true;
    entriesToAddOnCommit.clear();
  }
  //提交
  public void commit() {
    if (clearOnCommit) {
      delegate.clear();
    }
    //刷新等待的实体
    flushPendingEntries();
    //重置事务
    reset();
  }
  //回滚事务
  public void rollback() {
    //未锁定的被错过的实体
    unlockMissedEntries();
    //重置事务
    reset();
  }
  //全部清空
  private void reset() {
    clearOnCommit = false;
    entriesToAddOnCommit.clear();
    entriesMissedInCache.clear();
  }
  //刷新等待的实体
  private void flushPendingEntries() {
    for (Map.Entry<Object, Object> entry : entriesToAddOnCommit.entrySet()) {
      delegate.putObject(entry.getKey(), entry.getValue());
    }
    //将错过的缓存对象，都存放缓存
    for (Object entry : entriesMissedInCache) {
      if (!entriesToAddOnCommit.containsKey(entry)) {
        delegate.putObject(entry, null);
      }
    }
  }
  //未锁定的错过的实体对象被移除缓存
  private void unlockMissedEntries() {
    for (Object entry : entriesMissedInCache) {
      try {
        delegate.removeObject(entry);
      } catch (Exception e) {
        log.warn("Unexpected exception while notifiying a rollback to the cache adapter."
            + "Consider upgrading your cache adapter to the latest version.  Cause: " + e);
      }
    }
  }

}
```

#### 5.3.3.10、WeakCache

```java
/**
 * 缓存装饰器 -虚引用
 * Weak Reference cache decorator.
 * Thanks to Dr. Heinz Kabutz for his guidance here.
 *
 * @author Clinton Begin
 */
public class WeakCache implements Cache {
  //强引用
  private final Deque<Object> hardLinksToAvoidGarbageCollection;
  //引用队列
  private final ReferenceQueue<Object> queueOfGarbageCollectedEntries;
  //缓存
  private final Cache delegate;
  //强引用数量
  private int numberOfHardLinks;
  //缓存构造器
  public WeakCache(Cache delegate) {
    this.delegate = delegate;
    this.numberOfHardLinks = 256;
    this.hardLinksToAvoidGarbageCollection = new LinkedList<>();
    this.queueOfGarbageCollectedEntries = new ReferenceQueue<>();
  }
  //获取ID
  @Override
  public String getId() {
    return delegate.getId();
  }
  //获取大小
  @Override
  public int getSize() {
    removeGarbageCollectedItems();
    return delegate.getSize();
  }
  //设置大小
  public void setSize(int size) {
    this.numberOfHardLinks = size;
  }
  //存放对象，并移除虚引用的item
  @Override
  public void putObject(Object key, Object value) {
    removeGarbageCollectedItems();
    delegate.putObject(key, new WeakEntry(key, value, queueOfGarbageCollectedEntries));
  }
  //获取对象
  @Override
  public Object getObject(Object key) {
    Object result = null;
    @SuppressWarnings("unchecked") // assumed delegate cache is totally managed by this cache
    WeakReference<Object> weakReference = (WeakReference<Object>) delegate.getObject(key);
    if (weakReference != null) {
      result = weakReference.get();
      if (result == null) {
        delegate.removeObject(key);
      } else {
        hardLinksToAvoidGarbageCollection.addFirst(result);
        if (hardLinksToAvoidGarbageCollection.size() > numberOfHardLinks) {
          hardLinksToAvoidGarbageCollection.removeLast();
        }
      }
    }
    return result;
  }
  //移除对象
  @Override
  public Object removeObject(Object key) {
    removeGarbageCollectedItems();
    return delegate.removeObject(key);
  }
  //移除缓存
  @Override
  public void clear() {
    hardLinksToAvoidGarbageCollection.clear();
    removeGarbageCollectedItems();
    delegate.clear();
  }
  //移除垃圾回收的item
  private void removeGarbageCollectedItems() {
    WeakEntry sv;
    while ((sv = (WeakEntry) queueOfGarbageCollectedEntries.poll()) != null) {
      delegate.removeObject(sv.key);
    }
  }
  //虚引用
  private static class WeakEntry extends WeakReference<Object> {
    private final Object key;

    private WeakEntry(Object key, Object value, ReferenceQueue<Object> garbageCollectionQueue) {
      super(value, garbageCollectionQueue);
      this.key = key;
    }
  }

}
```

### 5.3.4、小结

* 缓存利用了数据结构的特点，比如先进先出，阻塞队列
* 利用了虚引用，弱引用的回收
* 利用了定时回收机制
* 利用了同步锁
* 利用了字节流的序列化和反序列化
* 还有事务的缓存和日志的功能缓存

## 5.4、org.apache.ibatis.datasource 数据源模块

### 5.4.1、DataSourceFactory数据源工厂

```java
/**
 * 数据源工厂--叫工厂的一般符合工厂模式
 * @author Clinton Begin
 */
public interface DataSourceFactory {
  /**
   * 设置属性文件
   * @param props
   */
  void setProperties(Properties props);

  /**
   * 获取数据源
   * @return
   */
  DataSource getDataSource();

}
```

### 5.4.2、DataSourceException数据源异常

```java
/**
 * 数据源异常，看到异常模块的知道PersistenceException其实封装的是运行异常
 * @author Clinton Begin
 */
public class DataSourceException extends PersistenceException {

  private static final long serialVersionUID = -5251396250407091334L;
  //空构造函数
  public DataSourceException() {
    super();
  }
  //传递信息的构造函数
  public DataSourceException(String message) {
    super(message);
  }
  //传递信息和Throwable的构造函数
  public DataSourceException(String message, Throwable cause) {
    super(message, cause);
  }
  //传递Throwable的构造函数
  public DataSourceException(Throwable cause) {
    super(cause);
  }

}
```

### 5.4.3、Jndi（*Java* Naming and Directory Interface ）

可以参考9.2.2.3对于JNDI有一个初步基础认识。

```java
/**
 * jndi数据源工厂
 * @author Clinton Begin
 */
public class JndiDataSourceFactory implements DataSourceFactory {
  //初始化上下文
  public static final String INITIAL_CONTEXT = "initial_context";
  //数据源
  public static final String DATA_SOURCE = "data_source";
  //环境前缀
  public static final String ENV_PREFIX = "env.";
  //数据源
  private DataSource dataSource;

  /**
   * 实现数据源工厂的设置属性方法
   * @param properties
   */
  @Override
  public void setProperties(Properties properties) {
    try {
      //初始化上下文对象
      InitialContext initCtx;
      //获取环境属性
      Properties env = getEnvProperties(properties);
      if (env == null) {
        //如果环境变量没设置则初始化默认的
        initCtx = new InitialContext();
      } else {
        //否则初始化环境变量下的上下文
        initCtx = new InitialContext(env);
      }
      //如果属性包含初始化上下文并且包含数据源
      if (properties.containsKey(INITIAL_CONTEXT)
          && properties.containsKey(DATA_SOURCE)) {
       // 上下文对象获取对应的上下文
        Context ctx = (Context) initCtx.lookup(properties.getProperty(INITIAL_CONTEXT));
        dataSource = (DataSource) ctx.lookup(properties.getProperty(DATA_SOURCE));
      } else if (properties.containsKey(DATA_SOURCE)) {
        //如果包含数据源，则直接获取
        dataSource = (DataSource) initCtx.lookup(properties.getProperty(DATA_SOURCE));
      }

    } catch (NamingException e) {
      throw new DataSourceException("There was an error configuring JndiDataSourceTransactionPool. Cause: " + e, e);
    }
  }

  /**
   * 获取数据源
   * @return
   */
  @Override
  public DataSource getDataSource() {
    return dataSource;
  }

  /**
   * 获取环境属性
   * @param allProps
   * @return
   */
  private static Properties getEnvProperties(Properties allProps) {
    //环境前缀
    final String PREFIX = ENV_PREFIX;
    //上下文属性对象
    Properties contextProperties = null;
    //遍历所有的k-v配置属性，如果key是以前缀为开始的，则将截断的属性作为key，并封装该环境变量下的所有属性值。
    for (Entry<Object, Object> entry : allProps.entrySet()) {
      String key = (String) entry.getKey();
      String value = (String) entry.getValue();
      if (key.startsWith(PREFIX)) {
        if (contextProperties == null) {
          contextProperties = new Properties();
        }
        contextProperties.put(key.substring(PREFIX.length()), value);
      }
    }
    return contextProperties;
  }

}
```

### 5.4.4、unpooled (超简单数据源)

#### 5.4.4.1、UnpooledDataSourceFactory未池化的数据源工厂

```java
/**
 * 未池化的数据源工厂
 * @author Clinton Begin
 */
public class UnpooledDataSourceFactory implements DataSourceFactory {
  //驱动属性的前缀
  private static final String DRIVER_PROPERTY_PREFIX = "driver.";
  //驱动属性前缀长度
  private static final int DRIVER_PROPERTY_PREFIX_LENGTH = DRIVER_PROPERTY_PREFIX.length();
  //数据源
  protected DataSource dataSource;
  //空构造函数初始化数据源
  public UnpooledDataSourceFactory() {
    this.dataSource = new UnpooledDataSource();
  }

  /**
   * 设置属性
   * @param properties
   */
  @Override
  public void setProperties(Properties properties) {
    //初始化驱动属性
    Properties driverProperties = new Properties();
    //系统元数据对象获取关于数据源的元对象--可以参考反射模块
    MetaObject metaDataSource = SystemMetaObject.forObject(dataSource);
    //遍历map的集合
    for (Object key : properties.keySet()) {
      //获取属性名称
      String propertyName = (String) key;
      //属性名称如果以驱动属性作为前缀
      if (propertyName.startsWith(DRIVER_PROPERTY_PREFIX)) {
        //获取对应的value值
        String value = properties.getProperty(propertyName);
        //驱动属性设置属性
        driverProperties.setProperty(propertyName.substring(DRIVER_PROPERTY_PREFIX_LENGTH), value);
      } else if (metaDataSource.hasSetter(propertyName)) {
        //如果是有setter封装
        String value = (String) properties.get(propertyName);
        //转化下对象进行封装
        Object convertedValue = convertValue(metaDataSource, propertyName, value);
        metaDataSource.setValue(propertyName, convertedValue);
      } else {
        throw new DataSourceException("Unknown DataSource property: " + propertyName);
      }
    }
    //如果驱动属性大小大于0，则设置该驱动属性给元数据源
    if (driverProperties.size() > 0) {
      metaDataSource.setValue("driverProperties", driverProperties);
    }
  }

  /**
   * 获取数据源
   * @return
   */
  @Override
  public DataSource getDataSource() {
    return dataSource;
  }

  /**
   * 封装value---根据不同的值返回
   * @param metaDataSource
   * @param propertyName
   * @param value
   * @return
   */
  private Object convertValue(MetaObject metaDataSource, String propertyName, String value) {
    Object convertedValue = value;
    Class<?> targetType = metaDataSource.getSetterType(propertyName);
    if (targetType == Integer.class || targetType == int.class) {
      convertedValue = Integer.valueOf(value);
    } else if (targetType == Long.class || targetType == long.class) {
      convertedValue = Long.valueOf(value);
    } else if (targetType == Boolean.class || targetType == boolean.class) {
      convertedValue = Boolean.valueOf(value);
    }
    return convertedValue;
  }

}
```

#### 5.4.4.2、UnpooledDataSource未池化的数据源

```java
/**
 * 未池化数据源
 * @author Clinton Begin
 * @author Eduardo Macarron
 */
public class UnpooledDataSource implements DataSource {
  //类加载驱动
  private ClassLoader driverClassLoader;
  //驱动属性文件
  private Properties driverProperties;
  //注册驱动容器
  private static Map<String, Driver> registeredDrivers = new ConcurrentHashMap<>();
  //驱动
  private String driver;
  //链接url
  private String url;
  //用户名
  private String username;
  //密码
  private String password;
  //是否自动提交
  private Boolean autoCommit;
  //默认事物级别
  private Integer defaultTransactionIsolationLevel;
  //默认网络超时
  private Integer defaultNetworkTimeout;

  //驱动管理获取驱动并放到注册驱动容器
  static {
    Enumeration<Driver> drivers = DriverManager.getDrivers();
    while (drivers.hasMoreElements()) {
      Driver driver = drivers.nextElement();
      registeredDrivers.put(driver.getClass().getName(), driver);
    }
  }
  //空构造函数
  public UnpooledDataSource() {
  }
  //未池化的数据源封装
  public UnpooledDataSource(String driver, String url, String username, String password) {
    this.driver = driver;
    this.url = url;
    this.username = username;
    this.password = password;
  }
  //未池化的数据源 和驱动属性封装
  public UnpooledDataSource(String driver, String url, Properties driverProperties) {
    this.driver = driver;
    this.url = url;
    this.driverProperties = driverProperties;
  }
  //未池化的数据源，和类加载驱动封装
  public UnpooledDataSource(ClassLoader driverClassLoader, String driver, String url, String username, String password) {
    this.driverClassLoader = driverClassLoader;
    this.driver = driver;
    this.url = url;
    this.username = username;
    this.password = password;
  }
  //未池化的数据源，类加载驱动和驱动属性。
  public UnpooledDataSource(ClassLoader driverClassLoader, String driver, String url, Properties driverProperties) {
    this.driverClassLoader = driverClassLoader;
    this.driver = driver;
    this.url = url;
    this.driverProperties = driverProperties;
  }
  //获取链接
  @Override
  public Connection getConnection() throws SQLException {
    return doGetConnection(username, password);
  }
  //根据用户名和密码获取链接
  @Override
  public Connection getConnection(String username, String password) throws SQLException {
    return doGetConnection(username, password);
  }
 //设置登陆超时
  @Override
  public void setLoginTimeout(int loginTimeout) {
    DriverManager.setLoginTimeout(loginTimeout);
  }
 //获取登陆超时
  @Override
  public int getLoginTimeout() {
    return DriverManager.getLoginTimeout();
  }
 //设置日志输出器
  @Override
  public void setLogWriter(PrintWriter logWriter) {
    DriverManager.setLogWriter(logWriter);
  }
  //获取日志输出器
  @Override
  public PrintWriter getLogWriter() {
    return DriverManager.getLogWriter();
  }
  //获取类加载驱动
  public ClassLoader getDriverClassLoader() {
    return driverClassLoader;
  }
  //设置类加载驱动
  public void setDriverClassLoader(ClassLoader driverClassLoader) {
    this.driverClassLoader = driverClassLoader;
  }
  //获取驱动属性
  public Properties getDriverProperties() {
    return driverProperties;
  }
  //设置驱动属性
  public void setDriverProperties(Properties driverProperties) {
    this.driverProperties = driverProperties;
  }
  //获取驱动
  public synchronized String getDriver() {
    return driver;
  }
  //设置驱动
  public synchronized void setDriver(String driver) {
    this.driver = driver;
  }
  //获取URL
  public String getUrl() {
    return url;
  }
  //设置URL
  public void setUrl(String url) {
    this.url = url;
  }
  //获取用户名
  public String getUsername() {
    return username;
  }
  //设置用户名
  public void setUsername(String username) {
    this.username = username;
  }
  //获取密码
  public String getPassword() {
    return password;
  }
  //设置密码
  public void setPassword(String password) {
    this.password = password;
  }
  //是否自动提交
  public Boolean isAutoCommit() {
    return autoCommit;
  }
  //设置自动提交
  public void setAutoCommit(Boolean autoCommit) {
    this.autoCommit = autoCommit;
  }
  //获取默认事物隔离级别
  public Integer getDefaultTransactionIsolationLevel() {
    return defaultTransactionIsolationLevel;
  }
  //设置事物隔离级别
  public void setDefaultTransactionIsolationLevel(Integer defaultTransactionIsolationLevel) {
    this.defaultTransactionIsolationLevel = defaultTransactionIsolationLevel;
  }

  /**
   * 3。5。2获取默认的网络超时时间
   * @since 3.5.2
   */
  public Integer getDefaultNetworkTimeout() {
    return defaultNetworkTimeout;
  }

  /**
   * 设置默认的等待数据库完成操作的时间
   * Sets the default network timeout value to wait for the database operation to complete. See {@link Connection#setNetworkTimeout(java.util.concurrent.Executor, int)}
   *
   * @param defaultNetworkTimeout
   *          The time in milliseconds to wait for the database operation to complete.
   * @since 3.5.2
   */
  public void setDefaultNetworkTimeout(Integer defaultNetworkTimeout) {
    this.defaultNetworkTimeout = defaultNetworkTimeout;
  }
  //获取链接
  private Connection doGetConnection(String username, String password) throws SQLException {
    Properties props = new Properties();
    if (driverProperties != null) {
      props.putAll(driverProperties);
    }
    if (username != null) {
      props.setProperty("user", username);
    }
    if (password != null) {
      props.setProperty("password", password);
    }
    return doGetConnection(props);
  }
  //根据属性获取链接
  private Connection doGetConnection(Properties properties) throws SQLException {
    //初始化驱动
    initializeDriver();
    //获取驱动链接
    Connection connection = DriverManager.getConnection(url, properties);
    //配置链接
    configureConnection(connection);
    return connection;
  }
  //初始化驱动
  private synchronized void initializeDriver() throws SQLException {
    //注册驱动器是否包含驱动
    if (!registeredDrivers.containsKey(driver)) {
      Class<?> driverType;
      try {
        //jdbc的第一步骤 获取类加载驱动类型，比如mysql
        if (driverClassLoader != null) {
          driverType = Class.forName(driver, true, driverClassLoader);
        } else {
          //直接根据类名获取类加载驱动类型
          driverType = Resources.classForName(driver);
        }
        // DriverManager requires the driver to be loaded via the system ClassLoader.
        // http://www.kfu.com/~nsayer/Java/dyn-jdbc.html
        // 驱动管理器 要求驱动被代理系统的类加载器 加载
        Driver driverInstance = (Driver)driverType.getDeclaredConstructor().newInstance();
        //驱动管理器注册驱动
        DriverManager.registerDriver(new DriverProxy(driverInstance));
        registeredDrivers.put(driver, driverInstance);
      } catch (Exception e) {
        throw new SQLException("Error setting driver on UnpooledDataSource. Cause: " + e);
      }
    }
  }
  //配置链接，网络超时时间，是否自动提交，默认的事物隔离级别
  private void configureConnection(Connection conn) throws SQLException {
    if (defaultNetworkTimeout != null) {
      conn.setNetworkTimeout(Executors.newSingleThreadExecutor(), defaultNetworkTimeout);
    }
    if (autoCommit != null && autoCommit != conn.getAutoCommit()) {
      conn.setAutoCommit(autoCommit);
    }
    if (defaultTransactionIsolationLevel != null) {
      conn.setTransactionIsolation(defaultTransactionIsolationLevel);
    }
  }
  //驱动代理类
  private static class DriverProxy implements Driver {
    //驱动类
    private Driver driver;
   //初始化驱动类
    DriverProxy(Driver d) {
      this.driver = d;
    }
   //驱动类 加载url
    @Override
    public boolean acceptsURL(String u) throws SQLException {
      return this.driver.acceptsURL(u);
    }
   //驱动类链接用户url和属性文件
    @Override
    public Connection connect(String u, Properties p) throws SQLException {
      return this.driver.connect(u, p);
    }
    //获取主要的版本
    @Override
    public int getMajorVersion() {
      return this.driver.getMajorVersion();
    }
    //获取小版本
    @Override
    public int getMinorVersion() {
      return this.driver.getMinorVersion();
    }
   //根据url和属性文件获取驱动属性
    @Override
    public DriverPropertyInfo[] getPropertyInfo(String u, Properties p) throws SQLException {
      return this.driver.getPropertyInfo(u, p);
    }
    //jdbc是否符合
    @Override
    public boolean jdbcCompliant() {
      return this.driver.jdbcCompliant();
    }
    //获取父类日志器
    @Override
    public Logger getParentLogger() {
      return Logger.getLogger(Logger.GLOBAL_LOGGER_NAME);
    }
  }

  /**
   * 不能封装
   * @param iface
   * @param <T>
   * @return
   * @throws SQLException
   */
  @Override
  public <T> T unwrap(Class<T> iface) throws SQLException {
    throw new SQLException(getClass().getName() + " is not a wrapper.");
  }

  /**
   * 是否封装
   * @param iface
   * @return
   * @throws SQLException
   */
  @Override
  public boolean isWrapperFor(Class<?> iface) throws SQLException {
    return false;
  }

  /**
   * 获取父类的日志器，要求jdk6+
   * @return
   */
  @Override
  public Logger getParentLogger() {
    // requires JDK version 1.6
    return Logger.getLogger(Logger.GLOBAL_LOGGER_NAME);
  }

}
```



### 5.4.5、pooled（简单的单线程数据源）

#### 5.4.5.1、PooledDataSourceFactory池化的数据源工厂

```java
/**
 * 池化的数据源工厂
 * @author Clinton Begin
 */
public class PooledDataSourceFactory extends UnpooledDataSourceFactory {
  //初始化当前的数据源为池化数据源
  public PooledDataSourceFactory() {
    this.dataSource = new PooledDataSource();
  }

}
```

#### 5.4.5.2、PooledDataSource未池化的数据源

```java
/**
 * 这是一个简单 同步，线程安全的数据链接池
 * This is a simple, synchronous, thread-safe database connection pool.
 *
 * @author Clinton Begin
 */
public class PooledDataSource implements DataSource {
  //日志
  private static final Log log = LogFactory.getLog(PooledDataSource.class);
  //数据链接池状态
  private final PoolState state = new PoolState(this);
  //未池化的数据源
  private final UnpooledDataSource dataSource;
  //可选的配置属性
  // OPTIONAL CONFIGURATION FIELDS
  //最大连接数
  protected int poolMaximumActiveConnections = 10;
  //最小连接数
  protected int poolMaximumIdleConnections = 5;
  //最大链接时间
  protected int poolMaximumCheckoutTime = 20000;
  //等待超时时间
  protected int poolTimeToWait = 20000;
  //最大容忍不成功链接的次数
  protected int poolMaximumLocalBadConnectionTolerance = 3;
  //数据链接池的Ping查询
  protected String poolPingQuery = "NO PING QUERY SET";
  //是否开启数据链接池的ping查询
  protected boolean poolPingEnabled;
  //数据链接池的ping查询不用于什么场景
  protected int poolPingConnectionsNotUsedFor;
  //期待链接的类型
  private int expectedConnectionTypeCode;
  //初始化空构造器，初始化数据源--未池化的数据源
  public PooledDataSource() {
    dataSource = new UnpooledDataSource();
  }
  //初始化有参的未池化数据源
  public PooledDataSource(UnpooledDataSource dataSource) {
    this.dataSource = dataSource;
  }
  //下面的方法重载目前没有调用方
  public PooledDataSource(String driver, String url, String username, String password) {
    dataSource = new UnpooledDataSource(driver, url, username, password);
    expectedConnectionTypeCode = assembleConnectionTypeCode(dataSource.getUrl(), dataSource.getUsername(), dataSource.getPassword());
  }

  public PooledDataSource(String driver, String url, Properties driverProperties) {
    dataSource = new UnpooledDataSource(driver, url, driverProperties);
    expectedConnectionTypeCode = assembleConnectionTypeCode(dataSource.getUrl(), dataSource.getUsername(), dataSource.getPassword());
  }

  public PooledDataSource(ClassLoader driverClassLoader, String driver, String url, String username, String password) {
    dataSource = new UnpooledDataSource(driverClassLoader, driver, url, username, password);
    expectedConnectionTypeCode = assembleConnectionTypeCode(dataSource.getUrl(), dataSource.getUsername(), dataSource.getPassword());
  }

  public PooledDataSource(ClassLoader driverClassLoader, String driver, String url, Properties driverProperties) {
    dataSource = new UnpooledDataSource(driverClassLoader, driver, url, driverProperties);
    expectedConnectionTypeCode = assembleConnectionTypeCode(dataSource.getUrl(), dataSource.getUsername(), dataSource.getPassword());
  }
 //获取链接
  @Override
  public Connection getConnection() throws SQLException {
    return popConnection(dataSource.getUsername(), dataSource.getPassword()).getProxyConnection();
  }
  //获取链接
  @Override
  public Connection getConnection(String username, String password) throws SQLException {
    return popConnection(username, password).getProxyConnection();
  }
  //设置登陆超时
  @Override
  public void setLoginTimeout(int loginTimeout) {
    DriverManager.setLoginTimeout(loginTimeout);
  }
  //获取登陆超时
  @Override
  public int getLoginTimeout() {
    return DriverManager.getLoginTimeout();
  }
  //设置日志器
  @Override
  public void setLogWriter(PrintWriter logWriter) {
    DriverManager.setLogWriter(logWriter);
  }
  //获取日志器
  @Override
  public PrintWriter getLogWriter() {
    return DriverManager.getLogWriter();
  }
  //设置驱动
  public void setDriver(String driver) {
    dataSource.setDriver(driver);
    //强制关闭所有
    forceCloseAll();
  }
  //设置url
  public void setUrl(String url) {
    dataSource.setUrl(url);
    forceCloseAll();
  }
  //设置用户名
  public void setUsername(String username) {
    dataSource.setUsername(username);
    forceCloseAll();
  }
  //设置密码
  public void setPassword(String password) {
    dataSource.setPassword(password);
    forceCloseAll();
  }
  //设置是否默认提交
  public void setDefaultAutoCommit(boolean defaultAutoCommit) {
    dataSource.setAutoCommit(defaultAutoCommit);
    forceCloseAll();
  }
  //设置默认事物隔离级别
  public void setDefaultTransactionIsolationLevel(Integer defaultTransactionIsolationLevel) {
    dataSource.setDefaultTransactionIsolationLevel(defaultTransactionIsolationLevel);
    forceCloseAll();
  }
  //设置驱动属性
  public void setDriverProperties(Properties driverProps) {
    dataSource.setDriverProperties(driverProps);
    forceCloseAll();
  }

  /**
   * 设置默认的网络超时时间，3。5。2版本以后
   * Sets the default network timeout value to wait for the database operation to complete. See {@link Connection#setNetworkTimeout(java.util.concurrent.Executor, int)}
   *
   * @param milliseconds
   *          The time in milliseconds to wait for the database operation to complete.
   * @since 3.5.2
   */
  public void setDefaultNetworkTimeout(Integer milliseconds) {
    dataSource.setDefaultNetworkTimeout(milliseconds);
    forceCloseAll();
  }

  /**
   * 最大的连接数
   * The maximum number of active connections.
   *
   * @param poolMaximumActiveConnections The maximum number of active connections
   */
  public void setPoolMaximumActiveConnections(int poolMaximumActiveConnections) {
    this.poolMaximumActiveConnections = poolMaximumActiveConnections;
    forceCloseAll();
  }

  /**
   * 最小链接数
   * The maximum number of idle connections.
   *
   * @param poolMaximumIdleConnections The maximum number of idle connections
   */
  public void setPoolMaximumIdleConnections(int poolMaximumIdleConnections) {
    this.poolMaximumIdleConnections = poolMaximumIdleConnections;
    forceCloseAll();
  }

  /**
   * 最多不成功的链接次数
   * The maximum number of tolerance for bad connection happens in one thread
   * which are applying for new {@link PooledConnection}.
   *
   * @param poolMaximumLocalBadConnectionTolerance
   * max tolerance for bad connection happens in one thread
   *
   * @since 3.4.5
   */
  public void setPoolMaximumLocalBadConnectionTolerance(
      int poolMaximumLocalBadConnectionTolerance) {
    this.poolMaximumLocalBadConnectionTolerance = poolMaximumLocalBadConnectionTolerance;
  }

  /**
   * 释放前最大的链接时间
   * The maximum time a connection can be used before it *may* be
   * given away again.
   *
   * @param poolMaximumCheckoutTime The maximum time
   */
  public void setPoolMaximumCheckoutTime(int poolMaximumCheckoutTime) {
    this.poolMaximumCheckoutTime = poolMaximumCheckoutTime;
    forceCloseAll();
  }

  /**
   * 最大等待时间
   * The time to wait before retrying to get a connection.
   *
   * @param poolTimeToWait The time to wait
   */
  public void setPoolTimeToWait(int poolTimeToWait) {
    this.poolTimeToWait = poolTimeToWait;
    forceCloseAll();
  }

  /**
   * 检查链接的查询
   * The query to be used to check a connection.
   *
   * @param poolPingQuery The query
   */
  public void setPoolPingQuery(String poolPingQuery) {
    this.poolPingQuery = poolPingQuery;
    forceCloseAll();
  }

  /**
   * 决定ping查询是否被启用
   * Determines if the ping query should be used.
   *
   * @param poolPingEnabled True if we need to check a connection before using it
   */
  public void setPoolPingEnabled(boolean poolPingEnabled) {
    this.poolPingEnabled = poolPingEnabled;
    forceCloseAll();
  }

  /**
   * 如果一个链接没有被使用在这段时间期间，ping这个数据库保证链接有效
   * If a connection has not been used in this many milliseconds, ping the
   * database to make sure the connection is still good.
   *
   * @param milliseconds the number of milliseconds of inactivity that will trigger a ping
   */
  public void setPoolPingConnectionsNotUsedFor(int milliseconds) {
    this.poolPingConnectionsNotUsedFor = milliseconds;
    forceCloseAll();
  }
  //获取驱动
  public String getDriver() {
    return dataSource.getDriver();
  }
  //获取url
  public String getUrl() {
    return dataSource.getUrl();
  }
  //获取用户名称
  public String getUsername() {
    return dataSource.getUsername();
  }
  //获取密码
  public String getPassword() {
    return dataSource.getPassword();
  }
  //获取是否自动提交
  public boolean isAutoCommit() {
    return dataSource.isAutoCommit();
  }
  //获取事物隔离级别机制
  public Integer getDefaultTransactionIsolationLevel() {
    return dataSource.getDefaultTransactionIsolationLevel();
  }
  //获取驱动属性
  public Properties getDriverProperties() {
    return dataSource.getDriverProperties();
  }

  /**
   * 获取默认的网络超时时间
   * @since 3.5.2
   */
  public Integer getDefaultNetworkTimeout() {
    return dataSource.getDefaultNetworkTimeout();
  }
  //获取最大链接数

  public int getPoolMaximumActiveConnections() {
    return poolMaximumActiveConnections;
  }
  //获取最小链接数

  public int getPoolMaximumIdleConnections() {
    return poolMaximumIdleConnections;
  }
  //获取数据链接池最多不成功链接尝试次数

  public int getPoolMaximumLocalBadConnectionTolerance() {
    return poolMaximumLocalBadConnectionTolerance;
  }
  //获取释放前链接最大时间

  public int getPoolMaximumCheckoutTime() {
    return poolMaximumCheckoutTime;
  }
  //获取等待时间

  public int getPoolTimeToWait() {
    return poolTimeToWait;
  }
  //获取ping查询

  public String getPoolPingQuery() {
    return poolPingQuery;
  }
  //获取是否开启ping查询

  public boolean isPoolPingEnabled() {
    return poolPingEnabled;
  }
  //如果一个链接没有被使用在这段时间期间，ping这个数据库保证链接有效

  public int getPoolPingConnectionsNotUsedFor() {
    return poolPingConnectionsNotUsedFor;
  }

  /**
   * 关闭所有的数据链接池中的活跃链接
   * Closes all active and idle connections in the pool.
   */
  public void forceCloseAll() {
    //加同步锁
    synchronized (state) {
      //封装链接类型
      expectedConnectionTypeCode = assembleConnectionTypeCode(dataSource.getUrl(), dataSource.getUsername(), dataSource.getPassword());
      //激活的链接处理
      for (int i = state.activeConnections.size(); i > 0; i--) {
        try {
          PooledConnection conn = state.activeConnections.remove(i - 1);
          conn.invalidate();
          //如果不是自动提交则回滚
          Connection realConn = conn.getRealConnection();
          if (!realConn.getAutoCommit()) {
            realConn.rollback();
          }
          //否则直接关闭链接
          realConn.close();
        } catch (Exception e) {
          // ignore
        }
      }
      //空闲的链接处理，与上面一致，这块可以重构公共函数
      for (int i = state.idleConnections.size(); i > 0; i--) {
        try {
          PooledConnection conn = state.idleConnections.remove(i - 1);
          conn.invalidate();

          Connection realConn = conn.getRealConnection();
          if (!realConn.getAutoCommit()) {
            realConn.rollback();
          }
          realConn.close();
        } catch (Exception e) {
          // ignore
        }
      }
    }
    if (log.isDebugEnabled()) {
      log.debug("PooledDataSource forcefully closed/removed all connections.");
    }
  }
  //获取数据链接池状态
  public PoolState getPoolState() {
    return state;
  }
  //封装链接类型
  private int assembleConnectionTypeCode(String url, String username, String password) {
    return ("" + url + username + password).hashCode();
  }
  //推送链接
  protected void pushConnection(PooledConnection conn) throws SQLException {
    //加同步锁
    synchronized (state) {
      //活跃链接移除该数据链接池
      state.activeConnections.remove(conn);
      //如果链接池有效
      if (conn.isValid()) {
        if (state.idleConnections.size() < poolMaximumIdleConnections && conn.getConnectionTypeCode() == expectedConnectionTypeCode) {
          state.accumulatedCheckoutTime += conn.getCheckoutTime();
          //非自动提交 的直接回滚
          if (!conn.getRealConnection().getAutoCommit()) {
            conn.getRealConnection().rollback();
          }
          //初始化新的链接池，如果仍然存在额实时链接
          PooledConnection newConn = new PooledConnection(conn.getRealConnection(), this);
          state.idleConnections.add(newConn);
          newConn.setCreatedTimestamp(conn.getCreatedTimestamp());
          newConn.setLastUsedTimestamp(conn.getLastUsedTimestamp());
          //使链接池无效
          conn.invalidate();
          if (log.isDebugEnabled()) {
            log.debug("Returned connection " + newConn.getRealHashCode() + " to pool.");
          }
          //通知所有的线程，竞争该锁
          state.notifyAll();
        } else {
          //不是自动提交 链接回滚
          state.accumulatedCheckoutTime += conn.getCheckoutTime();
          if (!conn.getRealConnection().getAutoCommit()) {
            conn.getRealConnection().rollback();
          }
          //获取的实时链接关闭
          conn.getRealConnection().close();
          if (log.isDebugEnabled()) {
            log.debug("Closed connection " + conn.getRealHashCode() + ".");
          }
          //使链接池无效
          conn.invalidate();
        }
      } else {
        if (log.isDebugEnabled()) {
          log.debug("A bad connection (" + conn.getRealHashCode() + ") attempted to return to the pool, discarding connection.");
        }
        state.badConnectionCount++;
      }
    }
  }
  //封装链接池
  private PooledConnection popConnection(String username, String password) throws SQLException {
    boolean countedWait = false;
    PooledConnection conn = null;
    long t = System.currentTimeMillis();
    int localBadConnectionCount = 0;
    //如果链接为空
    while (conn == null) {
      //加同步锁
      synchronized (state) {
        //如果空闲链接不为空
        if (!state.idleConnections.isEmpty()) {
          // Pool has available connection
          //数据链接池有可用链接
          conn = state.idleConnections.remove(0);
          if (log.isDebugEnabled()) {
            log.debug("Checked out connection " + conn.getRealHashCode() + " from pool.");
          }
        } else {
          //数据链接池没有可用链接
          // Pool does not have available connection
          if (state.activeConnections.size() < poolMaximumActiveConnections) {
            // Can create new connection
            //创建一个新的数据链接
            conn = new PooledConnection(dataSource.getConnection(), this);
            if (log.isDebugEnabled()) {
              log.debug("Created connection " + conn.getRealHashCode() + ".");
            }
          } else {
            //不能创建新的链接
            // Cannot create new connection
            PooledConnection oldestActiveConnection = state.activeConnections.get(0);
            long longestCheckoutTime = oldestActiveConnection.getCheckoutTime();
            if (longestCheckoutTime > poolMaximumCheckoutTime) {
              // Can claim overdue connection
              // 申请过期链接
              state.claimedOverdueConnectionCount++;
              state.accumulatedCheckoutTimeOfOverdueConnections += longestCheckoutTime;
              state.accumulatedCheckoutTime += longestCheckoutTime;
              state.activeConnections.remove(oldestActiveConnection);
              //如果没有设置自动提交，则直接回滚
              if (!oldestActiveConnection.getRealConnection().getAutoCommit()) {
                try {
                  oldestActiveConnection.getRealConnection().rollback();
                } catch (SQLException e) {
                  /*
                     Just log a message for debug and continue to execute the following
                     statement like nothing happened.
                     Wrap the bad connection with a new PooledConnection, this will help
                     to not interrupt current executing thread and give current thread a
                     chance to join the next competition for another valid/good database
                     connection. At the end of this loop, bad {@link @conn} will be set as null.
                   */
                  log.debug("Bad connection. Could not roll back");
                }
              }
              conn = new PooledConnection(oldestActiveConnection.getRealConnection(), this);
              conn.setCreatedTimestamp(oldestActiveConnection.getCreatedTimestamp());
              conn.setLastUsedTimestamp(oldestActiveConnection.getLastUsedTimestamp());
              //将链接置为无效
              oldestActiveConnection.invalidate();
              if (log.isDebugEnabled()) {
                log.debug("Claimed overdue connection " + conn.getRealHashCode() + ".");
              }
            } else {
              // Must wait
              // 必须等待链接处理完毕
              try {
                if (!countedWait) {
                  state.hadToWaitCount++;
                  countedWait = true;
                }
                if (log.isDebugEnabled()) {
                  log.debug("Waiting as long as " + poolTimeToWait + " milliseconds for connection.");
                }
                long wt = System.currentTimeMillis();
                state.wait(poolTimeToWait);
                state.accumulatedWaitTime += System.currentTimeMillis() - wt;
              } catch (InterruptedException e) {
                break;
              }
            }
          }
        }
        if (conn != null) {
          // ping to server and check the connection is valid or not
          //如果链接不为空，则ping下服务器 来检查链接是否还有效
          if (conn.isValid()) {
            //如果链接没设置自动提交，则直接回滚
            if (!conn.getRealConnection().getAutoCommit()) {
              conn.getRealConnection().rollback();
            }
            conn.setConnectionTypeCode(assembleConnectionTypeCode(dataSource.getUrl(), username, password));
            conn.setCheckoutTimestamp(System.currentTimeMillis());
            conn.setLastUsedTimestamp(System.currentTimeMillis());
            state.activeConnections.add(conn);
            state.requestCount++;
            state.accumulatedRequestTime += System.currentTimeMillis() - t;
          } else {
            if (log.isDebugEnabled()) {
              log.debug("A bad connection (" + conn.getRealHashCode() + ") was returned from the pool, getting another connection.");
            }
            state.badConnectionCount++;
            localBadConnectionCount++;
            conn = null;
            if (localBadConnectionCount > (poolMaximumIdleConnections + poolMaximumLocalBadConnectionTolerance)) {
              if (log.isDebugEnabled()) {
                log.debug("PooledDataSource: Could not get a good connection to the database.");
              }
              throw new SQLException("PooledDataSource: Could not get a good connection to the database.");
            }
          }
        }
      }

    }

    if (conn == null) {
      if (log.isDebugEnabled()) {
        log.debug("PooledDataSource: Unknown severe error condition.  The connection pool returned a null connection.");
      }
      throw new SQLException("PooledDataSource: Unknown severe error condition.  The connection pool returned a null connection.");
    }

    return conn;
  }

  /**
   * 检查一个链接是否仍然是使用的
   * Method to check to see if a connection is still usable
   *
   * @param conn - the connection to check
   * @return True if the connection is still usable
   */
  protected boolean pingConnection(PooledConnection conn) {
    boolean result = true;

    try {
      //如果实时链接已经关闭
      result = !conn.getRealConnection().isClosed();
    } catch (SQLException e) {
      if (log.isDebugEnabled()) {
        log.debug("Connection " + conn.getRealHashCode() + " is BAD: " + e.getMessage());
      }
      result = false;
    }
    //如果未关闭
    if (result) {
      //是否可以ping通
      if (poolPingEnabled) {
        if (poolPingConnectionsNotUsedFor >= 0 && conn.getTimeElapsedSinceLastUse() > poolPingConnectionsNotUsedFor) {
          try {
            if (log.isDebugEnabled()) {
              log.debug("Testing connection " + conn.getRealHashCode() + " ...");
            }
            //进行数据库链接
            Connection realConn = conn.getRealConnection();
            //执行ping会话关闭
            try (Statement statement = realConn.createStatement()) {
              statement.executeQuery(poolPingQuery).close();
            }
            //如果不是主动提交则回滚
            if (!realConn.getAutoCommit()) {
              realConn.rollback();
            }
            result = true;
            if (log.isDebugEnabled()) {
              log.debug("Connection " + conn.getRealHashCode() + " is GOOD!");
            }
          } catch (Exception e) {
            log.warn("Execution of ping query '" + poolPingQuery + "' failed: " + e.getMessage());
            try {
              //如果期间发生异常，则直接断开链接
              conn.getRealConnection().close();
            } catch (Exception e2) {
              //ignore
            }
            result = false;
            if (log.isDebugEnabled()) {
              log.debug("Connection " + conn.getRealHashCode() + " is BAD: " + e.getMessage());
            }
          }
        }
      }
    }
    return result;
  }

  /**
   * 为了获取实时链接，不封装链接池
   * Unwraps a pooled connection to get to the 'real' connection
   *
   * @param conn - the pooled connection to unwrap
   * @return The 'real' connection
   */
  public static Connection unwrapConnection(Connection conn) {
    if (Proxy.isProxyClass(conn.getClass())) {
      InvocationHandler handler = Proxy.getInvocationHandler(conn);
      if (handler instanceof PooledConnection) {
        return ((PooledConnection) handler).getRealConnection();
      }
    }
    return conn;
  }
 //释放链接池
  @Override
  protected void finalize() throws Throwable {
    forceCloseAll();
    super.finalize();
  }
 //未封装
  @Override
  public <T> T unwrap(Class<T> iface) throws SQLException {
    throw new SQLException(getClass().getName() + " is not a wrapper.");
  }
  //是否封装
  @Override
  public boolean isWrapperFor(Class<?> iface) {
    return false;
  }
 //获取父类日志器
  @Override
  public Logger getParentLogger() {
    return Logger.getLogger(Logger.GLOBAL_LOGGER_NAME);
  }

}
```

【关注学习点】

* mybatis通过ping机制来保证自己数据链接池的链接活性，以及链接的有效性。如果想做一个链接池，则需要维护探活机制。
* 如果出现链接池释放，则未自动提交的会直接回滚数据到原始状态，而自动提交的则会将正在链接的线程创建个新的链接执行完毕。否则报错。

#### 5.4.5.3、数据链接池的状态PoolState

```java
/**
 * 数据链接池的状态
 * @author Clinton Begin
 */
public class PoolState {
   //池化的数据源
  protected PooledDataSource dataSource;
  //空闲的链接
  protected final List<PooledConnection> idleConnections = new ArrayList<>();
  //获取的链接
  protected final List<PooledConnection> activeConnections = new ArrayList<>();
  //请求数
  protected long requestCount = 0;
  //累计请求时间
  protected long accumulatedRequestTime = 0;
  //累计释放切换时间
  protected long accumulatedCheckoutTime = 0;
  //超过到期链接数的次数
  protected long claimedOverdueConnectionCount = 0;
  //累计释放切换到期链接数的次数
  protected long accumulatedCheckoutTimeOfOverdueConnections = 0;
  //累计等待时间
  protected long accumulatedWaitTime = 0;
  //已经等待次数
  protected long hadToWaitCount = 0;
  //坏链接次数
  protected long badConnectionCount = 0;
  //池化的数据源
  public PoolState(PooledDataSource dataSource) {
    this.dataSource = dataSource;
  }
  //获取请求数
  public synchronized long getRequestCount() {
    return requestCount;
  }
  //获取平均请求时间
  public synchronized long getAverageRequestTime() {
    return requestCount == 0 ? 0 : accumulatedRequestTime / requestCount;
  }
 //获取平均等待时间
  public synchronized long getAverageWaitTime() {
    return hadToWaitCount == 0 ? 0 : accumulatedWaitTime / hadToWaitCount;

  }
  //获取等待次数
  public synchronized long getHadToWaitCount() {
    return hadToWaitCount;
  }
  //获取坏链接次数
  public synchronized long getBadConnectionCount() {
    return badConnectionCount;
  }
  //获取超过到期时间的次数
  public synchronized long getClaimedOverdueConnectionCount() {
    return claimedOverdueConnectionCount;
  }
  //获取平均释放切换超过到期时间次数
  public synchronized long getAverageOverdueCheckoutTime() {
    return claimedOverdueConnectionCount == 0 ? 0 : accumulatedCheckoutTimeOfOverdueConnections / claimedOverdueConnectionCount;
  }
  //获取平均释放切换时间
  public synchronized long getAverageCheckoutTime() {
    return requestCount == 0 ? 0 : accumulatedCheckoutTime / requestCount;
  }

 //获取空闲链接数量
  public synchronized int getIdleConnectionCount() {
    return idleConnections.size();
  }
 //获取活跃链接数量
  public synchronized int getActiveConnectionCount() {
    return activeConnections.size();
  }

  @Override
  public synchronized String toString() {
    StringBuilder builder = new StringBuilder();
    builder.append("\n===CONFINGURATION==============================================");
    builder.append("\n jdbcDriver                     ").append(dataSource.getDriver());
    builder.append("\n jdbcUrl                        ").append(dataSource.getUrl());
    builder.append("\n jdbcUsername                   ").append(dataSource.getUsername());
    builder.append("\n jdbcPassword                   ").append(dataSource.getPassword() == null ? "NULL" : "************");
    builder.append("\n poolMaxActiveConnections       ").append(dataSource.poolMaximumActiveConnections);
    builder.append("\n poolMaxIdleConnections         ").append(dataSource.poolMaximumIdleConnections);
    builder.append("\n poolMaxCheckoutTime            ").append(dataSource.poolMaximumCheckoutTime);
    builder.append("\n poolTimeToWait                 ").append(dataSource.poolTimeToWait);
    builder.append("\n poolPingEnabled                ").append(dataSource.poolPingEnabled);
    builder.append("\n poolPingQuery                  ").append(dataSource.poolPingQuery);
    builder.append("\n poolPingConnectionsNotUsedFor  ").append(dataSource.poolPingConnectionsNotUsedFor);
    builder.append("\n ---STATUS-----------------------------------------------------");
    builder.append("\n activeConnections              ").append(getActiveConnectionCount());
    builder.append("\n idleConnections                ").append(getIdleConnectionCount());
    builder.append("\n requestCount                   ").append(getRequestCount());
    builder.append("\n averageRequestTime             ").append(getAverageRequestTime());
    builder.append("\n averageCheckoutTime            ").append(getAverageCheckoutTime());
    builder.append("\n claimedOverdue                 ").append(getClaimedOverdueConnectionCount());
    builder.append("\n averageOverdueCheckoutTime     ").append(getAverageOverdueCheckoutTime());
    builder.append("\n hadToWait                      ").append(getHadToWaitCount());
    builder.append("\n averageWaitTime                ").append(getAverageWaitTime());
    builder.append("\n badConnectionCount             ").append(getBadConnectionCount());
    builder.append("\n===============================================================");
    return builder.toString();
  }

}
```

#### 5.4.5.4、PooledConnection数据链接池的链接

```java
/**
 * 池化的链接
 * @author Clinton Begin
 */
class PooledConnection implements InvocationHandler {
  //关闭
  private static final String CLOSE = "close";
  //是否ACES ？
  private static final Class<?>[] IFACES = new Class<?>[] { Connection.class };
  //hashcode值
  private final int hashCode;
  //数据源
  private final PooledDataSource dataSource;
  //实际链接
  private final Connection realConnection;
  //代理链接
  private final Connection proxyConnection;
  //释放切换时间
  private long checkoutTimestamp;
  //创建时间
  private long createdTimestamp;
  //上次使用时间
  private long lastUsedTimestamp;
  //链接类型code
  private int connectionTypeCode;
  //是否有效
  private boolean valid;

  /**
   * 使用入参链接和池化的数据源构造
   * Constructor for SimplePooledConnection that uses the Connection and PooledDataSource passed in.
   *
   * @param connection - the connection that is to be presented as a pooled connection
   * @param dataSource - the dataSource that the connection is from
   */
  public PooledConnection(Connection connection, PooledDataSource dataSource) {
    this.hashCode = connection.hashCode();
    this.realConnection = connection;
    this.dataSource = dataSource;
    this.createdTimestamp = System.currentTimeMillis();
    this.lastUsedTimestamp = System.currentTimeMillis();
    this.valid = true;
    this.proxyConnection = (Connection) Proxy.newProxyInstance(Connection.class.getClassLoader(), IFACES, this);
  }

  /**
   * 使链接无效
   * Invalidates the connection.
   */
  public void invalidate() {
    valid = false;
  }

  /**
   * 检查链接是否还有用，有效，并且实际链接不为空，数据源可以ping通
   * Method to see if the connection is usable.
   *
   * @return True if the connection is usable
   */
  public boolean isValid() {
    return valid && realConnection != null && dataSource.pingConnection(this);
  }

  /**
   * 获取真实的链接
   * Getter for the *real* connection that this wraps.
   *
   * @return The connection
   */
  public Connection getRealConnection() {
    return realConnection;
  }

  /**
   * 获取链接的代理
   * Getter for the proxy for the connection.
   *
   * @return The proxy
   */
  public Connection getProxyConnection() {
    return proxyConnection;
  }

  /**
   * 获取真实链接的hashCode ，如果为null则为0
   * Gets the hashcode of the real connection (or 0 if it is null).
   *
   * @return The hashcode of the real connection (or 0 if it is null)
   */
  public int getRealHashCode() {
    return realConnection == null ? 0 : realConnection.hashCode();
  }

  /**
   * 获取链接的类型
   * Getter for the connection type (based on url + user + password).
   *
   * @return The connection type
   */
  public int getConnectionTypeCode() {
    return connectionTypeCode;
  }

  /**
   * 设置链接类型
   * Setter for the connection type.
   *
   * @param connectionTypeCode - the connection type
   */
  public void setConnectionTypeCode(int connectionTypeCode) {
    this.connectionTypeCode = connectionTypeCode;
  }

  /**
   * 获取链接的创建时间
   * Getter for the time that the connection was created.
   *
   * @return The creation timestamp
   */
  public long getCreatedTimestamp() {
    return createdTimestamp;
  }

  /**
   * 设置链接的创建时间
   * Setter for the time that the connection was created.
   *
   * @param createdTimestamp - the timestamp
   */
  public void setCreatedTimestamp(long createdTimestamp) {
    this.createdTimestamp = createdTimestamp;
  }

  /**
   * 获取链接上次使用的时间
   * Getter for the time that the connection was last used.
   *
   * @return - the timestamp
   */
  public long getLastUsedTimestamp() {
    return lastUsedTimestamp;
  }

  /**
   * 设置链接上次使用的时间
   * Setter for the time that the connection was last used.
   *
   * @param lastUsedTimestamp - the timestamp
   */
  public void setLastUsedTimestamp(long lastUsedTimestamp) {
    this.lastUsedTimestamp = lastUsedTimestamp;
  }

  /**
   * 获取该链接自从上次使用到现在的时间
   * Getter for the time since this connection was last used.
   *
   * @return - the time since the last use
   */
  public long getTimeElapsedSinceLastUse() {
    return System.currentTimeMillis() - lastUsedTimestamp;
  }

  /**
   * 获取链接的年龄
   * Getter for the age of the connection.
   *
   * @return the age
   */
  public long getAge() {
    return System.currentTimeMillis() - createdTimestamp;
  }

  /**
   * 获取链接被切换的时间
   * Getter for the timestamp that this connection was checked out.
   *
   * @return the timestamp
   */
  public long getCheckoutTimestamp() {
    return checkoutTimestamp;
  }

  /**
   * 设置链接被切换的时间
   * Setter for the timestamp that this connection was checked out.
   *
   * @param timestamp the timestamp
   */
  public void setCheckoutTimestamp(long timestamp) {
    this.checkoutTimestamp = timestamp;
  }

  /**
   * 获取链接已经切换的时间
   * Getter for the time that this connection has been checked out.
   *
   * @return the time
   */
  public long getCheckoutTime() {
    return System.currentTimeMillis() - checkoutTimestamp;
  }

  @Override
  public int hashCode() {
    return hashCode;
  }

  /**
   * 两个链接之间的比较
   * Allows comparing this connection to another.
   *
   * @param obj - the other connection to test for equality
   * @see Object#equals(Object)
   */
  @Override
  public boolean equals(Object obj) {
    if (obj instanceof PooledConnection) {
      return realConnection.hashCode() == ((PooledConnection) obj).realConnection.hashCode();
    } else if (obj instanceof Connection) {
      return hashCode == obj.hashCode();
    } else {
      return false;
    }
  }

  /**
   * InvocationHandler的实现类
   * Required for InvocationHandler implementation.
   *
   * @param proxy  - not used
   * @param method - the method to be executed
   * @param args   - the parameters to be passed to the method
   * @see java.lang.reflect.InvocationHandler#invoke(Object, java.lang.reflect.Method, Object[])
   */
  @Override
  public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    String methodName = method.getName();
    //如果是关闭
    if (CLOSE.equals(methodName)) {
      //推送给链接
      dataSource.pushConnection(this);
      return null;
    }
    try {
      //如果类与声明的不一致跑出异常
      if (!Object.class.equals(method.getDeclaringClass())) {
        // issue #579 toString() should never fail
        // throw an SQLException instead of a Runtime
        checkConnection();
      }
      //执行链接
      return method.invoke(realConnection, args);
    } catch (Throwable t) {
      throw ExceptionUtil.unwrapThrowable(t);
    }

  }
  //检查链接是否有效
  private void checkConnection() throws SQLException {
    if (!valid) {
      throw new SQLException("Error accessing PooledConnection. Connection is invalid.");
    }
  }

}
```

#### 5.4.5.5、小结

1. 从本模块来看，其实是对JDBC链接和事务控制的封装。为了提升性能增加了数据链接池，为了保障数据链接池的链接有效性（ping查询探活机制来保证链接的。）

## 5.5、org.apache.ibatis.exceptions  异常定义模块

### 5.5.1、Exceptions包UML图概览

![exceptions](/Users/didi/Documents/design/mybatis/chapter_5/exceptions/exceptions.png)

### 5.5.2、TooManyResultsException太多数据异常

```java
/**
 * 太多数据异常
 */
public class TooManyResultsException extends PersistenceException {
  //序列化版本号
  private static final long serialVersionUID = 8935197089745865786L;
  //空构造函数，暂时未使用
  public TooManyResultsException() {
    super();
  }
  //重载构造函数，根据传递信息调用父类
  public TooManyResultsException(String message) {
    super(message);
  }
  //重载构造函数，根据传递信息和Throwable抛出异常调用父类，暂时未使用
  public TooManyResultsException(String message, Throwable cause) {
    super(message, cause);
  }
  //重载构造函数，根据传递Throwable抛出异常信息调用父类，暂时未使用
  public TooManyResultsException(Throwable cause) {
    super(cause);
  }
}
```

目前框架只使用了TooManyResultsException(String message)构造函数，所以我们关注它就可以了。

### 5.5.3、PersistenceException

```java
/**
 * SuppressWarnings用于抑制编译废弃的IbatisException提醒
 */
@SuppressWarnings("deprecation")
public class PersistenceException extends IbatisException {

  private static final long serialVersionUID = -7537395265357977271L;
  //重载空构造函数
  public PersistenceException() {
    super();
  }
 //重载传递消息的构造函数
  public PersistenceException(String message) {
    super(message);
  }
  //重载传递消息和Throwable的构造函数
  public PersistenceException(String message, Throwable cause) {
    super(message, cause);
  }
  //重载Throwable的构造函数
  public PersistenceException(Throwable cause) {
    super(cause);
  }
}
```

### 5.5.4、IbatisException（已废弃）

```java
/**
 * Deprecated废弃类
 * @author Clinton Begin
 */
@Deprecated
public class IbatisException extends RuntimeException {

  private static final long serialVersionUID = 3880206998166270511L;
  //重载空构造函数
  public IbatisException() {
    super();
  }
  //重载传递消息的构造函数
  public IbatisException(String message) {
    super(message);
  }
  //重载传递消息和Throwable的构造函数
  public IbatisException(String message, Throwable cause) {
    super(message, cause);
  }
  //重载传递Throwable的构造函数
  public IbatisException(Throwable cause) {
    super(cause);
  }

}

```

小结：我们可以看到父类最终来源于Java的运行异常RuntimeException

### 5.5.5、异常工厂类ExceptionFactory

```java
/**
 * 异常工厂类
 * @author Clinton Begin
 */
public class ExceptionFactory {
  private ExceptionFactory() {
  //私有属性防止空构造器实例化
  }
  //包装异常函数：将传递的信息，和异常进行封装并返回运行异常
  public static RuntimeException wrapException(String message, Exception e) {
    //返回持久化异常并封装了具体传递的信息
    return new PersistenceException(ErrorContext.instance().message(message).cause(e).toString(), e);
  }

}
```

## 【todo】 ErrorContext

### 5.5.6、归纳小结

异常模块的几个类，主要核心为ExceptionFactory封装了运行级异常RuntimeException，以及对于太多数据集的异常封装TooManyResultsException。借鉴之处，工厂类的工厂模式保证全局只有一个实例，可以将构造器设置为private阻止实例化，只能通过static静态类方法获取。

![image-20200222160948484](/Users/didi/Library/Application Support/typora-user-images/image-20200222160948484.png)

## 5.6、org.apache.ibatis.io 资源加载工具类

### 5.6.1、VFS抽象类API

```java
/**
 * 提供一个非常简单的API 为了访问资源在应用服务内
 * Provides a very simple API for accessing resources within an application server.
 *
 * @author Ben Gunter
 */
public abstract class VFS {
  //日志
  private static final Log log = LogFactory.getLog(VFS.class);
  //默认实现类
  /** The built-in implementations. */
  public static final Class<?>[] IMPLEMENTATIONS = { JBoss6VFS.class, DefaultVFS.class };
  //用户实现类集合
  /** The list to which implementations are added by {@link #addImplClass(Class)}. */
  public static final List<Class<? extends VFS>> USER_IMPLEMENTATIONS = new ArrayList<>();
  //单例持有器
  /** Singleton instance holder. */
  private static class VFSHolder {
    static final VFS INSTANCE = createVFS();

    @SuppressWarnings("unchecked")
    static VFS createVFS() {
      //首先尝试用户的实现类，然后是默认的实现
      // Try the user implementations first, then the built-ins
      List<Class<? extends VFS>> impls = new ArrayList<>();
      impls.addAll(USER_IMPLEMENTATIONS);
      impls.addAll(Arrays.asList((Class<? extends VFS>[]) IMPLEMENTATIONS));
      //找到有效的实现类之前，遍历所有的
      // Try each implementation class until a valid one is found
      VFS vfs = null;
      for (int i = 0; vfs == null || !vfs.isValid(); i++) {
        Class<? extends VFS> impl = impls.get(i);
        try {
          vfs = impl.getDeclaredConstructor().newInstance();
          if (!vfs.isValid()) {
            if (log.isDebugEnabled()) {
              log.debug("VFS implementation " + impl.getName() +
                  " is not valid in this environment.");
            }
          }
        } catch (InstantiationException | IllegalAccessException | NoSuchMethodException | InvocationTargetException e) {
          log.error("Failed to instantiate " + impl, e);
          return null;
        }
      }

      if (log.isDebugEnabled()) {
        log.debug("Using VFS adapter " + vfs.getClass().getName());
      }

      return vfs;
    }
  }

  /**
   * 获取单个实例，如果没有实现类被找到对于当前的环境，则返回null
   */
  public static VFS getInstance() {
    return VFSHolder.INSTANCE;
  }

  /**
   * 给VFS实现添加指定的类。用户添加的是在默认的实现类之前
   */
  public static void addImplClass(Class<? extends VFS> clazz) {
    if (clazz != null) {
      USER_IMPLEMENTATIONS.add(clazz);
    }
  }
  //根据名字获取一个类，如果这个类没找到则返回空
  protected static Class<?> getClass(String className) {
    try {
      return Thread.currentThread().getContextClassLoader().loadClass(className);
//      return ReflectUtil.findClass(className);
    } catch (ClassNotFoundException e) {
      if (log.isDebugEnabled()) {
        log.debug("Class not found: " + className);
      }
      return null;
    }
  }

  /**
   * 通过名字和参数类型获取方法，如果这个方法没找到返回空
   */
  protected static Method getMethod(Class<?> clazz, String methodName, Class<?>... parameterTypes) {
    if (clazz == null) {
      return null;
    }
    try {
      return clazz.getMethod(methodName, parameterTypes);
    } catch (SecurityException e) {
      log.error("Security exception looking for method " + clazz.getName() + "." + methodName + ".  Cause: " + e);
      return null;
    } catch (NoSuchMethodException e) {
      log.error("Method not found " + clazz.getName() + "." + methodName + "." + methodName + ".  Cause: " + e);
      return null;
    }
  }

  /**
   * 执行一个方法关于某个对象，并返回结果。
   */
  @SuppressWarnings("unchecked")
  protected static <T> T invoke(Method method, Object object, Object... parameters)
      throws IOException, RuntimeException {
    try {
      return (T) method.invoke(object, parameters);
    } catch (IllegalArgumentException | IllegalAccessException e) {
      throw new RuntimeException(e);
    } catch (InvocationTargetException e) {
      if (e.getTargetException() instanceof IOException) {
        throw (IOException) e.getTargetException();
      } else {
        throw new RuntimeException(e);
      }
    }
  }

  /**
   * 获取来自所有从上下文类加载器中指定路径下的资源的URL集合
   */
  protected static List<URL> getResources(String path) throws IOException {
    return Collections.list(Thread.currentThread().getContextClassLoader().getResources(path));
  }
  //如果当前环境的VFS实现有效，返回true
  public abstract boolean isValid();

  /**
   * 通过URL 递归列举指定下的所有资源全路径
   */
  protected abstract List<String> list(URL url, String forPath) throws IOException;

  /**
   * 递归列举在指定路径下被发现的所有资源的子集 全资源路径
   */
  public List<String> list(String path) throws IOException {
    List<String> names = new ArrayList<>();
    for (URL url : getResources(path)) {
      names.addAll(list(url, path));
    }
    return names;
  }
}

```

### 5.6.2、JBoss6VFS

```java
/**
 * JBoss 6提供的VFS API实现
 * A {@link VFS} implementation that works with the VFS API provided by JBoss 6.
 *
 * @author Ben Gunter
 */
public class JBoss6VFS extends VFS {
  //日志
  private static final Log log = LogFactory.getLog(JBoss6VFS.class);
  //一个类，它模拟JBoss VFS类的一个子集
  /** A class that mimics a tiny subset of the JBoss VirtualFile class. */
  static class VirtualFile {
    //虚拟文件
    static Class<?> VirtualFile;
    //获取路径参数相关，获取子集递归
    static Method getPathNameRelativeTo, getChildrenRecursively;
    //声明自身
    Object virtualFile;
    //构造函数
    VirtualFile(Object virtualFile) {
      this.virtualFile = virtualFile;
    }
    //根据虚拟文件获取路径参数
    String getPathNameRelativeTo(VirtualFile parent) {
      try {
        return invoke(getPathNameRelativeTo, virtualFile, parent.virtualFile);
      } catch (IOException e) {
        // This exception is not thrown by the called method
        log.error("This should not be possible. VirtualFile.getPathNameRelativeTo() threw IOException.");
        return null;
      }
    }
    //获取子集
    List<VirtualFile> getChildren() throws IOException {
      List<?> objects = invoke(getChildrenRecursively, virtualFile);
      List<VirtualFile> children = new ArrayList<>(objects.size());
      for (Object object : objects) {
        children.add(new VirtualFile(object));
      }
      return children;
    }
  }
  //一个类，它模拟JBoss VFS类的一个子集
  /** A class that mimics a tiny subset of the JBoss VFS class. */
  static class VFS {
    static Class<?> VFS;
    static Method getChild;

    private VFS() {
      // Prevent Instantiation
    }

    static VirtualFile getChild(URL url) throws IOException {
      Object o = invoke(getChild, VFS, url);
      return o == null ? null : new VirtualFile(o);
    }
  }

  /** Flag that indicates if this VFS is valid for the current environment. */
  private static Boolean valid;
  //找访问 JBoss 6 VFS 要求的 所有的类和方法
  /** Find all the classes and methods that are required to access the JBoss 6 VFS. */
  protected static synchronized void initialize() {
    if (valid == null) {
      // 假设有效，如果出错 将会之后返回
      // Assume valid. It will get flipped later if something goes wrong.
      valid = Boolean.TRUE;
      // 浏览 并且找到需要的类
      // Look up and verify required classes
      VFS.VFS = checkNotNull(getClass("org.jboss.vfs.VFS"));
      VirtualFile.VirtualFile = checkNotNull(getClass("org.jboss.vfs.VirtualFile"));
      //浏览并找到需要的类
      // Look up and verify required methods
      VFS.getChild = checkNotNull(getMethod(VFS.VFS, "getChild", URL.class));
      VirtualFile.getChildrenRecursively = checkNotNull(getMethod(VirtualFile.VirtualFile,
          "getChildrenRecursively"));
      VirtualFile.getPathNameRelativeTo = checkNotNull(getMethod(VirtualFile.VirtualFile,
          "getPathNameRelativeTo", VirtualFile.VirtualFile));
      //校验API没有被改变
      // Verify that the API has not changed
      checkReturnType(VFS.getChild, VirtualFile.VirtualFile);
      checkReturnType(VirtualFile.getChildrenRecursively, List.class);
      checkReturnType(VirtualFile.getPathNameRelativeTo, String.class);
    }
  }

  /**
   * 校验提供的对象引用不为空，如果为空，那么VFS在当前环境被标记为无效
   * Verifies that the provided object reference is null. If it is null, then this VFS is marked
   * as invalid for the current environment.
   *
   * @param object The object reference to check for null.
   */
  protected static <T> T checkNotNull(T object) {
    if (object == null) {
      setInvalid();
    }
    return object;
  }

  /**
   * 校验返回的方法的类型是期望的，如果不是，那么VFS在当前环境被标记为无效
   * Verifies that the return type of a method is what it is expected to be. If it is not, then
   * this VFS is marked as invalid for the current environment.
   *
   * @param method The method whose return type is to be checked.
   * @param expected A type to which the method's return type must be assignable.
   * @see Class#isAssignableFrom(Class)
   */
  protected static void checkReturnType(Method method, Class<?> expected) {
    if (method != null && !expected.isAssignableFrom(method.getReturnType())) {
      log.error("Method " + method.getClass().getName() + "." + method.getName()
          + "(..) should return " + expected.getName() + " but returns "
          + method.getReturnType().getName() + " instead.");
      setInvalid();
    }
  }
  //标记当前环境的VFS无效/
  /** Mark this {@link VFS} as invalid for the current environment. */
  protected static void setInvalid() {
    if (JBoss6VFS.valid == Boolean.TRUE) {
      log.debug("JBoss 6 VFS API is not available in this environment.");
      JBoss6VFS.valid = Boolean.FALSE;
    }
  }
  //静态代码块
  static {
    initialize();
  }
  //是否资源路径有效
  @Override
  public boolean isValid() {
    return valid;
  }
  //遍历URL的 路径
  @Override
  public List<String> list(URL url, String path) throws IOException {
    VirtualFile directory;
    directory = VFS.getChild(url);
    if (directory == null) {
      return Collections.emptyList();
    }

    if (!path.endsWith("/")) {
      path += "/";
    }

    List<VirtualFile> children = directory.getChildren();
    List<String> names = new ArrayList<>(children.size());
    for (VirtualFile vf : children) {
      names.add(path + vf.getPathNameRelativeTo(directory));
    }

    return names;
  }
}
```



### 5.6.3、DefaultVFS默认的VFS实现

```java
/**
 * 对于大多数应用服务器，VFS的默认实现
 * A default implementation of {@link VFS} that works for most application servers.
 *
 * @author Ben Gunter
 */
public class DefaultVFS extends VFS {
  //日志
  private static final Log log = LogFactory.getLog(DefaultVFS.class);
   //JAR (ZIP) 的魔数头
  /** The magic header that indicates a JAR (ZIP) file. */
  private static final byte[] JAR_MAGIC = { 'P', 'K', 3, 4 };
  //是否无效
  @Override
  public boolean isValid() {
    return true;
  }
  //遍历URL和路径
  @Override
  public List<String> list(URL url, String path) throws IOException {
    InputStream is = null;
    try {
      List<String> resources = new ArrayList<>();
      //首先 ，尝试发现包含必要资源的JAR文件。如果一个JAR文件被找到，那么我们将根据读取的JAR列举子资源。
      // First, try to find the URL of a JAR file containing the requested resource. If a JAR
      // file is found, then we'll list child resources by reading the JAR.
      URL jarUrl = findJarForResource(url);
      if (jarUrl != null) {
        is = jarUrl.openStream();
        if (log.isDebugEnabled()) {
          log.debug("Listing " + url);
        }
        resources = listResources(new JarInputStream(is), path);
      }
      else {
        List<String> children = new ArrayList<>();
        try {
          if (isJar(url)) {
            //一些JBoss VFS版本 也许给了 JAR 流，即使URL资源引用不是JAR
            // Some versions of JBoss VFS might give a JAR stream even if the resource
            // referenced by the URL isn't actually a JAR
            is = url.openStream();
            try (JarInputStream jarInput = new JarInputStream(is)) {
              if (log.isDebugEnabled()) {
                log.debug("Listing " + url);
              }
              for (JarEntry entry; (entry = jarInput.getNextJarEntry()) != null; ) {
                if (log.isDebugEnabled()) {
                  log.debug("Jar entry: " + entry.getName());
                }
                children.add(entry.getName());
              }
            }
          }
          else {
            //一些Servlet容器允许读取目录资源比如 文本文件，列举子资源 一行行的。然而 在目录和文件资源通过读的时候 没有方式区分。
            //为了解决这个问题，，当读取每行，尝试遍历代理的类加载器，作为当前资源的子类。如果任意行失败了，那么我们认为当前环境不是一个目录。
            /*
             * Some servlet containers allow reading from directory resources like a
             * text file, listing the child resources one per line. However, there is no
             * way to differentiate between directory and file resources just by reading
             * them. To work around that, as each line is read, try to look it up via
             * the class loader as a child of the current resource. If any line fails
             * then we assume the current resource is not a directory.
             */
            is = url.openStream();
            List<String> lines = new ArrayList<>();
            try (BufferedReader reader = new BufferedReader(new InputStreamReader(is))) {
              for (String line; (line = reader.readLine()) != null;) {
                if (log.isDebugEnabled()) {
                  log.debug("Reader entry: " + line);
                }
                lines.add(line);
                if (getResources(path + "/" + line).isEmpty()) {
                  lines.clear();
                  break;
                }
              }
            }
            if (!lines.isEmpty()) {
              if (log.isDebugEnabled()) {
                log.debug("Listing " + url);
              }
              children.addAll(lines);
            }
          }
        } catch (FileNotFoundException e) {
          /*对于openStream() 文件URLS的调可能会失败，依赖于servlet容器，因为目录不能依赖读。如果那个发生了，那么直接列出当前目录代替。
           * For file URLs the openStream() call might fail, depending on the servlet
           * container, because directories can't be opened for reading. If that happens,
           * then list the directory directly instead.
           */
          if ("file".equals(url.getProtocol())) {
            File file = new File(url.getFile());
            if (log.isDebugEnabled()) {
                log.debug("Listing directory " + file.getAbsolutePath());
            }
            if (file.isDirectory()) {
              if (log.isDebugEnabled()) {
                  log.debug("Listing " + url);
              }
              children = Arrays.asList(file.list());
            }
          }
          else {
            //不知道异常从哪里来的，所以重复一遍
            // No idea where the exception came from so rethrow it
            throw e;
          }
        }
        //当递归列举子资源 URL前缀 使用
        // The URL prefix to use when recursively listing child resources
        String prefix = url.toExternalForm();
        if (!prefix.endsWith("/")) {
          prefix = prefix + "/";
        }
         //遍历当前子节点，添加文件和递归目录里面
        // Iterate over immediate children, adding files and recursing into directories
        for (String child : children) {
          String resourcePath = path + "/" + child;
          resources.add(resourcePath);
          URL childUrl = new URL(prefix + child);
          resources.addAll(list(childUrl, resourcePath));
        }
      }

      return resources;
    } finally {
      if (is != null) {
        try {
          is.close();
        } catch (Exception e) {
          // Ignore
        }
      }
    }
  }

  /**
   * 列举 {@link JarInputStream}给的所有集合名字，以指定的path路径开始的。条目将匹配或者不匹配前斜道杠
   *
   * List the names of the entries in the given {@link JarInputStream} that begin with the
   * specified {@code path}. Entries will match with or without a leading slash.
   *
   * @param jar The JAR input stream
   * @param path The leading path to match
   * @return The names of all the matching entries
   * @throws IOException If I/O errors occur
   */
  protected List<String> listResources(JarInputStream jar, String path) throws IOException {
    //包含这个开头和尾部的斜道杠 当匹配名字的时候
    // Include the leading and trailing slash when matching names
    if (!path.startsWith("/")) {
      path = "/" + path;
    }
    if (!path.endsWith("/")) {
      path = path + "/";
    }
    //跌代集合 并且 收集 那些必要路径的资源
    // Iterate over the entries and collect those that begin with the requested path
    List<String> resources = new ArrayList<>();
    for (JarEntry entry; (entry = jar.getNextJarEntry()) != null;) {
      if (!entry.isDirectory()) {
        // Add leading slash if it's missing
        StringBuilder name = new StringBuilder(entry.getName());
        if (name.charAt(0) != '/') {
          name.insert(0, '/');
        }

        // Check file name
        if (name.indexOf(path) == 0) {
          if (log.isDebugEnabled()) {
            log.debug("Found resource: " + name);
          }
          // Trim leading slash
          resources.add(name.substring(1));
        }
      }
    }
    return resources;
  }

  /**
   * 通过URL 企图 反构造化 这个被提供的URL 来找到包含资源引用的JAR 文件。那就是说，假设URL引用JAR实体，这个方法将返回一个包含实体的引用JAR文件资源
   * 。如果这个JAR 不能被定位，那么返回null。
   * Attempts to deconstruct the given URL to find a JAR file containing the resource referenced
   * by the URL. That is, assuming the URL references a JAR entry, this method will return a URL
   * that references the JAR file containing the entry. If the JAR cannot be located, then this
   * method returns null.
   *
   * @param url The URL of the JAR entry.
   * @return The URL of the JAR file, if one is found. Null if not.
   * @throws MalformedURLException
   */
  protected URL findJarForResource(URL url) throws MalformedURLException {
    if (log.isDebugEnabled()) {
      log.debug("Find JAR URL: " + url);
    }

    // If the file part of the URL is itself a URL, then that URL probably points to the JAR
    boolean continueLoop = true;
    while (continueLoop) {
      try {
        url = new URL(url.getFile());
        if (log.isDebugEnabled()) {
          log.debug("Inner URL: " + url);
        }
      } catch (MalformedURLException e) {
        // This will happen at some point and serves as a break in the loop
        continueLoop = false;
      }
    }

    // Look for the .jar extension and chop off everything after that
    StringBuilder jarUrl = new StringBuilder(url.toExternalForm());
    int index = jarUrl.lastIndexOf(".jar");
    if (index >= 0) {
      jarUrl.setLength(index + 4);
      if (log.isDebugEnabled()) {
        log.debug("Extracted JAR URL: " + jarUrl);
      }
    }
    else {
      if (log.isDebugEnabled()) {
        log.debug("Not a JAR: " + jarUrl);
      }
      return null;
    }

    // Try to open and test it
    try {
      URL testUrl = new URL(jarUrl.toString());
      if (isJar(testUrl)) {
        return testUrl;
      }
      else {
        // WebLogic fix: check if the URL's file exists in the filesystem.
        if (log.isDebugEnabled()) {
          log.debug("Not a JAR: " + jarUrl);
        }
        jarUrl.replace(0, jarUrl.length(), testUrl.getFile());
        File file = new File(jarUrl.toString());

        // File name might be URL-encoded
        if (!file.exists()) {
          try {
            file = new File(URLEncoder.encode(jarUrl.toString(), "UTF-8"));
          } catch (UnsupportedEncodingException e) {
            throw new RuntimeException("Unsupported encoding?  UTF-8?  That's unpossible.");
          }
        }

        if (file.exists()) {
          if (log.isDebugEnabled()) {
            log.debug("Trying real file: " + file.getAbsolutePath());
          }
          testUrl = file.toURI().toURL();
          if (isJar(testUrl)) {
            return testUrl;
          }
        }
      }
    } catch (MalformedURLException e) {
      log.warn("Invalid JAR URL: " + jarUrl);
    }

    if (log.isDebugEnabled()) {
      log.debug("Not a JAR: " + jarUrl);
    }
    return null;
  }

  /**
   * 为了可以调用 {@link ClassLoader#getResources(String)}. 将包名转换成路径名
   * Converts a Java package name to a path that can be looked up with a call to
   * {@link ClassLoader#getResources(String)}.
   *
   * @param packageName The Java package name to convert to a path
   */
  protected String getPackagePath(String packageName) {
    return packageName == null ? null : packageName.replace('.', '/');
  }

  /**
   * 如果资源路径被提供的URL定位是个JAR文件 返回true
   * Returns true if the resource located at the given URL is a JAR file.
   *
   * @param url The URL of the resource to test.
   */
  protected boolean isJar(URL url) {
    return isJar(url, new byte[JAR_MAGIC.length]);
  }

  /**
   * 如果被定位的资源在这个提供URL 是JAR文件返回True
   * Returns true if the resource located at the given URL is a JAR file.
   *
   * @param url The URL of the resource to test.
   * @param buffer A buffer into which the first few bytes of the resource are read. The buffer
   *            must be at least the size of {@link #JAR_MAGIC}. (The same buffer may be reused
   *            for multiple calls as an optimization.)
   */
  protected boolean isJar(URL url, byte[] buffer) {
    InputStream is = null;
    try {
      is = url.openStream();
      is.read(buffer, 0, JAR_MAGIC.length);
      if (Arrays.equals(buffer, JAR_MAGIC)) {
        if (log.isDebugEnabled()) {
          log.debug("Found JAR: " + url);
        }
        return true;
      }
    } catch (Exception e) {
      // Failure to read the stream means this is not a JAR
    } finally {
      if (is != null) {
        try {
          is.close();
        } catch (Exception e) {
          // Ignore
        }
      }
    }

    return false;
  }
}

```

### 5.6.4、ClassLoaderWrapper类加载器包装器

```java
/**
 * 包装访问多个类加载器 使他们像一个工作
 * A class to wrap access to multiple class loaders making them work as one
 *
 * @author Clinton Begin
 */
public class ClassLoaderWrapper {
  //默认的类加载器
  ClassLoader defaultClassLoader;
  //系统类加载器
  ClassLoader systemClassLoader;
  //类加载器包装器构造
  ClassLoaderWrapper() {
    try {
      systemClassLoader = ClassLoader.getSystemClassLoader();
    } catch (SecurityException ignored) {
      // AccessControlException on Google App Engine
    }
  }

  /**
   * 使用当前类路径获取一个资源作为URL
   * Get a resource as a URL using the current class path
   *
   * @param resource - the resource to locate
   * @return the resource or null
   */
  public URL getResourceAsURL(String resource) {
    return getResourceAsURL(resource, getClassLoaders(null));
  }

  /**
   * 从类路径下获取资源，开始以一个指定的类加载器
   * Get a resource from the classpath, starting with a specific class loader
   *
   * @param resource    - the resource to find
   * @param classLoader - the first classloader to try
   * @return the stream or null
   */
  public URL getResourceAsURL(String resource, ClassLoader classLoader) {
    return getResourceAsURL(resource, getClassLoaders(classLoader));
  }

  /**
   * 从类路径下获取资源
   * Get a resource from the classpath
   *
   * @param resource - the resource to find
   * @return the stream or null
   */
  public InputStream getResourceAsStream(String resource) {
    return getResourceAsStream(resource, getClassLoaders(null));
  }

  /**
   * 从类路径下获取资源，以指定的类加载器开始
   * Get a resource from the classpath, starting with a specific class loader
   *
   * @param resource    - the resource to find
   * @param classLoader - the first class loader to try
   * @return the stream or null
   */
  public InputStream getResourceAsStream(String resource, ClassLoader classLoader) {
    return getResourceAsStream(resource, getClassLoaders(classLoader));
  }

  /**
   * 找到类路径上的一个类 或者一直尝试
   * Find a class on the classpath (or die trying)
   *
   * @param name - the class to look for
   * @return - the class
   * @throws ClassNotFoundException Duh.
   */
  public Class<?> classForName(String name) throws ClassNotFoundException {
    return classForName(name, getClassLoaders(null));
  }

  /**
   * 找到类路径上的一个类，以指定类加载器开始的 或者一直尝试。
   * Find a class on the classpath, starting with a specific classloader (or die trying)
   *
   * @param name        - the class to look for
   * @param classLoader - the first classloader to try
   * @return - the class
   * @throws ClassNotFoundException Duh.
   */
  public Class<?> classForName(String name, ClassLoader classLoader) throws ClassNotFoundException {
    return classForName(name, getClassLoaders(classLoader));
  }

  /**
   * 尝试从一组类加载器中获取资源
   * Try to get a resource from a group of classloaders
   *
   * @param resource    - the resource to get
   * @param classLoader - the classloaders to examine
   * @return the resource or null
   */
  InputStream getResourceAsStream(String resource, ClassLoader[] classLoader) {
    for (ClassLoader cl : classLoader) {
      if (null != cl) {
        //尝试找到通过的资源
        // try to find the resource as passed
        InputStream returnValue = cl.getResourceAsStream(resource);
        //现在 一些类加载器 想要头斜杠，因此 我们将添加它 并且尝试如果我们没找到资源
        // now, some class loaders want this leading "/", so we'll add it and try again if we didn't find the resource
        if (null == returnValue) {
          returnValue = cl.getResourceAsStream("/" + resource);
        }

        if (null != returnValue) {
          return returnValue;
        }
      }
    }
    return null;
  }

  /**
   * 使用当前的类路径获取一个资源作为URL
   * Get a resource as a URL using the current class path
   *
   * @param resource    - the resource to locate
   * @param classLoader - the class loaders to examine
   * @return the resource or null
   */
  URL getResourceAsURL(String resource, ClassLoader[] classLoader) {

    URL url;

    for (ClassLoader cl : classLoader) {

      if (null != cl) {
        //寻找资源作为传入进来的
        // look for the resource as passed in...
        url = cl.getResource(resource);
        //但是一些类加载器想要头部斜杠，因此我们将添加它，并且如果我们没找到资源再继续尝试。
        // ...but some class loaders want this leading "/", so we'll add it
        // and try again if we didn't find the resource
        if (null == url) {
          url = cl.getResource("/" + resource);
        }
        //总是在最后的位置找到，所以就不需要继续寻找了，因此已经停止寻找
        // "It's always in the last place I look for it!"
        // ... because only an idiot would keep looking for it after finding it, so stop looking already.
        if (null != url) {
          return url;
        }

      }

    }
    //什么也没找到
    // didn't find it anywhere.
    return null;

  }

  /**
   * 企图从一组类加载器中加载一个类
   * Attempt to load a class from a group of classloaders
   *
   * @param name        - the class to load
   * @param classLoader - the group of classloaders to examine
   * @return the class
   * @throws ClassNotFoundException - Remember the wisdom of Judge Smails: Well, the world needs ditch diggers, too.
   */
  Class<?> classForName(String name, ClassLoader[] classLoader) throws ClassNotFoundException {

    for (ClassLoader cl : classLoader) {

      if (null != cl) {

        try {

          Class<?> c = Class.forName(name, true, cl);

          if (null != c) {
            return c;
          }

        } catch (ClassNotFoundException e) {
          // we'll ignore this until all classloaders fail to locate the class
        }

      }

    }

    throw new ClassNotFoundException("Cannot find class: " + name);

  }
  //获取类加载器
  ClassLoader[] getClassLoaders(ClassLoader classLoader) {
    return new ClassLoader[]{
        classLoader,
        defaultClassLoader,
        Thread.currentThread().getContextClassLoader(),
        getClass().getClassLoader(),
        systemClassLoader};
  }

}

```

### 5.6.5、Resources资源类

```java
/**
 * 通过类加载器简单访问资源的类
 * A class to simplify access to resources through the classloader.
 *
 * @author Clinton Begin
 */
public class Resources {
  //类加载器包装器
  private static ClassLoaderWrapper classLoaderWrapper = new ClassLoaderWrapper();

  /**
   * 字符集当调用getResourceAsReader，空意味使用了系统默认的
   * Charset to use when calling getResourceAsReader.
   * null means use the system default.
   */
  private static Charset charset;
 //构造函数
  Resources() {
  }

  /**
   * //返回默认的类加载器
   * Returns the default classloader (may be null).
   *
   * @return The default classloader
   */
  public static ClassLoader getDefaultClassLoader() {
    return classLoaderWrapper.defaultClassLoader;
  }

  /**
   * 设置默认的类加载器
   * Sets the default classloader
   *
   * @param defaultClassLoader - the new default ClassLoader
   */
  public static void setDefaultClassLoader(ClassLoader defaultClassLoader) {
    classLoaderWrapper.defaultClassLoader = defaultClassLoader;
  }

  /**
   *返回类路径上的资源的URL
   * Returns the URL of the resource on the classpath
   *
   * @param resource The resource to find
   * @return The resource
   * @throws java.io.IOException If the resource cannot be found or read
   */
  public static URL getResourceURL(String resource) throws IOException {
      // issue #625
      return getResourceURL(null, resource);
  }

  /**
   * 返回类路径上资源的URL
   * Returns the URL of the resource on the classpath
   *
   * @param loader   The classloader used to fetch the resource
   * @param resource The resource to find
   * @return The resource
   * @throws java.io.IOException If the resource cannot be found or read
   */
  public static URL getResourceURL(ClassLoader loader, String resource) throws IOException {
    URL url = classLoaderWrapper.getResourceAsURL(resource, loader);
    if (url == null) {
      throw new IOException("Could not find resource " + resource);
    }
    return url;
  }

  /**
   * 返回类路径上的资源作为流对象
   * Returns a resource on the classpath as a Stream object
   *
   * @param resource The resource to find
   * @return The resource
   * @throws java.io.IOException If the resource cannot be found or read
   */
  public static InputStream getResourceAsStream(String resource) throws IOException {
    return getResourceAsStream(null, resource);
  }

  /**
   * 返回类路径上的资源作为流对象
   * Returns a resource on the classpath as a Stream object
   *
   * @param loader   The classloader used to fetch the resource
   * @param resource The resource to find
   * @return The resource
   * @throws java.io.IOException If the resource cannot be found or read
   */
  public static InputStream getResourceAsStream(ClassLoader loader, String resource) throws IOException {
    InputStream in = classLoaderWrapper.getResourceAsStream(resource, loader);
    if (in == null) {
      throw new IOException("Could not find resource " + resource);
    }
    return in;
  }

  /**
   * 返回类路径上的资源作为属性对象
   * Returns a resource on the classpath as a Properties object
   *
   * @param resource The resource to find
   * @return The resource
   * @throws java.io.IOException If the resource cannot be found or read
   */
  public static Properties getResourceAsProperties(String resource) throws IOException {
    Properties props = new Properties();
    try (InputStream in = getResourceAsStream(resource)) {
      props.load(in);
    }
    return props;
  }

  /**
   * 返回类路径上的资源作为属性对象
   * Returns a resource on the classpath as a Properties object
   *
   * @param loader   The classloader used to fetch the resource
   * @param resource The resource to find
   * @return The resource
   * @throws java.io.IOException If the resource cannot be found or read
   */
  public static Properties getResourceAsProperties(ClassLoader loader, String resource) throws IOException {
    Properties props = new Properties();
    try (InputStream in = getResourceAsStream(loader, resource)) {
      props.load(in);
    }
    return props;
  }

  /**
   * 返回类路径上的资源作为Reader对象
   * Returns a resource on the classpath as a Reader object
   *
   * @param resource The resource to find
   * @return The resource
   * @throws java.io.IOException If the resource cannot be found or read
   */
  public static Reader getResourceAsReader(String resource) throws IOException {
    Reader reader;
    if (charset == null) {
      reader = new InputStreamReader(getResourceAsStream(resource));
    } else {
      reader = new InputStreamReader(getResourceAsStream(resource), charset);
    }
    return reader;
  }

  /**
   * 返回类路径的资源作为Reader对象
   * Returns a resource on the classpath as a Reader object
   *
   * @param loader   The classloader used to fetch the resource
   * @param resource The resource to find
   * @return The resource
   * @throws java.io.IOException If the resource cannot be found or read
   */
  public static Reader getResourceAsReader(ClassLoader loader, String resource) throws IOException {
    Reader reader;
    if (charset == null) {
      reader = new InputStreamReader(getResourceAsStream(loader, resource));
    } else {
      reader = new InputStreamReader(getResourceAsStream(loader, resource), charset);
    }
    return reader;
  }

  /**
   * 返回类路径的资源作为文件对象
   * Returns a resource on the classpath as a File object
   *
   * @param resource The resource to find
   * @return The resource
   * @throws java.io.IOException If the resource cannot be found or read
   */
  public static File getResourceAsFile(String resource) throws IOException {
    return new File(getResourceURL(resource).getFile());
  }

  /**
   * 返回类路径的资源作为文件对象
   * Returns a resource on the classpath as a File object
   *
   * @param loader   - the classloader used to fetch the resource
   * @param resource - the resource to find
   * @return The resource
   * @throws java.io.IOException If the resource cannot be found or read
   */
  public static File getResourceAsFile(ClassLoader loader, String resource) throws IOException {
    return new File(getResourceURL(loader, resource).getFile());
  }

  /**
   * 获取URL作为输入流
   * Gets a URL as an input stream
   *
   * @param urlString - the URL to get
   * @return An input stream with the data from the URL
   * @throws java.io.IOException If the resource cannot be found or read
   */
  public static InputStream getUrlAsStream(String urlString) throws IOException {
    URL url = new URL(urlString);
    URLConnection conn = url.openConnection();
    return conn.getInputStream();
  }

  /**
   * 获取URL作为Reader流
   * Gets a URL as a Reader
   *
   * @param urlString - the URL to get
   * @return A Reader with the data from the URL
   * @throws java.io.IOException If the resource cannot be found or read
   */
  public static Reader getUrlAsReader(String urlString) throws IOException {
    Reader reader;
    if (charset == null) {
      reader = new InputStreamReader(getUrlAsStream(urlString));
    } else {
      reader = new InputStreamReader(getUrlAsStream(urlString), charset);
    }
    return reader;
  }

  /**
   * 获取URL作为属性对象
   * Gets a URL as a Properties object
   *
   * @param urlString - the URL to get
   * @return A Properties object with the data from the URL
   * @throws java.io.IOException If the resource cannot be found or read
   */
  public static Properties getUrlAsProperties(String urlString) throws IOException {
    Properties props = new Properties();
    try (InputStream in = getUrlAsStream(urlString)) {
      props.load(in);
    }
    return props;
  }

  /**
   * 加载类
   * Loads a class
   *
   * @param className - the class to fetch
   * @return The loaded class
   * @throws ClassNotFoundException If the class cannot be found (duh!)
   */
  public static Class<?> classForName(String className) throws ClassNotFoundException {
    return classLoaderWrapper.classForName(className);
  }
  //获取字符集
  public static Charset getCharset() {
    return charset;
  }
  //设置字符集
  public static void setCharset(Charset charset) {
    Resources.charset = charset;
  }

}
```

###  5.6.6、ResolverUtil解决工具类

```java
/**
 * 资源工具类是被用来定位在类路径有效的类，并且遇见任意条件。这两个最大的公共条件是一个实现或者继承其他的类，或者他被指定注解注定的。
 * 然而通过使用 {@link Test}类，它可能搜索有用的任意条件。
 * <p>ResolverUtil is used to locate classes that are available in the/a class path and meet
 * arbitrary conditions. The two most common conditions are that a class implements/extends
 * another class, or that is it annotated with a specific annotation. However, through the use
 * of the {@link Test} class it is possible to search using arbitrary conditions.</p>
 *一个类加载器是被用来定位所有的位置（目录和jar文件）在这类路径包含没有指定的包，并且然后加载那些类，并且检查他们。
 * 默认的这个类加载器被{@code Thread.currentThread().getContextClassLoader()}返回，但是这个也可能被调用{@link #setClassLoader(ClassLoader)}重写
 * 优先执行 {@code find()}方法
 * <p>A ClassLoader is used to locate all locations (directories and jar files) in the class
 * path that contain classes within certain packages, and then to load those classes and
 * check them. By default the ClassLoader returned by
 * {@code Thread.currentThread().getContextClassLoader()} is used, but this can be overridden
 * by calling {@link #setClassLoader(ClassLoader)} prior to invoking any of the {@code find()}
 * methods.</p>
 *通用的检索是被{@link #find(org.apache.ibatis.io.ResolverUtil.Test, String)} ()}方法调用的并且应用一个包名和一个测试实例。
 * 这将导致这个被命名的包<b>and all sub-packages</b>所有的子集包被扫描所有的类来匹配测试。也有工具方法对于通用的例子扫描多个扩展的特殊类，或者指定注解的类。
 * <p>General searches are initiated by calling the
 * {@link #find(org.apache.ibatis.io.ResolverUtil.Test, String)} ()} method and supplying
 * a package name and a Test instance. This will cause the named package <b>and all sub-packages</b>
 * to be scanned for classes that meet the test. There are also utility methods for the common
 * use cases of scanning multiple packages for extensions of particular classes, or classes
 * annotated with a specific annotation.</p>
 *
 * 标准的使用方式 如下。
 * <p>The standard usage pattern for the ResolverUtil class is as follows:</p>
 *
 * <pre>
 * ResolverUtil&lt;ActionBean&gt; resolver = new ResolverUtil&lt;ActionBean&gt;();
 * resolver.findImplementation(ActionBean.class, pkg1, pkg2);
 * resolver.find(new CustomTest(), pkg1);
 * resolver.find(new CustomTest(), pkg2);
 * Collection&lt;ActionBean&gt; beans = resolver.getClasses();
 * </pre>
 *
 * @author Tim Fennell
 */
public class ResolverUtil<T> {
  /*
  当前类的日志实现
   * An instance of Log to use for logging in this class.
   */
  private static final Log log = LogFactory.getLog(ResolverUtil.class);

  /**
   * 一个简单的接口 可以指定怎么测试类来决定是否他们被ResolverUtil产生的结果包含
   * A simple interface that specifies how to test classes to determine if they
   * are to be included in the results produced by the ResolverUtil.
   */
  public interface Test {
    /**
     * 候选类将被重复的调用，如果一个类被结果包含 一定返回true，否则返回false
     * Will be called repeatedly with candidate classes. Must return True if a class
     * is to be included in the results, false otherwise.
     */
    boolean matches(Class<?> type);
  }

  /**
   * 检查是否每个类被分配给提供的类。注意 这个测试将匹配自身父类型，如果为了匹配展现的
   * A Test that checks to see if each class is assignable to the provided class. Note
   * that this test will match the parent type itself if it is presented for matching.
   */
  public static class IsA implements Test {
    private Class<?> parent;
    //构造一个IsA测试使用其作为父类或者接口
    /** Constructs an IsA test using the supplied Class as the parent class/interface. */
    public IsA(Class<?> parentType) {
      this.parent = parentType;
    }
    //如果类型被分配给父类型在构造器中，则返回true
    /** Returns true if type is assignable to the parent type supplied in the constructor. */
    @Override
    public boolean matches(Class<?> type) {
      return type != null && parent.isAssignableFrom(type);
    }
    //toString
    @Override
    public String toString() {
      return "is assignable to " + parent.getSimpleName();
    }
  }

  /**
   * 检查看下是否每个类是用指定的注解，如果是，返回true，否则fasle
   * A Test that checks to see if each class is annotated with a specific annotation. If it
   * is, then the test returns true, otherwise false.
   */
  public static class AnnotatedWith implements Test {
    private Class<? extends Annotation> annotation;
     //构造指定注解类型的测试
    /** Constructs an AnnotatedWith test for the specified annotation type. */
    public AnnotatedWith(Class<? extends Annotation> annotation) {
      this.annotation = annotation;
    }
   //如果类提供的构造器已经含有注解，则返回true
    /** Returns true if the type is annotated with the class provided to the constructor. */
    @Override
    public boolean matches(Class<?> type) {
      return type != null && type.isAnnotationPresent(annotation);
    }

    @Override
    public String toString() {
      return "annotated with @" + annotation.getSimpleName();
    }
  }
  //被获取的匹配的集合
  /** The set of matches being accumulated. */
  private Set<Class<? extends T>> matches = new HashSet<>();

  /**
   * 类加载器为了寻找类使用，如果空，那么这个类加载器返回Thread.currentThread().getContextClassLoader()
   * The ClassLoader to use when looking for classes. If null then the ClassLoader returned
   * by Thread.currentThread().getContextClassLoader() will be used.
   */
  private ClassLoader classloader;

  /**
   * 到目前为止 提供访问被发现的类。如果没有调用完成{@code find()}方法，这个集合将为空
   * Provides access to the classes discovered so far. If no calls have been made to
   * any of the {@code find()} methods, this set will be empty.
   *
   * @return the set of classes that have been discovered.
   */
  public Set<Class<? extends T>> getClasses() {
    return matches;
  }

  /**
   * 返回被使用扫描类的类加载器 。如果没有明确的类加载器被调用设置，则会使用上下文类加载器
   * Returns the classloader that will be used for scanning for classes. If no explicit
   * ClassLoader has been set by the calling, the context class loader will be used.
   *
   * @return the ClassLoader that will be used to scan for classes
   */
  public ClassLoader getClassLoader() {
    return classloader == null ? Thread.currentThread().getContextClassLoader() : classloader;
  }

  /**
   * 当扫描类的时候，应该使用设置明确的类加载。如果没有设置，则默认使用上下文类加载器
   * Sets an explicit ClassLoader that should be used when scanning for classes. If none
   * is set then the context classloader will be used.
   *
   * @param classloader a ClassLoader to use when scanning for classes
   */
  public void setClassLoader(ClassLoader classloader) {
    this.classloader = classloader;
  }

  /**
   * 企图发现分配给提供类型的类。在这个场景中，一个提供这个方法的接口将收集实现。在这个非接口的场景中，子类被收集。可以通过 {@link #getClasses()}.获取类
   * Attempts to discover classes that are assignable to the type provided. In the case
   * that an interface is provided this method will collect implementations. In the case
   * of a non-interface class, subclasses will be collected.  Accumulated classes can be
   * accessed by calling {@link #getClasses()}.
   *
   * @param parent the class of interface to find subclasses or implementations of
   * @param packageNames one or more package names to scan (including subpackages) for classes
   */
  public ResolverUtil<T> findImplementations(Class<?> parent, String... packageNames) {
    if (packageNames == null) {
      return this;
    }

    Test test = new IsA(parent);
    for (String pkg : packageNames) {
      find(test, pkg);
    }

    return this;
  }

  /**
   * 企图发现带有注解的注解类，通过{@link #getClasses()}.获取能被访问的类
   * Attempts to discover classes that are annotated with the annotation. Accumulated
   * classes can be accessed by calling {@link #getClasses()}.
   *
   * @param annotation the annotation that should be present on matching classes
   * @param packageNames one or more package names to scan (including subpackages) for classes
   */
  public ResolverUtil<T> findAnnotated(Class<? extends Annotation> annotation, String... packageNames) {
    if (packageNames == null) {
      return this;
    }

    Test test = new AnnotatedWith(annotation);
    for (String pkg : packageNames) {
      find(test, pkg);
    }

    return this;
  }

  /**
   *
   * 扫描类在提供的包里，并且递减到子包。每个类是被提供给测试，当他被发现的时候。并且如果这个测试返回true，这个类被保留。依然是通过{@link #getClasses()}.获取类
   * Scans for classes starting at the package provided and descending into subpackages.
   * Each class is offered up to the Test as it is discovered, and if the Test returns
   * true the class is retained.  Accumulated classes can be fetched by calling
   * {@link #getClasses()}.
   *
   * @param test an instance of {@link Test} that will be used to filter classes
   * @param packageName the name of the package from which to start scanning for
   *        classes, e.g. {@code net.sourceforge.stripes}
   */
  public ResolverUtil<T> find(Test test, String packageName) {
    String path = getPackagePath(packageName);

    try {
      List<String> children = VFS.getInstance().list(path);
      for (String child : children) {
        if (child.endsWith(".class")) {
          addIfMatching(test, child);
        }
      }
    } catch (IOException ioe) {
      log.error("Could not read package: " + packageName, ioe);
    }

    return this;
  }

  /**
   * {@link ClassLoader#getResources(String)}调用寻找，将包名转化为路径
   * Converts a Java package name to a path that can be looked up with a call to
   * {@link ClassLoader#getResources(String)}.
   *
   * @param packageName The Java package name to convert to a path
   */
  protected String getPackagePath(String packageName) {
    return packageName == null ? null : packageName.replace('.', '/');
  }

  /**
   * 添加被完全指定名的提供的本设计的类到已经解决的类集合，即使是被测试通过的
   * Add the class designated by the fully qualified class name provided to the set of
   * resolved classes if and only if it is approved by the Test supplied.
   *
   * @param test the test used to determine if the class matches
   * @param fqn the fully qualified name of a class
   */
  @SuppressWarnings("unchecked")
  protected void addIfMatching(Test test, String fqn) {
    try {
      String externalName = fqn.substring(0, fqn.indexOf('.')).replace('/', '.');
      ClassLoader loader = getClassLoader();
      if (log.isDebugEnabled()) {
        log.debug("Checking to see if class " + externalName + " matches criteria [" + test + "]");
      }

      Class<?> type = loader.loadClass(externalName);
      if (test.matches(type)) {
        matches.add((Class<T>) type);
      }
    } catch (Throwable t) {
      log.warn("Could not examine class '" + fqn + "'" + " due to a " +
          t.getClass().getName() + " with message: " + t.getMessage());
    }
  }
}

```



## 5.7、org.apache.ibatis.jdbc  Jdbc工具模块

该模块主要是JDBC工具类的封装

### 5.7.1、JDBC的UML图概览

<img src="/Users/didi/Documents/design/mybatis/chapter_5/JDBC/JDBC.png" alt="JDBC" style="zoom:100%;" />

### 5.7.2、RuntimeSqlException运行SQL异常

```java
/**
 * 运行SQL异常
 */
public class RuntimeSqlException extends RuntimeException {

  private static final long serialVersionUID = 5224696788505678598L;
  //重载空构造函数
  public RuntimeSqlException() {
    super();
  }
  //重载传递信息的构造函数
  public RuntimeSqlException(String message) {
    super(message);
  }
  //重载传递信息和Throwable的构造函数
  public RuntimeSqlException(String message, Throwable cause) {
    super(message, cause);
  }
  //重载Throwable的构造函数
  public RuntimeSqlException(Throwable cause) {
    super(cause);
  }

}
```

### 5.7.3、枚举类Null值

```java
/**
 * 枚举类 Null值
 */
public enum Null {
  //布尔类型
  BOOLEAN(new BooleanTypeHandler(), JdbcType.BOOLEAN),
  //自符类型
  //字节类型--tinyint
  BYTE(new ByteTypeHandler(), JdbcType.TINYINT),
  //字符类型--smallint
  SHORT(new ShortTypeHandler(), JdbcType.SMALLINT),
  //整数类型--integer
  INTEGER(new IntegerTypeHandler(), JdbcType.INTEGER),
  //长整数类型--bigint
  LONG(new LongTypeHandler(), JdbcType.BIGINT),
  //单精度类型--float
  FLOAT(new FloatTypeHandler(), JdbcType.FLOAT),
  //双精度类型--double
  DOUBLE(new DoubleTypeHandler(), JdbcType.DOUBLE),
  //商业计算精度用的BIGDECIMAL
  BIGDECIMAL(new BigDecimalTypeHandler(), JdbcType.DECIMAL),

  //文本类型
  //字符串类型
  STRING(new StringTypeHandler(), JdbcType.VARCHAR),
  //字符串大对象--比如文件头等超过varchar信息的内容
  CLOB(new ClobTypeHandler(), JdbcType.CLOB),
  //char是定长字符串（1-255）
  //varchar是非定长字符串（1-32672）
  //Long Varchar也是非定长字符串（1-32700）
  //char是最简单的，系统分配固定长度的空间给它，容易被系统调整空间，性能比较高；
  //Varchar必须要存在于一个Page之内，当varchar数据很常的时候，会形成一条记录跨页的现象，成为Overflow Page现象，这在作Reorgchk的时候是用来判断是否需要Reorg的一个参数，一旦形成跨页存储，性能将降低；
  //LongVarChar在Varchar的长度扩大至32672的时候已经没有太大的意义了，每个Longvarchar在记录中只占24个字节，估计是作为一种索引，真正的内容存放在单独的Page中，这样就可以达到很大的长度。这样的方式致使性能更加降低。
  LONGVARCHAR(new ClobTypeHandler(), JdbcType.LONGVARCHAR),

  //字节数组
  BYTEARRAY(new ByteArrayTypeHandler(), JdbcType.LONGVARBINARY),
  //文本类型
  BLOB(new BlobTypeHandler(), JdbcType.BLOB),
  //长字符串二进制类型
  LONGVARBINARY(new BlobTypeHandler(), JdbcType.LONGVARBINARY),
  
  //对象类型
  OBJECT(new ObjectTypeHandler(), JdbcType.OTHER),
  //其他类型
  OTHER(new ObjectTypeHandler(), JdbcType.OTHER),
  //时间戳类型
  TIMESTAMP(new DateTypeHandler(), JdbcType.TIMESTAMP),
  //时间日期类型
  DATE(new DateOnlyTypeHandler(), JdbcType.DATE),
  //时间类型
  TIME(new TimeOnlyTypeHandler(), JdbcType.TIME),
  //SQL时间戳类型
  SQLTIMESTAMP(new SqlTimestampTypeHandler(), JdbcType.TIMESTAMP),
  //SQL的date类型
  SQLDATE(new SqlDateTypeHandler(), JdbcType.DATE),
  //SQL的时间类型
  SQLTIME(new SqlTimeTypeHandler(), JdbcType.TIME);

  //类型处理器
  private TypeHandler<?> typeHandler;
  //JDBC类型
  private JdbcType jdbcType;
  //有参构造函数
  Null(TypeHandler<?> typeHandler, JdbcType jdbcType) {
    this.typeHandler = typeHandler;
    this.jdbcType = jdbcType;
  }
  //获取类型处理器
  public TypeHandler<?> getTypeHandler() {
    return typeHandler;
  }
 //获取JDBC类型
  public JdbcType getJdbcType() {
    return jdbcType;
  }
}
```

### todo类型可以参考type包

### 5.7.4、SQL类

```java
/**
 * SQL类
 * @author Clinton Begin
 */
public class SQL extends AbstractSQL<SQL> {
 //重写父类getSelf()函数获取本身对象
  @Override
  public SQL getSelf() {
    return this;
  }

}
```

### 5.7.5、SelectBuilder选择构建器（已废弃）

<img src="/Users/didi/Library/Application Support/typora-user-images/image-20200223122120699.png" alt="image-20200223122120699" style="zoom:50%;" />

```java
/**
 * 已经废弃，使用SQL类替代
 */
@Deprecated
public class SelectBuilder {
  //线程本地变量，为啥用ThreadLocal,是为了解决变量共享问题，可以参考9.2.1
  private static final ThreadLocal<SQL> localSQL = new ThreadLocal<>();
  //静态代码块，包含了BEGIN()，类加载的时候调用
  static {
    BEGIN();
  }
  //私有构造函数，防止被外部实例化
  private SelectBuilder() {
    // Prevent Instantiation
  }
  //调用重置函数
  public static void BEGIN() {
    RESET();
  }
  //重置函数调用了线程本地变量存放了SQL对象
  public static void RESET() {
    localSQL.set(new SQL());
  }
  //封装select关键字段
  public static void SELECT(String columns) {
    sql().SELECT(columns);
  }
 //封装SELECT_DISTINCT关键字段，没有依赖使用方，被select替代了
  public static void SELECT_DISTINCT(String columns) {
    sql().SELECT_DISTINCT(columns);
  }
  //封装FROM关键字段
  public static void FROM(String table) {
    sql().FROM(table);
  }
  //封装JOIN关键字段
  public static void JOIN(String join) {
    sql().JOIN(join);
  }
  //封装INNER_JOIN关键字段
  public static void INNER_JOIN(String join) {
    sql().INNER_JOIN(join);
  }
  //封装LEFT_OUTER_JOIN关键字段，没有依赖使用方
  public static void LEFT_OUTER_JOIN(String join) {
    sql().LEFT_OUTER_JOIN(join);
  }
  //封装RIGHT_OUTER_JOIN关键字段，没有依赖使用方
  public static void RIGHT_OUTER_JOIN(String join) {
    sql().RIGHT_OUTER_JOIN(join);
  }
  //封装OUTER_JOIN关键字段，没有依赖使用方
  public static void OUTER_JOIN(String join) {
    sql().OUTER_JOIN(join);
  }
  //封装WHERE关键字段
  public static void WHERE(String conditions) {
    sql().WHERE(conditions);
  }
  //封装OR关键字段
  public static void OR() {
    sql().OR();
  }
  //封装AND关键字段
  public static void AND() {
    sql().AND();
  }
  //封装GROUP_BY关键字段
  public static void GROUP_BY(String columns) {
    sql().GROUP_BY(columns);
  }
  //封装HAVING关键字段
  public static void HAVING(String conditions) {
    sql().HAVING(conditions);
  }
  //封装ORDER_BY关键字段
  public static void ORDER_BY(String columns) {
    sql().ORDER_BY(columns);
  }
  //将线程本地变量的SQL对象转化为string字符串，并重置线程本地变量设置为空SQL对象
  public static String SQL() {
    try {
      return sql().toString();
    } finally {
      RESET();
    }
  }
  //获取本地线程变量存储的SQL对象
  private static SQL sql() {
    return localSQL.get();
  }

}
```

### 5.7.6、SqlBuilder构建器（已废弃）

```java
/**
 * 已经废弃，使用SQL类代替
 * @deprecated Use the {@link SQL} Class
 *
 * @author Jeff Butler
 */
public class SqlBuilder {
  //线程本地变量
  private static final ThreadLocal<SQL> localSQL = new ThreadLocal<>();
  //静态代码块，调用Begin函数
  static {
    BEGIN();
  }
  //阻止外部实例化
  private SqlBuilder() {
    // Prevent Instantiation
  }
  //调用reset函数进行内容重置
  public static void BEGIN() {
    RESET();
  }
  //重置函数主要实现了线程本地变量的sql对象重新赋值
  public static void RESET() {
    localSQL.set(new SQL());
  }
  //Update更新某个表
  public static void UPDATE(String table) {
    sql().UPDATE(table);
  }
 //set某些字段
  public static void SET(String sets) {
    sql().SET(sets);
  }
  //将SQL对象封装字符串并重置
  public static String SQL() {
    try {
      return sql().toString();
    } finally {
        RESET();
    }
  }
  //INSERT_INTO 某个表
  public static void INSERT_INTO(String tableName) {
    sql().INSERT_INTO(tableName);
  }
  // values值
  public static void VALUES(String columns, String values) {
    sql().VALUES(columns, values);
  }
  //查询
  public static void SELECT(String columns) {
    sql().SELECT(columns);
  }
  //查询去重
  public static void SELECT_DISTINCT(String columns) {
    sql().SELECT_DISTINCT(columns);
  }
  //从表删除
  public static void DELETE_FROM(String table) {
    sql().DELETE_FROM(table);
  }
  //来源某个表
  public static void FROM(String table) {
    sql().FROM(table);
  }
  //连表
  public static void JOIN(String join) {
    sql().JOIN(join);
  }
  //内连
  public static void INNER_JOIN(String join) {
    sql().INNER_JOIN(join);
  }
  //左连
  public static void LEFT_OUTER_JOIN(String join) {
    sql().LEFT_OUTER_JOIN(join);
  }
  //右连
  public static void RIGHT_OUTER_JOIN(String join) {
    sql().RIGHT_OUTER_JOIN(join);
  }
  //外连
  public static void OUTER_JOIN(String join) {
    sql().OUTER_JOIN(join);
  }
  //where关键字
  public static void WHERE(String conditions) {
    sql().WHERE(conditions);
  }
  //or关键字
  public static void OR() {
    sql().OR();
  }
  //and关键字
  public static void AND() {
    sql().AND();
  }
  //group_by关键字
  public static void GROUP_BY(String columns) {
    sql().GROUP_BY(columns);
  }
  //having关键字
  public static void HAVING(String conditions) {
    sql().HAVING(conditions);
  }
  //order by关键字
  public static void ORDER_BY(String columns) {
    sql().ORDER_BY(columns);
  }
  //获取线程本地变量
  private static SQL sql() {
    return localSQL.get();
  }

}
```

### 5.7.7、AbstractSQL抽象SQL类

![image-20200223170831243](/Users/didi/Library/Application Support/typora-user-images/image-20200223170831243.png)

#### 5.7.7.1、StatementType枚举类

```java
//定义内部枚举类声明关键字DML操作
    public enum StatementType {
      DELETE, INSERT, SELECT, UPDATE
    }

```

#### 5.7.7.2、LimitingRowsStrategy限制行数策略枚举类

前提提示：主要是应对不同数据库分页的语法区分

```java
//私有枚举类限制函数策略
    private enum LimitingRowsStrategy {
      //不需要分页或者适用于SQLServer数据库
      NOP {
        @Override
        protected void appendClause(SafeAppendable builder, String offset, String limit) {
          // NOP
        }
      },
     //处理DB2数据库
      ISO {
        @Override
        protected void appendClause(SafeAppendable builder, String offset, String limit) {
          if (offset != null) {
            builder.append(" OFFSET ").append(offset).append(" ROWS");
          }
          if (limit != null) {
            builder.append(" FETCH FIRST ").append(limit).append(" ROWS ONLY");
          }
        }
      },

      //处理mysql
      OFFSET_LIMIT {
        @Override
        protected void appendClause(SafeAppendable builder, String offset, String limit) {
          if (limit != null) {
            builder.append(" LIMIT ").append(limit);
          }
          if (offset != null) {
            builder.append(" OFFSET ").append(offset);
          }
        }
      };

      protected abstract void appendClause(SafeAppendable builder, String offset, String limit);

    }
```

【tips】这里的核心点在于枚举类里面进行重写函数的使用，比较新颖值得学习。

#### 5.7.7.3、SQLStatement申明类

```java
 //静态内部私有类
  private static class SQLStatement {
    //定义内部枚举类声明关键字DML操作
    public enum StatementType {
      DELETE, INSERT, SELECT, UPDATE
    }
    //私有枚举类限制函数策略
    private enum LimitingRowsStrategy {
      //不需要分页或者适用于SQLServer数据库
      NOP {
        @Override
        protected void appendClause(SafeAppendable builder, String offset, String limit) {
          // NOP
        }
      },
     //处理DB2数据库
      ISO {
        @Override
        protected void appendClause(SafeAppendable builder, String offset, String limit) {
          if (offset != null) {
            builder.append(" OFFSET ").append(offset).append(" ROWS");
          }
          if (limit != null) {
            builder.append(" FETCH FIRST ").append(limit).append(" ROWS ONLY");
          }
        }
      },

      //处理mysql
      OFFSET_LIMIT {
        @Override
        protected void appendClause(SafeAppendable builder, String offset, String limit) {
          if (limit != null) {
            builder.append(" LIMIT ").append(limit);
          }
          if (offset != null) {
            builder.append(" OFFSET ").append(offset);
          }
        }
      };

      protected abstract void appendClause(SafeAppendable builder, String offset, String limit);

    }
    //DML关键字类型声明
    StatementType statementType;
    //set字段集合
    List<String> sets = new ArrayList<>();
    //select字段集合
    List<String> select = new ArrayList<>();
    //tables集合
    List<String> tables = new ArrayList<>();
    //join集合
    List<String> join = new ArrayList<>();
    //内连join集合
    List<String> innerJoin = new ArrayList<>();
    //外连join集合
    List<String> outerJoin = new ArrayList<>();
    //左连join集合
    List<String> leftOuterJoin = new ArrayList<>();
    //右连join集合
    List<String> rightOuterJoin = new ArrayList<>();
    //where集合
    List<String> where = new ArrayList<>();
    //having集合
    List<String> having = new ArrayList<>();
    //groupBy集合
    List<String> groupBy = new ArrayList<>();
    //orderBy集合
    List<String> orderBy = new ArrayList<>();
    //需要填充的内容值集合
    List<String> lastList = new ArrayList<>();
    //字段列集合
    List<String> columns = new ArrayList<>();
    //字段值集合
    List<List<String>> valuesList = new ArrayList<>();
    //区别boolean值
    boolean distinct;
    //下标
    String offset;
    //行数
    String limit;
    //限制行策略默认不限制
    LimitingRowsStrategy limitingRowsStrategy = LimitingRowsStrategy.NOP;
    //空构造函数初始化字段值
    public SQLStatement() {
      // Prevent Synthetic Access
      valuesList.add(new ArrayList<>());
    }
    //sql约定条款处理builder内容
    private void sqlClause(SafeAppendable builder, String keyword, List<String> parts, String open, String close,
                           String conjunction) {
      //如果关键字不为空
      if (!parts.isEmpty()) {
        //如果构建关键字不为空则追加换行
        if (!builder.isEmpty()) {
          builder.append("\n");
        }
        //构建器追加DML的关键字
        builder.append(keyword);
        //追加空格区分sql语句
        builder.append(" ");
        //追加左边符号
        builder.append(open);
        //封装关键字局部内容
        String last = "________";
        for (int i = 0, n = parts.size(); i < n; i++) {
          String part = parts.get(i);
          if (i > 0 && !part.equals(AND) && !part.equals(OR) && !last.equals(AND) && !last.equals(OR)) {
            builder.append(conjunction);
          }
          builder.append(part);
          last = part;
        }
        //追加右边括号
        builder.append(close);
      }
    }
    //selectSQL
    private String selectSQL(SafeAppendable builder) {
      //如果去重则select拼接去重逻辑
      if (distinct) {
        sqlClause(builder, "SELECT DISTINCT", select, "", "", ", ");
      } else {
        sqlClause(builder, "SELECT", select, "", "", ", ");
      }
      //from拼接
      sqlClause(builder, "FROM", tables, "", "", ", ");
      //连表拼接
      joins(builder);
      //where连接
      sqlClause(builder, "WHERE", where, "(", ")", " AND ");
      //group by连接
      sqlClause(builder, "GROUP BY", groupBy, "", "", ", ");
      //having 连接
      sqlClause(builder, "HAVING", having, "(", ")", " AND ");
      //order by连接
      sqlClause(builder, "ORDER BY", orderBy, "", "", ", ");
      //限制行数策略
      limitingRowsStrategy.appendClause(builder, offset, limit);
      //返回SQL拼接
      return builder.toString();
    }
    //封装连表查询的内容
    private void joins(SafeAppendable builder) {
      sqlClause(builder, "JOIN", join, "", "", "\nJOIN ");
      sqlClause(builder, "INNER JOIN", innerJoin, "", "", "\nINNER JOIN ");
      sqlClause(builder, "OUTER JOIN", outerJoin, "", "", "\nOUTER JOIN ");
      sqlClause(builder, "LEFT OUTER JOIN", leftOuterJoin, "", "", "\nLEFT OUTER JOIN ");
      sqlClause(builder, "RIGHT OUTER JOIN", rightOuterJoin, "", "", "\nRIGHT OUTER JOIN ");
    }
    //插入SQL语句的封装
    private String insertSQL(SafeAppendable builder) {
      sqlClause(builder, "INSERT INTO", tables, "", "", "");
      sqlClause(builder, "", columns, "(", ")", ", ");
      for (int i = 0; i < valuesList.size(); i++) {
        sqlClause(builder, i > 0 ? "," : "VALUES", valuesList.get(i), "(", ")", ", ");
      }
      return builder.toString();
    }
    //删除SQL语句的封装
    private String deleteSQL(SafeAppendable builder) {
      sqlClause(builder, "DELETE FROM", tables, "", "", "");
      sqlClause(builder, "WHERE", where, "(", ")", " AND ");
      limitingRowsStrategy.appendClause(builder, null, limit);
      return builder.toString();
    }
    //更新SQL语句的封装
    private String updateSQL(SafeAppendable builder) {
      sqlClause(builder, "UPDATE", tables, "", "", "");
      joins(builder);
      sqlClause(builder, "SET", sets, "", "", ", ");
      sqlClause(builder, "WHERE", where, "(", ")", " AND ");
      limitingRowsStrategy.appendClause(builder, null, limit);
      return builder.toString();
    }
    //将SQL对象转化为String的SQL语句
    public String sql(Appendable a) {
      SafeAppendable builder = new SafeAppendable(a);
      if (statementType == null) {
        return null;
      }

      String answer;

      switch (statementType) {
        case DELETE:
          answer = deleteSQL(builder);
          break;

        case INSERT:
          answer = insertSQL(builder);
          break;

        case SELECT:
          answer = selectSQL(builder);
          break;

        case UPDATE:
          answer = updateSQL(builder);
          break;

        default:
          answer = null;
      }

      return answer;
    }
  }
```



#### 5.7.7.4、SafeAppendable安全SQL扩展类

```java
//静态内部安全可扩展类，
  private static class SafeAppendable {
    //来源jdk，并非线程安全类，由继承者或者实现者来保护接口的安全性
    private final Appendable a;
    private boolean empty = true;
    //实现了SafeAppendable创建时候，封装的内部为Appendable接口对象
    public SafeAppendable(Appendable a) {
      super();
      this.a = a;
    }
    //追加字符串
    public SafeAppendable append(CharSequence s) {
      try {
        if (empty && s.length() > 0) {
          empty = false;
        }
        a.append(s);
      } catch (IOException e) {
        throw new RuntimeException(e);
      }
      return this;
    }
   //判断是否为空对象
    public boolean isEmpty() {
      return empty;
    }

  }
```

【tips】主要内部实现依赖了Java的Appendable接口类，实现效果与StringBuilder类效果相同，由于Appendable接口类非安全需要由实现的类实现，这里是通过判断当前对象是否为空来使用，如果该对象不为空，则不适用，来保证安全。原理是不共享对象。

#### 5.7.7.5、AbstractSQL源码（合并上述内部类）

```java
/**
 * 抽象SQL类
 * @author Clinton Begin
 * @author Jeff Butler
 * @author Adam Gent
 * @author Kazuki Shimizu
 */
public abstract class AbstractSQL<T> {
  //私有属性 And
  private static final String AND = ") \nAND (";
  //私有属性 or
  private static final String OR = ") \nOR (";
  //私有SQL声明
  private final SQLStatement sql = new SQLStatement();

  public abstract T getSelf();
  //更新函数
  public T UPDATE(String table) {
    sql().statementType = SQLStatement.StatementType.UPDATE;
    sql().tables.add(table);
    return getSelf();
  }
  //set字段
  public T SET(String sets) {
    sql().sets.add(sets);
    return getSelf();
  }

  /**
   * set字段
   * @since 3.4.2
   */
  public T SET(String... sets) {
    sql().sets.addAll(Arrays.asList(sets));
    return getSelf();
  }
  //insert函数
  public T INSERT_INTO(String tableName) {
    sql().statementType = SQLStatement.StatementType.INSERT;
    sql().tables.add(tableName);
    return getSelf();
  }
  //插入VALUES关键字
  public T VALUES(String columns, String values) {
    INTO_COLUMNS(columns);
    INTO_VALUES(values);
    return getSelf();
  }

  /**
   * 插入field列
   * @since 3.4.2
   */
  public T INTO_COLUMNS(String... columns) {
    sql().columns.addAll(Arrays.asList(columns));
    return getSelf();
  }

  /**
   * 插入字段
   * @since 3.4.2
   */
  public T INTO_VALUES(String... values) {
    List<String> list = sql().valuesList.get(sql().valuesList.size() - 1);
    Collections.addAll(list, values);
    return getSelf();
  }
  //查询SQL函数
  public T SELECT(String columns) {
    sql().statementType = SQLStatement.StatementType.SELECT;
    sql().select.add(columns);
    return getSelf();
  }

  /**
   * //查询SQL函数
   * @since 3.4.2
   */
  public T SELECT(String... columns) {
    sql().statementType = SQLStatement.StatementType.SELECT;
    sql().select.addAll(Arrays.asList(columns));
    return getSelf();
  }
  //去重字段
  public T SELECT_DISTINCT(String columns) {
    sql().distinct = true;
    SELECT(columns);
    return getSelf();
  }

  /**
   * 去重字段
   * @since 3.4.2
   */
  public T SELECT_DISTINCT(String... columns) {
    sql().distinct = true;
    SELECT(columns);
    return getSelf();
  }
  //删除表
  public T DELETE_FROM(String table) {
    sql().statementType = SQLStatement.StatementType.DELETE;
    sql().tables.add(table);
    return getSelf();
  }
  //from表追加
  public T FROM(String table) {
    sql().tables.add(table);
    return getSelf();
  }

  /**
   * from表追加
   * @since 3.4.2
   */
  public T FROM(String... tables) {
    sql().tables.addAll(Arrays.asList(tables));
    return getSelf();
  }
  //连表
  public T JOIN(String join) {
    sql().join.add(join);
    return getSelf();
  }

  /**
   * 连表
   * @since 3.4.2
   */
  public T JOIN(String... joins) {
    sql().join.addAll(Arrays.asList(joins));
    return getSelf();
  }
  //内连
  public T INNER_JOIN(String join) {
    sql().innerJoin.add(join);
    return getSelf();
  }

  /**
   * 内连表
   * @since 3.4.2
   */
  public T INNER_JOIN(String... joins) {
    sql().innerJoin.addAll(Arrays.asList(joins));
    return getSelf();
  }
  //左连表
  public T LEFT_OUTER_JOIN(String join) {
    sql().leftOuterJoin.add(join);
    return getSelf();
  }

  /**
   * 左连表
   * @since 3.4.2
   */
  public T LEFT_OUTER_JOIN(String... joins) {
    sql().leftOuterJoin.addAll(Arrays.asList(joins));
    return getSelf();
  }
  //右连表
  public T RIGHT_OUTER_JOIN(String join) {
    sql().rightOuterJoin.add(join);
    return getSelf();
  }

  /**
   * 右连表
   * @since 3.4.2
   */
  public T RIGHT_OUTER_JOIN(String... joins) {
    sql().rightOuterJoin.addAll(Arrays.asList(joins));
    return getSelf();
  }
  //外连表
  public T OUTER_JOIN(String join) {
    sql().outerJoin.add(join);
    return getSelf();
  }

  /**
   * 外连表
   * @since 3.4.2
   */
  public T OUTER_JOIN(String... joins) {
    sql().outerJoin.addAll(Arrays.asList(joins));
    return getSelf();
  }
   //追加where条件
  public T WHERE(String conditions) {
    sql().where.add(conditions);
    sql().lastList = sql().where;
    return getSelf();
  }

  /**
   * 追加where条件
   * @since 3.4.2
   */
  public T WHERE(String... conditions) {
    sql().where.addAll(Arrays.asList(conditions));
    sql().lastList = sql().where;
    return getSelf();
  }
  //追加or
  public T OR() {
    sql().lastList.add(OR);
    return getSelf();
  }
  //追加and
  public T AND() {
    sql().lastList.add(AND);
    return getSelf();
  }
  //给group by增加一列属性
  public T GROUP_BY(String columns) {
    sql().groupBy.add(columns);
    return getSelf();
  }

  /**
   * 给group by增加多列排序
   * @since 3.4.2
   */
  public T GROUP_BY(String... columns) {
    sql().groupBy.addAll(Arrays.asList(columns));
    return getSelf();
  }
  //给having增加单独条件属性
  public T HAVING(String conditions) {
    sql().having.add(conditions);
    sql().lastList = sql().having;
    return getSelf();
  }

  /**
   * 给having增加条件
   * @since 3.4.2
   */
  public T HAVING(String... conditions) {
    sql().having.addAll(Arrays.asList(conditions));
    sql().lastList = sql().having;
    return getSelf();
  }
  //给order by增加一列属性
  public T ORDER_BY(String columns) {
    sql().orderBy.add(columns);
    return getSelf();
  }

  /**
   * 给order by增加列字段属性
   * @since 3.4.2
   */
  public T ORDER_BY(String... columns) {
    sql().orderBy.addAll(Arrays.asList(columns));
    return getSelf();
  }

  /**
   * 给#{limit}赋值内容
   * Set the limit variable string(e.g. {@code "#{limit}"}).
   *
   * @param variable a limit variable string
   * @return a self instance
   * @see #OFFSET(String)
   * @since 3.5.2
   */
  public T LIMIT(String variable) {
    sql().limit = variable;
    sql().limitingRowsStrategy = SQLStatement.LimitingRowsStrategy.OFFSET_LIMIT;
    return getSelf();
  }

  /**
   * 设置limit值
   * Set the limit value.
   *
   * @param value an offset value
   * @return a self instance
   * @see #OFFSET(long)
   * @since 3.5.2
   */
  public T LIMIT(int value) {
    return LIMIT(String.valueOf(value));
  }

  /**
   * 给游标#{offset}赋值内容
   * Set the offset variable string(e.g. {@code "#{offset}"}).
   *
   * @param variable a offset variable string
   * @return a self instance
   * @see #LIMIT(String)
   * @since 3.5.2
   */
  public T OFFSET(String variable) {
    sql().offset = variable;
    sql().limitingRowsStrategy = SQLStatement.LimitingRowsStrategy.OFFSET_LIMIT;
    return getSelf();
  }

  /**
   * 设置游标值
   * Set the offset value.
   *
   * @param value an offset value
   * @return a self instance
   * @see #LIMIT(int)
   * @since 3.5.2
   */
  public T OFFSET(long value) {
    return OFFSET(String.valueOf(value));
  }

  /**
   * Set the fetch first rows variable string(e.g. {@code "#{fetchFirstRows}"}).
   * 给fetchFirstRows变量赋值
   * @param variable a fetch first rows variable string
   * @return a self instance
   * @see #OFFSET_ROWS(String)
   * @since 3.5.2
   */
  public T FETCH_FIRST_ROWS_ONLY(String variable) {
    sql().limit = variable;
    sql().limitingRowsStrategy = SQLStatement.LimitingRowsStrategy.ISO;
    return getSelf();
  }

  /**
   * 用于DB2数据库的分页查询返回值
   * Set the fetch first rows value.
   *
   * @param value a fetch first rows value
   * @return a self instance
   * @see #OFFSET_ROWS(long)
   * @since 3.5.2
   */
  public T FETCH_FIRST_ROWS_ONLY(int value) {
    return FETCH_FIRST_ROWS_ONLY(String.valueOf(value));
  }

  /**
   * 用于DB2数据库的分页下标查询
   * Set the offset rows variable string(e.g. {@code "#{offset}"}).
   *
   * @param variable a offset rows variable string
   * @return a self instance
   * @see #FETCH_FIRST_ROWS_ONLY(String)
   * @since 3.5.2
   */
  public T OFFSET_ROWS(String variable) {
    sql().offset = variable;
    sql().limitingRowsStrategy = SQLStatement.LimitingRowsStrategy.ISO;
    return getSelf();
  }

  /**
   * 设置行数查询的下标
   * Set the offset rows value.
   *
   * @param value an offset rows value
   * @return a self instance
   * @see #FETCH_FIRST_ROWS_ONLY(int)
   * @since 3.5.2
   */
  public T OFFSET_ROWS(long value) {
    return OFFSET_ROWS(String.valueOf(value));
  }

  /*
   * used to add a new inserted row while do multi-row insert.
   * 用于多行插入的时候，多添加一行SQL
   * @since 3.5.2
   */
  public T ADD_ROW() {
    sql().valuesList.add(new ArrayList<>());
    return getSelf();
  }

  private SQLStatement sql() {
    return sql;
  }
  //使用usingAppender函数返回原对象
  public <A extends Appendable> A usingAppender(A a) {
    sql().sql(a);
    return a;
  }

  @Override
  public String toString() {
    StringBuilder sb = new StringBuilder();
    sql().sql(sb);
    return sb.toString();
  }
  //静态内部安全可扩展类，
  private static class SafeAppendable {
    //来源jdk，并非线程安全类，由继承者或者实现者来保护接口的安全性
    private final Appendable a;
    private boolean empty = true;
    //实现了SafeAppendable创建时候，封装的内部为Appendable接口对象
    public SafeAppendable(Appendable a) {
      super();
      this.a = a;
    }
    //追加字符串
    public SafeAppendable append(CharSequence s) {
      try {
        if (empty && s.length() > 0) {
          empty = false;
        }
        a.append(s);
      } catch (IOException e) {
        throw new RuntimeException(e);
      }
      return this;
    }
   //判断是否为空对象
    public boolean isEmpty() {
      return empty;
    }

  }
  //静态内部私有类
  private static class SQLStatement {
    //定义内部枚举类声明关键字DML操作
    public enum StatementType {
      DELETE, INSERT, SELECT, UPDATE
    }
    //私有枚举类限制函数策略
    private enum LimitingRowsStrategy {
      //不需要分页或者适用于SQLServer数据库
      NOP {
        @Override
        protected void appendClause(SafeAppendable builder, String offset, String limit) {
          // NOP
        }
      },
     //处理DB2数据库
      ISO {
        @Override
        protected void appendClause(SafeAppendable builder, String offset, String limit) {
          if (offset != null) {
            builder.append(" OFFSET ").append(offset).append(" ROWS");
          }
          if (limit != null) {
            builder.append(" FETCH FIRST ").append(limit).append(" ROWS ONLY");
          }
        }
      },

      //处理mysql
      OFFSET_LIMIT {
        @Override
        protected void appendClause(SafeAppendable builder, String offset, String limit) {
          if (limit != null) {
            builder.append(" LIMIT ").append(limit);
          }
          if (offset != null) {
            builder.append(" OFFSET ").append(offset);
          }
        }
      };

      protected abstract void appendClause(SafeAppendable builder, String offset, String limit);

    }
    //DML关键字类型声明
    StatementType statementType;
    //set字段集合
    List<String> sets = new ArrayList<>();
    //select字段集合
    List<String> select = new ArrayList<>();
    //tables集合
    List<String> tables = new ArrayList<>();
    //join集合
    List<String> join = new ArrayList<>();
    //内连join集合
    List<String> innerJoin = new ArrayList<>();
    //外连join集合
    List<String> outerJoin = new ArrayList<>();
    //左连join集合
    List<String> leftOuterJoin = new ArrayList<>();
    //右连join集合
    List<String> rightOuterJoin = new ArrayList<>();
    //where集合
    List<String> where = new ArrayList<>();
    //having集合
    List<String> having = new ArrayList<>();
    //groupBy集合
    List<String> groupBy = new ArrayList<>();
    //orderBy集合
    List<String> orderBy = new ArrayList<>();
    //
    List<String> lastList = new ArrayList<>();
    //字段列集合
    List<String> columns = new ArrayList<>();
    //字段值集合
    List<List<String>> valuesList = new ArrayList<>();
    //区别boolean值
    boolean distinct;
    //下标
    String offset;
    //行数
    String limit;
    //限制行策略默认不限制
    LimitingRowsStrategy limitingRowsStrategy = LimitingRowsStrategy.NOP;
    //空构造函数初始化字段值
    public SQLStatement() {
      // Prevent Synthetic Access
      valuesList.add(new ArrayList<>());
    }
    //sql约定条款处理builder内容
    private void sqlClause(SafeAppendable builder, String keyword, List<String> parts, String open, String close,
                           String conjunction) {
      //如果关键字不为空
      if (!parts.isEmpty()) {
        //如果构建关键字不为空则追加换行
        if (!builder.isEmpty()) {
          builder.append("\n");
        }
        //构建器追加DML的关键字
        builder.append(keyword);
        //追加空格区分sql语句
        builder.append(" ");
        //追加左边符号
        builder.append(open);
        //封装关键字局部内容
        String last = "________";
        for (int i = 0, n = parts.size(); i < n; i++) {
          String part = parts.get(i);
          if (i > 0 && !part.equals(AND) && !part.equals(OR) && !last.equals(AND) && !last.equals(OR)) {
            builder.append(conjunction);
          }
          builder.append(part);
          last = part;
        }
        //追加右边括号
        builder.append(close);
      }
    }
    //selectSQL
    private String selectSQL(SafeAppendable builder) {
      //如果去重则select拼接去重逻辑
      if (distinct) {
        sqlClause(builder, "SELECT DISTINCT", select, "", "", ", ");
      } else {
        sqlClause(builder, "SELECT", select, "", "", ", ");
      }
      //from拼接
      sqlClause(builder, "FROM", tables, "", "", ", ");
      //连表拼接
      joins(builder);
      //where连接
      sqlClause(builder, "WHERE", where, "(", ")", " AND ");
      //group by连接
      sqlClause(builder, "GROUP BY", groupBy, "", "", ", ");
      //having 连接
      sqlClause(builder, "HAVING", having, "(", ")", " AND ");
      //order by连接
      sqlClause(builder, "ORDER BY", orderBy, "", "", ", ");
      //限制行数策略
      limitingRowsStrategy.appendClause(builder, offset, limit);
      //返回SQL拼接
      return builder.toString();
    }
    //封装连表查询的内容
    private void joins(SafeAppendable builder) {
      sqlClause(builder, "JOIN", join, "", "", "\nJOIN ");
      sqlClause(builder, "INNER JOIN", innerJoin, "", "", "\nINNER JOIN ");
      sqlClause(builder, "OUTER JOIN", outerJoin, "", "", "\nOUTER JOIN ");
      sqlClause(builder, "LEFT OUTER JOIN", leftOuterJoin, "", "", "\nLEFT OUTER JOIN ");
      sqlClause(builder, "RIGHT OUTER JOIN", rightOuterJoin, "", "", "\nRIGHT OUTER JOIN ");
    }
    //插入SQL语句的封装
    private String insertSQL(SafeAppendable builder) {
      sqlClause(builder, "INSERT INTO", tables, "", "", "");
      sqlClause(builder, "", columns, "(", ")", ", ");
      for (int i = 0; i < valuesList.size(); i++) {
        sqlClause(builder, i > 0 ? "," : "VALUES", valuesList.get(i), "(", ")", ", ");
      }
      return builder.toString();
    }
    //删除SQL语句的封装
    private String deleteSQL(SafeAppendable builder) {
      sqlClause(builder, "DELETE FROM", tables, "", "", "");
      sqlClause(builder, "WHERE", where, "(", ")", " AND ");
      limitingRowsStrategy.appendClause(builder, null, limit);
      return builder.toString();
    }
    //更新SQL语句的封装
    private String updateSQL(SafeAppendable builder) {
      sqlClause(builder, "UPDATE", tables, "", "", "");
      joins(builder);
      sqlClause(builder, "SET", sets, "", "", ", ");
      sqlClause(builder, "WHERE", where, "(", ")", " AND ");
      limitingRowsStrategy.appendClause(builder, null, limit);
      return builder.toString();
    }
    //将SQL对象转化为String的SQL语句
    public String sql(Appendable a) {
      SafeAppendable builder = new SafeAppendable(a);
      if (statementType == null) {
        return null;
      }

      String answer;

      switch (statementType) {
        case DELETE:
          answer = deleteSQL(builder);
          break;

        case INSERT:
          answer = insertSQL(builder);
          break;

        case SELECT:
          answer = selectSQL(builder);
          break;

        case UPDATE:
          answer = updateSQL(builder);
          break;

        default:
          answer = null;
      }

      return answer;
    }
  }
}
```

#### 5.7.7.6、归纳小结

从之前的源码SelectBuilder和SqlBuilder迁移到SQL类的使用，而SQL类完全实现类抽象父类的功能，核心类为AbstractSQL，他符合了高内聚，低耦合，以及抽象封装的特性。

### 5.7.8、ScriptRunner脚本管理器

```java
/**
 * 脚本管理器
 * @author Clinton Begin
 */
public class ScriptRunner {
  //行分割定义
  private static final String LINE_SEPARATOR = System.getProperty("line.separator", "\n");
  //默认封号定义
  private static final String DEFAULT_DELIMITER = ";";
  //默认分割符匹配
  private static final Pattern DELIMITER_PATTERN = Pattern.compile("^\\s*((--)|(//))?\\s*(//)?\\s*@DELIMITER\\s+([^\\s]+)", Pattern.CASE_INSENSITIVE);
  //JDBC连接
  private final Connection connection;
  //停止错误标识
  private boolean stopOnError;
  //抛出警告标识
  private boolean throwWarning;
  //自动提交标识
  private boolean autoCommit;
  //发送完整的脚本标识
  private boolean sendFullScript;
  //移除CRs标识
  private boolean removeCRs;
  //转义标识
  private boolean escapeProcessing = true;
  //系统输出器
  private PrintWriter logWriter = new PrintWriter(System.out);
  //系统error输出器
  private PrintWriter errorLogWriter = new PrintWriter(System.err);
  //默认的分割符
  private String delimiter = DEFAULT_DELIMITER;
  //全行分割标识
  private boolean fullLineDelimiter;
  //脚本管理器的构造函数，赋值connection链接
  public ScriptRunner(Connection connection) {
    this.connection = connection;
  }

  //下面是set方法
  public void setStopOnError(boolean stopOnError) {
    this.stopOnError = stopOnError;
  }

  public void setThrowWarning(boolean throwWarning) {
    this.throwWarning = throwWarning;
  }

  public void setAutoCommit(boolean autoCommit) {
    this.autoCommit = autoCommit;
  }

  public void setSendFullScript(boolean sendFullScript) {
    this.sendFullScript = sendFullScript;
  }

  public void setRemoveCRs(boolean removeCRs) {
    this.removeCRs = removeCRs;
  }

  /**
   * @since 3.1.1
   */
  public void setEscapeProcessing(boolean escapeProcessing) {
    this.escapeProcessing = escapeProcessing;
  }

  public void setLogWriter(PrintWriter logWriter) {
    this.logWriter = logWriter;
  }

  public void setErrorLogWriter(PrintWriter errorLogWriter) {
    this.errorLogWriter = errorLogWriter;
  }

  public void setDelimiter(String delimiter) {
    this.delimiter = delimiter;
  }

  public void setFullLineDelimiter(boolean fullLineDelimiter) {
    this.fullLineDelimiter = fullLineDelimiter;
  }

  //运行脚本调用方为Test测试类
  public void runScript(Reader reader) {
    //设置自动提交
    setAutoCommit();

    try {
      //如果全部脚本
      if (sendFullScript) {
        //执行全部脚本
        executeFullScript(reader);
      } else {
        //按行执行脚本
        executeLineByLine(reader);
      }
    } finally {
      //回滚链接
      rollbackConnection();
    }
  }
//执行全量脚本
  private void executeFullScript(Reader reader) {
    //脚本构建器
    StringBuilder script = new StringBuilder();
    try {
      //获取读到的数据
      BufferedReader lineReader = new BufferedReader(reader);
      String line;
      while ((line = lineReader.readLine()) != null) {
        script.append(line);
        script.append(LINE_SEPARATOR);
      }
      //脚本转化为命令行
      String command = script.toString();
      //输出SQL命令
      println(command);
      //执行SQL语句
      executeStatement(command);
      //提交链接
      commitConnection();
    } catch (Exception e) {
      String message = "Error executing: " + script + ".  Cause: " + e;
      printlnError(message);
      throw new RuntimeSqlException(message, e);
    }
  }
  //按行执行
  private void executeLineByLine(Reader reader) {
    StringBuilder command = new StringBuilder();
    try {
      //获取io流数据
      BufferedReader lineReader = new BufferedReader(reader);
      String line;
      while ((line = lineReader.readLine()) != null) {
        //处理每行命令
        handleLine(command, line);
      }
      //提交链接
      commitConnection();
      //检查是否丢失行内容命令
      checkForMissingLineTerminator(command);
    } catch (Exception e) {
      String message = "Error executing: " + command + ".  Cause: " + e;
      printlnError(message);
      throw new RuntimeSqlException(message, e);
    }
  }

  /**
   * 废弃该方法，请通过这个类外面的connection关闭
   * @deprecated Since 3.5.4, this method is deprecated. Please close the {@link Connection} outside of this class.
   */
  @Deprecated
  public void closeConnection() {
    try {
      connection.close();
    } catch (Exception e) {
      // ignore
    }
  }
  //设置自动提交
  private void setAutoCommit() {
    try {
      if (autoCommit != connection.getAutoCommit()) {
        connection.setAutoCommit(autoCommit);
      }
    } catch (Throwable t) {
      throw new RuntimeSqlException("Could not set AutoCommit to " + autoCommit + ". Cause: " + t, t);
    }
  }
  //手动提交链接
  private void commitConnection() {
    try {
      if (!connection.getAutoCommit()) {
        connection.commit();
      }
    } catch (Throwable t) {
      throw new RuntimeSqlException("Could not commit transaction. Cause: " + t, t);
    }
  }
  //回滚链接
  private void rollbackConnection() {
    try {
      if (!connection.getAutoCommit()) {
        connection.rollback();
      }
    } catch (Throwable t) {
      // ignore
    }
  }
  //检查是否有中断的命令行
  private void checkForMissingLineTerminator(StringBuilder command) {
    if (command != null && command.toString().trim().length() > 0) {
      throw new RuntimeSqlException("Line missing end-of-line terminator (" + delimiter + ") => " + command);
    }
  }
  //处理每行命令
  private void handleLine(StringBuilder command, String line) throws SQLException {
    String trimmedLine = line.trim();
    if (lineIsComment(trimmedLine)) {
      Matcher matcher = DELIMITER_PATTERN.matcher(trimmedLine);
      if (matcher.find()) {
        delimiter = matcher.group(5);
      }
      println(trimmedLine);
    } else if (commandReadyToExecute(trimmedLine)) {
      command.append(line.substring(0, line.lastIndexOf(delimiter)));
      command.append(LINE_SEPARATOR);
      println(command);
      executeStatement(command.toString());
      command.setLength(0);
    } else if (trimmedLine.length() > 0) {
      command.append(line);
      command.append(LINE_SEPARATOR);
    }
  }
  //行是内容
  private boolean lineIsComment(String trimmedLine) {
    return trimmedLine.startsWith("//") || trimmedLine.startsWith("--");
  }
  //准备执行的命令行
  private boolean commandReadyToExecute(String trimmedLine) {
    // issue #561 remove anything after the delimiter
    return !fullLineDelimiter && trimmedLine.contains(delimiter) || fullLineDelimiter && trimmedLine.equals(delimiter);
  }
  //执行SQL会话
  private void executeStatement(String command) throws SQLException {
    //创建SQL会话
    Statement statement = connection.createStatement();
    try {
      //处理转义字符
      statement.setEscapeProcessing(escapeProcessing);
      String sql = command;
      //是否移除回车符
      if (removeCRs) {
        sql = sql.replaceAll("\r\n", "\n");
      }
      try {
        //执行SQL获取结果
        boolean hasResults = statement.execute(sql);
        while (!(!hasResults && statement.getUpdateCount() == -1)) {
          checkWarnings(statement);
          //输出结果
          printResults(statement, hasResults);
          //判断是否还有结果
          hasResults = statement.getMoreResults();
        }
      } catch (SQLWarning e) {
        throw e;
      } catch (SQLException e) {
        if (stopOnError) {
          throw e;
        } else {
          String message = "Error executing: " + command + ".  Cause: " + e;
          printlnError(message);
        }
      }
    } finally {
      try {
        //关闭会话
        statement.close();
      } catch (Exception ignored) {
        // Ignore to workaround a bug in some connection pools
        // (Does anyone know the details of the bug?)
      }
    }
  }
  //检查是否有语法警告
  private void checkWarnings(Statement statement) throws SQLException {
    if (!throwWarning) {
      return;
    }
    // In Oracle, CREATE PROCEDURE, FUNCTION, etc. returns warning
    // instead of throwing exception if there is compilation error.
    //在Oracle数据库中，CREATE PROCEDURE, FUNCTION等，如果编译错误会返回警告而不是抛出异常
    SQLWarning warning = statement.getWarnings();
    if (warning != null) {
      throw warning;
    }
  }
  //输出结果
  private void printResults(Statement statement, boolean hasResults) {
    //判断是否有数据
    if (!hasResults) {
      return;
    }
    //会话获取数据结果
    try (ResultSet rs = statement.getResultSet()) {
      ResultSetMetaData md = rs.getMetaData();
      int cols = md.getColumnCount();
      for (int i = 0; i < cols; i++) {
        String name = md.getColumnLabel(i + 1);
        print(name + "\t");
      }
      println("");
      while (rs.next()) {
        for (int i = 0; i < cols; i++) {
          String value = rs.getString(i + 1);
          print(value + "\t");
        }
        println("");
      }
    } catch (SQLException e) {
      printlnError("Error printing results: " + e.getMessage());
    }
  }
  //打印日志对象
  private void print(Object o) {
    if (logWriter != null) {
      logWriter.print(o);
      logWriter.flush();
    }
  }
 //换行打印日志对象
  private void println(Object o) {
    if (logWriter != null) {
      logWriter.println(o);
      logWriter.flush();
    }
  }
  //打印错误日志信息
  private void printlnError(Object o) {
    if (errorLogWriter != null) {
      errorLogWriter.println(o);
      errorLogWriter.flush();
    }
  }

}

```

【tips】其实主要实现了命令行的命令转化为BufferReader对象并封装JDBC链接与数据库交互。

### 5.7.9、SqlRunner管理器

```java
/**
 * SQL管理器
 * @author Clinton Begin
 */
public class SqlRunner {
  //不生成key
  public static final int NO_GENERATED_KEY = Integer.MIN_VALUE + 1001;
  //jdbc链接器
  private final Connection connection;
  //类型处理注册器
  private final TypeHandlerRegistry typeHandlerRegistry;
  //是否生成主键key
  private boolean useGeneratedKeySupport;
  //脚本管理器 构造函数 赋值链接器和类型处理注册器
  public SqlRunner(Connection connection) {
    this.connection = connection;
    this.typeHandlerRegistry = new TypeHandlerRegistry();
  }
  //设置是否生成主键key
  public void setUseGeneratedKeySupport(boolean useGeneratedKeySupport) {
    this.useGeneratedKeySupport = useGeneratedKeySupport;
  }

  /**
   * 执行Select查询会话并返回单条数据，如果返回多条抛出异常
   * Executes a SELECT statement that returns one row.
   *
   * @param sql  The SQL
   * @param args The arguments to be set on the statement.
   * @return The row expected.
   * @throws SQLException If less or more than one row is returned
   */
  public Map<String, Object> selectOne(String sql, Object... args) throws SQLException {
    List<Map<String, Object>> results = selectAll(sql, args);
    if (results.size() != 1) {
      throw new SQLException("Statement returned " + results.size() + " results where exactly one (1) was expected.");
    }
    return results.get(0);
  }

  /**
   * 执行select 查询并返回多行数据
   * Executes a SELECT statement that returns multiple rows.
   *
   * @param sql  The SQL
   * @param args The arguments to be set on the statement.
   * @return The list of rows expected.
   * @throws SQLException If statement preparation or execution fails
   */
  public List<Map<String, Object>> selectAll(String sql, Object... args) throws SQLException {
    //建立预会话
    PreparedStatement ps = connection.prepareStatement(sql);
    try {
      //设置参数
      setParameters(ps, args);
      //执行会话查询
      ResultSet rs = ps.executeQuery();
      //返回查询的结果集
      return getResults(rs);
    } finally {
      try {
        //会话关闭
        ps.close();
      } catch (SQLException e) {
        //ignore
      }
    }
  }

  /**
   * 执行insert插入会话
   * Executes an INSERT statement.
   *
   * @param sql  The SQL
   * @param args The arguments to be set on the statement.
   * @return The number of rows impacted or BATCHED_RESULTS if the statements are being batched.
   * @throws SQLException If statement preparation or execution fails
   */
  public int insert(String sql, Object... args) throws SQLException {
    PreparedStatement ps;
    //判断是否需要生成主键ID
    if (useGeneratedKeySupport) {
      ps = connection.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);
    } else {
      ps = connection.prepareStatement(sql);
    }

    try {
      //设置参数
      setParameters(ps, args);
      //执行更新操作
      ps.executeUpdate();
      //如果支持生成主键ID
      if (useGeneratedKeySupport) {
        //获取生成的key
        List<Map<String, Object>> keys = getResults(ps.getGeneratedKeys());
        //如果只生成唯一的key
        if (keys.size() == 1) {
          //获取该值
          Map<String, Object> key = keys.get(0);
          //迭代该对象
          Iterator<Object> i = key.values().iterator();
          if (i.hasNext()) {
            Object genkey = i.next();
            if (genkey != null) {
              try {
                //返回该值
                return Integer.parseInt(genkey.toString());
              } catch (NumberFormatException e) {
                //ignore, no numeric key support
              }
            }
          }
        }
      }
      //返回不生成主键ID的定义值
      return NO_GENERATED_KEY;
    } finally {
      try {
        //关闭会话
        ps.close();
      } catch (SQLException e) {
        //ignore
      }
    }
  }

  /**
   * 执行更新会话
   * Executes an UPDATE statement.
   *
   * @param sql  The SQL
   * @param args The arguments to be set on the statement.
   * @return The number of rows impacted or BATCHED_RESULTS if the statements are being batched.
   * @throws SQLException If statement preparation or execution fails
   */
  public int update(String sql, Object... args) throws SQLException {
    //建立预会话
    PreparedStatement ps = connection.prepareStatement(sql);
    try {
      //设置参数
      setParameters(ps, args);
      //执行更新会话操作
      return ps.executeUpdate();
    } finally {
      try {
        //会话关闭
        ps.close();
      } catch (SQLException e) {
        //ignore
      }
    }
  }

  /**
   * 执行删除会话操作
   * Executes a DELETE statement.
   *
   * @param sql  The SQL
   * @param args The arguments to be set on the statement.
   * @return The number of rows impacted or BATCHED_RESULTS if the statements are being batched.
   * @throws SQLException If statement preparation or execution fails
   */
  public int delete(String sql, Object... args) throws SQLException {
    return update(sql, args);
  }

  /**
   * 执行作为JDBC的会话，方便DDL语句的执行
   * Executes any string as a JDBC Statement.
   * Good for DDL
   *
   * @param sql The SQL
   * @throws SQLException If statement preparation or execution fails
   */
  public void run(String sql) throws SQLException {
    //链接创建会话
    Statement stmt = connection.createStatement();
    try {
      //直接执行SQL
      stmt.execute(sql);
    } finally {
      try {
        //会话关闭
        stmt.close();
      } catch (SQLException e) {
        //ignore
      }
    }
  }

  /**
   * 废弃关闭该方法，可以使用connection类关闭  
   *  @deprecated Since 3.5.4, this method is deprecated. Please close the {@link Connection} outside of this class.
   */
  @Deprecated
  public void closeConnection() {
    try {
      connection.close();
    } catch (SQLException e) {
      //ignore
    }
  }
  //设置参数
  private void setParameters(PreparedStatement ps, Object... args) throws SQLException {
    for (int i = 0, n = args.length; i < n; i++) {
      if (args[i] == null) {
        throw new SQLException("SqlRunner requires an instance of Null to represent typed null values for JDBC compatibility");
        //如果是封装的关键字类型，则用类型处理器处理
      } else if (args[i] instanceof Null) {
        ((Null) args[i]).getTypeHandler().setParameter(ps, i + 1, null, ((Null) args[i]).getJdbcType());
      } else {
        //通过类型处理注册器获取类型处理器，然后处理对应的参数
        TypeHandler typeHandler = typeHandlerRegistry.getTypeHandler(args[i].getClass());
        if (typeHandler == null) {
          throw new SQLException("SqlRunner could not find a TypeHandler instance for " + args[i].getClass());
        } else {
          typeHandler.setParameter(ps, i + 1, args[i], null);
        }
      }
    }
  }
  //获取返回的数据结果
  private List<Map<String, Object>> getResults(ResultSet rs) throws SQLException {
    try {
      //返回的结果集
      List<Map<String, Object>> list = new ArrayList<>();
      //列集合
      List<String> columns = new ArrayList<>();
      //类型处理器集合
      List<TypeHandler<?>> typeHandlers = new ArrayList<>();
      //获取元数据
      ResultSetMetaData rsmd = rs.getMetaData();
      //根据列数量进行列处理
      for (int i = 0, n = rsmd.getColumnCount(); i < n; i++) {
        columns.add(rsmd.getColumnLabel(i + 1));
        try {
          //获取类类型
          Class<?> type = Resources.classForName(rsmd.getColumnClassName(i + 1));
          //根据类型获取类型处理器
          TypeHandler<?> typeHandler = typeHandlerRegistry.getTypeHandler(type);
          //如果为空，则根据对象获取Object类型的处理器
          if (typeHandler == null) {
            typeHandler = typeHandlerRegistry.getTypeHandler(Object.class);
          }
          //存放于类型处理器的集合
          typeHandlers.add(typeHandler);
        } catch (Exception e) {
          typeHandlers.add(typeHandlerRegistry.getTypeHandler(Object.class));
        }
      }
      //循环处理结果数据
      while (rs.next()) {
        Map<String, Object> row = new HashMap<>();
        //获取每列数据
        for (int i = 0, n = columns.size(); i < n; i++) {
          String name = columns.get(i);
          //根据名称类型处理器获取处理器
          TypeHandler<?> handler = typeHandlers.get(i);
          //将列名和处理器获取的结果数据存放行内
          row.put(name.toUpperCase(Locale.ENGLISH), handler.getResult(rs, name));
        }
        //添加到返回结果内 行成key-value键值对应
        list.add(row);
      }
      return list;
    } finally {
      if (rs != null) {
        try {
          //会话关闭
          rs.close();
        } catch (Exception e) {
          // ignore
        }
      }
    }
  }

}
```

【tips】这里借鉴之处就是类型处理器的注册器，然后将不同类型处理器进行注册，并在找到对应的值与其解析。

### 5.7.10、归纳小结

1. 两个已废弃的SQL内容构建器selectBuilder和SQL关键字构建器SQLBuilder统一由SQL替代。

2. 该包下核心类AbstractSQL类，符合了高内聚，低耦合，以及抽象封装的特性。并符合模版方法设计模式。可以参考9.3设计模式部分。

3. 设计模式的七大原则的如何设计应用可以借鉴

   

## 5.8、org.apache.ibatis.lang （ Java版本模块）



```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ ElementType.TYPE, ElementType.METHOD, ElementType.FIELD })
public @interface UsesJava8 {
}
```

【tips】注解的含义解析，<a href="https://blog.csdn.net/wolf_love666/article/details/85009082">链接</a>

## 5.9、org.apache.ibatis.logging（日志工具模块）

![image-20200314172825139](/Users/didi/Library/Application Support/typora-user-images/image-20200314172825139.png)

### 5.9.1、基础

#### 5.9.1.1、Log接口

```java
/**
 * 日志接口
 * @author Clinton Begin
 */
public interface Log {
  //是否开启debug模式
  boolean isDebugEnabled();
  //是否开启trace模式
  boolean isTraceEnabled();
  //输出错误信息
  void error(String s, Throwable e);
  //输出错误信息
  void error(String s);
  //输出debug信息
  void debug(String s);
  //输出trace信息
  void trace(String s);
  //输出war信息
  void warn(String s);

}
```

#### 5.9.1.2、LogFactory日志工厂

```java
/**
 * 日志工厂
 * @author Clinton Begin
 * @author Eduardo Macarron
 */
public final class LogFactory {

  /**
   * 被日志实现的类使用
   * Marker to be used by logging implementations that support markers.
   */
  public static final String MARKER = "MYBATIS";
  //日志构造器
  private static Constructor<? extends Log> logConstructor;
  //静态代码初始化
  static {
    //SLF4j
    tryImplementation(LogFactory::useSlf4jLogging);
    //CommonsLog
    tryImplementation(LogFactory::useCommonsLogging);
    //Log4j2
    tryImplementation(LogFactory::useLog4J2Logging);
    //Log4j
    tryImplementation(LogFactory::useLog4JLogging);
    //JdkLog
    tryImplementation(LogFactory::useJdkLogging);
    //NoLog
    tryImplementation(LogFactory::useNoLogging);
  }
  //阻止外部实例化
  private LogFactory() {
    // disable construction
  }
  //根据类获取日志接口
  public static Log getLog(Class<?> aClass) {
    return getLog(aClass.getName());
  }
  //根据类名获取Log
  public static Log getLog(String logger) {
    try {
      return logConstructor.newInstance(logger);
    } catch (Throwable t) {
      throw new LogException("Error creating logger for logger " + logger + ".  Cause: " + t, t);
    }
  }
  //使用定制的日志
  public static synchronized void useCustomLogging(Class<? extends Log> clazz) {
    setImplementation(clazz);
  }
  //使用slf4j
  public static synchronized void useSlf4jLogging() {
    setImplementation(org.apache.ibatis.logging.slf4j.Slf4jImpl.class);
  }
  //使用公共日志
  public static synchronized void useCommonsLogging() {
    setImplementation(org.apache.ibatis.logging.commons.JakartaCommonsLoggingImpl.class);
  }
  //使用log4j
  public static synchronized void useLog4JLogging() {
    setImplementation(org.apache.ibatis.logging.log4j.Log4jImpl.class);
  }
  //使用log4j2
  public static synchronized void useLog4J2Logging() {
    setImplementation(org.apache.ibatis.logging.log4j2.Log4j2Impl.class);
  }
  //使用jdkLog
  public static synchronized void useJdkLogging() {
    setImplementation(org.apache.ibatis.logging.jdk14.Jdk14LoggingImpl.class);
  }
  //使用标准输出日志
  public static synchronized void useStdOutLogging() {
    setImplementation(org.apache.ibatis.logging.stdout.StdOutImpl.class);
  }
  //不使用log 
  public static synchronized void useNoLogging() {
    setImplementation(org.apache.ibatis.logging.nologging.NoLoggingImpl.class);
  }
  //异步线程
  private static void tryImplementation(Runnable runnable) {
    if (logConstructor == null) {
      try {
        runnable.run();
      } catch (Throwable t) {
        // ignore
      }
    }
  }
  //设置日志实现
  private static void setImplementation(Class<? extends Log> implClass) {
    try {
      Constructor<? extends Log> candidate = implClass.getConstructor(String.class);
      Log log = candidate.newInstance(LogFactory.class.getName());
      if (log.isDebugEnabled()) {
        log.debug("Logging initialized using '" + implClass + "' adapter.");
      }
      logConstructor = candidate;
    } catch (Throwable t) {
      throw new LogException("Error setting Log implementation.  Cause: " + t, t);
    }
  }

}
```

#### 5.9.1.3、LogException日志异常

```java
/**
 * 继承运行期异常
 * @author Clinton Begin
 */
public class LogException extends PersistenceException {

  private static final long serialVersionUID = 1022924004852350942L;
  //空构造函数
  public LogException() {
    super();
  }
  //传递信息的构造函数
  public LogException(String message) {
    super(message);
  }
 //传递信息和throwable的构造函数
  public LogException(String message, Throwable cause) {
    super(message, cause);
  }
  //传递throwable的构造函数
  public LogException(Throwable cause) {
    super(cause);
  }

}
```

### 5.9.2、commons公共包

#### 5.9.2.1、JakartaCommonsLoggingImpl

```java
/**
 * 公共日志实现类
 * @author Clinton Begin
 */
public class JakartaCommonsLoggingImpl implements org.apache.ibatis.logging.Log {
  //日志接口
  private final Log log;
  //根据类名赋值日志接口实现
  public JakartaCommonsLoggingImpl(String clazz) {
    log = LogFactory.getLog(clazz);
  }
  //是否开启debug模式
  @Override
  public boolean isDebugEnabled() {
    return log.isDebugEnabled();
  }
  //是否开启trace模式
  @Override
  public boolean isTraceEnabled() {
    return log.isTraceEnabled();
  }
  //错误输出信息
  @Override
  public void error(String s, Throwable e) {
    log.error(s, e);
  }
  //错误输出
  @Override
  public void error(String s) {
    log.error(s);
  }
  //debug输出
  @Override
  public void debug(String s) {
    log.debug(s);
  }
  //trace输出
  @Override
  public void trace(String s) {
    log.trace(s);
  }
  //warn 输出
  @Override
  public void warn(String s) {
    log.warn(s);
  }

}
```

### 5.9.3、jdbc日志模块

#### 5.9.3.1、BaseJdbcLogger

```java
/**
 * 日志输出的代理基础类
 * Base class for proxies to do logging.
 *
 * @author Clinton Begin
 * @author Eduardo Macarron
 */
public abstract class BaseJdbcLogger {
  //设置方法集合
  protected static final Set<String> SET_METHODS;
  //执行方法集合
  protected static final Set<String> EXECUTE_METHODS = new HashSet<>();
  //列字段集合
  private final Map<Object, Object> columnMap = new HashMap<>();
  //列名集合
  private final List<Object> columnNames = new ArrayList<>();
  //列值集合
  private final List<Object> columnValues = new ArrayList<>();
  //会话日志接口
  protected final Log statementLog;
  //查询栈
  protected final int queryStack;

  /*
  默认的构造器
   * Default constructor
   */
  public BaseJdbcLogger(Log log, int queryStack) {
    this.statementLog = log;
    if (queryStack == 0) {
      this.queryStack = 1;
    } else {
      this.queryStack = queryStack;
    }
  }
  //静态代码块，将所有的声明的set方法并且参数个数大于1的放入set方法集合
  static {
    SET_METHODS = Arrays.stream(PreparedStatement.class.getDeclaredMethods())
            .filter(method -> method.getName().startsWith("set"))
            .filter(method -> method.getParameterCount() > 1)
            .map(Method::getName)
            .collect(Collectors.toSet());
    //执行方法添加 四种类型方法名称
    EXECUTE_METHODS.add("execute");
    EXECUTE_METHODS.add("executeUpdate");
    EXECUTE_METHODS.add("executeQuery");
    EXECUTE_METHODS.add("addBatch");
  }
  //设置列的字段
  protected void setColumn(Object key, Object value) {
    columnMap.put(key, value);
    columnNames.add(key);
    columnValues.add(value);
  }
  //根据某个key获取列对象
  protected Object getColumn(Object key) {
    return columnMap.get(key);
  }
  //获取参数值
  protected String getParameterValueString() {
    List<Object> typeList = new ArrayList<>(columnValues.size());
    for (Object value : columnValues) {
      if (value == null) {
        typeList.add("null");
      } else {
        typeList.add(objectValueString(value) + "(" + value.getClass().getSimpleName() + ")");
      }
    }
    final String parameters = typeList.toString();
    return parameters.substring(1, parameters.length() - 1);
  }
  //对象或者数组对象转string
  protected String objectValueString(Object value) {
    if (value instanceof Array) {
      try {
        return ArrayUtil.toString(((Array) value).getArray());
      } catch (SQLException e) {
        return value.toString();
      }
    }
    return value.toString();
  }
  //获取列名字
  protected String getColumnString() {
    return columnNames.toString();
  }
  //清除列信息
  protected void clearColumnInfo() {
    columnMap.clear();
    columnNames.clear();
    columnValues.clear();
  }
  //移除空格
  protected String removeBreakingWhitespace(String original) {
    StringTokenizer whitespaceStripper = new StringTokenizer(original);
    StringBuilder builder = new StringBuilder();
    while (whitespaceStripper.hasMoreTokens()) {
      builder.append(whitespaceStripper.nextToken());
      builder.append(" ");
    }
    return builder.toString();
  }
  //是否开启debug
  protected boolean isDebugEnabled() {
    return statementLog.isDebugEnabled();
  }
  //是否开启trace
  protected boolean isTraceEnabled() {
    return statementLog.isTraceEnabled();
  }
  //是否输入，来输出信息
  protected void debug(String text, boolean input) {
    if (statementLog.isDebugEnabled()) {
      statementLog.debug(prefix(input) + text);
    }
  } 
  //根据是否输入来输出信息
  protected void trace(String text, boolean input) {
    if (statementLog.isTraceEnabled()) {
      statementLog.trace(prefix(input) + text);
    }
  }

  // TODO: 2020/3/14 没能想到这个前缀是干啥的 
  //判断前缀 
  private String prefix(boolean isInput) {
    char[] buffer = new char[queryStack * 2 + 2];
    Arrays.fill(buffer, '=');
    buffer[queryStack * 2 + 1] = ' ';
    if (isInput) {
      buffer[queryStack * 2] = '>';
    } else {
      buffer[0] = '<';
    }
    return new String(buffer);
  }

}

```

#### 5.9.3.2、ConnectionLogger链接日志器

```java
/**
 * 添加日志的链接代理
 * Connection proxy to add logging.
 *
 * @author Clinton Begin
 * @author Eduardo Macarron
 *
 */
public final class ConnectionLogger extends BaseJdbcLogger implements InvocationHandler {
  //链接
  private final Connection connection;
  //类构造器
  private ConnectionLogger(Connection conn, Log statementLog, int queryStack) {
    super(statementLog, queryStack);
    this.connection = conn;
  }
  //实现动态代理模式的invoke，也就是说链接只支持这几种模式，否则直接报错
  @Override
  public Object invoke(Object proxy, Method method, Object[] params)
      throws Throwable {
    try {
      //如果传入的类是Object类就直接执行
      if (Object.class.equals(method.getDeclaringClass())) {
        return method.invoke(this, params);
      }
      //如果是预会话的方法
      if ("prepareStatement".equals(method.getName())) {
        //判断是否开启debug方法
        if (isDebugEnabled()) {
          debug(" Preparing: " + removeBreakingWhitespace((String) params[0]), true);
        }
        //预会话执行链接
        PreparedStatement stmt = (PreparedStatement) method.invoke(connection, params);
        //执行预会话日志实例输出
        stmt = PreparedStatementLogger.newInstance(stmt, statementLog, queryStack);
        return stmt;
        //如果是准备会话则按照会话方法执行
      } else if ("prepareCall".equals(method.getName())) {
        if (isDebugEnabled()) {
          debug(" Preparing: " + removeBreakingWhitespace((String) params[0]), true);
        }
        PreparedStatement stmt = (PreparedStatement) method.invoke(connection, params);
        stmt = PreparedStatementLogger.newInstance(stmt, statementLog, queryStack);
        return stmt;
        //如果是创建会话则创建会话执行
      } else if ("createStatement".equals(method.getName())) {
        Statement stmt = (Statement) method.invoke(connection, params);
        stmt = StatementLogger.newInstance(stmt, statementLog, queryStack);
        return stmt;
        //其余则直接执行
      } else {
        return method.invoke(connection, params);
      }
    } catch (Throwable t) {
      throw ExceptionUtil.unwrapThrowable(t);
    }
  }

  /**
   * 创建一个链接日志的版本
   * Creates a logging version of a connection.
   *
   * @param conn - the original connection
   * @return - the connection with logging
   */
  public static Connection newInstance(Connection conn, Log statementLog, int queryStack) {
    InvocationHandler handler = new ConnectionLogger(conn, statementLog, queryStack);
    ClassLoader cl = Connection.class.getClassLoader();
    return (Connection) Proxy.newProxyInstance(cl, new Class[]{Connection.class}, handler);
  }

  /**
   * 返回封装的链接--这个链接带有日志，将会话链接与日志绑定
   * return the wrapped connection.
   *
   * @return the connection
   */
  public Connection getConnection() {
    return connection;
  }

}
```

#### 5.9.3.3、PreparedStatementLogger预会话日志器

```java
/**
 * 添加日志的预会话代理
 * PreparedStatement proxy to add logging.
 *
 * @author Clinton Begin
 * @author Eduardo Macarron
 *
 */
public final class PreparedStatementLogger extends BaseJdbcLogger implements InvocationHandler {
  //预会话
  private final PreparedStatement statement;
  //预会话日志构造器
  private PreparedStatementLogger(PreparedStatement stmt, Log statementLog, int queryStack) {
    super(statementLog, queryStack);
    this.statement = stmt;
  }
  //代理执行方法
  @Override
  public Object invoke(Object proxy, Method method, Object[] params) throws Throwable {
    try {
      //同样和链接代理日志器一样校验方法是否是Object如果是的话直接执行
      if (Object.class.equals(method.getDeclaringClass())) {
        return method.invoke(this, params);
      }
      //是否是执行方法其中的一个
      if (EXECUTE_METHODS.contains(method.getName())) {
        if (isDebugEnabled()) {
          debug("Parameters: " + getParameterValueString(), true);
        }
        //清除列信息
        clearColumnInfo();
        //执行查询
        if ("executeQuery".equals(method.getName())) {
          //调用返回结果
          ResultSet rs = (ResultSet) method.invoke(statement, params);
          return rs == null ? null : ResultSetLogger.newInstance(rs, statementLog, queryStack);
        } else {
          return method.invoke(statement, params);
        }
        //是否是set方法中的
      } else if (SET_METHODS.contains(method.getName())) {
        //如果是set空
        if ("setNull".equals(method.getName())) {
          setColumn(params[0], null);
        } else {
          setColumn(params[0], params[1]);
        }
        return method.invoke(statement, params);
        //是否是获取返回结果
      } else if ("getResultSet".equals(method.getName())) {
        ResultSet rs = (ResultSet) method.invoke(statement, params);
        return rs == null ? null : ResultSetLogger.newInstance(rs, statementLog, queryStack);
        //获取更新行数
      } else if ("getUpdateCount".equals(method.getName())) {
        int updateCount = (Integer) method.invoke(statement, params);
        if (updateCount != -1) {
          debug("   Updates: " + updateCount, false);
        }
        return updateCount;
      } else {
        return method.invoke(statement, params);
      }
    } catch (Throwable t) {
      throw ExceptionUtil.unwrapThrowable(t);
    }
  }

  /**
   * 创建一个预会话的日志版本，这里的信息是回调会话
   * Creates a logging version of a PreparedStatement.
   *
   * @param stmt - the statement
   * @param statementLog - the statement log
   * @param queryStack - the query stack
   * @return - the proxy
   */
  public static PreparedStatement newInstance(PreparedStatement stmt, Log statementLog, int queryStack) {
    InvocationHandler handler = new PreparedStatementLogger(stmt, statementLog, queryStack);
    ClassLoader cl = PreparedStatement.class.getClassLoader();
    //回调会话
    return (PreparedStatement) Proxy.newProxyInstance(cl, new Class[]{PreparedStatement.class, CallableStatement.class}, handler);
  }

  /**
   * 返回封装的预会话信息
   * Return the wrapped prepared statement.
   *
   * @return the PreparedStatement
   */
  public PreparedStatement getPreparedStatement() {
    return statement;
  }

}
```

#### 5.9.3.4、StatementLogger会话日志器

```java
/**
 * 添加日志的会话代理
 * Statement proxy to add logging.
 *
 * @author Clinton Begin
 * @author Eduardo Macarron
 *
 */
public final class StatementLogger extends BaseJdbcLogger implements InvocationHandler {
  //会话
  private final Statement statement;
  //会话日志器的构造器
  private StatementLogger(Statement stmt, Log statementLog, int queryStack) {
    super(statementLog, queryStack);
    this.statement = stmt;
  }
  //执行代理方法
  @Override
  public Object invoke(Object proxy, Method method, Object[] params) throws Throwable {
    try {
      if (Object.class.equals(method.getDeclaringClass())) {
        return method.invoke(this, params);
      }
      //执行方法中
      if (EXECUTE_METHODS.contains(method.getName())) {
        if (isDebugEnabled()) {
          debug(" Executing: " + removeBreakingWhitespace((String) params[0]), true);
        }
        //执行查询
        if ("executeQuery".equals(method.getName())) {
          ResultSet rs = (ResultSet) method.invoke(statement, params);
          return rs == null ? null : ResultSetLogger.newInstance(rs, statementLog, queryStack);
        } else {
          return method.invoke(statement, params);
        }
        //获取返回结果信息
      } else if ("getResultSet".equals(method.getName())) {
        ResultSet rs = (ResultSet) method.invoke(statement, params);
        return rs == null ? null : ResultSetLogger.newInstance(rs, statementLog, queryStack);
      } else {
        //其余方法直接执行
        return method.invoke(statement, params);
      }
    } catch (Throwable t) {
      throw ExceptionUtil.unwrapThrowable(t);
    }
  }

  /**
   * 创建一个会话的日志版本
   * Creates a logging version of a Statement.
   *
   * @param stmt - the statement
   * @return - the proxy
   */
  public static Statement newInstance(Statement stmt, Log statementLog, int queryStack) {
    InvocationHandler handler = new StatementLogger(stmt, statementLog, queryStack);
    ClassLoader cl = Statement.class.getClassLoader();
    return (Statement) Proxy.newProxyInstance(cl, new Class[]{Statement.class}, handler);
  }

  /**
   * 返回封装的会话
   * return the wrapped statement.
   *
   * @return the statement
   */
  public Statement getStatement() {
    return statement;
  }

}
```

#### 5.9.3.5、ResultSetLogger结果集日志器

```java
/**
 * 添加日志的结果集代理
 * ResultSet proxy to add logging.
 *
 * @author Clinton Begin
 * @author Eduardo Macarron
 *
 */
public final class ResultSetLogger extends BaseJdbcLogger implements InvocationHandler {
  //BLOB类型集合
  private static final Set<Integer> BLOB_TYPES = new HashSet<>();
  //是否是首行
  private boolean first = true;
  //行
  private int rows;
  //结果集
  private final ResultSet rs;
  //Blob列
  private final Set<Integer> blobColumns = new HashSet<>();
  //静态BLOB类型添加
  static {
    BLOB_TYPES.add(Types.BINARY);
    BLOB_TYPES.add(Types.BLOB);
    BLOB_TYPES.add(Types.CLOB);
    BLOB_TYPES.add(Types.LONGNVARCHAR);
    BLOB_TYPES.add(Types.LONGVARBINARY);
    BLOB_TYPES.add(Types.LONGVARCHAR);
    BLOB_TYPES.add(Types.NCLOB);
    BLOB_TYPES.add(Types.VARBINARY);
  }
  //结果集构造器
  private ResultSetLogger(ResultSet rs, Log statementLog, int queryStack) {
    super(statementLog, queryStack);
    this.rs = rs;
  }
  //代理方法
  @Override
  public Object invoke(Object proxy, Method method, Object[] params) throws Throwable {
    try {
      if (Object.class.equals(method.getDeclaringClass())) {
        return method.invoke(this, params);
      }
      //执行结果集
      Object o = method.invoke(rs, params);
      //如果是获取下一批next方法
      if ("next".equals(method.getName())) {
        //判断是否执行了结果
        if ((Boolean) o) {
          //行数增加
          rows++;
          if (isTraceEnabled()) {
            //获取结果集元数据
            ResultSetMetaData rsmd = rs.getMetaData();
            //获取列行数
            final int columnCount = rsmd.getColumnCount();
            //如果是首行则打印列表头
            if (first) {
              first = false;
              printColumnHeaders(rsmd, columnCount);
            }
            //打印列内容
            printColumnValues(columnCount);
          }
        } else {
          debug("     Total: " + rows, false);
        }
      }
      //清除列信息
      clearColumnInfo();
      return o;
    } catch (Throwable t) {
      throw ExceptionUtil.unwrapThrowable(t);
    }
  }
   //打印列头信息
  private void printColumnHeaders(ResultSetMetaData rsmd, int columnCount) throws SQLException {
    StringJoiner row = new StringJoiner(", ", "   Columns: ", "");
    for (int i = 1; i <= columnCount; i++) {
      if (BLOB_TYPES.contains(rsmd.getColumnType(i))) {
        blobColumns.add(i);
      }
      row.add(rsmd.getColumnLabel(i));
    }
    trace(row.toString(), false);
  }
  //打印列内容信息
  private void printColumnValues(int columnCount) {
    StringJoiner row = new StringJoiner(", ", "       Row: ", "");
    for (int i = 1; i <= columnCount; i++) {
      try {
        if (blobColumns.contains(i)) {
          row.add("<<BLOB>>");
        } else {
          row.add(rs.getString(i));
        }
      } catch (SQLException e) {
        // generally can't call getString() on a BLOB column
        row.add("<<Cannot Display>>");
      }
    }
    trace(row.toString(), false);
  }

  /**
   * 创建结果集的日志版本
   * Creates a logging version of a ResultSet.
   *
   * @param rs - the ResultSet to proxy
   * @return - the ResultSet with logging
   */
  public static ResultSet newInstance(ResultSet rs, Log statementLog, int queryStack) {
    InvocationHandler handler = new ResultSetLogger(rs, statementLog, queryStack);
    ClassLoader cl = ResultSet.class.getClassLoader();
    return (ResultSet) Proxy.newProxyInstance(cl, new Class[]{ResultSet.class}, handler);
  }

  /**
   * 获取封装好的结果集
   * Get the wrapped result set.
   *
   * @return the resultSet
   */
  public ResultSet getRs() {
    return rs;
  }

}
```

### 5.9.4、jdk14

#### 5.9.4.1、Jdk14LoggingImpl实现

```java
/**
 * jdk14日志实现类
 * @author Clinton Begin
 */
public class Jdk14LoggingImpl implements Log {

  private final Logger log;
  //构造器
  public Jdk14LoggingImpl(String clazz) {
    log = Logger.getLogger(clazz);
  }
  //是否开启debug形式
  @Override
  public boolean isDebugEnabled() {
    return log.isLoggable(Level.FINE);
  }
  //是否开启trace模式
  @Override
  public boolean isTraceEnabled() {
    return log.isLoggable(Level.FINER);
  }
  //输出错误信息
  @Override
  public void error(String s, Throwable e) {
    log.log(Level.SEVERE, s, e);
  }
  //输出错误信息
  @Override
  public void error(String s) {
    log.log(Level.SEVERE, s);
  }
  //输出debug信息
  @Override
  public void debug(String s) {
    log.log(Level.FINE, s);
  }
  //输出trace信息
  @Override
  public void trace(String s) {
    log.log(Level.FINER, s);
  }
  //输出warn信息
  @Override
  public void warn(String s) {
    log.log(Level.WARNING, s);
  }

}
```

### 5.9.5、log4j

#### 5.9.5.1、Log4jImpl实现

```java
/**
 * log4j实现类
 * @author Eduardo Macarron
 */
public class Log4jImpl implements Log {

  private static final String FQCN = Log4jImpl.class.getName();
  //log4j.Logger
  private final Logger log;
  //实现类
  public Log4jImpl(String clazz) {
    log = Logger.getLogger(clazz);
  }
  //是否开启debug形式
  @Override
  public boolean isDebugEnabled() {
    return log.isDebugEnabled();
  }
  //是否开启trace形式
  @Override
  public boolean isTraceEnabled() {
    return log.isTraceEnabled();
  }
  //输出错误信息
  @Override
  public void error(String s, Throwable e) {
    log.log(FQCN, Level.ERROR, s, e);
  }
  //输出错误信息
  @Override
  public void error(String s) {
    log.log(FQCN, Level.ERROR, s, null);
  }
  //输出debug信息
  @Override
  public void debug(String s) {
    log.log(FQCN, Level.DEBUG, s, null);
  }
  //输出trace信息
  @Override
  public void trace(String s) {
    log.log(FQCN, Level.TRACE, s, null);
  }
  //输出warn信息
  @Override
  public void warn(String s) {
    log.log(FQCN, Level.WARN, s, null);
  }

}
```

### 5.9.6、log4j2

#### 5.9.6.1、Log4j2Impl--(ibatis的log4j2实现类的聚合)

```java
/**
 * ibatis日志实现类---Log4j2AbstractLoggerImpl  或者  Log4j2LoggerImpl
 * @author Eduardo Macarron
 */
public class Log4j2Impl implements Log {
  //logging.Log
  private final Log log;
  //构造器 根据类获取的log4j管理器来判断属于哪种
  public Log4j2Impl(String clazz) {
    Logger logger = LogManager.getLogger(clazz);

    if (logger instanceof AbstractLogger) {
      log = new Log4j2AbstractLoggerImpl((AbstractLogger) logger);
    } else {
      log = new Log4j2LoggerImpl(logger);
    }
  }
//是否开启debug
  @Override
  public boolean isDebugEnabled() {
    return log.isDebugEnabled();
  }
//是否开启trace
  @Override
  public boolean isTraceEnabled() {
    return log.isTraceEnabled();
  }
//输出error错误信息
  @Override
  public void error(String s, Throwable e) {
    log.error(s, e);
  }
//输出error错误信息
  @Override
  public void error(String s) {
    log.error(s);
  }
//输出debug信息
  @Override
  public void debug(String s) {
    log.debug(s);
  }
//输出trace信息
  @Override
  public void trace(String s) {
    log.trace(s);
  }
//输出warn信息
  @Override
  public void warn(String s) {
    log.warn(s);
  }

}
```

#### 5.9.6.2、Log4j2AbstractLoggerImpl抽象日志实现

```java
/**
 * spi的抽象日志实现类
 * @author Eduardo Macarron
 */
public class Log4j2AbstractLoggerImpl implements Log {
  //日志创建管理者
  private static final Marker MARKER = MarkerManager.getMarker(LogFactory.MARKER);
  //依赖的实现类
  private static final String FQCN = Log4j2Impl.class.getName();
  //log4j.spi.ExtendedLoggerWrapper
  private final ExtendedLoggerWrapper log;
  //构造器
  public Log4j2AbstractLoggerImpl(AbstractLogger abstractLogger) {
    log = new ExtendedLoggerWrapper(abstractLogger, abstractLogger.getName(), abstractLogger.getMessageFactory());
  }
  //是否开启debug
  @Override
  public boolean isDebugEnabled() {
    return log.isDebugEnabled();
  }
  //是否开启trace
  @Override
  public boolean isTraceEnabled() {
    return log.isTraceEnabled();
  }
  //输出错误信息，需要传入当前的实现类名字
  @Override
  public void error(String s, Throwable e) {
    log.logIfEnabled(FQCN, Level.ERROR, MARKER, (Message) new SimpleMessage(s), e);
  }
   //输出错误信息
  @Override
  public void error(String s) {
    log.logIfEnabled(FQCN, Level.ERROR, MARKER, (Message) new SimpleMessage(s), null);
  }
  //输出debug信息
  @Override 
  public void debug(String s) {
    log.logIfEnabled(FQCN, Level.DEBUG, MARKER, (Message) new SimpleMessage(s), null);
  }
  //输出trace信息
  @Override
  public void trace(String s) {
    log.logIfEnabled(FQCN, Level.TRACE, MARKER, (Message) new SimpleMessage(s), null);
  }
  //输出warn信息
  @Override
  public void warn(String s) {
    log.logIfEnabled(FQCN, Level.WARN, MARKER, (Message) new SimpleMessage(s), null);
  }

}
```

#### 5.9.6.3、Log4j2LoggerImpl实现

```java
/**
 * 
 * @author Eduardo Macarron
 */
public class Log4j2LoggerImpl implements Log {

  private static final Marker MARKER = MarkerManager.getMarker(LogFactory.MARKER);
  //log4j.Logger
  private final Logger log;
  //构造器
  public Log4j2LoggerImpl(Logger logger) {
    log = logger;
  }
  //是否开启debug
  @Override
  public boolean isDebugEnabled() {
    return log.isDebugEnabled();
  }
  //是否开启trace
  @Override
  public boolean isTraceEnabled() {
    return log.isTraceEnabled();
  }
  //输出错误信息
  @Override
  public void error(String s, Throwable e) {
    log.error(MARKER, s, e);
  }
  //输出错误信息
  @Override
  public void error(String s) {
    log.error(MARKER, s);
  }
  //输出debug信息
  @Override
  public void debug(String s) {
    log.debug(MARKER, s);
  }
  //输出trace信息
  @Override
  public void trace(String s) {
    log.trace(MARKER, s);
  }
  //输出warn信息
  @Override
  public void warn(String s) {
    log.warn(MARKER, s);
  }

}
```

### 5.9.7、nologging无日志

#### 5.9.7.1、NoLoggingImpl无日志实现

```java
/**
 * 不需要日志实现
 * @author Clinton Begin
 */
public class NoLoggingImpl implements Log {
  //什么也不做
  public NoLoggingImpl(String clazz) {
    // Do Nothing
  }
  //是否开启debug
  @Override
  public boolean isDebugEnabled() {
    return false;
  }
  //是否开启trace
  @Override
  public boolean isTraceEnabled() {
    return false;
  }
  //是否输出错误信息 ，什么也不做
  @Override
  public void error(String s, Throwable e) {
    // Do Nothing
  }
  //是否输出错误信息 ，什么也不做
  @Override
  public void error(String s) {
    // Do Nothing
  }
  //是否输出debug信息 ，什么也不做
  @Override
  public void debug(String s) {
    // Do Nothing
  }
  //是否输出trace信息 ，什么也不做
  @Override
  public void trace(String s) {
    // Do Nothing
  }
  //是否输出warn信息 ，什么也不做
  @Override
  public void warn(String s) {
    // Do Nothing
  }

}

```

### 5.9.8、slf4j

#### 5.9.8.1、Slf4jImpl实现

```java
/**
 * Slf4j实现代理类
 * @author Clinton Begin
 * @author Eduardo Macarron
 */
public class Slf4jImpl implements Log {
//mybatis接口
  private Log log;

  public Slf4jImpl(String clazz) {
    Logger logger = LoggerFactory.getLogger(clazz);
    
    if (logger instanceof LocationAwareLogger) {
      try {
        // slf4j >= 1.6的签名
        // check for slf4j >= 1.6 method signature
        logger.getClass().getMethod("log", Marker.class, String.class, int.class, String.class, Object[].class, Throwable.class);
        log = new Slf4jLocationAwareLoggerImpl((LocationAwareLogger) logger);
        return;
      } catch (SecurityException | NoSuchMethodException e) {
        // fail-back to Slf4jLoggerImpl
      }
    }
    // 没有LocationAwareLogger或者版本小于1.6
    // Logger is not LocationAwareLogger or slf4j version < 1.6
    log = new Slf4jLoggerImpl(logger);
  }
  //是否开启debug
  @Override
  public boolean isDebugEnabled() {
    return log.isDebugEnabled();
  }
  //是否开启trace
  @Override
  public boolean isTraceEnabled() {
    return log.isTraceEnabled();
  }
  //输出错误信息
  @Override
  public void error(String s, Throwable e) {
    log.error(s, e);
  }
  //输出错误信息
  @Override
  public void error(String s) {
    log.error(s);
  }
  //输出debug信息
  @Override
  public void debug(String s) {
    log.debug(s);
  }
  //输出trace信息
  @Override
  public void trace(String s) {
    log.trace(s);
  }
  //输出warn信息
  @Override
  public void warn(String s) {
    log.warn(s);
  }

}
```

#### 5.9.8.2、Slf4jLocationAwareLoggerImpl未知感知实现

```java
/**
 * 位置感知日志实现
 * @author Eduardo Macarron
 */
class Slf4jLocationAwareLoggerImpl implements Log {

  private static final Marker MARKER = MarkerFactory.getMarker(LogFactory.MARKER);
  //日志实现名
  private static final String FQCN = Slf4jImpl.class.getName();
  //slf4j.spi.LocationAwareLogger
  private final LocationAwareLogger logger;
  //构造器
  Slf4jLocationAwareLoggerImpl(LocationAwareLogger logger) {
    this.logger = logger;
  }
  //是否开启debug
  @Override
  public boolean isDebugEnabled() {
    return logger.isDebugEnabled();
  }
  //是否开启trace
  @Override
  public boolean isTraceEnabled() {
    return logger.isTraceEnabled();
  }
  //输出错误信息
  @Override
  public void error(String s, Throwable e) {
    logger.log(MARKER, FQCN, LocationAwareLogger.ERROR_INT, s, null, e);
  }
  //输出错误信息
  @Override
  public void error(String s) {
    logger.log(MARKER, FQCN, LocationAwareLogger.ERROR_INT, s, null, null);
  }
  //输出debug信息
  @Override
  public void debug(String s) {
    logger.log(MARKER, FQCN, LocationAwareLogger.DEBUG_INT, s, null, null);
  }
  //输出trace信息
  @Override
  public void trace(String s) {
    logger.log(MARKER, FQCN, LocationAwareLogger.TRACE_INT, s, null, null);
  }
  //输出warn信息
  @Override
  public void warn(String s) {
    logger.log(MARKER, FQCN, LocationAwareLogger.WARN_INT, s, null, null);
  }

}
```

#### 5.9.8.3、Slf4jLoggerImpl

```java
/**
 * 日志实现类
 * @author Eduardo Macarron
 */
class Slf4jLoggerImpl implements Log {
   //slf4j.Logger
  private final Logger log;
  //日志实现类构造器
  public Slf4jLoggerImpl(Logger logger) {
    log = logger;
  }
  //是否开启debug
  @Override
  public boolean isDebugEnabled() {
    return log.isDebugEnabled();
  }
  //是否开启trace
  @Override
  public boolean isTraceEnabled() {
    return log.isTraceEnabled();
  }
  //是否输出错误信息
  @Override
  public void error(String s, Throwable e) {
    log.error(s, e);
  }
  //输出错误信息
  @Override
  public void error(String s) {
    log.error(s);
  }
  //输出debug信息
  @Override
  public void debug(String s) {
    log.debug(s);
  }
  //输出trace信息
  @Override
  public void trace(String s) {
    log.trace(s);
  }
  //输出warn信息
  @Override
  public void warn(String s) {
    log.warn(s);
  }

}
```

### 5.9.9、stdout标准输出

#### 5.9.9.1、StdOutImpl标准输出实现

```java
/**标准输出实现类
 * @author Clinton Begin
 */
public class StdOutImpl implements Log {
  //空构造器
  public StdOutImpl(String clazz) {
    // Do Nothing
  }
 //是否开启debug
  @Override
  public boolean isDebugEnabled() {
    return true;
  }
  //是否开启trace
  @Override
  public boolean isTraceEnabled() {
    return true;
  }
  //输出错误信息，系统输出和栈溢出
  @Override
  public void error(String s, Throwable e) {
    System.err.println(s);
    e.printStackTrace(System.err);
  }
  //错误信息输出
  @Override
  public void error(String s) {
    System.err.println(s);
  }
  //输出debug
  @Override
  public void debug(String s) {
    System.out.println(s);
  }
  //输出trace
  @Override
  public void trace(String s) {
    System.out.println(s);
  }
  //输出warn
  @Override
  public void warn(String s) {
    System.out.println(s);
  }
}

```

### 5.9.10、小结

* 框架高内聚，低耦合的做法来看，日志包是非常漂亮，自身定义接口满足日志所需，分不同日志框架进行实现类封装，封装的具体实现来自于其他框架的实现。项目之间的调用实现也是同样的道理。

## 5.10、org.apache.ibatis.parsing（解析模块）



### 5.10.1、ParsingException解析异常

```java
/**
 * 解析异常
 * @author Clinton Begin
 */
public class ParsingException extends PersistenceException {
  private static final long serialVersionUID = -176685891441325943L;
  //空构造函数
  public ParsingException() {
    super();
  }
  //传递信息构造函数
  public ParsingException(String message) {
    super(message);
  }
  //传递信息和throwable信息构造函数
  public ParsingException(String message, Throwable cause) {
    super(message, cause);
  }
  //传递throwable构造函数
  public ParsingException(Throwable cause) {
    super(cause);
  }
}
```

### 5.10.2、TokenHandler处理器

```java
/**
 * Token处理器
 * @author Clinton Begin
 */
public interface TokenHandler {
  //处理内容的token
  String handleToken(String content);
}
```

### 5.10.3、GenericTokenParser解析器

```java
/**
 * 通用的token解析器
 * @author Clinton Begin
 */
public class GenericTokenParser {
  //（
  private final String openToken;
  // ）
  private final String closeToken;
  //处理器
  private final TokenHandler handler;
  //构造函数
  public GenericTokenParser(String openToken, String closeToken, TokenHandler handler) {
    this.openToken = openToken;
    this.closeToken = closeToken;
    this.handler = handler;
  }
  //解析内容
  public String parse(String text) {
    if (text == null || text.isEmpty()) {
      return "";
    }
    // 检索 （
    // search open token
    int start = text.indexOf(openToken);
    if (start == -1) {
      return text;
    }
    char[] src = text.toCharArray();
    int offset = 0;
    final StringBuilder builder = new StringBuilder();
    StringBuilder expression = null;
    while (start > -1) {
      if (start > 0 && src[start - 1] == '\\') {
        //此开启标记已转义。删除反斜杠并继续
        // this open token is escaped. remove the backslash and continue.
        builder.append(src, offset, start - offset - 1).append(openToken);
        offset = start + openToken.length();
      } else {
        //如果已经找到（ ，则继续找 ）
        // found open token. let's search close token.
        if (expression == null) {
          expression = new StringBuilder();
        } else {
          expression.setLength(0);
        }
        builder.append(src, offset, start - offset);
        offset = start + openToken.length();
        int end = text.indexOf(closeToken, offset);
        while (end > -1) {
          if (end > offset && src[end - 1] == '\\') {
            //此关闭标记已转义。删除反斜杠并继续
            // this close token is escaped. remove the backslash and continue.
            expression.append(src, offset, end - offset - 1).append(closeToken);
            offset = end + closeToken.length();
            end = text.indexOf(closeToken, offset);
          } else {
            expression.append(src, offset, end - offset);
            break;
          }
        }
        if (end == -1) {
          // （ 没有找到
          // close token was not found.
          builder.append(src, start, src.length - start);
          offset = src.length;
        } else {
          builder.append(handler.handleToken(expression.toString()));
          offset = end + closeToken.length();
        }
      }
      start = text.indexOf(openToken, offset);
    }
    if (offset < src.length) {
      builder.append(src, offset, src.length - offset);
    }
    return builder.toString();
  }
}
```

### 5.10.4、PropertyParser属性解析器

```java
/**
 * 属性解析器
 * @author Clinton Begin
 * @author Kazuki Shimizu
 */
public class PropertyParser {

  private static final String KEY_PREFIX = "org.apache.ibatis.parsing.PropertyParser.";
  /**
   * 特殊的属性key 暗示 是否开启默认的占位符
   * The special property key that indicate whether enable a default value on placeholder.
   * <p>
   *   The default value is {@code false} (indicate disable a default value on placeholder)
   *   If you specify the {@code true}, you can specify key and default value on placeholder (e.g. {@code ${db.username:postgres}}).
   * </p>
   * @since 3.4.2
   */
  public static final String KEY_ENABLE_DEFAULT_VALUE = KEY_PREFIX + "enable-default-value";

  /**
   * 为占位符上的键和默认值指定分隔符的特殊属性键
   * The special property key that specify a separator for key and default value on placeholder.
   * <p>
   *   The default separator is {@code ":"}.
   * </p>
   * @since 3.4.2
   */
  public static final String KEY_DEFAULT_VALUE_SEPARATOR = KEY_PREFIX + "default-value-separator";

  private static final String ENABLE_DEFAULT_VALUE = "false";
  private static final String DEFAULT_VALUE_SEPARATOR = ":";

  private PropertyParser() {
    // Prevent Instantiation
  }
  //解析属性变量
  public static String parse(String string, Properties variables) {
    VariableTokenHandler handler = new VariableTokenHandler(variables);
    GenericTokenParser parser = new GenericTokenParser("${", "}", handler);
    return parser.parse(string);
  }
  //变量token处理器
  private static class VariableTokenHandler implements TokenHandler {
    private final Properties variables;
    private final boolean enableDefaultValue;
    private final String defaultValueSeparator;
   //构造函数
    private VariableTokenHandler(Properties variables) {
      this.variables = variables;
      this.enableDefaultValue = Boolean.parseBoolean(getPropertyValue(KEY_ENABLE_DEFAULT_VALUE, ENABLE_DEFAULT_VALUE));
      this.defaultValueSeparator = getPropertyValue(KEY_DEFAULT_VALUE_SEPARATOR, DEFAULT_VALUE_SEPARATOR);
    }
    //获取属性值
    private String getPropertyValue(String key, String defaultValue) {
      return (variables == null) ? defaultValue : variables.getProperty(key, defaultValue);
    }
   //处理内容中的token
    @Override
    public String handleToken(String content) {
      if (variables != null) {
        String key = content;
        if (enableDefaultValue) {
          final int separatorIndex = content.indexOf(defaultValueSeparator);
          String defaultValue = null;
          if (separatorIndex >= 0) {
            key = content.substring(0, separatorIndex);
            defaultValue = content.substring(separatorIndex + defaultValueSeparator.length());
          }
          if (defaultValue != null) {
            return variables.getProperty(key, defaultValue);
          }
        }
        if (variables.containsKey(key)) {
          return variables.getProperty(key);
        }
      }
      return "${" + content + "}";
    }
  }

}
```

### 5.10.5、XPathParser路径解析器

```java
/**
 * XPath解析
 * @author Clinton Begin
 * @author Kazuki Shimizu
 */
public class XPathParser {
  //w3c.dom.Document
  private final Document document;
  //校验
  private boolean validation;
  //实体解析器
  private EntityResolver entityResolver;
  //属性变量
  private Properties variables;
  //xpath
  private XPath xpath;
  //解析xml为dom对象
  public XPathParser(String xml) {
    commonConstructor(false, null, null);
    this.document = createDocument(new InputSource(new StringReader(xml)));
  }
  //解析字符流流为dom对象
  public XPathParser(Reader reader) {
    commonConstructor(false, null, null);
    this.document = createDocument(new InputSource(reader));
  }
  //解析字节流为dom对象
  public XPathParser(InputStream inputStream) {
    commonConstructor(false, null, null);
    this.document = createDocument(new InputSource(inputStream));
  }
  //解析dom对象
  public XPathParser(Document document) {
    commonConstructor(false, null, null);
    this.document = document;
  }
  //解析xml和是否需要校验
  public XPathParser(String xml, boolean validation) {
    commonConstructor(validation, null, null);
    this.document = createDocument(new InputSource(new StringReader(xml)));
  }
  //解析字符流和是否需要校验
  public XPathParser(Reader reader, boolean validation) {
    commonConstructor(validation, null, null);
    this.document = createDocument(new InputSource(reader));
  }
  //解析字节流和是否需要校验
  public XPathParser(InputStream inputStream, boolean validation) {
    commonConstructor(validation, null, null);
    this.document = createDocument(new InputSource(inputStream));
  }
  //解析dom对象和是否需要校验
  public XPathParser(Document document, boolean validation) {
    commonConstructor(validation, null, null);
    this.document = document;
  }
  //解析xml和是否需要校验以及变量
  public XPathParser(String xml, boolean validation, Properties variables) {
    commonConstructor(validation, variables, null);
    this.document = createDocument(new InputSource(new StringReader(xml)));
  }
  //解析字符流和是否需要校验以及变量
  public XPathParser(Reader reader, boolean validation, Properties variables) {
    commonConstructor(validation, variables, null);
    this.document = createDocument(new InputSource(reader));
  }
  //解析字节流和是否需要校验
  public XPathParser(InputStream inputStream, boolean validation, Properties variables) {
    commonConstructor(validation, variables, null);
    this.document = createDocument(new InputSource(inputStream));
  }
  //解析dom和是否需要校验
  public XPathParser(Document document, boolean validation, Properties variables) {
    commonConstructor(validation, variables, null);
    this.document = document;
  }
  //解析xml和是否需要校验和变量属性和实体解析类
  public XPathParser(String xml, boolean validation, Properties variables, EntityResolver entityResolver) {
    commonConstructor(validation, variables, entityResolver);
    this.document = createDocument(new InputSource(new StringReader(xml)));
  }
  //解析字符流和是否需要校验和变量属性和实体解析类
  public XPathParser(Reader reader, boolean validation, Properties variables, EntityResolver entityResolver) {
    commonConstructor(validation, variables, entityResolver);
    this.document = createDocument(new InputSource(reader));
  }
  //解析字节流和是否需要校验和变量属性和实体解析类
  public XPathParser(InputStream inputStream, boolean validation, Properties variables, EntityResolver entityResolver) {
    commonConstructor(validation, variables, entityResolver);
    this.document = createDocument(new InputSource(inputStream));
  }
  //解析dom和是否需要校验和变量属性和实体解析类
  public XPathParser(Document document, boolean validation, Properties variables, EntityResolver entityResolver) {
    commonConstructor(validation, variables, entityResolver);
    this.document = document;
  }
  //设置变量
  public void setVariables(Properties variables) {
    this.variables = variables;
  }
  //获取string
  public String evalString(String expression) {
    return evalString(document, expression);
  }
  //根据root节点和正则获取属性
  public String evalString(Object root, String expression) {
    String result = (String) evaluate(expression, root, XPathConstants.STRING);
    result = PropertyParser.parse(result, variables);
    return result;
  }
  //获取boolean
  public Boolean evalBoolean(String expression) {
    return evalBoolean(document, expression);
  }
  //根据root节点和正则获取属性
  public Boolean evalBoolean(Object root, String expression) {
    return (Boolean) evaluate(expression, root, XPathConstants.BOOLEAN);
  }
  //解析short
  public Short evalShort(String expression) {
    return evalShort(document, expression);
  }
  //解析short

  public Short evalShort(Object root, String expression) {
    return Short.valueOf(evalString(root, expression));
  }
  //解析int

  public Integer evalInteger(String expression) {
    return evalInteger(document, expression);
  }
  //解析int

  public Integer evalInteger(Object root, String expression) {
    return Integer.valueOf(evalString(root, expression));
  }
  //解析long

  public Long evalLong(String expression) {
    return evalLong(document, expression);
  }
  //解析long

  public Long evalLong(Object root, String expression) {
    return Long.valueOf(evalString(root, expression));
  }
  //解析float

  public Float evalFloat(String expression) {
    return evalFloat(document, expression);
  }
  //解析float

  public Float evalFloat(Object root, String expression) {
    return Float.valueOf(evalString(root, expression));
  }
  //解析double

  public Double evalDouble(String expression) {
    return evalDouble(document, expression);
  }
  //解析double

  public Double evalDouble(Object root, String expression) {
    return (Double) evaluate(expression, root, XPathConstants.NUMBER);
  }
//解析Nodes集合
  public List<XNode> evalNodes(String expression) {
    return evalNodes(document, expression);
  }
//解析Nodes集合
  public List<XNode> evalNodes(Object root, String expression) {
    List<XNode> xnodes = new ArrayList<>();
    NodeList nodes = (NodeList) evaluate(expression, root, XPathConstants.NODESET);
    for (int i = 0; i < nodes.getLength(); i++) {
      xnodes.add(new XNode(this, nodes.item(i), variables));
    }
    return xnodes;
  }
//解析Node
  public XNode evalNode(String expression) {
    return evalNode(document, expression);
  }
//解析Node
  public XNode evalNode(Object root, String expression) {
    Node node = (Node) evaluate(expression, root, XPathConstants.NODE);
    if (node == null) {
      return null;
    }
    return new XNode(this, node, variables);
  }
//解析Value值
  private Object evaluate(String expression, Object root, QName returnType) {
    try {
      return xpath.evaluate(expression, root, returnType);
    } catch (Exception e) {
      throw new BuilderException("Error evaluating XPath.  Cause: " + e, e);
    }
  }
//  根据输入流创建dom对象
  private Document createDocument(InputSource inputSource) {
    // important: this must only be called AFTER common constructor
    try {
      DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
      factory.setFeature(XMLConstants.FEATURE_SECURE_PROCESSING, true);
      factory.setValidating(validation);

      factory.setNamespaceAware(false);
      factory.setIgnoringComments(true);
      factory.setIgnoringElementContentWhitespace(false);
      factory.setCoalescing(false);
      factory.setExpandEntityReferences(true);

      DocumentBuilder builder = factory.newDocumentBuilder();
      builder.setEntityResolver(entityResolver);
      builder.setErrorHandler(new ErrorHandler() {
        @Override
        public void error(SAXParseException exception) throws SAXException {
          throw exception;
        }

        @Override
        public void fatalError(SAXParseException exception) throws SAXException {
          throw exception;
        }

        @Override
        public void warning(SAXParseException exception) throws SAXException {
          // NOP
        }
      });
      return builder.parse(inputSource);
    } catch (Exception e) {
      throw new BuilderException("Error creating document instance.  Cause: " + e, e);
    }
  }
  //公共构造器
  private void commonConstructor(boolean validation, Properties variables, EntityResolver entityResolver) {
    this.validation = validation;
    this.entityResolver = entityResolver;
    this.variables = variables;
    XPathFactory factory = XPathFactory.newInstance();
    this.xpath = factory.newXPath();
  }

}
```

### 5.10.6、XNode节点

```java
/**
 * XNode
 * @author Clinton Begin
 */
public class XNode {
//w3c.dom.Node
  private final Node node;
  //名字
  private final String name;
  //body
  private final String body;
  //属性
  private final Properties attributes;
  //变量
  private final Properties variables;
  //XPathParser解析器
  private final XPathParser xpathParser;
  //构造函数
  public XNode(XPathParser xpathParser, Node node, Properties variables) {
    this.xpathParser = xpathParser;
    this.node = node;
    this.name = node.getNodeName();
    this.variables = variables;
    this.attributes = parseAttributes(node);
    this.body = parseBody(node);
  }
  //下一个节点
  public XNode newXNode(Node node) {
    return new XNode(xpathParser, node, variables);
  }
  //获取父节点
  public XNode getParent() {
    Node parent = node.getParentNode();
    if (!(parent instanceof Element)) {
      return null;
    } else {
      return new XNode(xpathParser, parent, variables);
    }
  }
  //获取路径
  public String getPath() {
    StringBuilder builder = new StringBuilder();
    Node current = node;
    while (current instanceof Element) {
      if (current != node) {
        builder.insert(0, "/");
      }
      builder.insert(0, current.getNodeName());
      current = current.getParentNode();
    }
    return builder.toString();
  }
  //获取基于指定的值
  public String getValueBasedIdentifier() {
    StringBuilder builder = new StringBuilder();
    XNode current = this;
    while (current != null) {
      if (current != this) {
        builder.insert(0, "_");
      }
      String value = current.getStringAttribute("id",
          current.getStringAttribute("value",
              current.getStringAttribute("property", null)));
      if (value != null) {
        value = value.replace('.', '_');
        builder.insert(0, "]");
        builder.insert(0,
            value);
        builder.insert(0, "[");
      }
      builder.insert(0, current.getName());
      current = current.getParent();
    }
    return builder.toString();
  }
  //校验正则值
  public String evalString(String expression) {
    return xpathParser.evalString(node, expression);
  }
  //校验boolean值
  public Boolean evalBoolean(String expression) {
    return xpathParser.evalBoolean(node, expression);
  }
  //校验double值
  public Double evalDouble(String expression) {
    return xpathParser.evalDouble(node, expression);
  }
  //获取XNode集合
  public List<XNode> evalNodes(String expression) {
    return xpathParser.evalNodes(node, expression);
  }
  //获取XNode
  public XNode evalNode(String expression) {
    return xpathParser.evalNode(node, expression);
  }
  //获取node节点
  public Node getNode() {
    return node;
  }
  //获取名字
  public String getName() {
    return name;
  }
   //获取string体
  public String getStringBody() {
    return getStringBody(null);
  }
  //获取string体
  public String getStringBody(String def) {
    if (body == null) {
      return def;
    } else {
      return body;
    }
  }
  //获取布尔体
  public Boolean getBooleanBody() {
    return getBooleanBody(null);
  }
  //获取布尔体
  public Boolean getBooleanBody(Boolean def) {
    if (body == null) {
      return def;
    } else {
      return Boolean.valueOf(body);
    }
  }
  //获取int体
  public Integer getIntBody() {
    return getIntBody(null);
  }
  //获取int体
  public Integer getIntBody(Integer def) {
    if (body == null) {
      return def;
    } else {
      return Integer.parseInt(body);
    }
  }
  //获取long体
  public Long getLongBody() {
    return getLongBody(null);
  }
  //获取long体
  public Long getLongBody(Long def) {
    if (body == null) {
      return def;
    } else {
      return Long.parseLong(body);
    }
  }
  //获取double体
  public Double getDoubleBody() {
    return getDoubleBody(null);
  }
  //获取double体
  public Double getDoubleBody(Double def) {
    if (body == null) {
      return def;
    } else {
      return Double.parseDouble(body);
    }
  }
  //获取float体
  public Float getFloatBody() {
    return getFloatBody(null);
  }
  //获取float体
  public Float getFloatBody(Float def) {
    if (body == null) {
      return def;
    } else {
      return Float.parseFloat(body);
    }
  }
  //获取枚举属性
  public <T extends Enum<T>> T getEnumAttribute(Class<T> enumType, String name) {
    return getEnumAttribute(enumType, name, null);
  }
  //获取枚举属性
  public <T extends Enum<T>> T getEnumAttribute(Class<T> enumType, String name, T def) {
    String value = getStringAttribute(name);
    if (value == null) {
      return def;
    } else {
      return Enum.valueOf(enumType, value);
    }
  }
  //获取string属性
  public String getStringAttribute(String name) {
    return getStringAttribute(name, null);
  }
  //获取string属性
  public String getStringAttribute(String name, String def) {
    String value = attributes.getProperty(name);
    if (value == null) {
      return def;
    } else {
      return value;
    }
  }
  //获取布尔属性
  public Boolean getBooleanAttribute(String name) {
    return getBooleanAttribute(name, null);
  }
  //获取布尔属性
  public Boolean getBooleanAttribute(String name, Boolean def) {
    String value = attributes.getProperty(name);
    if (value == null) {
      return def;
    } else {
      return Boolean.valueOf(value);
    }
  }
  //获取int属性
  public Integer getIntAttribute(String name) {
    return getIntAttribute(name, null);
  }
  //获取int属性
  public Integer getIntAttribute(String name, Integer def) {
    String value = attributes.getProperty(name);
    if (value == null) {
      return def;
    } else {
      return Integer.parseInt(value);
    }
  }
  //获取long属性
  public Long getLongAttribute(String name) {
    return getLongAttribute(name, null);
  }
  //获取long属性
  public Long getLongAttribute(String name, Long def) {
    String value = attributes.getProperty(name);
    if (value == null) {
      return def;
    } else {
      return Long.parseLong(value);
    }
  }
  //获取double属性
  public Double getDoubleAttribute(String name) {
    return getDoubleAttribute(name, null);
  }
  //获取double属性
  public Double getDoubleAttribute(String name, Double def) {
    String value = attributes.getProperty(name);
    if (value == null) {
      return def;
    } else {
      return Double.parseDouble(value);
    }
  }
  //获取float属性
  public Float getFloatAttribute(String name) {
    return getFloatAttribute(name, null);
  }
  //获取float属性
  public Float getFloatAttribute(String name, Float def) {
    String value = attributes.getProperty(name);
    if (value == null) {
      return def;
    } else {
      return Float.parseFloat(value);
    }
  }
  //获取子集
  public List<XNode> getChildren() {
    List<XNode> children = new ArrayList<>();
    NodeList nodeList = node.getChildNodes();
    if (nodeList != null) {
      for (int i = 0, n = nodeList.getLength(); i < n; i++) {
        Node node = nodeList.item(i);
        if (node.getNodeType() == Node.ELEMENT_NODE) {
          children.add(new XNode(xpathParser, node, variables));
        }
      }
    }
    return children;
  }
  //获取子集作为属性
  public Properties getChildrenAsProperties() {
    Properties properties = new Properties();
    for (XNode child : getChildren()) {
      String name = child.getStringAttribute("name");
      String value = child.getStringAttribute("value");
      if (name != null && value != null) {
        properties.setProperty(name, value);
      }
    }
    return properties;
  }

  @Override
  public String toString() {
    StringBuilder builder = new StringBuilder();
    toString(builder, 0);
    return builder.toString();
  }

  private void toString(StringBuilder builder, int level) {
    builder.append("<");
    builder.append(name);
    for (Map.Entry<Object, Object> entry : attributes.entrySet()) {
      builder.append(" ");
      builder.append(entry.getKey());
      builder.append("=\"");
      builder.append(entry.getValue());
      builder.append("\"");
    }
    List<XNode> children = getChildren();
    if (!children.isEmpty()) {
      builder.append(">\n");
      for (XNode child : children) {
        indent(builder, level + 1);
        child.toString(builder, level + 1);
      }
      indent(builder, level);
      builder.append("</");
      builder.append(name);
      builder.append(">");
    } else if (body != null) {
      builder.append(">");
      builder.append(body);
      builder.append("</");
      builder.append(name);
      builder.append(">");
    } else {
      builder.append("/>");
      indent(builder, level);
    }
    builder.append("\n");
  }
  //缩进
  private void indent(StringBuilder builder, int level) {
    for (int i = 0; i < level; i++) {
      builder.append("    ");
    }
  }
  //解析Node属性
  private Properties parseAttributes(Node n) {
    Properties attributes = new Properties();
    NamedNodeMap attributeNodes = n.getAttributes();
    if (attributeNodes != null) {
      for (int i = 0; i < attributeNodes.getLength(); i++) {
        Node attribute = attributeNodes.item(i);
        String value = PropertyParser.parse(attribute.getNodeValue(), variables);
        attributes.put(attribute.getNodeName(), value);
      }
    }
    return attributes;
  }
  //解析body体
  private String parseBody(Node node) {
    String data = getBodyData(node);
    if (data == null) {
      NodeList children = node.getChildNodes();
      for (int i = 0; i < children.getLength(); i++) {
        Node child = children.item(i);
        data = getBodyData(child);
        if (data != null) {
          break;
        }
      }
    }
    return data;
  }
  //获取Node节点
  private String getBodyData(Node child) {
    if (child.getNodeType() == Node.CDATA_SECTION_NODE
        || child.getNodeType() == Node.TEXT_NODE) {
      String data = ((CharacterData) child).getData();
      data = PropertyParser.parse(data, variables);
      return data;
    }
    return null;
  }

}
```

## 5.11、org.apache.ibatis.transaction（事务模块）

### 5.11.1、工厂模块

![image-20200310224126148](/Users/didi/Library/Application Support/typora-user-images/image-20200310224126148.png)

#### 5.11.1.1、事务工厂接口TransactionFactory

```java
/**
 * 创建事务实例
 * Creates {@link Transaction} instances.
 *
 * @author Clinton Begin
 */
public interface TransactionFactory {

  /**
   * 设置事务工厂定制属性
   * Sets transaction factory custom properties.
   * @param props
   */
  default void setProperties(Properties props) {
    // NOP
  }

  /**
   * 从现有链接创造事务
   *
   * Creates a {@link Transaction} out of an existing connection.
   * @param conn Existing database connection
   * @return Transaction
   * @since 3.1.0
   */
  Transaction newTransaction(Connection conn);

  /**
   * 从数据源创建事务
   * Creates a {@link Transaction} out of a datasource.
   * @param dataSource DataSource to take the connection from
   * @param level Desired isolation level
   * @param autoCommit Desired autocommit
   * @return Transaction
   * @since 3.1.0
   */
  Transaction newTransaction(DataSource dataSource, TransactionIsolationLevel level, boolean autoCommit);

}
```

【小结】工厂接口是提供了三种创建事务的模式，自己定制化，从链接运行中，从数据源。

#### 5.11.1.2、ManagedTransactionFactory被管理事务工厂

```java
/**
 * 创建被管理事务实例
 * Creates {@link ManagedTransaction} instances.
 *
 * @author Clinton Begin
 *
 * @see ManagedTransaction
 */
public class ManagedTransactionFactory implements TransactionFactory {
  //关闭链接，默认是true
  private boolean closeConnection = true;
  //设置相关事务管理属性，如果没有传入属性，则默认关闭的。，否则根据传入的属性判断是否关闭
  @Override
  public void setProperties(Properties props) {
    if (props != null) {
      String closeConnectionProperty = props.getProperty("closeConnection");
      if (closeConnectionProperty != null) {
        closeConnection = Boolean.parseBoolean(closeConnectionProperty);
      }
    }
  }
  //根据链接初始化被管理的事务
  @Override
  public Transaction newTransaction(Connection conn) {
    return new ManagedTransaction(conn, closeConnection);
  }
  //无提示的忽略自动提交和隔离级别，因为被管理事务完全被外部管理。目的是为了保证管理与被管理之间的可移植性
  @Override
  public Transaction newTransaction(DataSource ds, TransactionIsolationLevel level, boolean autoCommit) {
    return new ManagedTransaction(ds, level, closeConnection);
  }
}
```

#### 5.11.1.3、JdbcTransactionFactory数据链接事务工厂

```java
/**
 * 创建jdbc事务实例
 * Creates {@link JdbcTransaction} instances.
 *
 * @author Clinton Begin
 *
 * @see JdbcTransaction
 */
public class JdbcTransactionFactory implements TransactionFactory {
  // TODO: 2020/3/10 这里没实现setProperties是因为默认的是接口的实现 
  
  //根据链接创建事务
  @Override
  public Transaction newTransaction(Connection conn) {
    return new JdbcTransaction(conn);
  }
  //根据数据源实现
  @Override
  public Transaction newTransaction(DataSource ds, TransactionIsolationLevel level, boolean autoCommit) {
    return new JdbcTransaction(ds, level, autoCommit);
  }
}
```

【学习的点】

 接口的默认实现，可交由父类实现。

### 5.11.2、事务实例模块

![image-20200310223953521](/Users/didi/Library/Application Support/typora-user-images/image-20200310223953521.png)

#### 5.11.2.1、Transaction事务接口

```java
/**
 * 封装一个数据源链接
 * Wraps a database connection.
 * 处理链接的生命周期 包括：它的创建，准备，提交/回滚 和关闭
 * Handles the connection lifecycle that comprises: its creation, preparation, commit/rollback and close.
 *
 * @author Clinton Begin
 */
public interface Transaction {

  /**
   *取内部数据源链接
   * Retrieve inner database connection.
   * @return DataBase connection
   * @throws SQLException
   */
  Connection getConnection() throws SQLException;

  /**
   * 提交内部数据源链接
   * Commit inner database connection.
   * @throws SQLException
   */
  void commit() throws SQLException;

  /**
   * 回滚内部数据源链接
   * Rollback inner database connection.
   * @throws SQLException
   */
  void rollback() throws SQLException;

  /**
   * 关闭内部数据源链接
   * Close inner database connection.
   * @throws SQLException
   */
  void close() throws SQLException;

  /**
   * 如果设置了获取事务超时时间
   * Get transaction timeout if set.
   * @throws SQLException
   */
  Integer getTimeout() throws SQLException;

}
```

#### 5.11.2.2、ManagedTransaction被管理事务

```java
/**
 * 事务允许容器管理整个事务生命周期，延迟链接获取 直到getConnection()函数被调用。忽略所有提交或者回滚请求。默认关闭链接，但是可以被配置默认不关闭链接
 *
 * @author Clinton Begin
 *
 * @see ManagedTransactionFactory
 */
public class ManagedTransaction implements Transaction {
  //日志
  private static final Log log = LogFactory.getLog(ManagedTransaction.class);
  //数据源
  private DataSource dataSource;
  //事务隔离级别
  private TransactionIsolationLevel level;
  //链接
  private Connection connection;
  //是否关闭链接
  private final boolean closeConnection;
  //被管理事务构造器
  public ManagedTransaction(Connection connection, boolean closeConnection) {
    this.connection = connection;
    this.closeConnection = closeConnection;
  }
  //被管理事务构造器
  public ManagedTransaction(DataSource ds, TransactionIsolationLevel level, boolean closeConnection) {
    this.dataSource = ds;
    this.level = level;
    this.closeConnection = closeConnection;
  }
  //获取链接
  @Override
  public Connection getConnection() throws SQLException {
    if (this.connection == null) {
      openConnection();
    }
    return this.connection;
  }
  //提交操作，什么也没做
  @Override
  public void commit() throws SQLException {
    // Does nothing
  }
  //回滚操作，什么也没做
  @Override
  public void rollback() throws SQLException {
    // Does nothing
  }
  //关闭链接
  @Override
  public void close() throws SQLException {
    if (this.closeConnection && this.connection != null) {
      if (log.isDebugEnabled()) {
        log.debug("Closing JDBC Connection [" + this.connection + "]");
      }
      this.connection.close();
    }
  } 
  //打开链接，数据源获取链接，事务隔离级别部位空 设置事务隔离级别
  protected void openConnection() throws SQLException {
    if (log.isDebugEnabled()) {
      log.debug("Opening JDBC Connection");
    }
    this.connection = this.dataSource.getConnection();
    if (this.level != null) {
      this.connection.setTransactionIsolation(this.level.getLevel());
    }
  }
  //获取超时时间，没有超时
  @Override
  public Integer getTimeout() throws SQLException {
    return null;
  }

}
```

#### 5.11.2.3、JdbcTransaction数据链接事务

```java
/**
 * 事务允许使用JDBC提交和回滚直接操作，为了管理事务的范围 ，依赖从数据源获取的链接 。直到getConnection()被调用延迟链接获取。
 * 忽略提交或者回滚请求当自动提交设置以后
 * {@link Transaction} that makes use of the JDBC commit and rollback facilities directly.
 * It relies on the connection retrieved from the dataSource to manage the scope of the transaction.
 * Delays connection retrieval until getConnection() is called.
 * Ignores commit or rollback requests when autocommit is on.
 *
 * @author Clinton Begin
 *
 * @see JdbcTransactionFactory
 */
public class JdbcTransaction implements Transaction {
  //日志
  private static final Log log = LogFactory.getLog(JdbcTransaction.class);
  //链接
  protected Connection connection;
  //数据源
  protected DataSource dataSource;
  //事务隔离级别
  protected TransactionIsolationLevel level;
  //是否自动提交
  protected boolean autoCommit;
  //构造器
  public JdbcTransaction(DataSource ds, TransactionIsolationLevel desiredLevel, boolean desiredAutoCommit) {
    dataSource = ds;
    level = desiredLevel;
    autoCommit = desiredAutoCommit;
  }
  //构造器
  public JdbcTransaction(Connection connection) {
    this.connection = connection;
  }
  //获取链接
  @Override
  public Connection getConnection() throws SQLException {
    if (connection == null) {
      openConnection();
    }
    return connection;
  }
  //提交，判断是否自动提交
  @Override
  public void commit() throws SQLException {
    if (connection != null && !connection.getAutoCommit()) {
      if (log.isDebugEnabled()) {
        log.debug("Committing JDBC Connection [" + connection + "]");
      }
      connection.commit();
    }
  }
  //回滚，判断是否自动提交
  @Override
  public void rollback() throws SQLException {
    if (connection != null && !connection.getAutoCommit()) {
      if (log.isDebugEnabled()) {
        log.debug("Rolling back JDBC Connection [" + connection + "]");
      }
      connection.rollback();
    }
  }
 //关闭链接
  @Override
  public void close() throws SQLException {
    if (connection != null) {
      resetAutoCommit();
      if (log.isDebugEnabled()) {
        log.debug("Closing JDBC Connection [" + connection + "]");
      }
      connection.close();
    }
  }
 //设置要求的自动提交
  protected void setDesiredAutoCommit(boolean desiredAutoCommit) {
    try {
      if (connection.getAutoCommit() != desiredAutoCommit) {
        if (log.isDebugEnabled()) {
          log.debug("Setting autocommit to " + desiredAutoCommit + " on JDBC Connection [" + connection + "]");
        }
        connection.setAutoCommit(desiredAutoCommit);
      }
    } catch (SQLException e) {
      // Only a very poorly implemented driver would fail here,
      // and there's not much we can do about that.
      throw new TransactionException("Error configuring AutoCommit.  "
          + "Your driver may not support getAutoCommit() or setAutoCommit(). "
          + "Requested setting: " + desiredAutoCommit + ".  Cause: " + e, e);
    }
  }
  //重置自动提交
  protected void resetAutoCommit() {
    try {
      if (!connection.getAutoCommit()) {
        // MyBatis does not call commit/rollback on a connection if just selects were performed.
        // Some databases start transactions with select statements
        // and they mandate a commit/rollback before closing the connection.
        // A workaround is setting the autocommit to true before closing the connection.
        // Sybase throws an exception here.
        if (log.isDebugEnabled()) {
          log.debug("Resetting autocommit to true on JDBC Connection [" + connection + "]");
        }
        connection.setAutoCommit(true);
      }
    } catch (SQLException e) {
      if (log.isDebugEnabled()) {
        log.debug("Error resetting autocommit to true "
            + "before closing the connection.  Cause: " + e);
      }
    }
  }
  //打开链接，从数据源获取链接
  protected void openConnection() throws SQLException {
    if (log.isDebugEnabled()) {
      log.debug("Opening JDBC Connection");
    }
    connection = dataSource.getConnection();
    if (level != null) {
      connection.setTransactionIsolation(level.getLevel());
    }
    setDesiredAutoCommit(autoCommit);
  }
  //获取超时，返回空
  @Override
  public Integer getTimeout() throws SQLException {
    return null;
  }

}

```



### 5.11.3、TransactionException事务异常

```java
/**
 * 依然是继承了运行期异常
 * @author Clinton Begin
 */
public class TransactionException extends PersistenceException {

  private static final long serialVersionUID = -433589569461084605L;
  //空构造器
  public TransactionException() {
    super();
  }
  //传递信息构造器
  public TransactionException(String message) {
    super(message);
  }
  //传递信息和throwable构造器
  public TransactionException(String message, Throwable cause) {
    super(message, cause);
  }
  //传递throwable构造器
  public TransactionException(Throwable cause) {
    super(cause);
  }

}
```

### 5.11.4、小结

事务模块主要操作了jdbc的管理，统一由事务实例来替代。被管理事务和数据连接事务2种方式。

## 5.12、org.apache.ibatis.type（类型处理器模块）

模块类型处理器比较多，所以思路是先将基础的拆解出来讲源码实现（5.12.1～5.12.5节），然后根据该图依次讲解具体类型处理器的源码实现（5.12.6节（含）以后）。

<img src="/Users/didi/Documents/design/mybatis/chapter_5/type/typeFrame.png" alt="typeFrame" />



### 5.12.1、 Alias别名注解类

```java
/**
 * 指定别名的注解
 *用法是在类上直接使用
 * &#064;Alias("Email")
 * public class UserEmail {
 *   // ...
 * }
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Alias {
  /**
   * 返回别名
   */
  String value();
}
```

### 5.12.2、TypeAliasRegistry类型别名注册器

```java
/**
 * 类型别名注册器
 */
public class TypeAliasRegistry {
  //初始化类型别名Map容器
  private final Map<String, Class<?>> typeAliases = new HashMap<>();

  //类构造器
  public TypeAliasRegistry() {
    //字符串类型
    registerAlias("string", String.class);
    //字节类型
    registerAlias("byte", Byte.class);
    //长整型
    registerAlias("long", Long.class);
    //短整型
    registerAlias("short", Short.class);
    //整数型
    registerAlias("int", Integer.class);
    //整数型
    registerAlias("integer", Integer.class);
    //双精度
    registerAlias("double", Double.class);
    //单精度
    registerAlias("float", Float.class);
    //布尔
    registerAlias("boolean", Boolean.class);
    //字节数组
    registerAlias("byte[]", Byte[].class);
    //长整型数组
    registerAlias("long[]", Long[].class);
    //短整型数组
    registerAlias("short[]", Short[].class);
    //整数型数组
    registerAlias("int[]", Integer[].class);
    //整数型包装类数组
    registerAlias("integer[]", Integer[].class);
    //双精度数组
    registerAlias("double[]", Double[].class);
    //单精度数组
    registerAlias("float[]", Float[].class);
    //布尔类型数组
    registerAlias("boolean[]", Boolean[].class);

    //字节类型
    registerAlias("_byte", byte.class);
    //长整型类型
    registerAlias("_long", long.class);
    //短整型类型
    registerAlias("_short", short.class);
    //整数型类型
    registerAlias("_int", int.class);
    //包装类整数类型
    registerAlias("_integer", int.class);
    //双精度类型
    registerAlias("_double", double.class);
    //单精度类型
    registerAlias("_float", float.class);
    //布尔类型
    registerAlias("_boolean", boolean.class);

    //字节数组类型
    registerAlias("_byte[]", byte[].class);
    //长整型数组类型
    registerAlias("_long[]", long[].class);
    //短整型数组类型
    registerAlias("_short[]", short[].class);
    //整数型类型数组
    registerAlias("_int[]", int[].class);
    //包装类整数型数组
    registerAlias("_integer[]", int[].class);
    //双精度数组
    registerAlias("_double[]", double[].class);
    //单精度数组
    registerAlias("_float[]", float[].class);
    //布尔类型数组
    registerAlias("_boolean[]", boolean[].class);

    //日期类型
    registerAlias("date", Date.class);
    //金融计算bigdecimal
    registerAlias("decimal", BigDecimal.class);
    registerAlias("bigdecimal", BigDecimal.class);
    //大整型
    registerAlias("biginteger", BigInteger.class);
    //对象类型
    registerAlias("object", Object.class);

    //日期数组类型
    registerAlias("date[]", Date[].class);
    //bigdecimal类型数组
    registerAlias("decimal[]", BigDecimal[].class);
    registerAlias("bigdecimal[]", BigDecimal[].class);
    //biginteger数组
    registerAlias("biginteger[]", BigInteger[].class);
    //对象数组
    registerAlias("object[]", Object[].class);

    //map数据结构
    registerAlias("map", Map.class);
    //hashmap
    registerAlias("hashmap", HashMap.class);
    //list集合
    registerAlias("list", List.class);
    //数组集合
    registerAlias("arraylist", ArrayList.class);
    //collection集合
    registerAlias("collection", Collection.class);
    //迭代器
    registerAlias("iterator", Iterator.class);
    //返回结果集
    registerAlias("ResultSet", ResultSet.class);
  }

  @SuppressWarnings("unchecked")
  //如果类型未注册则抛出类型转化异常
  public <T> Class<T> resolveAlias(String string) {
    try {
      //如果传入别名为空，则返回为空
      if (string == null) {
        return null;
      }
      // issue #748   https://github.com/mybatis/mybatis-3/issues/748
      String key = string.toLowerCase(Locale.ENGLISH);
      Class<T> value;
      //注册器是否包含该别名
      if (typeAliases.containsKey(key)) {
        //直接获取该别名处理器
        value = (Class<T>) typeAliases.get(key);
      } else {
        //根据资源器通过类名获取
        value = (Class<T>) Resources.classForName(string);
      }
      return value;
    } catch (ClassNotFoundException e) {
      throw new TypeException("Could not resolve type alias '" + string + "'.  Cause: " + e, e);
    }
  }

  /**
   * 注册包下的所有类,不包括内部类和接口
   * @param packageName
   */
  public void registerAliases(String packageName) {
    registerAliases(packageName, Object.class);
  }

  public void registerAliases(String packageName, Class<?> superType) {
    ResolverUtil<Class<?>> resolverUtil = new ResolverUtil<>();
    resolverUtil.find(new ResolverUtil.IsA(superType), packageName);
    Set<Class<? extends Class<?>>> typeSet = resolverUtil.getClasses();
    for (Class<?> type : typeSet) {
    //See issue #6
      if (!type.isAnonymousClass() && !type.isInterface() && !type.isMemberClass()) {
        registerAlias(type);
      }
    }
  }

  /**
   * 注册别名
   * @param type
   */
  public void registerAlias(Class<?> type) {
    String alias = type.getSimpleName();
    Alias aliasAnnotation = type.getAnnotation(Alias.class);
    if (aliasAnnotation != null) {
      alias = aliasAnnotation.value();
    }
    registerAlias(alias, type);
  }

  /**
   * 注册别名，别名和指定的类
   * @param alias
   * @param value
   */
  public void registerAlias(String alias, Class<?> value) {
    if (alias == null) {
      throw new TypeException("The parameter alias cannot be null");
    }
    // issue #748
    String key = alias.toLowerCase(Locale.ENGLISH);
    if (typeAliases.containsKey(key) && typeAliases.get(key) != null && !typeAliases.get(key).equals(value)) {
      throw new TypeException("The alias '" + alias + "' is already mapped to the value '" + typeAliases.get(key).getName() + "'.");
    }
    typeAliases.put(key, value);
  }

  /**
   * 注册别名，别名和资源加载的类
   * @param alias
   * @param value
   */
  public void registerAlias(String alias, String value) {
    try {
      registerAlias(alias, Resources.classForName(value));
    } catch (ClassNotFoundException e) {
      throw new TypeException("Error registering type alias " + alias + " for " + value + ". Cause: " + e, e);
    }
  }

  /**
   * 获取只读权限的注册容器map
   * @since 3.2.2
   */
  public Map<String, Class<?>> getTypeAliases() {
    return Collections.unmodifiableMap(typeAliases);
  }

}
```

【归纳小结】借鉴之处：

* 注册器可通过map在类构造器加载的时候初始化与之前我写的枚举类通过静态属性随类加载一样。相当于本地缓存，为后期校验和获取提供内存查询。
* 获取只读权限的注册容器，防止外部对内部容器数据修改。
* 【todo】待追究的，注册器容器注册了基本数据类型，基本数据类型数组，包装类型，集合，一些工具类型，还有下划线的基本数据类型和数组

### 5.12.3、TypeException类型异常

```java
/**
 * 类型异常，之前在异常的包 解释过PersistenceException 继承 已经废弃的IbatisException 继承 RuntimeException
 * 所以从根本上讲继承了运行异常，但是统一外部异常父类PersistenceException
 * @author Clinton Begin
 */
public class TypeException extends PersistenceException {
  //序列化版本号
  private static final long serialVersionUID = 8614420898975117130L;
  //空构造器
  public TypeException() {
    super();
  }
 //传递信息构造器
  public TypeException(String message) {
    super(message);
  }
 //传递信息和Throwable的构造器
  public TypeException(String message, Throwable cause) {
    super(message, cause);
  }
  //传递Throwable的构造器
  public TypeException(Throwable cause) {
    super(cause);
  }

}

```

### 5.12.4、MappedTypes类型指定注解

```java
/**
 * 这个注解指定map集合中的类型处理器：xxxTypeHandler
 *使用方法
 * &#064;MappedTypes(String.class)
 * public class StringTrimmingTypeHandler implements TypeHandler&lt;String&gt; {
 *   // ...
 * }
 * </pre>
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface MappedTypes {
  /**
   * 返回map中的类型处理器
   */
  Class<?>[] value();
}
```

### 5.12.5、MappedJdbcTypes，jdbc类型指定注解

```java
/**
 * 这个注解也指定了集合中的关于jdbc的类型处理器：xxxTypeHandler
 *使用方法
 * <pre>
 * &#064;MappedJdbcTypes({JdbcType.CHAR, JdbcType.VARCHAR})
 * public class StringTrimmingTypeHandler implements TypeHandler&lt;String&gt; {
 *   // ...
 * }
 * </pre>
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface MappedJdbcTypes {
  /**
   * 返回JDBC的类型
   */
  JdbcType[] value();

  /**
   * 集合中不存在是否返回
   */
  boolean includeNullJdbcType() default false;
}
```

【tips】这里的jdbc类型是通过TypeHandlerRegistry注册进去的，可以参考5.12.7节

### 5.12.6、 TypeHandler<T>类型处理器

```java
/**
 * 范型类型处理器
 */
public interface TypeHandler<T> {
  /**
   * 设置参数，会话，位置，参数，jdbc类型
   */
  void setParameter(PreparedStatement ps, int i, T parameter, JdbcType jdbcType) throws SQLException;

  /**
   * 当配置useColumnLabel is false, 根据列名获取结果
   */
  T getResult(ResultSet rs, String columnName) throws SQLException;

  /**
   * 根据列下标获取结果
   */
  T getResult(ResultSet rs, int columnIndex) throws SQLException;

  /**
   * 根据回调会话，列下标获取结果
   */
  T getResult(CallableStatement cs, int columnIndex) throws SQLException;

}
```

### 5.12.7、TypeReference类型引用

```java
/**
 * 引用泛型类型
 */
public abstract class TypeReference<T> {
  //原生类型
  private final Type rawType;
  //类构造函数，初始化原生类型 通过运行期间返回当前运行的类，然后获取原生类型
  protected TypeReference() {
    rawType = getSuperclassTypeParameter(getClass());
  }

  /**
   * 根据类文件获取原生类型
   */
  Type getSuperclassTypeParameter(Class<?> clazz) {
    //获取类，接口函数等返回的实体类型
    Type genericSuperclass = clazz.getGenericSuperclass();
    //如果返回的实体类型属于class类
    if (genericSuperclass instanceof Class) {
      // try to climb up the hierarchy until meet something useful
      //继续获取上层的有用的类型
      if (TypeReference.class != genericSuperclass) {
        return getSuperclassTypeParameter(clazz.getSuperclass());
      }
      //未找到类型引用
      throw new TypeException("'" + getClass() + "' extends TypeReference but misses the type parameter. "
        + "Remove the extension or add a type parameter to it.");
    }
    //获取实际类型参数
    Type rawType = ((ParameterizedType) genericSuperclass).getActualTypeArguments()[0];
    // TODO remove this when Reflector is fixed to return Types
    //如果反射修复可以返回类型，可以移除下面的这个返回逻辑
    if (rawType instanceof ParameterizedType) {
      rawType = ((ParameterizedType) rawType).getRawType();
    }

    return rawType;
  }
  //获取原生类型 不可变
  public final Type getRawType() {
    return rawType;
  }

  @Override
  public String toString() {
    return rawType.toString();
  }

}
```

### 5.12.8、TypeHandlerRegistry类型处理器注册

![TypeHandlerRegistry](/Users/didi/Documents/design/mybatis/chapter_5/type/TypeHandlerRegistry.png)

```java
/**
 * 类型处理注册器，不可变类，意味类加载之后不可变
 * @author Clinton Begin
 * @author Kazuki Shimizu
 */
public final class TypeHandlerRegistry {
  //存放jdbc类型处理器---枚举类map-------jdbcType
  private final Map<JdbcType, TypeHandler<?>>  jdbcTypeHandlerMap = new EnumMap<>(JdbcType.class);
  //存放类型处理器 ------Type
  private final Map<Type, Map<JdbcType, TypeHandler<?>>> typeHandlerMap = new ConcurrentHashMap<>();
  //未知类型处理器----unknownType
  private final TypeHandler<Object> unknownTypeHandler;
  //所有类型处理器集合
  private final Map<Class<?>, TypeHandler<?>> allTypeHandlersMap = new HashMap<>();
  //空类型处理器集合-----jdbc,这里为啥存在的这个集合我觉得在于org.apache.ibatis.type.MappedJdbcTypes.includeNullJdbcType
  private static final Map<JdbcType, TypeHandler<?>> NULL_TYPE_HANDLER_MAP = Collections.emptyMap();
  //默认的枚举类型处理器
  private Class<? extends TypeHandler> defaultEnumTypeHandler = EnumTypeHandler.class;

  /**
   * 默认的构造器初始化配置,调用有参构造器初始化
   * The default constructor.
   */
  public TypeHandlerRegistry() {
    this(new Configuration());
  }

  /**
   * 传递MyBatis 配置 的构造器
   * The constructor that pass the MyBatis configuration.
   *
   * @param configuration a MyBatis configuration
   * @since 3.5.4
   */
  public TypeHandlerRegistry(Configuration configuration) {
    //未知类型处理器的赋值
    this.unknownTypeHandler = new UnknownTypeHandler(configuration);
    //注册常用的--布尔类型包装类型，基本数据类型，jdbc对应的type，boolean和bit
    register(Boolean.class, new BooleanTypeHandler());
    register(boolean.class, new BooleanTypeHandler());
    register(JdbcType.BOOLEAN, new BooleanTypeHandler());
    register(JdbcType.BIT, new BooleanTypeHandler());

    //注册常用的byte字节类型，包装类型，基本数据类型，jdbc对应的tinyint类型。
    register(Byte.class, new ByteTypeHandler());
    register(byte.class, new ByteTypeHandler());
    register(JdbcType.TINYINT, new ByteTypeHandler());

    //注册常用的short字符类型，包装类型，基本数据类型，jdbc对应的smallint类型
    register(Short.class, new ShortTypeHandler());
    register(short.class, new ShortTypeHandler());
    register(JdbcType.SMALLINT, new ShortTypeHandler());

    //注册常用的整数类型，包装基本整数类型，基本数据类型，jdbc对应的Integer类型
    register(Integer.class, new IntegerTypeHandler());
    register(int.class, new IntegerTypeHandler());
    register(JdbcType.INTEGER, new IntegerTypeHandler());

    //注册常用的长整型类型，包装类型，基本数据类型，
    register(Long.class, new LongTypeHandler());
    register(long.class, new LongTypeHandler());

    //注册单精度类型，包装类型，基本数据类型，jdbc的FLOAT类型
    register(Float.class, new FloatTypeHandler());
    register(float.class, new FloatTypeHandler());
    register(JdbcType.FLOAT, new FloatTypeHandler());

    //注册双精度类型，包装类型，基本数据类型，jdbc的Double类型
    register(Double.class, new DoubleTypeHandler());
    register(double.class, new DoubleTypeHandler());
    register(JdbcType.DOUBLE, new DoubleTypeHandler());

    //注册Clob文件类型处理器，字符串类型处理器，jdbc的字符和文本处理器
    register(Reader.class, new ClobReaderTypeHandler());
    register(String.class, new StringTypeHandler());
    register(String.class, JdbcType.CHAR, new StringTypeHandler());
    register(String.class, JdbcType.CLOB, new ClobTypeHandler());
    register(String.class, JdbcType.VARCHAR, new StringTypeHandler());
    register(String.class, JdbcType.LONGVARCHAR, new StringTypeHandler());
    register(String.class, JdbcType.NVARCHAR, new NStringTypeHandler());
    register(String.class, JdbcType.NCHAR, new NStringTypeHandler());
    register(String.class, JdbcType.NCLOB, new NClobTypeHandler());
    register(JdbcType.CHAR, new StringTypeHandler());
    register(JdbcType.VARCHAR, new StringTypeHandler());
    register(JdbcType.CLOB, new ClobTypeHandler());
    register(JdbcType.LONGVARCHAR, new StringTypeHandler());
    register(JdbcType.NVARCHAR, new NStringTypeHandler());
    register(JdbcType.NCHAR, new NStringTypeHandler());
    register(JdbcType.NCLOB, new NClobTypeHandler());

    //注册数组类型处理器
    register(Object.class, JdbcType.ARRAY, new ArrayTypeHandler());
    register(JdbcType.ARRAY, new ArrayTypeHandler());

    //注册大整型处理器-》长整型
    register(BigInteger.class, new BigIntegerTypeHandler());
    register(JdbcType.BIGINT, new LongTypeHandler());

    //注册金融精度计算处理器
    register(BigDecimal.class, new BigDecimalTypeHandler());
    register(JdbcType.REAL, new BigDecimalTypeHandler());
    register(JdbcType.DECIMAL, new BigDecimalTypeHandler());
    register(JdbcType.NUMERIC, new BigDecimalTypeHandler());
    //注册文本类型的，字节数组，包装类字节数组，BLOB类型等
    register(InputStream.class, new BlobInputStreamTypeHandler());
    register(Byte[].class, new ByteObjectArrayTypeHandler());
    register(Byte[].class, JdbcType.BLOB, new BlobByteObjectArrayTypeHandler());
    register(Byte[].class, JdbcType.LONGVARBINARY, new BlobByteObjectArrayTypeHandler());
    register(byte[].class, new ByteArrayTypeHandler());
    register(byte[].class, JdbcType.BLOB, new BlobTypeHandler());
    register(byte[].class, JdbcType.LONGVARBINARY, new BlobTypeHandler());
    register(JdbcType.LONGVARBINARY, new BlobTypeHandler());
    register(JdbcType.BLOB, new BlobTypeHandler());

    //注册未知类型处理器
    register(Object.class, unknownTypeHandler);
    register(Object.class, JdbcType.OTHER, unknownTypeHandler);
    register(JdbcType.OTHER, unknownTypeHandler);

    //注册时间相关的处理器
    register(Date.class, new DateTypeHandler());
    register(Date.class, JdbcType.DATE, new DateOnlyTypeHandler());
    register(Date.class, JdbcType.TIME, new TimeOnlyTypeHandler());
    register(JdbcType.TIMESTAMP, new DateTypeHandler());
    register(JdbcType.DATE, new DateOnlyTypeHandler());
    register(JdbcType.TIME, new TimeOnlyTypeHandler());

    //注册数据库SQL相关的处理器
    register(java.sql.Date.class, new SqlDateTypeHandler());
    register(java.sql.Time.class, new SqlTimeTypeHandler());
    register(java.sql.Timestamp.class, new SqlTimestampTypeHandler());
    //注册SQLXML类型的处理器
    register(String.class, JdbcType.SQLXML, new SqlxmlTypeHandler());
    //注册时间线上的瞬时点，以及各种日期
    register(Instant.class, new InstantTypeHandler());
    register(LocalDateTime.class, new LocalDateTimeTypeHandler());
    register(LocalDate.class, new LocalDateTypeHandler());
    register(LocalTime.class, new LocalTimeTypeHandler());
    register(OffsetDateTime.class, new OffsetDateTimeTypeHandler());
    register(OffsetTime.class, new OffsetTimeTypeHandler());
    register(ZonedDateTime.class, new ZonedDateTimeTypeHandler());
    register(Month.class, new MonthTypeHandler());
    register(Year.class, new YearTypeHandler());
    register(YearMonth.class, new YearMonthTypeHandler());
    register(JapaneseDate.class, new JapaneseDateTypeHandler());

    // issue #273
    //注册字符处理器
    register(Character.class, new CharacterTypeHandler());
    register(char.class, new CharacterTypeHandler());
  }

  /**
   * 设置默认的枚举类型处理器
   * Set a default {@link TypeHandler} class for {@link Enum}.
   * A default {@link TypeHandler} is {@link org.apache.ibatis.type.EnumTypeHandler}.
   * @param typeHandler a type handler class for {@link Enum}
   * @since 3.4.5
   */
  public void setDefaultEnumTypeHandler(Class<? extends TypeHandler> typeHandler) {
    this.defaultEnumTypeHandler = typeHandler;
  }

  /**
   * 根据类判断是否有类型处理器
   * @param javaType
   * @return
   */
  public boolean hasTypeHandler(Class<?> javaType) {
    return hasTypeHandler(javaType, null);
  }

  /**
   * 根据类型引用判断是否有类型处理器
   * @param javaTypeReference
   * @return
   */
  public boolean hasTypeHandler(TypeReference<?> javaTypeReference) {
    return hasTypeHandler(javaTypeReference, null);
  }

  /**
   * Java类型不为空，并且既有Java类型也有jdbc类型
   * @param javaType
   * @param jdbcType
   * @return
   */
  public boolean hasTypeHandler(Class<?> javaType, JdbcType jdbcType) {
    return javaType != null && getTypeHandler((Type) javaType, jdbcType) != null;
  }

  /**
   * Java类型引用不为空，并且既有Java引用类型也有jdbc类型
   * @param javaTypeReference
   * @param jdbcType
   * @return
   */
  public boolean hasTypeHandler(TypeReference<?> javaTypeReference, JdbcType jdbcType) {
    return javaTypeReference != null && getTypeHandler(javaTypeReference, jdbcType) != null;
  }

  /**
   * 获取指定的具体类型处理器
   * @param handlerType
   * @return
   */
  public TypeHandler<?> getMappingTypeHandler(Class<? extends TypeHandler<?>> handlerType) {
    return allTypeHandlersMap.get(handlerType);
  }

  /**
   * 根据类型获取类型处理器
   * @param type
   * @param <T>
   * @return
   */
  public <T> TypeHandler<T> getTypeHandler(Class<T> type) {
    return getTypeHandler((Type) type, null);
  }

  /**
   * 根据类型引用获取类型处理器
   * @param javaTypeReference
   * @param <T>
   * @return
   */
  public <T> TypeHandler<T> getTypeHandler(TypeReference<T> javaTypeReference) {
    return getTypeHandler(javaTypeReference, null);
  }

  /**
   * 根据jdbc获取类型处理器
   * @param jdbcType
   * @return
   */
  public TypeHandler<?> getTypeHandler(JdbcType jdbcType) {
    return jdbcTypeHandlerMap.get(jdbcType);
  }

  /**
   * 根据类类型和jdbc类型获取类型处理器
   * @param type
   * @param jdbcType
   * @param <T>
   * @return
   */
  public <T> TypeHandler<T> getTypeHandler(Class<T> type, JdbcType jdbcType) {
    return getTypeHandler((Type) type, jdbcType);
  }

  /**
   * 根据类型引用和jdbc类型获取类型处理器
   * @param javaTypeReference
   * @param jdbcType
   * @param <T>
   * @return
   */
  public <T> TypeHandler<T> getTypeHandler(TypeReference<T> javaTypeReference, JdbcType jdbcType) {
    return getTypeHandler(javaTypeReference.getRawType(), jdbcType);
  }

  @SuppressWarnings("unchecked")
  /**
   * 根据类型和jdbc类型获取对应的类型处理器
   */
  private <T> TypeHandler<T> getTypeHandler(Type type, JdbcType jdbcType) {
    //如果已经绑定过则直接返回
    if (ParamMap.class.equals(type)) {
      return null;
    }
    Map<JdbcType, TypeHandler<?>> jdbcHandlerMap = getJdbcHandlerMap(type);
    TypeHandler<?> handler = null;
    if (jdbcHandlerMap != null) {
      handler = jdbcHandlerMap.get(jdbcType);
      if (handler == null) {
        handler = jdbcHandlerMap.get(null);
      }
      if (handler == null) {
        // #591
        handler = pickSoleHandler(jdbcHandlerMap);
      }
    }
    // type drives generics here
    return (TypeHandler<T>) handler;
  }

  /**
   * 根据jdbc类型获取jdbc类型处理器
   * @param type
   * @return
   */
  private Map<JdbcType, TypeHandler<?>> getJdbcHandlerMap(Type type) {
    Map<JdbcType, TypeHandler<?>> jdbcHandlerMap = typeHandlerMap.get(type);
    if (NULL_TYPE_HANDLER_MAP.equals(jdbcHandlerMap)) {
      return null;
    }
    if (jdbcHandlerMap == null && type instanceof Class) {
      Class<?> clazz = (Class<?>) type;
      if (Enum.class.isAssignableFrom(clazz)) {
        Class<?> enumClass = clazz.isAnonymousClass() ? clazz.getSuperclass() : clazz;
        jdbcHandlerMap = getJdbcHandlerMapForEnumInterfaces(enumClass, enumClass);
        if (jdbcHandlerMap == null) {
          register(enumClass, getInstance(enumClass, defaultEnumTypeHandler));
          return typeHandlerMap.get(enumClass);
        }
      } else {
        jdbcHandlerMap = getJdbcHandlerMapForSuperclass(clazz);
      }
    }
    typeHandlerMap.put(type, jdbcHandlerMap == null ? NULL_TYPE_HANDLER_MAP : jdbcHandlerMap);
    return jdbcHandlerMap;
  }

  /**
   * 根据class和枚举class获取对应的处理器
   * @param clazz
   * @param enumClazz
   * @return
   */
  private Map<JdbcType, TypeHandler<?>> getJdbcHandlerMapForEnumInterfaces(Class<?> clazz, Class<?> enumClazz) {
    for (Class<?> iface : clazz.getInterfaces()) {
      Map<JdbcType, TypeHandler<?>> jdbcHandlerMap = typeHandlerMap.get(iface);
      if (jdbcHandlerMap == null) {
        jdbcHandlerMap = getJdbcHandlerMapForEnumInterfaces(iface, enumClazz);
      }
      if (jdbcHandlerMap != null) {
        // Found a type handler regsiterd to a super interface
        HashMap<JdbcType, TypeHandler<?>> newMap = new HashMap<>();
        for (Entry<JdbcType, TypeHandler<?>> entry : jdbcHandlerMap.entrySet()) {
          // Create a type handler instance with enum type as a constructor arg
          newMap.put(entry.getKey(), getInstance(enumClazz, entry.getValue().getClass()));
        }
        return newMap;
      }
    }
    return null;
  }

  /**
   * 根据class获取父类处理器
   * @param clazz
   * @return
   */
  private Map<JdbcType, TypeHandler<?>> getJdbcHandlerMapForSuperclass(Class<?> clazz) {
    Class<?> superclass =  clazz.getSuperclass();
    if (superclass == null || Object.class.equals(superclass)) {
      return null;
    }
    Map<JdbcType, TypeHandler<?>> jdbcHandlerMap = typeHandlerMap.get(superclass);
    if (jdbcHandlerMap != null) {
      return jdbcHandlerMap;
    } else {
      return getJdbcHandlerMapForSuperclass(superclass);
    }
  }

  /**
   * 根据jdbc处理器处理器去除重复的
   * @param jdbcHandlerMap
   * @return
   */
  private TypeHandler<?> pickSoleHandler(Map<JdbcType, TypeHandler<?>> jdbcHandlerMap) {
    TypeHandler<?> soleHandler = null;
    for (TypeHandler<?> handler : jdbcHandlerMap.values()) {
      if (soleHandler == null) {
        soleHandler = handler;
      } else if (!handler.getClass().equals(soleHandler.getClass())) {
        // More than one type handlers registered.
        return null;
      }
    }
    return soleHandler;
  }

  /**
   * 获取未知类型处理器
   * @return
   */
  public TypeHandler<Object> getUnknownTypeHandler() {
    return unknownTypeHandler;
  }

  /**
   * 注册 jdbc类型和对应的处理器
   * @param jdbcType
   * @param handler
   */
  public void register(JdbcType jdbcType, TypeHandler<?> handler) {
    jdbcTypeHandlerMap.put(jdbcType, handler);
  }

  //
  // REGISTER INSTANCE
  //

  // Only handler

  @SuppressWarnings("unchecked")
  /**
   * 注册处理器的实例
   */
  public <T> void register(TypeHandler<T> typeHandler) {
    boolean mappedTypeFound = false;
    MappedTypes mappedTypes = typeHandler.getClass().getAnnotation(MappedTypes.class);
    if (mappedTypes != null) {
      for (Class<?> handledType : mappedTypes.value()) {
        register(handledType, typeHandler);
        mappedTypeFound = true;
      }
    }
    // @since 3.1.0 - try to auto-discover the mapped type
    if (!mappedTypeFound && typeHandler instanceof TypeReference) {
      try {
        TypeReference<T> typeReference = (TypeReference<T>) typeHandler;
        register(typeReference.getRawType(), typeHandler);
        mappedTypeFound = true;
      } catch (Throwable t) {
        // maybe users define the TypeReference with a different type and are not assignable, so just ignore it
      }
    }
    if (!mappedTypeFound) {
      register((Class<T>) null, typeHandler);
    }
  }

  // java type + handler

  /**
   * Java类型和处理器的注册
   * @param javaType
   * @param typeHandler
   * @param <T>
   */
  public <T> void register(Class<T> javaType, TypeHandler<? extends T> typeHandler) {
    register((Type) javaType, typeHandler);
  }

  /**
   * 根据Java类型，类型处理器获取MappedJdbcTypes实例
   * @param javaType
   * @param typeHandler
   * @param <T>
   */
  private <T> void register(Type javaType, TypeHandler<? extends T> typeHandler) {
    MappedJdbcTypes mappedJdbcTypes = typeHandler.getClass().getAnnotation(MappedJdbcTypes.class);
    if (mappedJdbcTypes != null) {
      for (JdbcType handledJdbcType : mappedJdbcTypes.value()) {
        register(javaType, handledJdbcType, typeHandler);
      }
      if (mappedJdbcTypes.includeNullJdbcType()) {
        register(javaType, null, typeHandler);
      }
    } else {
      register(javaType, null, typeHandler);
    }
  }

  /**
   * 注册引用类型和类型处理器
   * @param javaTypeReference
   * @param handler
   * @param <T>
   */
  public <T> void register(TypeReference<T> javaTypeReference, TypeHandler<? extends T> handler) {
    register(javaTypeReference.getRawType(), handler);
  }

  // java type + jdbc type + handler

  /**
   * 注册java type + jdbc type + handler
   * @param type
   * @param jdbcType
   * @param handler
   * @param <T>
   */
  public <T> void register(Class<T> type, JdbcType jdbcType, TypeHandler<? extends T> handler) {
    register((Type) type, jdbcType, handler);
  }

  /**
   * 实现上面的函数
   * @param javaType
   * @param jdbcType
   * @param handler
   */
  private void register(Type javaType, JdbcType jdbcType, TypeHandler<?> handler) {
    if (javaType != null) {
      Map<JdbcType, TypeHandler<?>> map = typeHandlerMap.get(javaType);
      if (map == null || map == NULL_TYPE_HANDLER_MAP) {
        map = new HashMap<>();
        typeHandlerMap.put(javaType, map);
      }
      map.put(jdbcType, handler);
    }
    allTypeHandlersMap.put(handler.getClass(), handler);
  }

  //
  // REGISTER CLASS
  //

  // Only handler type

  /**
   * 注册类
   * @param typeHandlerClass
   */
  public void register(Class<?> typeHandlerClass) {
    boolean mappedTypeFound = false;
    MappedTypes mappedTypes = typeHandlerClass.getAnnotation(MappedTypes.class);
    if (mappedTypes != null) {
      for (Class<?> javaTypeClass : mappedTypes.value()) {
        register(javaTypeClass, typeHandlerClass);
        mappedTypeFound = true;
      }
    }
    if (!mappedTypeFound) {
      register(getInstance(null, typeHandlerClass));
    }
  }

  // java type + handler type

  /**
   * 注册java type + handler type
   * @param javaTypeClassName
   * @param typeHandlerClassName
   * @throws ClassNotFoundException
   */
  public void register(String javaTypeClassName, String typeHandlerClassName) throws ClassNotFoundException {
    register(Resources.classForName(javaTypeClassName), Resources.classForName(typeHandlerClassName));
  }

  /**
   * 实现上面的函数
   * @param javaTypeClass
   * @param typeHandlerClass
   */
  public void register(Class<?> javaTypeClass, Class<?> typeHandlerClass) {
    register(javaTypeClass, getInstance(javaTypeClass, typeHandlerClass));
  }

  // java type + jdbc type + handler type
 //注册Java类型，jdbc类型，根据类型和处理类获取的实例
  public void register(Class<?> javaTypeClass, JdbcType jdbcType, Class<?> typeHandlerClass) {
    register(javaTypeClass, jdbcType, getInstance(javaTypeClass, typeHandlerClass));
  }

  // Construct a handler (used also from Builders)

  @SuppressWarnings("unchecked")
  //从构建者来获取的 构造一个处理器
  public <T> TypeHandler<T> getInstance(Class<?> javaTypeClass, Class<?> typeHandlerClass) {
    if (javaTypeClass != null) {
      try {
        Constructor<?> c = typeHandlerClass.getConstructor(Class.class);
        return (TypeHandler<T>) c.newInstance(javaTypeClass);
      } catch (NoSuchMethodException ignored) {
        // ignored
      } catch (Exception e) {
        throw new TypeException("Failed invoking constructor for handler " + typeHandlerClass, e);
      }
    }
    try {
      Constructor<?> c = typeHandlerClass.getConstructor();
      return (TypeHandler<T>) c.newInstance();
    } catch (Exception e) {
      throw new TypeException("Unable to find a usable constructor for " + typeHandlerClass, e);
    }
  }

  // scan
  //注册包下的类型处理器
  public void register(String packageName) {
    ResolverUtil<Class<?>> resolverUtil = new ResolverUtil<>();
    resolverUtil.find(new ResolverUtil.IsA(TypeHandler.class), packageName);
    Set<Class<? extends Class<?>>> handlerSet = resolverUtil.getClasses();
    for (Class<?> type : handlerSet) {
      //Ignore inner classes and interfaces (including package-info.java) and abstract classes
      if (!type.isAnonymousClass() && !type.isInterface() && !Modifier.isAbstract(type.getModifiers())) {
        register(type);
      }
    }
  }

  // get information

  /**
   * 只提供集合的只读权限
   * @since 3.2.2
   */
  public Collection<TypeHandler<?>> getTypeHandlers() {
    return Collections.unmodifiableCollection(allTypeHandlersMap.values());
  }

}
```

【归纳小结】

1. 根据源码可以知道类型处理器主要处理三类：

* 1-Java类型
* 2-jdbc类型
* 3-类型处理器

2. 该源码在类加载的时候初始化了Mybatis核心Configruation类（来源Java或者配置文件）

### 5.12.9、BaseTypeHandler基本类型处理器

```java
/**
 * TypeHandler是为了处理通用的范型
 * The base {@link TypeHandler} for references a generic type.
 * 重要：从3.5.0版本后，对于处理SQL空值，这个类从不调用ResultSet#wasNull()和CallableStatement#wasNull()方法。
 * 换句话说，空值处理应该放到其子类
 * <p>
 * Important: Since 3.5.0, This class never call the {@link ResultSet#wasNull()} and
 * {@link CallableStatement#wasNull()} method for handling the SQL {@code NULL} value.
 * In other words, {@code null} value handling should be performed on subclass.
 * </p>
 *
 * @author Clinton Begin
 * @author Simone Tripodi
 * @author Kzuki Shimizu
 */
public abstract class BaseTypeHandler<T> extends TypeReference<T> implements TypeHandler<T> {

  /**
   * 这个属性3.5.0版本以后被移除了
   * @deprecated Since 3.5.0 - See https://github.com/mybatis/mybatis-3/issues/1203. This field will remove future.
   */
  @Deprecated
  protected Configuration configuration;

  /**
   * 这个属性3.5.0版本以后被移除了
   * @deprecated Since 3.5.0 - See https://github.com/mybatis/mybatis-3/issues/1203. This property will remove future.
   */
  @Deprecated
  public void setConfiguration(Configuration c) {
    this.configuration = c;
  }

  /**
   * 设置参数 预会话，位置，参数，jdbc类型
   * @param ps
   * @param i
   * @param parameter
   * @param jdbcType
   * @throws SQLException
   */
  @Override
  public void setParameter(PreparedStatement ps, int i, T parameter, JdbcType jdbcType) throws SQLException {
    //如果参数是空的
    if (parameter == null) {
      //jdbc类型不能为空，参数可以为空
      if (jdbcType == null) {
        throw new TypeException("JDBC requires that the JdbcType must be specified for all nullable parameters.");
      }
      try {
        //参数为空，会话设置空
        ps.setNull(i, jdbcType.TYPE_CODE);
      } catch (SQLException e) {
        throw new TypeException("Error setting null for parameter #" + i + " with JdbcType " + jdbcType + " . "
              + "Try setting a different JdbcType for this parameter or a different jdbcTypeForNull configuration property. "
              + "Cause: " + e, e);
      }
    } else {
      //参数不为空
      try {
        setNonNullParameter(ps, i, parameter, jdbcType);
      } catch (Exception e) {
        throw new TypeException("Error setting non null for parameter #" + i + " with JdbcType " + jdbcType + " . "
              + "Try setting a different JdbcType for this parameter or a different configuration property. "
              + "Cause: " + e, e);
      }
    }
  }

  /**
   * 根据列名获取返回结果，当配置useColumnLabel=false的时候
   * @param rs
   * @param columnName Colunm name, when configuration <code>useColumnLabel</code> is <code>false</code>
   * @return
   * @throws SQLException
   */
  @Override
  public T getResult(ResultSet rs, String columnName) throws SQLException {
    try {
      return getNullableResult(rs, columnName);
    } catch (Exception e) {
      throw new ResultMapException("Error attempting to get column '" + columnName + "' from result set.  Cause: " + e, e);
    }
  }

  /**
   * 根据列的索引获取返回结果
   * @param rs
   * @param columnIndex
   * @return
   * @throws SQLException
   */
  @Override
  public T getResult(ResultSet rs, int columnIndex) throws SQLException {
    try {
      return getNullableResult(rs, columnIndex);
    } catch (Exception e) {
      throw new ResultMapException("Error attempting to get column #" + columnIndex + " from result set.  Cause: " + e, e);
    }
  }

  /**
   * 根据列的索引获取返回结果
   * @param cs
   * @param columnIndex
   * @return
   * @throws SQLException
   */
  @Override
  public T getResult(CallableStatement cs, int columnIndex) throws SQLException {
    try {
      return getNullableResult(cs, columnIndex);
    } catch (Exception e) {
      throw new ResultMapException("Error attempting to get column #" + columnIndex + " from callable statement.  Cause: " + e, e);
    }
  }

  /**
   * 设置非空参数，如上面所说，这些都会交给子类去实现。定义为抽象方法
   * @param ps
   * @param i
   * @param parameter
   * @param jdbcType
   * @throws SQLException
   */
  public abstract void setNonNullParameter(PreparedStatement ps, int i, T parameter, JdbcType jdbcType) throws SQLException;

  /**
   * 根据列名获取返回结果  当配置useColumnLabel=false
   * @param columnName Colunm name, when configuration <code>useColumnLabel</code> is <code>false</code>
   */
  public abstract T getNullableResult(ResultSet rs, String columnName) throws SQLException;

  /**
   * 根据列下标获取可为空的返回结果
   * @param rs
   * @param columnIndex
   * @return
   * @throws SQLException
   */
  public abstract T getNullableResult(ResultSet rs, int columnIndex) throws SQLException;

  /**
   * 根据列下标和回调会话获取可为空的返回结果
   * @param cs
   * @param columnIndex
   * @return
   * @throws SQLException
   */
  public abstract T getNullableResult(CallableStatement cs, int columnIndex) throws SQLException;

}
```

【学习关注点】

* 抽象类，抽象方法和接口的使用，经典示例。

### 5.12.10、JdbcType数据jdbc类型

```java
/**
 * jdbc枚举类型
 * @author Clinton Begin
 */
public enum JdbcType {
  /*
  添加该类型是能够基础支持数组数据类型--但是定制的类型处理器依然是必须要的
   * This is added to enable basic support for the
   * ARRAY data type - but a custom type handler is still required
   */
  ARRAY(Types.ARRAY),
  BIT(Types.BIT),
  TINYINT(Types.TINYINT),
  SMALLINT(Types.SMALLINT),
  INTEGER(Types.INTEGER),
  BIGINT(Types.BIGINT),
  FLOAT(Types.FLOAT),
  REAL(Types.REAL),
  DOUBLE(Types.DOUBLE),
  NUMERIC(Types.NUMERIC),
  DECIMAL(Types.DECIMAL),
  CHAR(Types.CHAR),
  VARCHAR(Types.VARCHAR),
  LONGVARCHAR(Types.LONGVARCHAR),
  DATE(Types.DATE),
  TIME(Types.TIME),
  TIMESTAMP(Types.TIMESTAMP),
  BINARY(Types.BINARY),
  VARBINARY(Types.VARBINARY),
  LONGVARBINARY(Types.LONGVARBINARY),
  NULL(Types.NULL),
  OTHER(Types.OTHER),
  BLOB(Types.BLOB),
  CLOB(Types.CLOB),
  BOOLEAN(Types.BOOLEAN),
  CURSOR(-10), // Oracle
  UNDEFINED(Integer.MIN_VALUE + 1000),
  NVARCHAR(Types.NVARCHAR), // JDK6
  NCHAR(Types.NCHAR), // JDK6
  NCLOB(Types.NCLOB), // JDK6
  STRUCT(Types.STRUCT),
  JAVA_OBJECT(Types.JAVA_OBJECT),
  DISTINCT(Types.DISTINCT),
  REF(Types.REF),
  DATALINK(Types.DATALINK),
  ROWID(Types.ROWID), // JDK6
  LONGNVARCHAR(Types.LONGNVARCHAR), // JDK6
  SQLXML(Types.SQLXML), // JDK6
  DATETIMEOFFSET(-155), // SQL Server 2008
  TIME_WITH_TIMEZONE(Types.TIME_WITH_TIMEZONE), // JDBC 4.2 JDK8
  TIMESTAMP_WITH_TIMEZONE(Types.TIMESTAMP_WITH_TIMEZONE); // JDBC 4.2 JDK8

  public final int TYPE_CODE;
  //利用hashmap作为本地缓存，这点值得借鉴
  private static Map<Integer,JdbcType> codeLookup = new HashMap<>();

  static {
    for (JdbcType type : JdbcType.values()) {
      codeLookup.put(type.TYPE_CODE, type);
    }
  }

  JdbcType(int code) {
    this.TYPE_CODE = code;
  }

  public static JdbcType forCode(int code)  {
    return codeLookup.get(code);
  }

}
```

【学习关注点】

* 本地缓存的使用，类加载顺序

### 5.12.11、EnumTypeHandler枚举类型处理器

```java
/**
 * 枚举类型处理器
 * @author Clinton Begin
 */
public class EnumTypeHandler<E extends Enum<E>> extends BaseTypeHandler<E> {
  //不可变的class类型
  private final Class<E> type;

  public EnumTypeHandler(Class<E> type) {
    if (type == null) {
      throw new IllegalArgumentException("Type argument cannot be null");
    }
    this.type = type;
  }

  /**
   * 实现抽象父类 的设置可空参数
   * @param ps
   * @param i
   * @param parameter
   * @param jdbcType
   * @throws SQLException
   */
  @Override
  public void setNonNullParameter(PreparedStatement ps, int i, E parameter, JdbcType jdbcType) throws SQLException {
    if (jdbcType == null) {
      ps.setString(i, parameter.name());
    } else {
      ps.setObject(i, parameter.name(), jdbcType.TYPE_CODE); // see r3589
    }
  }

  /**
   * 获取可空结果
   * @param rs
   * @param columnName Colunm name, when configuration <code>useColumnLabel</code> is <code>false</code>
   * @return
   * @throws SQLException
   */
  @Override
  public E getNullableResult(ResultSet rs, String columnName) throws SQLException {
    String s = rs.getString(columnName);
    return s == null ? null : Enum.valueOf(type, s);
  }

  /**
   * 获取可空结果
   * @param rs
   * @param columnIndex
   * @return
   * @throws SQLException
   */
  @Override
  public E getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
    String s = rs.getString(columnIndex);
    return s == null ? null : Enum.valueOf(type, s);
  }

  /**
   * 获取可空结果
   * @param cs
   * @param columnIndex
   * @return
   * @throws SQLException
   */
  @Override
  public E getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
    String s = cs.getString(columnIndex);
    return s == null ? null : Enum.valueOf(type, s);
  }
}
```

### 5.12.12、ArrayTypeHandler数组类型处理器

```java
/**
 * 数组类型处理器
 * @author Clinton Begin
 */
public class ArrayTypeHandler extends BaseTypeHandler<Object> {
//  初始化标准的映射容器
  private static final ConcurrentHashMap<Class<?>, String> STANDARD_MAPPING;
  static {
    STANDARD_MAPPING = new ConcurrentHashMap<>();
    STANDARD_MAPPING.put(BigDecimal.class, JdbcType.NUMERIC.name());
    STANDARD_MAPPING.put(BigInteger.class, JdbcType.BIGINT.name());
    STANDARD_MAPPING.put(boolean.class, JdbcType.BOOLEAN.name());
    STANDARD_MAPPING.put(Boolean.class, JdbcType.BOOLEAN.name());
    STANDARD_MAPPING.put(byte[].class, JdbcType.VARBINARY.name());
    STANDARD_MAPPING.put(byte.class, JdbcType.TINYINT.name());
    STANDARD_MAPPING.put(Byte.class, JdbcType.TINYINT.name());
    STANDARD_MAPPING.put(Calendar.class, JdbcType.TIMESTAMP.name());
    STANDARD_MAPPING.put(java.sql.Date.class, JdbcType.DATE.name());
    STANDARD_MAPPING.put(java.util.Date.class, JdbcType.TIMESTAMP.name());
    STANDARD_MAPPING.put(double.class, JdbcType.DOUBLE.name());
    STANDARD_MAPPING.put(Double.class, JdbcType.DOUBLE.name());
    STANDARD_MAPPING.put(float.class, JdbcType.REAL.name());
    STANDARD_MAPPING.put(Float.class, JdbcType.REAL.name());
    STANDARD_MAPPING.put(int.class, JdbcType.INTEGER.name());
    STANDARD_MAPPING.put(Integer.class, JdbcType.INTEGER.name());
    STANDARD_MAPPING.put(LocalDate.class, JdbcType.DATE.name());
    STANDARD_MAPPING.put(LocalDateTime.class, JdbcType.TIMESTAMP.name());
    STANDARD_MAPPING.put(LocalTime.class, JdbcType.TIME.name());
    STANDARD_MAPPING.put(long.class, JdbcType.BIGINT.name());
    STANDARD_MAPPING.put(Long.class, JdbcType.BIGINT.name());
    STANDARD_MAPPING.put(OffsetDateTime.class, JdbcType.TIMESTAMP_WITH_TIMEZONE.name());
    STANDARD_MAPPING.put(OffsetTime.class, JdbcType.TIME_WITH_TIMEZONE.name());
    STANDARD_MAPPING.put(Short.class, JdbcType.SMALLINT.name());
    STANDARD_MAPPING.put(String.class, JdbcType.VARCHAR.name());
    STANDARD_MAPPING.put(Time.class, JdbcType.TIME.name());
    STANDARD_MAPPING.put(Timestamp.class, JdbcType.TIMESTAMP.name());
    STANDARD_MAPPING.put(URL.class, JdbcType.DATALINK.name());
  }

  public ArrayTypeHandler() {
    super();
  }

  /**
   * 设置非空参数
   * @param ps
   * @param i
   * @param parameter
   * @param jdbcType
   * @throws SQLException
   */
  @Override
  public void setNonNullParameter(PreparedStatement ps, int i, Object parameter, JdbcType jdbcType)
      throws SQLException {
    if (parameter instanceof Array) {
      // it's the user's responsibility to properly free() the Array instance
      ps.setArray(i, (Array) parameter);
    } else {
      if (!parameter.getClass().isArray()) {
        throw new TypeException(
            "ArrayType Handler requires SQL array or java array parameter and does not support type "
                + parameter.getClass());
      }
      Class<?> componentType = parameter.getClass().getComponentType();
      String arrayTypeName = resolveTypeName(componentType);
      Array array = ps.getConnection().createArrayOf(arrayTypeName, (Object[]) parameter);
      ps.setArray(i, array);
      //该方法主要是释放array数据库返回的结果
      array.free();
    }
  }

  /**
   * 解析类型名称
   * @param type
   * @return
   */
  protected String resolveTypeName(Class<?> type) {
    return STANDARD_MAPPING.getOrDefault(type, JdbcType.JAVA_OBJECT.name());
  }

  /**
   * 获取可空结果
   * @param rs
   * @param columnName Colunm name, when configuration <code>useColumnLabel</code> is <code>false</code>
   * @return
   * @throws SQLException
   */
  @Override
  public Object getNullableResult(ResultSet rs, String columnName) throws SQLException {
    return extractArray(rs.getArray(columnName));
  }

  /**
   * 获取可空结果
   * @param rs
   * @param columnIndex
   * @return
   * @throws SQLException
   */
  @Override
  public Object getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
    return extractArray(rs.getArray(columnIndex));
  }

  /**
   * 获取可空结果
   * @param cs
   * @param columnIndex
   * @return
   * @throws SQLException
   */
  @Override
  public Object getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
    return extractArray(cs.getArray(columnIndex));
  }

  /**
   * 提取数组内容
   * @param array
   * @return
   * @throws SQLException
   */
  protected Object extractArray(Array array) throws SQLException {
    if (array == null) {
      return null;
    }
    Object result = array.getArray();
    array.free();
    return result;
  }

}
```

【关注学习点】

* 返回的结果集的释放，避免内存的持续占有内存空间。

### 5.12.13、....其他内容类似，不再赘述

## 5.13、org.apache.ibatis.reflection反射模块

### 5.13.1、反射工具类

#### 5.13.1.1、ReflectorFactory

```java
/**
 * 反射工厂
 */
public interface ReflectorFactory {
  /**
   * 是否开启类缓存
   * @return
   */
  boolean isClassCacheEnabled();

  /**
   * 设置类缓存开启
   * @param classCacheEnabled
   */
  void setClassCacheEnabled(boolean classCacheEnabled);

  /**
   * 根据class类型 找到反射类
   * @param type
   * @return
   */
  Reflector findForClass(Class<?> type);
}
```

#### 5.13.1.2、DefaultRefletorFactory默认反射工厂

```java
/**
 * 默认的反射工厂 实现反射工厂接口
 */
public class DefaultReflectorFactory implements ReflectorFactory {
  /**
   * 类缓存 默认开启
   */
  private boolean classCacheEnabled = true;
  /**
   * 反射容器
   */
  private final ConcurrentMap<Class<?>, Reflector> reflectorMap = new ConcurrentHashMap<>();

  /**
   * 默认的反射工厂
   */
  public DefaultReflectorFactory() {
  }

  /**
   * 是否开启类缓存
   * @return
   */
  @Override
  public boolean isClassCacheEnabled() {
    return classCacheEnabled;
  }

  /**
   * 设置类缓存
   * @param classCacheEnabled
   */
  @Override
  public void setClassCacheEnabled(boolean classCacheEnabled) {
    this.classCacheEnabled = classCacheEnabled;
  }

  /**
   * 如果开启类缓存，则直接从反射并非容器中获取，否则 new反射类
   * @param type
   * @return
   */
  @Override
  public Reflector findForClass(Class<?> type) {
    if (classCacheEnabled) {
      // synchronized (type) removed see issue #461
      return reflectorMap.computeIfAbsent(type, Reflector::new);
    } else {
      return new Reflector(type);
    }
  }

}
```

【归纳小结】反射类获取，会根据是否开启设置类加载缓存判断从本地缓存取，还是直接new。在框架接口实现，往往会 有默认的实现，以及定制化私人的可配实现。

#### 5.13.1.3、Reflector反射类

```java
/**
 * 这个类代表了一个被缓存的类定义信息集合，这个集合允许很轻松映射属性名字和get/set方法。
 * This class represents a cached set of class definition information that
 * allows for easy mapping between property names and getter/setter methods.
 *
 * @author Clinton Begin
 */
public class Reflector {
  // class 范型类型
  private final Class<?> type;
  // 可读属性名字列表
  private final String[] readablePropertyNames;
  //可写的属性名字列表
  private final String[] writablePropertyNames;
  // set方法的容器
  private final Map<String, Invoker> setMethods = new HashMap<>();
  // get方法的容器
  private final Map<String, Invoker> getMethods = new HashMap<>();
  // set的类型容器
  private final Map<String, Class<?>> setTypes = new HashMap<>();
  // get的类型容器
  private final Map<String, Class<?>> getTypes = new HashMap<>();
  // 默认的构造器
  private Constructor<?> defaultConstructor;
  //不区分大小写的属性集合
  private Map<String, String> caseInsensitivePropertyMap = new HashMap<>();
  //根据class类入参的构造器
  public Reflector(Class<?> clazz) {
    //初始化type类型
    type = clazz;
    //添加默认的构造器
    addDefaultConstructor(clazz);
    //添加class的get方法
    addGetMethods(clazz);
    //添加class的set方法
    addSetMethods(clazz);
    //添加class的属性
    addFields(clazz);
    //赋值的get和set方法放入容器
    readablePropertyNames = getMethods.keySet().toArray(new String[0]);
    writablePropertyNames = setMethods.keySet().toArray(new String[0]);
    //遍历get和set方法名字放入不区分大小写的容器中
    for (String propName : readablePropertyNames) {
      caseInsensitivePropertyMap.put(propName.toUpperCase(Locale.ENGLISH), propName);
    }
    for (String propName : writablePropertyNames) {
      caseInsensitivePropertyMap.put(propName.toUpperCase(Locale.ENGLISH), propName);
    }
  }

  /**
   * 根据class类添加默认的构造器
   * @param clazz
   */
  private void addDefaultConstructor(Class<?> clazz) {
    //class获取声明的构造器
    Constructor<?>[] constructors = clazz.getDeclaredConstructors();
    //从数组中找默认无参构造器
    Arrays.stream(constructors).filter(constructor -> constructor.getParameterTypes().length == 0)
      .findAny().ifPresent(constructor -> this.defaultConstructor = constructor);
  }

  /**
   * 根据class类添加get方法
   * @param clazz
   */
  private void addGetMethods(Class<?> clazz) {
    Map<String, List<Method>> conflictingGetters = new HashMap<>();
    Method[] methods = getClassMethods(clazz);
    Arrays.stream(methods).filter(m -> m.getParameterTypes().length == 0 && PropertyNamer.isGetter(m.getName()))
      .forEach(m -> addMethodConflict(conflictingGetters, PropertyNamer.methodToProperty(m.getName()), m));
    //移除get冲突
    resolveGetterConflicts(conflictingGetters);
  }

  /**
   * 移除get冲突
   * @param conflictingGetters
   */
  private void resolveGetterConflicts(Map<String, List<Method>> conflictingGetters) {
    for (Entry<String, List<Method>> entry : conflictingGetters.entrySet()) {
      Method winner = null;
      String propName = entry.getKey();
      boolean isAmbiguous = false;
      //遍历get方法
      for (Method candidate : entry.getValue()) {
        if (winner == null) {
          winner = candidate;
          continue;
        }
        Class<?> winnerType = winner.getReturnType();
        Class<?> candidateType = candidate.getReturnType();
        //校验返回值类型
        if (candidateType.equals(winnerType)) {
          if (!boolean.class.equals(candidateType)) {
            //不明确的类型
            isAmbiguous = true;
            break;
            //get方法属性包含is开头
          } else if (candidate.getName().startsWith("is")) {
            winner = candidate;
          }
         // winnerType是candidateType父类
        } else if (candidateType.isAssignableFrom(winnerType)) {
          // OK getter type is descendant
          //candidateType是winner的父类
        } else if (winnerType.isAssignableFrom(candidateType)) {
          winner = candidate;
        } else {
          //不明确的类型
          isAmbiguous = true;
          break;
        }
      }
      //添加get方法
      addGetMethod(propName, winner, isAmbiguous);
    }
  }
  //添加get方法
  private void addGetMethod(String name, Method method, boolean isAmbiguous) {
    MethodInvoker invoker = isAmbiguous
        ? new AmbiguousMethodInvoker(method, MessageFormat.format(
            "Illegal overloaded getter method with ambiguous type for property ''{0}'' in class ''{1}''. This breaks the JavaBeans specification and can cause unpredictable results.",
            name, method.getDeclaringClass().getName()))
        : new MethodInvoker(method);
    //赋值get方法集合
    getMethods.put(name, invoker);
    //根据方法解析返回类型
    Type returnType = TypeParameterResolver.resolveReturnType(method, type);
    //赋值get类型集合
    getTypes.put(name, typeToClass(returnType));
  }
  //添加set方法
  private void addSetMethods(Class<?> clazz) {
    Map<String, List<Method>> conflictingSetters = new HashMap<>();
    Method[] methods = getClassMethods(clazz);
    //遍历数组获取set方法
    Arrays.stream(methods).filter(m -> m.getParameterTypes().length == 1 && PropertyNamer.isSetter(m.getName()))
      .forEach(m -> addMethodConflict(conflictingSetters, PropertyNamer.methodToProperty(m.getName()), m));
    //移除冲突的set方法
    resolveSetterConflicts(conflictingSetters);
  }

  /**
   * 去重方法冲突
   * @param conflictingMethods
   * @param name
   * @param method
   */
  private void addMethodConflict(Map<String, List<Method>> conflictingMethods, String name, Method method) {
    if (isValidPropertyName(name)) {
      List<Method> list = conflictingMethods.computeIfAbsent(name, k -> new ArrayList<>());
      list.add(method);
    }
  }
  //解决set方法冲突
  private void resolveSetterConflicts(Map<String, List<Method>> conflictingSetters) {
    //遍历冲突的属性
    for (String propName : conflictingSetters.keySet()) {
      //获取set方法集合
      List<Method> setters = conflictingSetters.get(propName);
      //获取get类型
      Class<?> getterType = getTypes.get(propName);
      //是否是语义不清的名称
      boolean isGetterAmbiguous = getMethods.get(propName) instanceof AmbiguousMethodInvoker;
      boolean isSetterAmbiguous = false;
      Method match = null;
      //进行匹配筛选最合适的
      for (Method setter : setters) {
        //如果不是语义不明的get方法，并且参数类型和get类型一致，则是最匹配的
        if (!isGetterAmbiguous && setter.getParameterTypes()[0].equals(getterType)) {
          // should be the best match
          match = setter;
          break;
        }
        if (!isSetterAmbiguous) {
          //获取最匹配的set方法
          match = pickBetterSetter(match, setter, propName);
          isSetterAmbiguous = match == null;
        }
      }
      if (match != null) {
        addSetMethod(propName, match);
      }
    }
  }
  //获取最匹配的set方法
  private Method pickBetterSetter(Method setter1, Method setter2, String property) {
    if (setter1 == null) {
      return setter2;
    }
    //获取匹配的参数类型
    Class<?> paramType1 = setter1.getParameterTypes()[0];
    //获取set的参数类型
    Class<?> paramType2 = setter2.getParameterTypes()[0];
    //校验是否派生类型
    if (paramType1.isAssignableFrom(paramType2)) {
      return setter2;
    } else if (paramType2.isAssignableFrom(paramType1)) {
      return setter1;
    }
    //语义不明 的方法执行
    MethodInvoker invoker = new AmbiguousMethodInvoker(setter1,
        MessageFormat.format(
            "Ambiguous setters defined for property ''{0}'' in class ''{1}'' with types ''{2}'' and ''{3}''.",
            property, setter2.getDeclaringClass().getName(), paramType1.getName(), paramType2.getName()));
    //set方法集合存储
    setMethods.put(property, invoker);
    Type[] paramTypes = TypeParameterResolver.resolveParamTypes(setter1, type);
    //set类型集合存储
    setTypes.put(property, typeToClass(paramTypes[0]));
    return null;
  }
   //添加set方法，方法集合和类型集合
  private void addSetMethod(String name, Method method) {
    MethodInvoker invoker = new MethodInvoker(method);
    setMethods.put(name, invoker);
    Type[] paramTypes = TypeParameterResolver.resolveParamTypes(method, type);
    setTypes.put(name, typeToClass(paramTypes[0]));
  }
  //type转class类型
  private Class<?> typeToClass(Type src) {
    Class<?> result = null;
    if (src instanceof Class) {
      result = (Class<?>) src;
    } else if (src instanceof ParameterizedType) {
      result = (Class<?>) ((ParameterizedType) src).getRawType();
    } else if (src instanceof GenericArrayType) {
      Type componentType = ((GenericArrayType) src).getGenericComponentType();
      if (componentType instanceof Class) {
        result = Array.newInstance((Class<?>) componentType, 0).getClass();
      } else {
        Class<?> componentClass = typeToClass(componentType);
        result = Array.newInstance(componentClass, 0).getClass();
      }
    }
    if (result == null) {
      result = Object.class;
    }
    return result;
  }
  //添加属性
  private void addFields(Class<?> clazz) {
    Field[] fields = clazz.getDeclaredFields();
    for (Field field : fields) {
      if (!setMethods.containsKey(field.getName())) {
        // issue #379 - removed the check for final because JDK 1.5 allows
        // modification of final fields through reflection (JSR-133). (JGB)
        // pr #16 - final static can only be set by the classloader
        int modifiers = field.getModifiers();
        if (!(Modifier.isFinal(modifiers) && Modifier.isStatic(modifiers))) {
          addSetField(field);
        }
      }
      if (!getMethods.containsKey(field.getName())) {
        addGetField(field);
      }
    }
    if (clazz.getSuperclass() != null) {
      addFields(clazz.getSuperclass());
    }
  }
  //添加set属性，set方法集合，set类型集合
  private void addSetField(Field field) {
    if (isValidPropertyName(field.getName())) {
      setMethods.put(field.getName(), new SetFieldInvoker(field));
      Type fieldType = TypeParameterResolver.resolveFieldType(field, type);
      setTypes.put(field.getName(), typeToClass(fieldType));
    }
  }
  //添加get属性，get方法集合，get类型集合
  private void addGetField(Field field) {
    if (isValidPropertyName(field.getName())) {
      getMethods.put(field.getName(), new GetFieldInvoker(field));
      Type fieldType = TypeParameterResolver.resolveFieldType(field, type);
      getTypes.put(field.getName(), typeToClass(fieldType));
    }
  }
  //是否有效属性名称，不包含$或者serialVersionUID或者class
  private boolean isValidPropertyName(String name) {
    return !(name.startsWith("$") || "serialVersionUID".equals(name) || "class".equals(name));
  }

  /**
   * 这个方法返回一串数组 包含所有的在类声明的方法和一些子类。我们使用这个方法代替简单的 <code>Class.getMethods()</code>，因为可以private的方法也遍历出来。
   * This method returns an array containing all methods
   * declared in this class and any superclass.
   * We use this method, instead of the simpler <code>Class.getMethods()</code>,
   * because we want to look for private methods as well.
   *
   * @param clazz The class
   * @return An array containing all methods in this class
   */
  private Method[] getClassMethods(Class<?> clazz) {
    Map<String, Method> uniqueMethods = new HashMap<>();
    Class<?> currentClass = clazz;
    while (currentClass != null && currentClass != Object.class) {
      addUniqueMethods(uniqueMethods, currentClass.getDeclaredMethods());

      // we also need to look for interface methods -
      // because the class may be abstract
      Class<?>[] interfaces = currentClass.getInterfaces();
      for (Class<?> anInterface : interfaces) {
        addUniqueMethods(uniqueMethods, anInterface.getMethods());
      }

      currentClass = currentClass.getSuperclass();
    }

    Collection<Method> methods = uniqueMethods.values();

    return methods.toArray(new Method[0]);
  }
  //添加唯一的方法
  private void addUniqueMethods(Map<String, Method> uniqueMethods, Method[] methods) {
    for (Method currentMethod : methods) {
      if (!currentMethod.isBridge()) {
        String signature = getSignature(currentMethod);
        // check to see if the method is already known
        // if it is known, then an extended class must have
        // overridden a method
        if (!uniqueMethods.containsKey(signature)) {
          uniqueMethods.put(signature, currentMethod);
        }
      }
    }
  }
  //获取方法签名
  private String getSignature(Method method) {
    StringBuilder sb = new StringBuilder();
    Class<?> returnType = method.getReturnType();
    if (returnType != null) {
      sb.append(returnType.getName()).append('#');
    }
    sb.append(method.getName());
    Class<?>[] parameters = method.getParameterTypes();
    for (int i = 0; i < parameters.length; i++) {
      sb.append(i == 0 ? ':' : ',').append(parameters[i].getName());
    }
    return sb.toString();
  }

  /**
   * 检查是否能控制成员访问
   * Checks whether can control member accessible.
   *
   * @return If can control member accessible, it return {@literal true}
   * @since 3.5.0
   */
  public static boolean canControlMemberAccessible() {
    try {
      SecurityManager securityManager = System.getSecurityManager();
      if (null != securityManager) {
        securityManager.checkPermission(new ReflectPermission("suppressAccessChecks"));
      }
    } catch (SecurityException e) {
      return false;
    }
    return true;
  }

  /**
   * 获取类实例提供的信息名称
   * Gets the name of the class the instance provides information for.
   *
   * @return The class name
   */
  public Class<?> getType() {
    return type;
  }
  //获取默认的构造器
  public Constructor<?> getDefaultConstructor() {
    if (defaultConstructor != null) {
      return defaultConstructor;
    } else {
      throw new ReflectionException("There is no default constructor for " + type);
    }
  }
  //判断是否有默认的构造器
  public boolean hasDefaultConstructor() {
    return defaultConstructor != null;
  }
  //获取set方法执行器
  public Invoker getSetInvoker(String propertyName) {
    Invoker method = setMethods.get(propertyName);
    if (method == null) {
      throw new ReflectionException("There is no setter for property named '" + propertyName + "' in '" + type + "'");
    }
    return method;
  }
  //获取get方法执行器
  public Invoker getGetInvoker(String propertyName) {
    Invoker method = getMethods.get(propertyName);
    if (method == null) {
      throw new ReflectionException("There is no getter for property named '" + propertyName + "' in '" + type + "'");
    }
    return method;
  }

  /**
   * 获取set属性的type类型
   * Gets the type for a property setter.
   *
   * @param propertyName - the name of the property
   * @return The Class of the property setter
   */
  public Class<?> getSetterType(String propertyName) {
    Class<?> clazz = setTypes.get(propertyName);
    if (clazz == null) {
      throw new ReflectionException("There is no setter for property named '" + propertyName + "' in '" + type + "'");
    }
    return clazz;
  }

  /**
   * 获取get属性的type类型
   * Gets the type for a property getter.
   *
   * @param propertyName - the name of the property
   * @return The Class of the property getter
   */
  public Class<?> getGetterType(String propertyName) {
    Class<?> clazz = getTypes.get(propertyName);
    if (clazz == null) {
      throw new ReflectionException("There is no getter for property named '" + propertyName + "' in '" + type + "'");
    }
    return clazz;
  }

  /**
   * 获取一个对象的可读属性
   * Gets an array of the readable properties for an object.
   *
   * @return The array
   */
  public String[] getGetablePropertyNames() {
    return readablePropertyNames;
  }

  /**
   * 获取一个对象的可写属性
   * Gets an array of the writable properties for an object.
   *
   * @return The array
   */
  public String[] getSetablePropertyNames() {
    return writablePropertyNames;
  }

  /**
   * 检查是否类属性可以写权限
   * Check to see if a class has a writable property by name.
   *
   * @param propertyName - the name of the property to check
   * @return True if the object has a writable property by the name
   */
  public boolean hasSetter(String propertyName) {
    return setMethods.keySet().contains(propertyName);
  }

  /**
   * 检查是否类属性可以读取权限
   * Check to see if a class has a readable property by name.
   *
   * @param propertyName - the name of the property to check
   * @return True if the object has a readable property by the name
   */
  public boolean hasGetter(String propertyName) {
    return getMethods.keySet().contains(propertyName);
  }
  //根据属性名称找到对象属性
  public String findPropertyName(String name) {
    return caseInsensitivePropertyMap.get(name.toUpperCase(Locale.ENGLISH));
  }
}
```

【充电】

* 一个中间件框架肯定会用到反射，反射的通用性和可扩展性是一个框架好坏的衡量指标。

#### 5.13.1.4、MetaClass元类

```java
/**
 * 元类
 * @author Clinton Begin
 */
public class MetaClass {
  //反射工厂接口
  private final ReflectorFactory reflectorFactory;
  //反射类
  private final Reflector reflector;
  //根据类型和反射工厂初始化私有元类构造器
  private MetaClass(Class<?> type, ReflectorFactory reflectorFactory) {
    this.reflectorFactory = reflectorFactory;
    this.reflector = reflectorFactory.findForClass(type);
  }
 //根据类型和反射工厂获取元类
  public static MetaClass forClass(Class<?> type, ReflectorFactory reflectorFactory) {
    return new MetaClass(type, reflectorFactory);
  }
  //根据属性名称获取元类
  public MetaClass metaClassForProperty(String name) {
    Class<?> propType = reflector.getGetterType(name);
    return MetaClass.forClass(propType, reflectorFactory);
  }
  //根据名称找属性
  public String findProperty(String name) {
    StringBuilder prop = buildProperty(name, new StringBuilder());
    return prop.length() > 0 ? prop.toString() : null;
  }
  //根据名称和是否使用下划线来找属性
  public String findProperty(String name, boolean useCamelCaseMapping) {
    if (useCamelCaseMapping) {
      name = name.replace("_", "");
    }
    return findProperty(name);
  }
  //获取get属性集合
  public String[] getGetterNames() {
    return reflector.getGetablePropertyNames();
  }
  //获取set属性集合
  public String[] getSetterNames() {
    return reflector.getSetablePropertyNames();
  }
  //根据名字获取某个set类型
  public Class<?> getSetterType(String name) {
    PropertyTokenizer prop = new PropertyTokenizer(name);
    if (prop.hasNext()) {
      MetaClass metaProp = metaClassForProperty(prop.getName());
      return metaProp.getSetterType(prop.getChildren());
    } else {
      return reflector.getSetterType(prop.getName());
    }
  }
  //根据名字获取某个get类型
  public Class<?> getGetterType(String name) {
    //初始化属性标记器
    PropertyTokenizer prop = new PropertyTokenizer(name);
    if (prop.hasNext()) {
      MetaClass metaProp = metaClassForProperty(prop);
      return metaProp.getGetterType(prop.getChildren());
    }
    // issue #506. Resolve the type inside a Collection Object
    return getGetterType(prop);
  }
  //根据属性标记器获取元类
  private MetaClass metaClassForProperty(PropertyTokenizer prop) {
    Class<?> propType = getGetterType(prop);
    return MetaClass.forClass(propType, reflectorFactory);
  }
  //根据属性标记器获取get类型
  private Class<?> getGetterType(PropertyTokenizer prop) {
    Class<?> type = reflector.getGetterType(prop.getName());
    if (prop.getIndex() != null && Collection.class.isAssignableFrom(type)) {
      Type returnType = getGenericGetterType(prop.getName());
      if (returnType instanceof ParameterizedType) {
        Type[] actualTypeArguments = ((ParameterizedType) returnType).getActualTypeArguments();
        if (actualTypeArguments != null && actualTypeArguments.length == 1) {
          returnType = actualTypeArguments[0];
          if (returnType instanceof Class) {
            type = (Class<?>) returnType;
          } else if (returnType instanceof ParameterizedType) {
            type = (Class<?>) ((ParameterizedType) returnType).getRawType();
          }
        }
      }
    }
    return type;
  }
  //根据属性名称获取范型get类型
  private Type getGenericGetterType(String propertyName) {
    try {
      Invoker invoker = reflector.getGetInvoker(propertyName);
      if (invoker instanceof MethodInvoker) {
        Field _method = MethodInvoker.class.getDeclaredField("method");
        _method.setAccessible(true);
        Method method = (Method) _method.get(invoker);
        return TypeParameterResolver.resolveReturnType(method, reflector.getType());
      } else if (invoker instanceof GetFieldInvoker) {
        Field _field = GetFieldInvoker.class.getDeclaredField("field");
        _field.setAccessible(true);
        Field field = (Field) _field.get(invoker);
        return TypeParameterResolver.resolveFieldType(field, reflector.getType());
      }
    } catch (NoSuchFieldException | IllegalAccessException ignored) {
    }
    return null;
  }
   //判断是否属性有set方法
  public boolean hasSetter(String name) {
    PropertyTokenizer prop = new PropertyTokenizer(name);
    if (prop.hasNext()) {
      if (reflector.hasSetter(prop.getName())) {
        MetaClass metaProp = metaClassForProperty(prop.getName());
        return metaProp.hasSetter(prop.getChildren());
      } else {
        return false;
      }
    } else {
      return reflector.hasSetter(prop.getName());
    }
  }
  //判断属性是否有get方法
  public boolean hasGetter(String name) {
    PropertyTokenizer prop = new PropertyTokenizer(name);
    if (prop.hasNext()) {
      if (reflector.hasGetter(prop.getName())) {
        MetaClass metaProp = metaClassForProperty(prop);
        return metaProp.hasGetter(prop.getChildren());
      } else {
        return false;
      }
    } else {
      return reflector.hasGetter(prop.getName());
    }
  }
  //获取get的方法执行器
  public Invoker getGetInvoker(String name) {
    return reflector.getGetInvoker(name);
  }
  //获取set方法的执行器
  public Invoker getSetInvoker(String name) {
    return reflector.getSetInvoker(name);
  }
  //构建属性
  private StringBuilder buildProperty(String name, StringBuilder builder) {
    PropertyTokenizer prop = new PropertyTokenizer(name);
    if (prop.hasNext()) {
      String propertyName = reflector.findPropertyName(prop.getName());
      if (propertyName != null) {
        builder.append(propertyName);
        builder.append(".");
        MetaClass metaProp = metaClassForProperty(propertyName);
        metaProp.buildProperty(prop.getChildren(), builder);
      }
    } else {
      String propertyName = reflector.findPropertyName(name);
      if (propertyName != null) {
        builder.append(propertyName);
      }
    }
    return builder;
  }
  //是否有默认的构造器
  public boolean hasDefaultConstructor() {
    return reflector.hasDefaultConstructor();
  }

}
```

#### 5.13.1.5、MetaObject元对象

```java
/**
 * 元对象
 * @author Clinton Begin
 */
public class MetaObject {
  //源对象
  private final Object originalObject;
  //对象包装器
  private final ObjectWrapper objectWrapper;
  //对象工厂
  private final ObjectFactory objectFactory;
  //对象包装工厂
  private final ObjectWrapperFactory objectWrapperFactory;
  //反射工厂
  private final ReflectorFactory reflectorFactory;
  //根据入参的构造器
  private MetaObject(Object object, ObjectFactory objectFactory, ObjectWrapperFactory objectWrapperFactory, ReflectorFactory reflectorFactory) {
    //源对象
    this.originalObject = object;
    //对象工厂
    this.objectFactory = objectFactory;
    //对象包装工厂
    this.objectWrapperFactory = objectWrapperFactory;
    //反射工厂
    this.reflectorFactory = reflectorFactory;
    //包装器赋值 map,collection,bean,object
    if (object instanceof ObjectWrapper) {
      this.objectWrapper = (ObjectWrapper) object;
    } else if (objectWrapperFactory.hasWrapperFor(object)) {
      this.objectWrapper = objectWrapperFactory.getWrapperFor(this, object);
    } else if (object instanceof Map) {
      this.objectWrapper = new MapWrapper(this, (Map) object);
    } else if (object instanceof Collection) {
      this.objectWrapper = new CollectionWrapper(this, (Collection) object);
    } else {
      this.objectWrapper = new BeanWrapper(this, object);
    }
  }
  //获取元对象
  public static MetaObject forObject(Object object, ObjectFactory objectFactory, ObjectWrapperFactory objectWrapperFactory, ReflectorFactory reflectorFactory) {
    if (object == null) {
      return SystemMetaObject.NULL_META_OBJECT;
    } else {
      return new MetaObject(object, objectFactory, objectWrapperFactory, reflectorFactory);
    }
  }
  //获取对象工厂
  public ObjectFactory getObjectFactory() {
    return objectFactory;
  }
  //获取对象包装器工厂
  public ObjectWrapperFactory getObjectWrapperFactory() {
    return objectWrapperFactory;
  }
  //获取反射工厂
  public ReflectorFactory getReflectorFactory() {
    return reflectorFactory;
  }
  //获取源对象
  public Object getOriginalObject() {
    return originalObject;
  }
  //找到对象包装器的属性
  public String findProperty(String propName, boolean useCamelCaseMapping) {
    return objectWrapper.findProperty(propName, useCamelCaseMapping);
  }
  //获取get属性数组
  public String[] getGetterNames() {
    return objectWrapper.getGetterNames();
  }
  //获取set属性数组
  public String[] getSetterNames() {
    return objectWrapper.getSetterNames();
  }
  //获取set类型
  public Class<?> getSetterType(String name) {
    return objectWrapper.getSetterType(name);
  }
  //获取get类型
  public Class<?> getGetterType(String name) {
    return objectWrapper.getGetterType(name);
  }
  //是否有对应的set属性
  public boolean hasSetter(String name) {
    return objectWrapper.hasSetter(name);
  }
  //是否有对应的get属性
  public boolean hasGetter(String name) {
    return objectWrapper.hasGetter(name);
  }
  //根据属性获取值
  public Object getValue(String name) {
    PropertyTokenizer prop = new PropertyTokenizer(name);
    if (prop.hasNext()) {
      MetaObject metaValue = metaObjectForProperty(prop.getIndexedName());
      if (metaValue == SystemMetaObject.NULL_META_OBJECT) {
        return null;
      } else {
        return metaValue.getValue(prop.getChildren());
      }
    } else {
      return objectWrapper.get(prop);
    }
  }
  //给属性赋值
  public void setValue(String name, Object value) {
    PropertyTokenizer prop = new PropertyTokenizer(name);
    if (prop.hasNext()) {
      MetaObject metaValue = metaObjectForProperty(prop.getIndexedName());
      if (metaValue == SystemMetaObject.NULL_META_OBJECT) {
        if (value == null) {
          // don't instantiate child path if value is null
          return;
        } else {
          metaValue = objectWrapper.instantiatePropertyValue(name, prop, objectFactory);
        }
      }
      metaValue.setValue(prop.getChildren(), value);
    } else {
      objectWrapper.set(prop, value);
    }
  }
   //某个属性生成元对象
  public MetaObject metaObjectForProperty(String name) {
    Object value = getValue(name);
    return MetaObject.forObject(value, objectFactory, objectWrapperFactory, reflectorFactory);
  }
  //获取对象包装器
  public ObjectWrapper getObjectWrapper() {
    return objectWrapper;
  }
  //是否是集合
  public boolean isCollection() {
    return objectWrapper.isCollection();
  }
  //添加对象元素
  public void add(Object element) {
    objectWrapper.add(element);
  }
  //添加集合
  public <E> void addAll(List<E> list) {
    objectWrapper.addAll(list);
  }

}
```

#### 5.13.1.6、SystemMetaObject系统元对象

```java
/**
 * 系统元对象
 * @author Clinton Begin
 */
public final class SystemMetaObject {
   //默认的对象工厂
  public static final ObjectFactory DEFAULT_OBJECT_FACTORY = new DefaultObjectFactory();
  //默认的对象包装器工厂
  public static final ObjectWrapperFactory DEFAULT_OBJECT_WRAPPER_FACTORY = new DefaultObjectWrapperFactory();
  //空元对象
  public static final MetaObject NULL_META_OBJECT = MetaObject.forObject(NullObject.class, DEFAULT_OBJECT_FACTORY, DEFAULT_OBJECT_WRAPPER_FACTORY, new DefaultReflectorFactory());
  //防止实例化
  private SystemMetaObject() {
    // Prevent Instantiation of Static Class
  }
  //静态内部类空对象
  private static class NullObject {
  }
  //根据对象获取元对象
  public static MetaObject forObject(Object object) {
    return MetaObject.forObject(object, DEFAULT_OBJECT_FACTORY, DEFAULT_OBJECT_WRAPPER_FACTORY, new DefaultReflectorFactory());
  }

}

```

#### 5.13.1.7、ParamNameResolver参数名字解析器

```java
/**
 * 参数名字解析器
 */
public class ParamNameResolver {

  public static final String GENERIC_NAME_PREFIX = "param";

  /**
   * key是索引，value值是参数名字。名字是从{@link Param}获取的，如果指定的话，当参数没有指定的话，这个参数索引被使用。
   * 注意这个索引不同于实际索引，当这个方法有特殊参数的时候，例如(i.e. {@link RowBounds} or {@link ResultHandler}).
   * 
   * <p>
   * The key is the index and the value is the name of the parameter.<br />
   * The name is obtained from {@link Param} if specified. When {@link Param} is not specified,
   * the parameter index is used. Note that this index could be different from the actual index
   * when the method has special parameters (i.e. {@link RowBounds} or {@link ResultHandler}).
   * </p>
   * <ul>
   * <li>aMethod(@Param("M") int a, @Param("N") int b) -&gt; {{0, "M"}, {1, "N"}}</li>
   * <li>aMethod(int a, int b) -&gt; {{0, "0"}, {1, "1"}}</li>
   * <li>aMethod(int a, RowBounds rb, int b) -&gt; {{0, "0"}, {2, "1"}}</li>
   * </ul>
   */
  private final SortedMap<Integer, String> names;
  //有参数注解
  private boolean hasParamAnnotation;
  //参数名字解析，根据入参配置整个config对象和method方法
  public ParamNameResolver(Configuration config, Method method) {
    //获取参数类型
    final Class<?>[] paramTypes = method.getParameterTypes();
    //获取参数注解
    final Annotation[][] paramAnnotations = method.getParameterAnnotations();
    //treemap容器
    final SortedMap<Integer, String> map = new TreeMap<>();
     //参数注解长度
    int paramCount = paramAnnotations.length;
    // get names from @Param annotations
    //从@Param获取名字
    for (int paramIndex = 0; paramIndex < paramCount; paramIndex++) {
      if (isSpecialParameter(paramTypes[paramIndex])) {
        //跳过特殊参数
        // skip special parameters
        continue;
      }
      String name = null;
      //处理是param注解的参数
      for (Annotation annotation : paramAnnotations[paramIndex]) {
        if (annotation instanceof Param) {
          hasParamAnnotation = true;
          name = ((Param) annotation).value();
          break;
        }
      }
      //如果注解对应的参数没指定
      if (name == null) {
        // @Param was not specified.
        if (config.isUseActualParamName()) {
          name = getActualParamName(method, paramIndex);
        }
        //使用参数索引作为名字
        if (name == null) {
          // use the parameter index as the name ("0", "1", ...)
          // gcode issue #71
          name = String.valueOf(map.size());
        }
      }
      map.put(paramIndex, name);
    }
    names = Collections.unmodifiableSortedMap(map);
  }
  //获取实际参数名字
  private String getActualParamName(Method method, int paramIndex) {
    return ParamNameUtil.getParamNames(method).get(paramIndex);
  }
  //是特殊的参数
  private static boolean isSpecialParameter(Class<?> clazz) {
    return RowBounds.class.isAssignableFrom(clazz) || ResultHandler.class.isAssignableFrom(clazz);
  }

  /**
   * 根据SQL服务引用返回参数名字
   * Returns parameter names referenced by SQL providers.
   */
  public String[] getNames() {
    return names.values().toArray(new String[0]);
  }

  /**
   * 单个非特殊的参数没有名字返回。多个参数被使用命名规则命名。另外对于默认的名字，方法添加通用名字(param1, param2,
   * <p>
   * A single non-special parameter is returned without a name.
   * Multiple parameters are named using the naming rule.
   * In addition to the default names, this method also adds the generic names (param1, param2,
   * ...).
   * </p>
   */
  public Object getNamedParams(Object[] args) {
    //获取对象
    final int paramCount = names.size();
    if (args == null || paramCount == 0) {
      return null;
    } else if (!hasParamAnnotation && paramCount == 1) {
      return args[names.firstKey()];
    } else {
      final Map<String, Object> param = new ParamMap<>();
      int i = 0;
      for (Map.Entry<Integer, String> entry : names.entrySet()) {
        param.put(entry.getValue(), args[entry.getKey()]);
        //添加通用的参数名字
        // add generic param names (param1, param2, ...)
        final String genericParamName = GENERIC_NAME_PREFIX + (i + 1);
        // ensure not to overwrite parameter named with @Param
        // 确保带有@Param注解参数的不被重写
        if (!names.containsValue(genericParamName)) {
          param.put(genericParamName, args[entry.getKey()]);
        }
        i++;
      }
      return param;
    }
  }
}
```

#### 5.13.1.8、TypeParameteResolver

```java
/**
 * 类型参数解析器
 * @author Iwao AVE!
 */
public class TypeParameterResolver {

  /**
   * 属性type，在声明中如果有类型参数，他们将被解析称实际运行的类型。
   * @return The field type as {@link Type}. If it has type parameters in the declaration,<br>
   *         they will be resolved to the actual runtime {@link Type}s.
   */
  public static Type resolveFieldType(Field field, Type srcType) {
    Type fieldType = field.getGenericType();
    Class<?> declaringClass = field.getDeclaringClass();
    return resolveType(fieldType, srcType, declaringClass);
  }

  /**
   * 方法的返回类型，在声明中如果有类型参数，他们将被解析给实际运行类型
   * @return The return type of the method as {@link Type}. If it has type parameters in the declaration,<br>
   *         they will be resolved to the actual runtime {@link Type}s.
   */
  public static Type resolveReturnType(Method method, Type srcType) {
    Type returnType = method.getGenericReturnType();
    Class<?> declaringClass = method.getDeclaringClass();
    return resolveType(returnType, srcType, declaringClass);
  }

  /**
   * 方法的参数类型比如数组type，在声明中如果有类型参数，他们将被解析给实际运行类型
   * @return The parameter types of the method as an array of {@link Type}s. If they have type parameters in the declaration,<br>
   *         they will be resolved to the actual runtime {@link Type}s.
   */
  public static Type[] resolveParamTypes(Method method, Type srcType) {
    Type[] paramTypes = method.getGenericParameterTypes();
    Class<?> declaringClass = method.getDeclaringClass();
    Type[] result = new Type[paramTypes.length];
    for (int i = 0; i < paramTypes.length; i++) {
      result[i] = resolveType(paramTypes[i], srcType, declaringClass);
    }
    return result;
  }
  //解析类型
  private static Type resolveType(Type type, Type srcType, Class<?> declaringClass) {
    //类型变量
    if (type instanceof TypeVariable) {
      return resolveTypeVar((TypeVariable<?>) type, srcType, declaringClass);
   //参数化类型
    } else if (type instanceof ParameterizedType) {
      return resolveParameterizedType((ParameterizedType) type, srcType, declaringClass);
   //通用数组类型
    } else if (type instanceof GenericArrayType) {
      return resolveGenericArrayType((GenericArrayType) type, srcType, declaringClass);
    } else {
      return type;
    }
  }
  //解析通用数组类型
  private static Type resolveGenericArrayType(GenericArrayType genericArrayType, Type srcType, Class<?> declaringClass) {
    Type componentType = genericArrayType.getGenericComponentType();
    Type resolvedComponentType = null;
    //类型变量
    if (componentType instanceof TypeVariable) {
      resolvedComponentType = resolveTypeVar((TypeVariable<?>) componentType, srcType, declaringClass);
   //通用数组类型
    } else if (componentType instanceof GenericArrayType) {
      resolvedComponentType = resolveGenericArrayType((GenericArrayType) componentType, srcType, declaringClass);
    //参数化类型
    } else if (componentType instanceof ParameterizedType) {
      resolvedComponentType = resolveParameterizedType((ParameterizedType) componentType, srcType, declaringClass);
    }//属于类
    if (resolvedComponentType instanceof Class) {
      return Array.newInstance((Class<?>) resolvedComponentType, 0).getClass();
    } else {
      //通用数组类型
      return new GenericArrayTypeImpl(resolvedComponentType);
    }
  }
  //解析参数化类型
  private static ParameterizedType resolveParameterizedType(ParameterizedType parameterizedType, Type srcType, Class<?> declaringClass) {
    Class<?> rawType = (Class<?>) parameterizedType.getRawType();
    Type[] typeArgs = parameterizedType.getActualTypeArguments();
    Type[] args = new Type[typeArgs.length];
    for (int i = 0; i < typeArgs.length; i++) {
      if (typeArgs[i] instanceof TypeVariable) {
        args[i] = resolveTypeVar((TypeVariable<?>) typeArgs[i], srcType, declaringClass);
      } else if (typeArgs[i] instanceof ParameterizedType) {
        args[i] = resolveParameterizedType((ParameterizedType) typeArgs[i], srcType, declaringClass);
      } else if (typeArgs[i] instanceof WildcardType) {
        args[i] = resolveWildcardType((WildcardType) typeArgs[i], srcType, declaringClass);
      } else {
        args[i] = typeArgs[i];
      }
    }
    return new ParameterizedTypeImpl(rawType, null, args);
  }
  //解析通配符类型
  private static Type resolveWildcardType(WildcardType wildcardType, Type srcType, Class<?> declaringClass) {
    Type[] lowerBounds = resolveWildcardTypeBounds(wildcardType.getLowerBounds(), srcType, declaringClass);
    Type[] upperBounds = resolveWildcardTypeBounds(wildcardType.getUpperBounds(), srcType, declaringClass);
    return new WildcardTypeImpl(lowerBounds, upperBounds);
  }
  //解析通配型类型
  private static Type[] resolveWildcardTypeBounds(Type[] bounds, Type srcType, Class<?> declaringClass) {
    Type[] result = new Type[bounds.length];
    for (int i = 0; i < bounds.length; i++) {
      if (bounds[i] instanceof TypeVariable) {
        result[i] = resolveTypeVar((TypeVariable<?>) bounds[i], srcType, declaringClass);
      } else if (bounds[i] instanceof ParameterizedType) {
        result[i] = resolveParameterizedType((ParameterizedType) bounds[i], srcType, declaringClass);
      } else if (bounds[i] instanceof WildcardType) {
        result[i] = resolveWildcardType((WildcardType) bounds[i], srcType, declaringClass);
      } else {
        result[i] = bounds[i];
      }
    }
    return result;
  }
  //解析类型变量
  private static Type resolveTypeVar(TypeVariable<?> typeVar, Type srcType, Class<?> declaringClass) {
    Type result;
    Class<?> clazz;
    if (srcType instanceof Class) {
      clazz = (Class<?>) srcType;
    } else if (srcType instanceof ParameterizedType) {
      ParameterizedType parameterizedType = (ParameterizedType) srcType;
      clazz = (Class<?>) parameterizedType.getRawType();
    } else {
      throw new IllegalArgumentException("The 2nd arg must be Class or ParameterizedType, but was: " + srcType.getClass());
    }

    if (clazz == declaringClass) {
      Type[] bounds = typeVar.getBounds();
      if (bounds.length > 0) {
        return bounds[0];
      }
      return Object.class;
    }

    Type superclass = clazz.getGenericSuperclass();
    result = scanSuperTypes(typeVar, srcType, declaringClass, clazz, superclass);
    if (result != null) {
      return result;
    }

    Type[] superInterfaces = clazz.getGenericInterfaces();
    for (Type superInterface : superInterfaces) {
      result = scanSuperTypes(typeVar, srcType, declaringClass, clazz, superInterface);
      if (result != null) {
        return result;
      }
    }
    return Object.class;
  }
 //扫描父级类型
  private static Type scanSuperTypes(TypeVariable<?> typeVar, Type srcType, Class<?> declaringClass, Class<?> clazz, Type superclass) {
    if (superclass instanceof ParameterizedType) {
      ParameterizedType parentAsType = (ParameterizedType) superclass;
      Class<?> parentAsClass = (Class<?>) parentAsType.getRawType();
      TypeVariable<?>[] parentTypeVars = parentAsClass.getTypeParameters();
      if (srcType instanceof ParameterizedType) {
        parentAsType = translateParentTypeVars((ParameterizedType) srcType, clazz, parentAsType);
      }
      if (declaringClass == parentAsClass) {
        for (int i = 0; i < parentTypeVars.length; i++) {
          if (typeVar.equals(parentTypeVars[i])) {
            return parentAsType.getActualTypeArguments()[i];
          }
        }
      }
      if (declaringClass.isAssignableFrom(parentAsClass)) {
        return resolveTypeVar(typeVar, parentAsType, declaringClass);
      }
    } else if (superclass instanceof Class && declaringClass.isAssignableFrom((Class<?>) superclass)) {
      return resolveTypeVar(typeVar, superclass, declaringClass);
    }
    return null;
  }
  //翻译父级类型变量
  private static ParameterizedType translateParentTypeVars(ParameterizedType srcType, Class<?> srcClass, ParameterizedType parentType) {
    Type[] parentTypeArgs = parentType.getActualTypeArguments();
    Type[] srcTypeArgs = srcType.getActualTypeArguments();
    TypeVariable<?>[] srcTypeVars = srcClass.getTypeParameters();
    Type[] newParentArgs = new Type[parentTypeArgs.length];
    boolean noChange = true;
    for (int i = 0; i < parentTypeArgs.length; i++) {
      if (parentTypeArgs[i] instanceof TypeVariable) {
        for (int j = 0; j < srcTypeVars.length; j++) {
          if (srcTypeVars[j].equals(parentTypeArgs[i])) {
            noChange = false;
            newParentArgs[i] = srcTypeArgs[j];
          }
        }
      } else {
        newParentArgs[i] = parentTypeArgs[i];
      }
    }
    return noChange ? parentType : new ParameterizedTypeImpl((Class<?>)parentType.getRawType(), null, newParentArgs);
  }
  //类型参数解析
  private TypeParameterResolver() {
    super();
  }
  //参数化类型实现类
  static class ParameterizedTypeImpl implements ParameterizedType {
    //原生类型
    private Class<?> rawType;
    //拥有者类型
    private Type ownerType;
    //实际类型参数集合
    private Type[] actualTypeArguments;
    //构造器
    public ParameterizedTypeImpl(Class<?> rawType, Type ownerType, Type[] actualTypeArguments) {
      super();
      this.rawType = rawType;
      this.ownerType = ownerType;
      this.actualTypeArguments = actualTypeArguments;
    }
    //获取实际类型参数
    @Override
    public Type[] getActualTypeArguments() {
      return actualTypeArguments;
    }

    //获取拥有者类型
    @Override
    public Type getOwnerType() {
      return ownerType;
    }

   //获取原生类型
    @Override
    public Type getRawType() {
      return rawType;
    }

    @Override
    public String toString() {
      return "ParameterizedTypeImpl [rawType=" + rawType + ", ownerType=" + ownerType + ", actualTypeArguments=" + Arrays.toString(actualTypeArguments) + "]";
    }
  }
  //通配符类型实现类
  static class WildcardTypeImpl implements WildcardType {
    //下限
    private Type[] lowerBounds;
    //上限
    private Type[] upperBounds;

    WildcardTypeImpl(Type[] lowerBounds, Type[] upperBounds) {
      super();
      this.lowerBounds = lowerBounds;
      this.upperBounds = upperBounds;
    }
    //获取下限类型
    @Override
    public Type[] getLowerBounds() {
      return lowerBounds;
    }

    //获取上限类型
    @Override
    public Type[] getUpperBounds() {
      return upperBounds;
    }
  }

  //通用数组类型实现类
  static class GenericArrayTypeImpl implements GenericArrayType {
    private Type genericComponentType;

    GenericArrayTypeImpl(Type genericComponentType) {
      super();
      this.genericComponentType = genericComponentType;
    }

    //获取通用的组件类型
    @Override
    public Type getGenericComponentType() {
      return genericComponentType;
    }
  }
}
```

#### 5.13.1.9、Jdk,Java类校验

```java
/**
 * 检查依赖的类版本是否存在
 * To check the existence of version dependent classes.
 */
public class Jdk {

  /**
   * 3。5。0版本以后移除这个属性
   * <code>true</code> if <code>java.lang.reflect.Parameter</code> is available.
   * @deprecated Since 3.5.0, Will remove this field at feature(next major version up)
   */
  @Deprecated
  public static final boolean parameterExists;
  //静态代码块 判断是否有反射类参数
  static {
    boolean available = false;
    try {
      Resources.classForName("java.lang.reflect.Parameter");
      available = true;
    } catch (ClassNotFoundException e) {
      // ignore
    }
    parameterExists = available;
  }

  /**
   * 3。5。0版本以后移除
   * @deprecated Since 3.5.0, Will remove this field at feature(next major version up)
   */
  @Deprecated
  public static final boolean dateAndTimeApiExists;
  //静态代码块判断是否存在date时间api
  static {
    boolean available = false;
    try {
      Resources.classForName("java.time.Clock");
      available = true;
    } catch (ClassNotFoundException e) {
      // ignore
    }
    dateAndTimeApiExists = available;
  }

  /**
   * 3.5.0以后移除
   * @deprecated Since 3.5.0, Will remove this field at feature(next major version up)
   */
  @Deprecated
  public static final boolean optionalExists;
  //判断是否有Optional
  static {
    boolean available = false;
    try {
      Resources.classForName("java.util.Optional");
      available = true;
    } catch (ClassNotFoundException e) {
      // ignore
    }
    optionalExists = available;
  }

  private Jdk() {
    super();
  }
}

```

#### 5.13.1.10、ArrayUtil数组工具类

```java
/**
 * 提供hashcode，equals和同String方法处理数组
 * Provides hashCode, equals and toString methods that can handle array.
 */
public class ArrayUtil {

  /**
   * 返回一个对象的hashcode值
   * Returns a hash code for {@code obj}.
   *
   * @param obj
   *          The object to get a hash code for. May be an array or <code>null</code>.
   * @return A hash code of {@code obj} or 0 if {@code obj} is <code>null</code>
   */
  public static int hashCode(Object obj) {
    if (obj == null) {
      // for consistency with Arrays#hashCode() and Objects#hashCode()
      return 0;
    }
    final Class<?> clazz = obj.getClass();
    if (!clazz.isArray()) {
      return obj.hashCode();
    }
    final Class<?> componentType = clazz.getComponentType();
    if (long.class.equals(componentType)) {
      return Arrays.hashCode((long[]) obj);
    } else if (int.class.equals(componentType)) {
      return Arrays.hashCode((int[]) obj);
    } else if (short.class.equals(componentType)) {
      return Arrays.hashCode((short[]) obj);
    } else if (char.class.equals(componentType)) {
      return Arrays.hashCode((char[]) obj);
    } else if (byte.class.equals(componentType)) {
      return Arrays.hashCode((byte[]) obj);
    } else if (boolean.class.equals(componentType)) {
      return Arrays.hashCode((boolean[]) obj);
    } else if (float.class.equals(componentType)) {
      return Arrays.hashCode((float[]) obj);
    } else if (double.class.equals(componentType)) {
      return Arrays.hashCode((double[]) obj);
    } else {
      return Arrays.hashCode((Object[]) obj);
    }
  }

  /**
   * 比较2个对象，返回boolean值
   * Compares two objects. Returns <code>true</code> if
   * <ul>
   * <li>{@code thisObj} and {@code thatObj} are both <code>null</code></li>
   * <li>{@code thisObj} and {@code thatObj} are instances of the same type and
   * {@link Object#equals(Object)} returns <code>true</code></li>
   * <li>{@code thisObj} and {@code thatObj} are arrays with the same component type and
   * equals() method of {@link Arrays} returns <code>true</code> (not deepEquals())</li>
   * </ul>
   *
   * @param thisObj
   *          The left hand object to compare. May be an array or <code>null</code>
   * @param thatObj
   *          The right hand object to compare. May be an array or <code>null</code>
   * @return <code>true</code> if two objects are equal; <code>false</code> otherwise.
   */
  public static boolean equals(Object thisObj, Object thatObj) {
    if (thisObj == null) {
      return thatObj == null;
    } else if (thatObj == null) {
      return false;
    }
    final Class<?> clazz = thisObj.getClass();
    if (!clazz.equals(thatObj.getClass())) {
      return false;
    }
    if (!clazz.isArray()) {
      return thisObj.equals(thatObj);
    }
    final Class<?> componentType = clazz.getComponentType();
    if (long.class.equals(componentType)) {
      return Arrays.equals((long[]) thisObj, (long[]) thatObj);
    } else if (int.class.equals(componentType)) {
      return Arrays.equals((int[]) thisObj, (int[]) thatObj);
    } else if (short.class.equals(componentType)) {
      return Arrays.equals((short[]) thisObj, (short[]) thatObj);
    } else if (char.class.equals(componentType)) {
      return Arrays.equals((char[]) thisObj, (char[]) thatObj);
    } else if (byte.class.equals(componentType)) {
      return Arrays.equals((byte[]) thisObj, (byte[]) thatObj);
    } else if (boolean.class.equals(componentType)) {
      return Arrays.equals((boolean[]) thisObj, (boolean[]) thatObj);
    } else if (float.class.equals(componentType)) {
      return Arrays.equals((float[]) thisObj, (float[]) thatObj);
    } else if (double.class.equals(componentType)) {
      return Arrays.equals((double[]) thisObj, (double[]) thatObj);
    } else {
      return Arrays.equals((Object[]) thisObj, (Object[]) thatObj);
    }
  }

  /**
   * 返回toString方法
   * If the {@code obj} is an array, toString() method of {@link Arrays} is called. Otherwise
   * {@link Object#toString()} is called. Returns "null" if {@code obj} is <code>null</code>.
   *
   * @param obj
   *          An object. May be an array or <code>null</code>.
   * @return String representation of the {@code obj}.
   */
  public static String toString(Object obj) {
    if (obj == null) {
      return "null";
    }
    final Class<?> clazz = obj.getClass();
    if (!clazz.isArray()) {
      return obj.toString();
    }
    final Class<?> componentType = obj.getClass().getComponentType();
    if (long.class.equals(componentType)) {
      return Arrays.toString((long[]) obj);
    } else if (int.class.equals(componentType)) {
      return Arrays.toString((int[]) obj);
    } else if (short.class.equals(componentType)) {
      return Arrays.toString((short[]) obj);
    } else if (char.class.equals(componentType)) {
      return Arrays.toString((char[]) obj);
    } else if (byte.class.equals(componentType)) {
      return Arrays.toString((byte[]) obj);
    } else if (boolean.class.equals(componentType)) {
      return Arrays.toString((boolean[]) obj);
    } else if (float.class.equals(componentType)) {
      return Arrays.toString((float[]) obj);
    } else if (double.class.equals(componentType)) {
      return Arrays.toString((double[]) obj);
    } else {
      return Arrays.toString((Object[]) obj);
    }
  }

}
```

#### 5.13.1.11、异常工具类

```java
/**
 * 异常工具类
 * @author Clinton Begin
 */
public class ExceptionUtil {
  //防止实例化
  private ExceptionUtil() {
    // Prevent Instantiation
  }
  //未包装的异常抛出
  public static Throwable unwrapThrowable(Throwable wrapped) {
    Throwable unwrapped = wrapped;
    while (true) {
      if (unwrapped instanceof InvocationTargetException) {
        unwrapped = ((InvocationTargetException) unwrapped).getTargetException();
      } else if (unwrapped instanceof UndeclaredThrowableException) {
        unwrapped = ((UndeclaredThrowableException) unwrapped).getUndeclaredThrowable();
      } else {
        return unwrapped;
      }
    }
  }

}
```

#### 5.13.1.12、ParamNameUtil参数名字工具类

```java
//参数名字工具类
public class ParamNameUtil {
  //根据方法获取参数名字
  public static List<String> getParamNames(Method method) {
    return getParameterNames(method);
  }
  //根据构造器获取参数名字
  public static List<String> getParamNames(Constructor<?> constructor) {
    return getParameterNames(constructor);
  }
  //根据执行任务类获取参数名字
  private static List<String> getParameterNames(Executable executable) {
    return Arrays.stream(executable.getParameters()).map(Parameter::getName).collect(Collectors.toList());
  }

  private ParamNameUtil() {
    super();
  }
}
```

#### 5.13.1.13、RelflectionException反射异常

```java
/**
 * 反射异常与之前的一样，封装了底层的runtime异常
 * @author Clinton Begin
 */
public class ReflectionException extends PersistenceException {

  private static final long serialVersionUID = 7642570221267566591L;
  //空构造
  public ReflectionException() {
    super();
  }
  //传递信息构造
  public ReflectionException(String message) {
    super(message);
  }
  //传递信息和throwable构造
  public ReflectionException(String message, Throwable cause) {
    super(message, cause);
  }
  //传递throwable构造
  public ReflectionException(Throwable cause) {
    super(cause);
  }

}
```

### 5.13.2、工厂包

#### 5.13.2.1、ObjectFactory

```java
/**
 * MyBatis使用对象工厂创建所有需要的新对象
 * MyBatis uses an ObjectFactory to create all needed new Objects.
 *
 * @author Clinton Begin
 */
public interface ObjectFactory {

  /**
   * 设置配置属性
   * Sets configuration properties.
   * @param properties configuration properties
   */
  default void setProperties(Properties properties) {
    // NOP
  }

  /**
   * 创建带有默认构造函数的新对象
   * Creates a new object with default constructor.
   * @param type Object type
   * @return
   */
  <T> T create(Class<T> type);

  /**
   * 创建带有指定参数构造器的对象
   * Creates a new object with the specified constructor and params.
   * @param type Object type
   * @param constructorArgTypes Constructor argument types
   * @param constructorArgs Constructor argument values
   * @return
   */
  <T> T create(Class<T> type, List<Class<?>> constructorArgTypes, List<Object> constructorArgs);

  /**
   * 如果这个对象有一系列其他对象，返回true，目的是支持非集合包下的对象 比如Scala集合
   * Returns true if this object can have a set of other objects.
   * It's main purpose is to support non-java.util.Collection objects like Scala collections.
   *
   * @param type Object type
   * @return whether it is a collection or not
   * @since 3.1.0
   */
  <T> boolean isCollection(Class<T> type);

}
```

#### 5.13.2.2、DefaultObjectFactory

```java
/**
 * 默认的对象工厂
 * @author Clinton Begin
 */
public class DefaultObjectFactory implements ObjectFactory, Serializable {
  //序列化版本号
  private static final long serialVersionUID = -8855120656740914948L;
  //根据类型创建
  @Override
  public <T> T create(Class<T> type) {
    return create(type, null, null);
  }
  //根据类型和构造器类型和构造参数创建
  @SuppressWarnings("unchecked")
  @Override
  public <T> T create(Class<T> type, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
    Class<?> classToCreate = resolveInterface(type);
    // we know types are assignable
    return (T) instantiateClass(classToCreate, constructorArgTypes, constructorArgs);
  }
  //具体的类的构造过程
  private  <T> T instantiateClass(Class<T> type, List<Class<?>> constructorArgTypes, List<Object> constructorArgs) {
    try {
      Constructor<T> constructor;
      if (constructorArgTypes == null || constructorArgs == null) {
        constructor = type.getDeclaredConstructor();
        try {
          return constructor.newInstance();
        } catch (IllegalAccessException e) {
          if (Reflector.canControlMemberAccessible()) {
            constructor.setAccessible(true);
            return constructor.newInstance();
          } else {
            throw e;
          }
        }
      }
      constructor = type.getDeclaredConstructor(constructorArgTypes.toArray(new Class[0]));
      try {
        return constructor.newInstance(constructorArgs.toArray(new Object[0]));
      } catch (IllegalAccessException e) {
        if (Reflector.canControlMemberAccessible()) {
          constructor.setAccessible(true);
          return constructor.newInstance(constructorArgs.toArray(new Object[0]));
        } else {
          throw e;
        }
      }
    } catch (Exception e) {
      String argTypes = Optional.ofNullable(constructorArgTypes).orElseGet(Collections::emptyList)
          .stream().map(Class::getSimpleName).collect(Collectors.joining(","));
      String argValues = Optional.ofNullable(constructorArgs).orElseGet(Collections::emptyList)
          .stream().map(String::valueOf).collect(Collectors.joining(","));
      throw new ReflectionException("Error instantiating " + type + " with invalid types (" + argTypes + ") or values (" + argValues + "). Cause: " + e, e);
    }
  }
  //根据类型解析接口
  protected Class<?> resolveInterface(Class<?> type) {
    Class<?> classToCreate;
    if (type == List.class || type == Collection.class || type == Iterable.class) {
      classToCreate = ArrayList.class;
    } else if (type == Map.class) {
      classToCreate = HashMap.class;
    } else if (type == SortedSet.class) { // issue #510 Collections Support
      classToCreate = TreeSet.class;
    } else if (type == Set.class) {
      classToCreate = HashSet.class;
    } else {
      classToCreate = type;
    }
    return classToCreate;
  }
  //判断是否是集合
  @Override
  public <T> boolean isCollection(Class<T> type) {
    return Collection.class.isAssignableFrom(type);
  }

}
```

### 5.13.3、执行包

![image-20200310163811447](/Users/didi/Library/Application Support/typora-user-images/image-20200310163811447.png)

#### 5.13.3.1、执行器接口Invoker(可以和java反射做对比)

```java
/**
 * 执行器接口
 * @author Clinton Begin
 */
public interface Invoker {
  //反射执行某个目标类
  Object invoke(Object target, Object[] args) throws IllegalAccessException, InvocationTargetException;
  //获取类型
  Class<?> getType();
}
```

#### 5.13.3.2、方法执行器MethodInvoker

```java
/**
 *方法执行器
 * @author Clinton Begin
 */
public class MethodInvoker implements Invoker {
  //类型
  private final Class<?> type;
  //方法
  private final Method method;
  //方法执行器
  public MethodInvoker(Method method) {
    this.method = method;
    //根据方法获取类型，如果没有则以返回值类型作为类型
    if (method.getParameterTypes().length == 1) {
      type = method.getParameterTypes()[0];
    } else {
      type = method.getReturnType();
    }
  }
  //执行目标方法
  @Override
  public Object invoke(Object target, Object[] args) throws IllegalAccessException, InvocationTargetException {
    try {
      return method.invoke(target, args);
    } catch (IllegalAccessException e) {
      //如果能控制成员访问变量
      if (Reflector.canControlMemberAccessible()) {
        //方法先设置可读
        method.setAccessible(true);
        return method.invoke(target, args);
      } else {
        throw e;
      }
    }
  }
  //获取类型
  @Override
  public Class<?> getType() {
    return type;
  }
}
```

#### 5.13.3.3、语义不明的方法执行器AmbiguousMethodInvoker

```java
/**
 * 语义不明的方法执行器
 */
public class AmbiguousMethodInvoker extends MethodInvoker {
  //异常信息
  private final String exceptionMessage;
  //构造器，调用父类构造器，封装异常信息
  public AmbiguousMethodInvoker(Method method, String exceptionMessage) {
    super(method);
    this.exceptionMessage = exceptionMessage;
  }
  //执行目标的方法
  @Override
  public Object invoke(Object target, Object[] args) throws IllegalAccessException, InvocationTargetException {
    throw new ReflectionException(exceptionMessage);
  }
}
```

#### 5.13.3.4、获取属性执行器GetFieldInvoker

```java
/**
 * 获取属性执行器
 * @author Clinton Begin
 */
public class GetFieldInvoker implements Invoker {
  //属性
  private final Field field;
  //获取属性执行器构造器
  public GetFieldInvoker(Field field) {
    this.field = field;
  }
  //执行目标方法的实现类
  @Override
  public Object invoke(Object target, Object[] args) throws IllegalAccessException {
    try {
      return field.get(target);
    } catch (IllegalAccessException e) {
      if (Reflector.canControlMemberAccessible()) {
        field.setAccessible(true);
        return field.get(target);
      } else {
        throw e;
      }
    }
  }
  //获取类型
  @Override
  public Class<?> getType() {
    return field.getType();
  }
}

```

#### 5.13.3.5、set属性执行器SetFieldInvoker

```java
/**
 * 设置属性执行器
 * @author Clinton Begin
 */
public class SetFieldInvoker implements Invoker {
  //属性
  private final Field field;
  //设置属性构造器
  public SetFieldInvoker(Field field) {
    this.field = field;
  }
  //执行目标方法
  @Override
  public Object invoke(Object target, Object[] args) throws IllegalAccessException {
    try {
      field.set(target, args[0]);
    } catch (IllegalAccessException e) {
      if (Reflector.canControlMemberAccessible()) {
      //设置属性的可访问权限
        field.setAccessible(true);
        field.set(target, args[0]);
      } else {
        throw e;
      }
    }
    return null;
  }
 //获取类型
  @Override
  public Class<?> getType() {
    return field.getType();
  }
}
```

【思考】set和get属性和方法区分开的真实核心意义,需要对比Java反射内容

### 5.13.4、属性包

#### 5.13.4.1、PropertyCopier属性拷贝器

```java
/**
 * 属性拷贝者
 * @author Clinton Begin
 */
public final class PropertyCopier {
  //属性拷贝空构造器设置私有防止外部实例化
  private PropertyCopier() {
    // Prevent Instantiation of Static Class
  }
  //拷贝bean的属性，类型，原bean，目标bean
  public static void copyBeanProperties(Class<?> type, Object sourceBean, Object destinationBean) {
    Class<?> parent = type;
    while (parent != null) {
      //获取类型的声明属性
      final Field[] fields = parent.getDeclaredFields();
      //遍历属性
      for (Field field : fields) {
        try {
          try {
            //属性设置目标bean通过原bean
            field.set(destinationBean, field.get(sourceBean));
          } catch (IllegalAccessException e) {
            if (Reflector.canControlMemberAccessible()) {
              //如果非法访问则开启访问属性权限
              field.setAccessible(true);
              field.set(destinationBean, field.get(sourceBean));
            } else {
              throw e;
            }
          }
        } catch (Exception e) {
          // Nothing useful to do, will only fail on final fields, which will be ignored.
        }
      }
      //获取父级类，相当于是递归
      parent = parent.getSuperclass();
    }
  }

}
```

#### 5.13.4.2、PropertyNamer属性命名

```java
/**
 * 属性命名
 * @author Clinton Begin
 */
public final class PropertyNamer {
  //防止外部实例话
  private PropertyNamer() {
    // Prevent Instantiation of Static Class
  }
  //方法到属性
  public static String methodToProperty(String name) {
    //如果名字以is开头截取后面
    if (name.startsWith("is")) {
      name = name.substring(2);
      //如果以get或者set开头截取后面
    } else if (name.startsWith("get") || name.startsWith("set")) {
      name = name.substring(3);
    } else {
      throw new ReflectionException("Error parsing property name '" + name + "'.  Didn't start with 'is', 'get' or 'set'.");
    }
    //如果名字长度=1，或者名字长度大于1并且字符是小写，则改为驼峰形式
    if (name.length() == 1 || (name.length() > 1 && !Character.isUpperCase(name.charAt(1)))) {
      name = name.substring(0, 1).toLowerCase(Locale.ENGLISH) + name.substring(1);
    }

    return name;
  }
  //是否是属性
  public static boolean isProperty(String name) {
    return isGetter(name) || isSetter(name);
  }
  //是否是get方法包含get和is的方式
  public static boolean isGetter(String name) {
    return (name.startsWith("get") && name.length() > 3) || (name.startsWith("is") && name.length() > 2);
  }
  //是否是set方法
  public static boolean isSetter(String name) {
    return name.startsWith("set") && name.length() > 3;
  }

}
```

#### 5.13.4.3、PropertyTokenizer属性标记器

```java
/**
 * 属性标记器
 * @author Clinton Begin
 */
public class PropertyTokenizer implements Iterator<PropertyTokenizer> {
  //名称
  private String name;
  //索引名称
  private final String indexedName;
  //索引
  private String index;
  //子属性
  private final String children;
  //根据完整的全名称构造器
  public PropertyTokenizer(String fullname) {
    int delim = fullname.indexOf('.');
    if (delim > -1) {
      name = fullname.substring(0, delim);
      children = fullname.substring(delim + 1);
    } else {
      name = fullname;
      children = null;
    }
    indexedName = name;
    delim = name.indexOf('[');
    if (delim > -1) {
      index = name.substring(delim + 1, name.length() - 1);
      name = name.substring(0, delim);
    }
  }
  //获取名称
  public String getName() {
    return name;
  }
  //获取索引
  public String getIndex() {
    return index;
  }
  //获取索引名称
  public String getIndexedName() {
    return indexedName;
  }
  //获取子名称
  public String getChildren() {
    return children;
  }
  //是否还有下一个属性
  @Override
  public boolean hasNext() {
    return children != null;
  }
  //遍历下一个属性
  @Override
  public PropertyTokenizer next() {
    return new PropertyTokenizer(children);
  }
  //移除能力没有
  @Override
  public void remove() {
    throw new UnsupportedOperationException("Remove is not supported, as it has no meaning in the context of properties.");
  }
}
```

### 5.13.5、wrapper包装器

![image-20200310220151255](/Users/didi/Library/Application Support/typora-user-images/image-20200310220151255.png)

#### 5.13.5.1、ObjectWrapperFactory对象包装器工厂

```java
/**
 * 对象包装器工厂
 * @author Clinton Begin
 */
public interface ObjectWrapperFactory {
  //是否该对象已经包装过
  boolean hasWrapperFor(Object object);
  //获取对象的包装器
  ObjectWrapper getWrapperFor(MetaObject metaObject, Object object);

}

```

#### 5.13.5.2、DefaultObjectWrapperFactory默认对象包装器工厂

```java
/**
 * 默认的对象包装器工厂
 * @author Clinton Begin
 */
public class DefaultObjectWrapperFactory implements ObjectWrapperFactory {
  //该对象是否有包装，返回false
  @Override
  public boolean hasWrapperFor(Object object) {
    return false;
  }
  //对于对象包装器来说，默认的对象包装器工厂应该从不会调用，走定制化包装器工厂
  @Override
  public ObjectWrapper getWrapperFor(MetaObject metaObject, Object object) {
    throw new ReflectionException("The DefaultObjectWrapperFactory should never be called to provide an ObjectWrapper.");
  }

}
```

![image-20200310220020628](/Users/didi/Library/Application Support/typora-user-images/image-20200310220020628.png)

#### 5.13.5.3、ObjectWrapper对象包装器

```java
/**
 * 对象包装器
 * @author Clinton Begin
 */
public interface ObjectWrapper {
  //根据属性标记器获取对象
  Object get(PropertyTokenizer prop);
  //设置属性标记器的某个值
  void set(PropertyTokenizer prop, Object value);
  //根据名字和是否使用驼峰来找属性
  String findProperty(String name, boolean useCamelCaseMapping);
  //获取get属性数组
  String[] getGetterNames();
  //获取set属性数组
  String[] getSetterNames();
  //根据名称获取set类型
  Class<?> getSetterType(String name);
  //根据名称获取get类型
  Class<?> getGetterType(String name);
  //该名称是否有set方法
  boolean hasSetter(String name);
  //该名称是否有get方法
  boolean hasGetter(String name);
  //实例化元对象
  MetaObject instantiatePropertyValue(String name, PropertyTokenizer prop, ObjectFactory objectFactory);
  //是否是集合
  boolean isCollection();
  //添加对象
  void add(Object element);
  //添加集合对象
  <E> void addAll(List<E> element);

}
```

#### 5.13.5.4、CollectionWrapper集合包装器

```java
/**
 * 集合包装器
 * @author Clinton Begin
 */
public class CollectionWrapper implements ObjectWrapper {
  //集合对象
  private final Collection<Object> object;
  //集合包装器构造对象
  public CollectionWrapper(MetaObject metaObject, Collection<Object> object) {
    this.object = object;
  }
  //获取属性标记器的对象
  @Override
  public Object get(PropertyTokenizer prop) {
    throw new UnsupportedOperationException();
  }
  //设置属性标记器的值
  @Override
  public void set(PropertyTokenizer prop, Object value) {
    throw new UnsupportedOperationException();
  }
  //找某个属性
  @Override
  public String findProperty(String name, boolean useCamelCaseMapping) {
    throw new UnsupportedOperationException();
  }
  //获取get名称属性数组
  @Override
  public String[] getGetterNames() {
    throw new UnsupportedOperationException();
  }
  //获取set属性名称数组
  @Override
  public String[] getSetterNames() {
    throw new UnsupportedOperationException();
  }
  //根据名称获取set类型
  @Override
  public Class<?> getSetterType(String name) {
    throw new UnsupportedOperationException();
  }
  //根据名称获取get类型
  @Override
  public Class<?> getGetterType(String name) {
    throw new UnsupportedOperationException();
  }
  //是否该名称有set方法
  @Override
  public boolean hasSetter(String name) {
    throw new UnsupportedOperationException();
  }
  //是否该名称有get方法
  @Override
  public boolean hasGetter(String name) {
    throw new UnsupportedOperationException();
  }
  //实例话属性值，抛出不支持的异常
  @Override
  public MetaObject instantiatePropertyValue(String name, PropertyTokenizer prop, ObjectFactory objectFactory) {
    throw new UnsupportedOperationException();
  }
  //是否是集合
  @Override
  public boolean isCollection() {
    return true;
  }
  //添加元素
  @Override
  public void add(Object element) {
    object.add(element);
  }
  //添加集合
  @Override
  public <E> void addAll(List<E> element) {
    object.addAll(element);
  }

}
```

#### 5.13.5.5、BaseWrapper抽象基础包装器

```java
/**
 * 抽象类基础包装器
 * @author Clinton Begin
 */
public abstract class BaseWrapper implements ObjectWrapper {
  //无参数
  protected static final Object[] NO_ARGUMENTS = new Object[0];
  //元对象
  protected final MetaObject metaObject;
  //基础构造器
  protected BaseWrapper(MetaObject metaObject) {
    this.metaObject = metaObject;
  }
  //解析集合
  protected Object resolveCollection(PropertyTokenizer prop, Object object) {
    //如果属性标记器没有名称返回该对象
    if ("".equals(prop.getName())) {
      return object;
    } else {
      //否则返回该属性对应的值
      return metaObject.getValue(prop.getName());
    }
  }
  //获取集合值，根据属性标记器和集合，支持map,list,array
  protected Object getCollectionValue(PropertyTokenizer prop, Object collection) {
    if (collection instanceof Map) {
      return ((Map) collection).get(prop.getIndex());
    } else {
      int i = Integer.parseInt(prop.getIndex());
      if (collection instanceof List) {
        return ((List) collection).get(i);
      } else if (collection instanceof Object[]) {
        return ((Object[]) collection)[i];
      } else if (collection instanceof char[]) {
        return ((char[]) collection)[i];
      } else if (collection instanceof boolean[]) {
        return ((boolean[]) collection)[i];
      } else if (collection instanceof byte[]) {
        return ((byte[]) collection)[i];
      } else if (collection instanceof double[]) {
        return ((double[]) collection)[i];
      } else if (collection instanceof float[]) {
        return ((float[]) collection)[i];
      } else if (collection instanceof int[]) {
        return ((int[]) collection)[i];
      } else if (collection instanceof long[]) {
        return ((long[]) collection)[i];
      } else if (collection instanceof short[]) {
        return ((short[]) collection)[i];
      } else {
        throw new ReflectionException("The '" + prop.getName() + "' property of " + collection + " is not a List or Array.");
      }
    }
  }
  //设置集合值支持map,list,array
  protected void setCollectionValue(PropertyTokenizer prop, Object collection, Object value) {
    if (collection instanceof Map) {
      ((Map) collection).put(prop.getIndex(), value);
    } else {
      int i = Integer.parseInt(prop.getIndex());
      if (collection instanceof List) {
        ((List) collection).set(i, value);
      } else if (collection instanceof Object[]) {
        ((Object[]) collection)[i] = value;
      } else if (collection instanceof char[]) {
        ((char[]) collection)[i] = (Character) value;
      } else if (collection instanceof boolean[]) {
        ((boolean[]) collection)[i] = (Boolean) value;
      } else if (collection instanceof byte[]) {
        ((byte[]) collection)[i] = (Byte) value;
      } else if (collection instanceof double[]) {
        ((double[]) collection)[i] = (Double) value;
      } else if (collection instanceof float[]) {
        ((float[]) collection)[i] = (Float) value;
      } else if (collection instanceof int[]) {
        ((int[]) collection)[i] = (Integer) value;
      } else if (collection instanceof long[]) {
        ((long[]) collection)[i] = (Long) value;
      } else if (collection instanceof short[]) {
        ((short[]) collection)[i] = (Short) value;
      } else {
        throw new ReflectionException("The '" + prop.getName() + "' property of " + collection + " is not a List or Array.");
      }
    }
  }

}
```

#### 5.13.5.6、MapWrapper

```java
/**
 * Map的包装器
 * @author Clinton Begin
 */
public class MapWrapper extends BaseWrapper {
  //map
  private final Map<String, Object> map;
  //初始化map包装器
  public MapWrapper(MetaObject metaObject, Map<String, Object> map) {
    super(metaObject);
    this.map = map;
  }
  //获取属性标记器对应的值
  @Override
  public Object get(PropertyTokenizer prop) {
    if (prop.getIndex() != null) {
      Object collection = resolveCollection(prop, map);
      return getCollectionValue(prop, collection);
    } else {
      return map.get(prop.getName());
    }
  }
  //设置属性标记器对应的值
  @Override
  public void set(PropertyTokenizer prop, Object value) {
    if (prop.getIndex() != null) {
      Object collection = resolveCollection(prop, map);
      setCollectionValue(prop, collection, value);
    } else {
      map.put(prop.getName(), value);
    }
  }
  //找某个属性
  @Override
  public String findProperty(String name, boolean useCamelCaseMapping) {
    return name;
  }
  //获取get名称属性数组
  @Override
  public String[] getGetterNames() {
    return map.keySet().toArray(new String[map.keySet().size()]);
  }
  //获取set名称属性数组
  @Override
  public String[] getSetterNames() {
    return map.keySet().toArray(new String[map.keySet().size()]);
  }
  //根据名称获取set类型
  @Override
  public Class<?> getSetterType(String name) {
    PropertyTokenizer prop = new PropertyTokenizer(name);
    if (prop.hasNext()) {
      MetaObject metaValue = metaObject.metaObjectForProperty(prop.getIndexedName());
      if (metaValue == SystemMetaObject.NULL_META_OBJECT) {
        return Object.class;
      } else {
        return metaValue.getSetterType(prop.getChildren());
      }
    } else {
      if (map.get(name) != null) {
        return map.get(name).getClass();
      } else {
        return Object.class;
      }
    }
  }
  //根据名称获取get类型
  @Override
  public Class<?> getGetterType(String name) {
    PropertyTokenizer prop = new PropertyTokenizer(name);
    if (prop.hasNext()) {
      MetaObject metaValue = metaObject.metaObjectForProperty(prop.getIndexedName());
      if (metaValue == SystemMetaObject.NULL_META_OBJECT) {
        return Object.class;
      } else {
        return metaValue.getGetterType(prop.getChildren());
      }
    } else {
      if (map.get(name) != null) {
        return map.get(name).getClass();
      } else {
        return Object.class;
      }
    }
  }
  //该名称是否有set方法
  @Override
  public boolean hasSetter(String name) {
    return true;
  }
 //该名称是否有get方法
  @Override
  public boolean hasGetter(String name) {
    PropertyTokenizer prop = new PropertyTokenizer(name);
    if (prop.hasNext()) {
      if (map.containsKey(prop.getIndexedName())) {
        MetaObject metaValue = metaObject.metaObjectForProperty(prop.getIndexedName());
        if (metaValue == SystemMetaObject.NULL_META_OBJECT) {
          return true;
        } else {
          return metaValue.hasGetter(prop.getChildren());
        }
      } else {
        return false;
      }
    } else {
      return map.containsKey(prop.getName());
    }
  }
  //实例化属性值
  @Override
  public MetaObject instantiatePropertyValue(String name, PropertyTokenizer prop, ObjectFactory objectFactory) {
    HashMap<String, Object> map = new HashMap<>();
    set(prop, map);
    return MetaObject.forObject(map, metaObject.getObjectFactory(), metaObject.getObjectWrapperFactory(), metaObject.getReflectorFactory());
  }
  //是否是集合
  @Override
  public boolean isCollection() {
    return false;
  }
  //添加元素 不支持
  @Override
  public void add(Object element) {
    throw new UnsupportedOperationException();
  }
  //添加集合 不支持
  @Override
  public <E> void addAll(List<E> element) {
    throw new UnsupportedOperationException();
  }

}
```

#### 5.13.5.7、BeanWrapper

```java
/**
 * Bean包装器
 * @author Clinton Begin
 */
public class BeanWrapper extends BaseWrapper {
  //对象
  private final Object object;
  //元类
  private final MetaClass metaClass;
  //Bean包装器构造器
  public BeanWrapper(MetaObject metaObject, Object object) {
    super(metaObject);
    this.object = object;
    this.metaClass = MetaClass.forClass(object.getClass(), metaObject.getReflectorFactory());
  }
  //根据属性标记器获取对象
  @Override
  public Object get(PropertyTokenizer prop) {
    if (prop.getIndex() != null) {
      Object collection = resolveCollection(prop, object);
      return getCollectionValue(prop, collection);
    } else {
      return getBeanProperty(prop, object);
    }
  }
  //根据属性标记器赋值
  @Override
  public void set(PropertyTokenizer prop, Object value) {
    if (prop.getIndex() != null) {
      Object collection = resolveCollection(prop, object);
      setCollectionValue(prop, collection, value);
    } else {
      setBeanProperty(prop, object, value);
    }
  }
  //根据名称找属性
  @Override
  public String findProperty(String name, boolean useCamelCaseMapping) {
    return metaClass.findProperty(name, useCamelCaseMapping);
  }
  //获取get属性数组
  @Override
  public String[] getGetterNames() {
    return metaClass.getGetterNames();
  }
  //获取set属性数组
  @Override
  public String[] getSetterNames() {
    return metaClass.getSetterNames();
  }
  //根据名称获取set类型
  @Override
  public Class<?> getSetterType(String name) {
    PropertyTokenizer prop = new PropertyTokenizer(name);
    if (prop.hasNext()) {
      MetaObject metaValue = metaObject.metaObjectForProperty(prop.getIndexedName());
      if (metaValue == SystemMetaObject.NULL_META_OBJECT) {
        return metaClass.getSetterType(name);
      } else {
        return metaValue.getSetterType(prop.getChildren());
      }
    } else {
      return metaClass.getSetterType(name);
    }
  }
  //根据名称获取get类型
  @Override
  public Class<?> getGetterType(String name) {
    PropertyTokenizer prop = new PropertyTokenizer(name);
    if (prop.hasNext()) {
      MetaObject metaValue = metaObject.metaObjectForProperty(prop.getIndexedName());
      if (metaValue == SystemMetaObject.NULL_META_OBJECT) {
        return metaClass.getGetterType(name);
      } else {
        return metaValue.getGetterType(prop.getChildren());
      }
    } else {
      return metaClass.getGetterType(name);
    }
  }
  //是否该名称有set方法
  @Override
  public boolean hasSetter(String name) {
    PropertyTokenizer prop = new PropertyTokenizer(name);
    if (prop.hasNext()) {
      if (metaClass.hasSetter(prop.getIndexedName())) {
        MetaObject metaValue = metaObject.metaObjectForProperty(prop.getIndexedName());
        if (metaValue == SystemMetaObject.NULL_META_OBJECT) {
          return metaClass.hasSetter(name);
        } else {
          return metaValue.hasSetter(prop.getChildren());
        }
      } else {
        return false;
      }
    } else {
      return metaClass.hasSetter(name);
    }
  }
  //是否该名称有get方法
  @Override
  public boolean hasGetter(String name) {
    PropertyTokenizer prop = new PropertyTokenizer(name);
    if (prop.hasNext()) {
      if (metaClass.hasGetter(prop.getIndexedName())) {
        MetaObject metaValue = metaObject.metaObjectForProperty(prop.getIndexedName());
        if (metaValue == SystemMetaObject.NULL_META_OBJECT) {
          return metaClass.hasGetter(name);
        } else {
          return metaValue.hasGetter(prop.getChildren());
        }
      } else {
        return false;
      }
    } else {
      return metaClass.hasGetter(name);
    }
  }
 //实例化属性值
  @Override
  public MetaObject instantiatePropertyValue(String name, PropertyTokenizer prop, ObjectFactory objectFactory) {
    MetaObject metaValue;
    Class<?> type = getSetterType(prop.getName());
    try {
      Object newObject = objectFactory.create(type);
      metaValue = MetaObject.forObject(newObject, metaObject.getObjectFactory(), metaObject.getObjectWrapperFactory(), metaObject.getReflectorFactory());
      set(prop, newObject);
    } catch (Exception e) {
      throw new ReflectionException("Cannot set value of property '" + name + "' because '" + name + "' is null and cannot be instantiated on instance of " + type.getName() + ". Cause:" + e.toString(), e);
    }
    return metaValue;
  }
 //获取Bean属性
  private Object getBeanProperty(PropertyTokenizer prop, Object object) {
    try {
      Invoker method = metaClass.getGetInvoker(prop.getName());
      try {
        return method.invoke(object, NO_ARGUMENTS);
      } catch (Throwable t) {
        throw ExceptionUtil.unwrapThrowable(t);
      }
    } catch (RuntimeException e) {
      throw e;
    } catch (Throwable t) {
      throw new ReflectionException("Could not get property '" + prop.getName() + "' from " + object.getClass() + ".  Cause: " + t.toString(), t);
    }
  }
  //设置Bean属性
  private void setBeanProperty(PropertyTokenizer prop, Object object, Object value) {
    try {
      Invoker method = metaClass.getSetInvoker(prop.getName());
      Object[] params = {value};
      try {
        method.invoke(object, params);
      } catch (Throwable t) {
        throw ExceptionUtil.unwrapThrowable(t);
      }
    } catch (Throwable t) {
      throw new ReflectionException("Could not set property '" + prop.getName() + "' of '" + object.getClass() + "' with value '" + value + "' Cause: " + t.toString(), t);
    }
  }
 //是否是集合
  @Override
  public boolean isCollection() {
    return false;
  }
 //添加元素
  @Override
  public void add(Object element) {
    throw new UnsupportedOperationException();
  }
  //添加集合
  @Override
  public <E> void addAll(List<E> list) {
    throw new UnsupportedOperationException();
  }


}
```

### 5.13.6、小结

从reflection包下的内容来看，涉及到的知识点，Java类的反射，和Collection，Map和Bean类的操作属性和方法的赋值和取值。

#  第6章、手写ORM框架

### 创建一个maven项目

通过idea创建一个项目比如xcxyz-mybatis，由于基本操作非重点则跳过。

### 托管github版本管理

```java
本地初始化库-> git init
查看本地文件->git status
添加当前目录所有文件->git add .
提交当前文件->git commit -m "init xcxyz-mybatis"
生成gen密钥->ssh-keygen -t rsa -C “wolf_slave666@sina.com"
配置github库->登录Github,找到右上角的图标，打开点进里面的Settings，再选中里面的SSH and GPG KEYS，点击右上角的New SSH key，然后Title里面随便填，再把刚才id_rsa.pub里面的内容复制到Title下面的Key内容框里面，最后点击Add SSH key，这样就完成了SSH Key的加密
在Github上创建一个Git仓库->直接点New repository来创建
跟远程github仓库建立关系->git remote add origin https://github.com/xiaochengxinyizhan/xcxyz-mybatis.git
推送到远端github->git push -u origin master
```

demo：自测能力模型框架。

# 第7章、项目实战

要使用 MyBatis， 只需将 [mybatis-x.x.x.jar](https://github.com/mybatis/mybatis-3/releases) 文件置于 classpath 中即可。

如果使用 Maven 来构建项目，则需将下面的 dependency 代码置于 pom.xml 文件中：

```java
 <dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>x.x.x</version>
</dependency>

```

# 第8章、看源码写框架之小感

## 1）定位、场景和矛盾

写之前框架的定位是什么？它的主要场景是什么，主要解决的矛盾和问题是什么？（如何下手写一个框架）

比如ORM框架：<font color=red>定位和主要场景</font>

ORM，即Object-Relational Mapping（对象关系映射），它的作用是在关系型数据库和业务实体对象之间作一个映射，这样，我们在具体的操作业务对象的时候，就不需要再去和复杂的SQL语句打交道，只需简单的操作对象的属性和方法。

mybatis本身是java领域最常用的一个轻量半自动数据库orm持久层框架，从oobject-relational-mapping的角度来说，<font color=red>它主要是解决数据返回层的java类映射的问题。从开发模式上，mybatis是把开发人员的sql放到xml配置文件中。</font>

## 2）基础知识

### 现有流程

操作JDBC直接连接MySQL操作数据库。

### 思路流程

输入class类对象参数-》解析对应的SQL函数-》数据库执行操作-》返回结果参数输出

### 基础设计

编写一个ORM框架，所有的类都只能动态定义，只有使用者才能根据表的结构定义出对应的类来。metaclass，直译为元类，metaclass允许你动态的控制类的创建行为。换句话说，你可以把类看成是metaclass创建出来的“实例”。

设计Field类，也就是映射数据库的字段，负责保存数据库表的**字段名**和字段类型。

设计Model类，也就是数据库会话基础操作，负责操作数据库DML语句等。

# 第9章、补充知识点

## 9.1、UML知识点

面向对象的分析与设计的目的可以描述为3步：

- 在面向对象的分析，最重要的目的是确定对象和描述他们以适当的方式。如果这些对象的有效识别，那么接下来的设计工作是很容易的。对象应确定职责。职责是对象所执行的功能。每一个对象具有某种类型的要执行的责任。当这些责任协作系统的目的达成。
- 第二阶段是面向对象的设计。在这个阶段的重点时要求及其履行情况。在这一阶段中的对象根据其预期的关联协作。协会完成设计后也完成了。
- 第三阶段是面向对象的执行。在这个阶段，设计采用面向对象语言，如Java，C++，Go,Python等。

类图是面向对象系统建模中重要的图，是定义其它图的基础。类图主要是用来展现软件系统中的类、接口以及它们之间的静态结构。

#### 9.1.1、UML类图中，类之间的关系

泛化（Generalization）,  实现（Realization），关联（Association)，聚合（Aggregation），组合(Composition)，依赖(Dependency)

##### 泛化（Generalization）

单向继承关系，属于A是B的子类。

![image-20200122151948540](/Users/didi/Library/Application Support/typora-user-images/image-20200122151948540.png)

##### 实现（Realization）

单向实现关系，属于A实现B的某个函数功能。

![image-20200223211758118](/Users/didi/Library/Application Support/typora-user-images/image-20200223211758118.png)

##### 关联（Association)

单向或双向（通常我们需要避免使用双向关联关系），是一种"has a"关系，如果A单向关联B，则可以说A has a B，通常表现为全局变量

<img src="https://img-blog.csdn.net/20150714100505486?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="img" style="zoom:200%;" />

```java
public class Person {
	public Phone phone;
	
	public void setPhone(Phone phone){		
		this.phone = phone;
	}
	
	public Phone getPhone(){		
		return phone;
	}
}
```



##### 聚合（Aggregation）

单向，关联关系的一种，与关联关系之间的区别是语义上的，关联的两个对象通常是平等的，聚合则一般不平等，有一种整体和局部的感觉，实现上区别不大。

![image-20200122151028180](/Users/didi/Library/Application Support/typora-user-images/image-20200122151028180.png)

```java
public class Team {
	public Person person;
	
	public Team(Person person){
		this.person = person;
	}
}
```



##### 组合(Composition)

单向，是一种强依赖的特殊聚合关系

![img](https://img-blog.csdn.net/20150714093001275?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

Head，Body，Arm和Leg组合成People，其生命周期相同.

```java
public class Person {
	public Head head;
	public Body body;
	public Arm arm;
	public Leg leg;
	
	public Person(){
		head = new Head();
		body = new Body();
		arm = new Arm();
		leg = new Leg();
	}
}
```



##### 依赖(Dependency)

单向，表示一个类依赖于另一个类的定义，其中一个类的变化将影响另外一个类，是一种“use a”关系。

如果A依赖于B，则B表现为A的局部变量，方法参数，静态方法调用等

<img src="https://img-blog.csdn.net/20150714095530396?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="img" style="zoom:200%;" />

```java
public class Person {
	public void doSomething(){
		Card card = new Card();//局部变量
		....
	}
}
```

```java
public class Person {
	public void doSomething(Card card){//方法参数
		....
	}
}
```

```java
public class Person {
	public void doSomething(){
		int id = Card.getId();//静态方法调用
		...
	}
}
```



#### 9.1.2、UML 支持的可见性类型的标志

![image-20200122130441548](/Users/didi/Library/Application Support/typora-user-images/image-20200122130441548.png)



## 9.2、Java知识点

### 9.2.1、并发编程模块

借用《码出高效》的一段描述：ThreadLocal初衷是在线程并发时候，<font color=red>解决变量共享问题</font>，但由于过度设计，比如弱引用和哈希碰撞，导致理解难度大，使用成本高，反而经常成为故障高发点，容易出现内存泄露/脏数据/共享对象更新等问题。

### 9.2.2、基础编程模块

#### 9.2.2.1、序列化版本号

- java原生序列化

  java可以通过实现Serializable接口来实现该类对象的序列化，兼容性最好，但不支持跨语言。
  实现接口以后如果不显示指定serialVersionUID，每次运行时候，编译器会根据类的内部实现，包括类名，接口名，方法和属性等来自动生成serialVersionUID,如果类的源代码有改动，则重新编译后的取值可能会变化，所以需要显示定义。
  如果是兼容升级，请不要修改serialVersionUID,避免反序列化失败
  如果是不兼容升级，需要修改serialVersionUID，避免反序列化混乱。
  注意点：Java反序列化时候不会调用类的无参构造方法，而是调用native方法将成员变量赋值为对象类型的初始值。基于性能和兼容性，不推荐。

- Hessian序列化
  Hessian序列化是一种支持动态类型，跨语言，基于对象传输的网络协议。Java对象的序列化的二进制流可以被其他语言（C++,Python）序列化。Hessian协议如下：
  自描述序列化类型。不依赖外部描述文件或接口定义，用一个字节表示常用基础类型，极大缩短二进制流。
  语言无关，支持脚本语言。
  协议简单，比java原生序列化高效
  2.0版本增加了压缩编码，二进制流大小是java序列化的50%，耗时是30%，反序列化是20%。
  Hessian会把复杂对象所有属性存储在一个Map中进行序列化，父类和子类存在同名成员变量的情况下，Hessian序列化会先序列化子类，然后序列化父类。因此反序列化会导致子类的同名成员变量被父类覆盖掉。

- JSON序列化
  轻量级的数据交换格式
  JSON序列化是将数据对象转换为JSON字符串。敏感属性不需要序列化传输，可以加transient，避免把此属性信息转化为序列化的二进制流。

#### 9.2.2.2、抽象类和接口

1. 表示内容：抽象类表示该类中可能已经有一些方法的具体定义，接口就仅仅只能定义各个方法的界面（方法名，参数列表，返回类型），并不关心具体细节。
2. 性质不同：抽象类是对象的抽象，接口是一种行为规范。
3. 成员变量不同：抽象类中的成员变量可以被不同的修饰符来修饰，接口中的成员变量默认的都是静态常量（static final）。

归纳小结：<font color=red>所以接口是一种对实现类的强制约束规范，抽象是对内容的一种提炼，属于类和接口的一种中间态，抽象方法强制子类去实现，也可以自己完成,并且可以实现接口后由自身实现或者子类实现2种实现方式。</font>

#### 9.2.2.3、JNDI（待完善）

概念

JNDI（Java Naming and Directory Interface ），类似于在一个中心注册一个东西，以后要用的时候，只需要根据名字去注册中心查找，注册中心返回你要的东西。web程序，我们可以将一些东西（比如数据库相关的）交给服务器软件去配置和管理（有全局配置和单个web程序的配置），在程序代码中只要通过名称查找就能得到我们注册的东西，而且如果注册的东西有变，比如更换了数据库，我们只需要修改注册信息，名称不改，因此代码也不需要修改。

在Java开发中，使用JDBC操作数据库的四个步骤如下：
 　　　①加载数据库驱动程序(Class.forName("数据库驱动类");)
 　　　②连接数据库(Connection con  = DriverManager.getConnection();)
 　　　③操作数据库(PreparedStatement stat = con.prepareStatement(sql);stat.executeQuery();)
 　　　④关闭数据库，释放连接(con.close();)
 　也就是说，所有的用户都需要经过此四步进行操作，但是这四步之中有三步(①加载数据库驱动程序、②连接数据库、④关闭数据库，释放连接)对所有人都是一样的，而所有人只有在操作数据库上是不一样，那么这就造成了性能的损耗。
 　那么最好的做法是，准备出一个空间，此空间里专门保存着全部的数据库连接，以后用户用数据库操作的时候不用再重新加载驱动、连接数据库之类的，而直接从此空间中取走连接，关闭的时候直接把连接放回到此空间之中。
 　那么此空间就可以称为连接池（保存所有的数据库连接），但是如果要想实现此空间的话，则必须有一个问题要考虑？
 　 　　1、 如果没有任何一个用户使用连接，那么那么应该维持一定数量的连接，等待用户使用。
 　　　2、 如果连接已经满了，则必须打开新的连接，供更多用户使用。
 　　　3、 如果一个服务器就只能有100个连接，那么如果有第101个人过来呢？应该等待其他用户释放连接
 　　　4、 如果一个用户等待时间太长了，则应该告诉用户，操作是失败的。
 　如果直接用程序实现以上功能，则会比较麻烦，所以在Tomcat 4.1.27之后，在服务器上就直接增加了数据源的配置选项，直接在服务器上配置好数据源连接池即可。在J2EE服务器上保存着一个数据库的多个连接。每一个连接通过DataSource可以找到。DataSource被绑定在了JNDI树上（为每一个DataSource提供一个名字）客户端通过名称找到在JNDI树上绑定的DataSource，再由DataSource找到一个连接。如下图所示

![image-20200304182038618](/Users/didi/Library/Application Support/typora-user-images/image-20200304182038618.png)

那么在以后的操作中，除了数据库的连接方式不一样之外，其他的所有操作都一样，只是关闭的时候不是彻底地关闭数据库，而是把数据库的连接放回到连接池中去。
 　如果要想使用数据源的配置，则必须配置虚拟目录，因为此配置是在虚拟目录之上起作用的。需要注意的是，如果要想完成以上的功能，在Tomcat服务器上一定要有各个数据库的驱动程序。

## 9.3、GOF设计模式的应用和扩展

https://mp.weixin.qq.com/s/LaEkNUiTwK8RXBCEZu89Ug

![image-20200313204707380](/Users/didi/Library/Application Support/typora-user-images/image-20200313204707380.png)

![image-20200313204731412](/Users/didi/Library/Application Support/typora-user-images/image-20200313204731412.png)

![image-20200313205311006](/Users/didi/Library/Application Support/typora-user-images/image-20200313205311006.png)

![image-20200313205443413](/Users/didi/Library/Application Support/typora-user-images/image-20200313205443413.png)

![image-20200313205822232](/Users/didi/Library/Application Support/typora-user-images/image-20200313205822232.png)

![image-20200313210547865](/Users/didi/Library/Application Support/typora-user-images/image-20200313210547865.png)
