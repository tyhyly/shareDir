title: Vue3 介绍与实践
speaker: lory lee(沐云)
url: https://github.com/ksky521/nodeppt
transition: kontext
files: /js/demo.js,/css/global.css,/img/bg1.png
theme: light

[slide]

# 一、Vue3介绍与实践 {:.text-lg}
## 演讲者：lory lee

[slide]
# Table of Contents  {:.text-super}

* Vue3相对Vue2的优势  {:.text-md}
  
   * 响应式原理 <br> (相关介绍:https://facebook.github.io/graphql/)
  
   * Options API and Composition API的区别 <br>
  
   * Vue3 hooks之于React hooks
   * Vue3 vdom diff 算法之于Vue2的改进
   * Vue3 其他新增功能（Fragment、Teleport）

* TypeScript入门  

[slide data-transition="zoomin"]

# 响应式原理  {:.text-lg.vleft}
* Object.defineProperty  
```
缺点无法监控 内建属性，如数组length，对象的增加和删除
```
* Proxy API
它提供了一种机制来拦截对象上的基本JavaScript操作。
 ```
优点：解决了对象的增加和删除的监听问题
 ``` 
 
[slide]


[slide]


[slide data-transition="vertical3d"]

#   {:&.flexbox.vleft}

[slide]


## 


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


[slide]

[slide]

[slide]

## 相关学习资料:   {:.text-lg &.flexbox.vleft}
----

 <br>Spec 规范： https://facebook.github.io/graphql/    
 相关在线参考资源:  {:.text-left .color-green}

[slide style="background-image:url('/img/bg1.png')"]

## Thks! {:.text-lg.text-success}
