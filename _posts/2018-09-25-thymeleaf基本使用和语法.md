# SpringBoot thymeleaf模板引擎

结合了通过模板引擎使html页面集成了页面对数据的处理功能，如下图：
![模板引擎原理](https://i.imgur.com/CxNObUr.png)

## thymeleaf使用以及语法

springBoot提供的thymeleaf版本会远远低于先行版本所以需要覆盖旧版本
在pom.xml的<properties>标签内设置
  

    <thymeleaf.version>3.0.9.RELEASE</thymeleaf.version>		
	<thymeleaf-layout-dialect.version>2.2.2</thymeleaf-layout-dialect.version>

以此来使用最新版本

欢迎页面默认路径是resources/static下名为index.html的文件
其他请求指向的页面是resources/templates下 xxxx.html的文件

使用thymeleaf模板步骤
1. 导入thymeleaf的名称空间


    <html lang="en" xmlns:th="http://www.thymeleaf.org">



2. 表达式使用
- 使用 ${..} 获取变量值
可以获取对象的属性、调用方法(方法可传参)
可以使用内置的而基本对象
可以使用内置的一些工具对象



    <div th:utext="${hello}"></div>
	<div th:utext="${#locale.country}"></div>
	<div th:utext="${Student.show('abc')}"></div>
	<div th:utext="${session.testSession}"></div>
	<div th:utext="${session.isEmpty()}"></div>
	<div th:utext="${session.size()}"></div>
	<h1 th:text=${#dates.createNow()}></h1>
	<div th:utext="${#strings.contains('aabbccabc','abc')}"></div> 



th:text在某个标签里就会替代标签里原本的值

- 选择变量表达式\*{..}
基本用法和${..}类似，但是扩展了变量表达式的一个用法
先将对象存入object，再用\*{..}获取属性




	<div th:object="${Student}">
		<p>Student Id: <span th:text=*{id}></span></p>
		<p>Student Name: <span th:text=*{name}></span></p>
	</div>  



 - 获取国际化内容#{..}
 


	<h1 class="h3 mb-3 font-weight-normal" th:text="#{login.tip}">Please sign in</h1>	



相应的在属性文件里面要设置**login.tip=Please sign in**属性

 - 定义URL @{..}
提交值来跳转页面不需要在url后拼接?xxxx= 而是


	<link th:href="@{/webjars/bootstrap/4.1.3/css/bootstrap.css}" rel="stylesheet">
	<img class="mb-4" th:src="@{asserts/img/bootstrap-solid.svg}" />



 - 片段模板表达式~{..} 
 - 支持基本数据类型的操作和使用
 - 支持比较运算
 - 支持布尔运算
 - 支持数学运算
 - 支持条件运算(三元运算符)
	 


	<div th:utext="${3>=5}"></div>
	<div th:utext="${1+2}"></div>
	<div th:utext="${true||false}"></div>
	<div th:utext="${(1+2>2)?(3):(2)}"></div>
	<div th:switch="${user.role}">
	  <p th:case="'admin'">User is an administrator</p>
	  <p th:case="#{roles.manager}">User is a manager</p>
	  <p th:case="*">User is some other thing</p>
	</div>




th:text 和 th:utext的区别，前者会对特殊字符进行转义，后者不会进行转义
 - 	对数组进行遍历




	<h4>
		<span th:each="user: ${users}">[[${user}]]</span>
	</h4>



以上有几点需要注意的
 th:each 写在哪个标签里，哪个标签在遍历的时候就会重复出现，以上代码是重复出现<span>标签
 thymeleaf模板不像jsp页面，可以直接引用变量，需要遵从行内写法的语法规则
**[[..]]** - - - - - 相当于 th:text
**[(..)]** - - - - - 相当于 th:utext