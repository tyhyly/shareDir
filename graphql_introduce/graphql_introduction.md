title: GraphQL 介绍与入门
speaker: lory lee(沐云)
url: https://github.com/ksky521/nodeppt
transition: kontext
files: /js/demo.js,/css/global.css,/img/bg1.png
theme: light

[slide]

# 一、GraphQL介绍 {:.text-lg}
## 演讲者：lory lee

[slide]
# Table of Contents  {:.text-super}

* 什么是GraphQL  {:.text-md}
  
   * a query langualge for APIs created by Facebook <br> (相关介绍:https://facebook.github.io/graphql/)
  
   * GraphQL由类型系统(Type System)<br> 查询语言和执行语义(Query Syntax) <br> 静态验证（Validation）<br> 类型内省组成(Introspection)
  
   * GraphQL.js 是GraphQL的JavaScript参考实现
   
* GraphQL入门 {:.text-md}   

* GraphQL与RESTFul api的对比(优缺点) {:.text-md}
* GraphQL的使用场景  {:.text-md}

[slide data-transition="zoomin"]

# GraphQL入门  {:.text-lg.vleft}
* Type Definitions  
```
Scalar Type        => scalar    
Object Type        => type
Interface Type     => interface
Union Type         => union
Enum Type          => enum
Input Object Type  => input
```
* Type System
 ```
  enum Hobbies {play, eat, sleep}

  type Human {
    id: String
    name: String
    home: String
    hobby: [Hobbies]
  }

  type Animal {
    id: String
    name: String
    food: String
    hobby: [Hobbies]
  }
 ``` 
 如果 Human和Animal也有其他的朋友friends，但是他们也可能有共同的朋友小明，如何定义类型？
 
[slide]

* 从type中抽离相同的字段为新的type 
  ```
  interface Role {
    id: String!
    name: String
    friends: [Role]
    hobby: [Hobbies]
  }

  type Human implements Role {
    id: String!
    name: String
    friends: [Role]
    home: String
    hobby: [Hobbies]
  }

  type Animal implements Role {
    id: String!
    name: String
    friends: [Role]
    food: String
    hobby: [Hobbies]
  }
 ```  
 加``` !```是为了设置返回字段值为非null值，默认情况下null在GraphQL中不存在的字段的默认值
 ```
  Type Markers
  ============
  Non-null Type                    => <type>!     e.g String!
  List Type                        => [<type>]    e.g [String]
  List of Non-null Types           => [<type>!]   e.g [String!]
  Non-null List Type               => [<type>]!   e.g [String]!
  Non-null List of Non-null Types  => [<type>!]!  e.g [String!]!
 ```

[slide]

* 定义top-level API (basis for all queries) 
  ```
  type Query {
    biology(hobby: Hobbies): Role
    human(id: String!): Human
    animal(id: String!): Animal
  }
  ```
 > 上述为自定义类型系统，GraphQL内置类型系统可参考https://github.com/graphql/graphql-js/tree/master/src/type

[slide data-transition="vertical3d"]

# Query Syntax  {:&.flexbox.vleft}
## 1、普通查询
```
query human {
   human {
      id
      name
      hobby
   }
}
```
返回值：
```
 {
   "human": {
      "id": 1,
      "name": "someone"
      "hobby": "eat watermelon"
    }
 }
```
[slide]

## 2、带参数的查询与重复字段的查询
```
query animal {
  dog: animal(id: "123") {
     name
  }
}

query human {
   lucy: human(id: "234") {
     name
   }
}
```
重复的片段，可以用fragment 来替换

```
fragment NameFragment on Role {
   name
}

query human {
   lucy: human(id: "234") {
     ...NameFragment
   }
}
```
[slide]

# GraphQL之于RESTful API
> RESTful的核心理念在于资源（resource），且荐酒一个RESTful接口仅操作单一资源  {:.text-md background="yellow"}
  
 * entry point 太多，而graphql接口一般是单一入口，配置在/graphql/下（也支持多入口，数量远小于RESTful）
 * 数据关联性方面，RESTful所操作的资源是相对离散的；GraphQL的数据更有整体性；
 * 更易于前端缓存数据（Relay和apollo-client的缓存原理）
 * RESTful API版本过多难于惯例； GraphQL则不存在版本管理问题（Versionless API）
 * 服务端/客户端数据不匹配，制定协议成本过高（协调、沟通、更改），联调繁琐
 * 多余的数据库调用及性能不佳（响应时间，带宽消耗）
 * RESTful API过多，学习成本高 

[slide]

# GraqpQL纯粹的优点  {:&.flexbox.vleft}
###1. 强类型 schema {:.vleft} 
`（后端api更新但是文档没跟上，不知到新的api是干什么用的）`
   * 无需编写api文档，因为根据shcema自动生成了
   * 可利用编译工具检查api调用是发生的错误，自动验证api请求

###2. 按需获取(解决了Overfetching和Underfetching)
   - 前端可以自己决定返回的数据及结构

###3. 支持快速产品开发
   - 无论UI如何变，后端不用变
   - 借助工具（GraphQL Faker），前后端可以独立工作
   - GraphQL API使得后端构建接口更轻松（schema-driven development，SDD）

###4. Composing GraphQL API
   - schema 拼接（组合和链接过个API，类似组件，大组件由不同的小组件构成）
     前端只需要处理单个API端点，与不同服务端通信的复杂性则从前端隐藏

###5. 生态繁荣，社区强大（牛X哄哄）
   - TL;DR

[slide]
## GraphQL的缺点
  * N+1问题：在实现GraphQL的服务端接口时，查询N+1次数据库    
  解决方式：    
     1. 针对一对一的关系（医院和医院详情）如果从数据库直接获取数据，将数据join到一张表    
     2. 针对多对一和多对多，使用DataLoader工具库
  * 缓存查询结果
    1. apollo-client和Relay库已经实现
    2. 参考 https://blog.apollographql.com/the-concepts-of-graphql-bc68bd819be3
  
[slide]

# 技术架构  {:&.flexbox.vleft}  
 >   {:.text-md background="yellow"}  

 ![xxx](/img/graphql_layer_1.png)   
 
  "graphql在技术架构中的位置"

[slide]
 
todo


[slide]

todo



[slide]
# GraphQL的使用场景（Big Picture)  {:&.flexbox.vleft}

## 1.连接数据库的GraphQL服务
  - GraphQL与传输层无关，gql服务器可能是基于TCP，websockets或者其他网络协议


## 2.集成现有系统的GraphQL层
  - 将多个现有系统集成在一个统一的GraphQL API后面(api太多，遗留基础设施，难以维护，GraphQL用于统一这些系统，提供一个友好的gql api，开发新的客户端应用，
    GraphQL服务器负责从现有系统提取数据)
  - 不关心数据源(GraphQL允许你隐藏现有系统的复杂性，在统一的GraphQL接口后面，集成微服务，传统基础架构和第三方API)

## 3.连接数据库与现有系统集成的混合方式
  - GraphQL服务器可以从单个数据库或者现有系统获取数据，从而达到灵活性，并将所有数据管理的复杂性交给到服务器

[slide]
# GraphQL客户端引入所引发的开发模式的变化  {:&.flexbox.vleft}

### 原有RESTful API获取数据时，大多数应用必须执行以下步骤：    

1. 构造和发送HTTP请求（例如，在Javascript中fetch）
2. 接收并解析服务器响应
3. 在本地存储数据（简单地存在内存或持久存储）
4. 在UI中显示数据

### 使用理想化的声明式数据获取方法，客户端不应该做以下两个步骤：
 1. 描述数据要求 
 2. 在UI中显示数据    
> 所有底层网络任务以及数据存储都应该被抽象出来，数据依赖关系的声明才应该是主要部分。
  换句话说，我们在API抽象阶梯上向上爬了一步，不再需要自己处理低级网络任务了。

[slide]
## GraphiQL使用
1. 直接下载客户端[http://electronjs.org/apps/graphiql] 
2. 使用graphiql依赖

[slide]

## 相关学习资料:   {:.text-lg &.flexbox.vleft}
----

 <br>Spec 规范： https://facebook.github.io/graphql/    
 相关在线参考资源:  {:.text-left .color-green}
 1. https://www.graphql.com/tutorials/
 2. [中文文档](https://graphql.cn)
 3. https://www.howtographql.com
 4. https://blog.apollographql.com/
 5. [GraphQL Schema Language Cheat Sheet](https://wehavefaces.net/graphql-shorthand-notation-cheatsheet-17cd715861b6)
[slide style="background-image:url('/img/bg1.png')"]

## Thks! {:.text-lg.text-success}
