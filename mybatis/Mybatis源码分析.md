# Mybatis源码分析

## 一、什么是Mybatis

- Mybaits是一个优秀持久层框架，支持自定义SQL、存储过程以及高级映射。封装JDBC的代码，免除设置参数以及一些结果集映射的工作。mybatis可以通过简单xml、注解配置和映射接口、原始类型、POJO的数据库字段。

## 二、功能使用指南



## 三、源码分析

### 1.测试框架代码

```java

@Test
void queryById() throws IOException {  
    String resource = "mybatis-config.xml";  
    InputStream inputStream = Resources.getResourceAsStream(resource); 
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream); 
    SqlSession sqlSession = sqlSessionFactory.openSession(); 
    UserMapper userMapper = sqlSession.getMapper(UserMapper.class); 
    User user = userMapper.queryById(1); 
    assertNotNull(user);
}
```

### 2.获取SqlSessionFactory对象（初始化过程）

```
//获取SqlSessionFactory对象(DefualtSqlSessionFactory)
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
```

<img src="F:%5Ctypora%5Cmybatis%5C%E8%8E%B7%E5%8F%96SqlSessionFactory%E6%97%B6%E5%BA%8F%E5%9B%BE.png" style="zoom: 200%;" />

大致流程：

   读取配置mybatis-config.xml文件配置，解析成XNode节点对象，根据Mybatis固定的节点配置解析，初始化Configuration中的属性，无配置的属性，Configuration配置了默认实现方式，最终configuration交给DefaultSqlSessionFactory对象。Configuration这个对象贯穿整个mybatis流程，只需初始化一次，一直保存在内存中。

### 3.获取SqlSession对象

```
//获取SqlSession对象（DefualtSqlSession）
SqlSession sqlSession = sqlSessionFactory.openSession(); 
```

<img src="F:%5Ctypora%5Cmybatis%5CMybatis%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90.assets%5C%E4%BC%81%E4%B8%9A%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20200408192408.png"  />

大致流程：

​		根据Configuration配置中，标签<transactionManager/>类型，创建具体Transation事务类型，没有具体事务类型，默认ManagedTransaction事务机制，然后根据ExecuterType类型，创建具体什么类型执行器，然后调用Plugin拦截器，最终创建DefaultSqlSession对象返回给SqlSession.

### 4.获取具体Mapper接口代理对象

```JAVA
//MapperProxy对象代理UserMapper对象
UserMapper userMapper = sqlSession.getMapper(UserMapper.class); 
```



### 5.执行SQL过程

```java
User user = userMapper.queryById(1); 
```

### 6.插件原理

### 7.生命周期和作用域

### 8.mybatis源码分析总结

## 四、自定义开发Mybatis功能

### 1.自定义插件

### 2.自定义映射TypeHandler