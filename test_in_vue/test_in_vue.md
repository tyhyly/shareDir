title: Vue开发中的单元测试
speaker: lory lee
url: https://github.com/ksky521/nodeppt
transition: kontext
files: /js/demo.js,/css/global.css,/img/bg1.png
theme: blue

[slide]

# Vue单元测试 {:.text-lg}
## 演讲者：lory lee

[slide]
## Table of Contents  {:.text-super}

* 常用的vue组件测试场景 {:.text-md}
   * 接收props {:.text-md}
   * 使用computed属性 {:.text-md}
   * render其他组件 {:.text-md}
   * 事件 {:.text-md}
   * 异步组件、异步请求 {:.text-md}    

* 其他场景 {:.text-md}
   * 测试router {:.text-md}
   * 测试vuex（在组件中/独立测试） {:.text-md}
   * 测试第三方组件 {:.text-md}

[slide data-transition="zoomin"] {:.padding-0}

# 开始-搭建项目  {:.text-lg.vleft}

## 一、 测试所用插件  {:.text-left}

  * vue-cli 和 jest（@vue/cli-plugin-unit-jest、jest、expect）
  * vue-cli 和 mocha （@vue/cli-plugin-unit-mocha、mocha、chai）

## 二、 搭建项目 {:.text-left.ol-fix}

   1. `npm install -g @vue/cli` 或者     
	`yarn global add @vue/cli`  
   2. `vue create projectName`
   3. `npm run test:unit`

[slide]

# 开始-简单案例 {:.text-left.text-md}




[slide]


## 相关链接:   {:.text-lg.text-left.p-left}

1.[Vue testing handbook](https://lmiller1990.github.io/vue-testing-handbook/#what-is-this-guide)    
2.[Testing Vue.js Application Book](https://www.manning.com/books/testing-vue-js-applications)    
3.[官方文档](https://vue-test-utils.vuejs.org/zh)    
4.[test with vue-router](https://medium.com/js-dojo/unit-testing-vue-router-1d091241312)

[slide style="background-image:url('/img/bg1.png')"]

## Thks! {:.text-lg.text-success}
