# jsp 笔记

## 1、JSP指令

- ```jsp
  <%@ page ... %>
  定义页面的依赖属性，比如脚本语言、error页面、缓存需求等等
  ```

- ```jsp
  <%@ include ... %>
  包含其他文件
  ```

- ```jsp
  <%@ taglib ... %>
  引入标签库的定义，可以是自定义标签
  ```

## 2、JSP 九大隐含对象

- request
  - **HttpServletRequest**类的实例
- response
  - **HttpServletResponse**类的实例
- out
  - **PrintWriter**类的实例，用于把结果输出至网页上
- session
  - **HttpSession**类的实例
- application
  - **ServletContext**类的实例，与应用上下文有关
- config
  - **ServletConfig**类的实例
- pageContext
  - **PageContext**类的实例，提供对JSP页面所有对象以及命名空间的访问
- page
  - 类似于Java类中的this关键字
- Exception
  - **Exception**类的对象，代表发生错误的JSP页面中对应的异常对象

## 3、四大作用域

- page
  - 一个页面起作用
- request
  - 一次请求中起作用
- session
  - 一次会话
- application
  - 整个项目





