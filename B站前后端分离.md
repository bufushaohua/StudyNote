## B站前后端分离

前后端分离就是将一个应用的前端代码和后端代码分开写，为什么要这样做？

如果不使用前后端的方式，会有哪些问题？

传统的Java Web开发中，前端使用JSP开发，JSP不是由后端开发者独立完成的

前端——》HTML静态页面——》后端——》JSP

这种开发方式效率极低，可以使用前后端分离的方式进行开发，就可以完美解决这一问题

前端只需独立编写客户端代码，后端也只需编写独立代码提供数据接口即可

前端通过Ajax请求来访问后端数据接口，将Model展示到View中即可

前后端开发者只需要提前约定好接口文档（URL、参数、数据类型），然后分别独立开发即可，前端可以造假数据测试，完全不需要依赖于后端。最后完成前后端集成即可。真正实现了前后端应用的解耦合，极大提升了开发效率。

单体——》前端应用+后端应用

前端应用：负责数据展示和用户交互

后端应用：负责提供数据处理接口

前端HTML——》Ajax——》RESTful后端数据接口

### 传统单体应用

![image-20200607150930997](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20200607150930997.png)

### 前后端分离的结构

![image-20200607151346688](C:\Users\CYC\AppData\Roaming\Typora\typora-user-images\image-20200607151346688.png)

前后端分离就是将一个单体应用拆分成两个独立的应用，前端应用和后端应用以JSON格式进行数据交互



### 实现技术

Spring Boot +Vue

使用Sring Boot进行后端应用开发，使用VUe进行前端应用开发



### Vue + Element UI

Vue集成Element UI

Element UI后台管理系统主要的标签

- el-container：构建整个页面框架
- el-aside：构建左侧菜单
- el-menu：左侧菜单内容，常用属性
  - :default-openeds：默认展开的菜单，通过菜单的index值来关联
  - :default-active：默认选中的菜单，通过菜单的index值来关联
- el-submenu：可展开的菜单，常用的属性
  - index：菜单的下标，文本类型，不能是数值类型
- template：对应el-submenu的菜单名
- i：设置菜单图标，通过class属性实则
  - el-icon-message
  - el-icon-menu
  - el-icon-setting
- el-menu-item：菜单的子节点，不可再展开，常用属性：index：菜单的下标，文本类型，不能是数值类型



### 使用VUE项目管理器新建一个VUE项目

> 文章链接地址：https://blog.csdn.net/ryan55/article/details/104944825



Vue router来动态构建左侧菜单

- 导航1
  - 页面1
  - 页面2
- 导航2
  - 页面3
  - 页面4



### menu与router的绑定

1. <el-menu>标签添加router属性
2. 在页面中添加<router-view>标签，它是一个容器，动态渲染你选择的router
3. <el-menu-item>标签的index值就是要跳转的router
4. :model绑定对象，:rules绑定校验规则



Element UI表单数据校验

定义rules对象，在rules对象中设置表单各个选项的校验规则

~~~js
rules: {
    name: [
      { required: true, message: '请输入活动名称', trigger: 'blur' },
      { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
    ]
}

~~~

required： true，是否为必填项

message： 'error'，提示信息

trigger：'blur'，触发光标失焦事件

