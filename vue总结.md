# vue总结

#### 1、html模板

- 插值表达式：
  - {{js表达式、数据模型}}：js表达式必须有返回值，缺点：出现插值闪烁
  - v-text：通常使用该方式
  - v-html：解析html，js，css。缺点：有安全隐患
- 双向渲染：双向绑定
  - v-mode="数据模型"：在表单元素中使用，才有意义
- 事件：
  - @click：点击事件
  - @contextMenu：右击事件，时间修饰符：.prevent阻止默认事件
  - @keyup：键盘事件
    - 13或者enter键：回车事件@keyup.13
    - 组合事件
- v-for：遍历

  - 数组：v-for="{item, index}  in  items"
  - 对象：v-for="(val, key, index)  in  user"
  - :key：提高渲染速度
- v-if：判断

  - v-if="布尔表达式"：true-渲染，false-不渲染
  - v-show="布尔表达式"：总是渲染，false-display：none
  - v-else-if="布尔表达式"
  - v-else：else一定要紧跟在if之后
 - v-bind：绑定属性，简写（:）
    - :class="{active：布尔表达式}"

#### 2、vue实例js

- el：选择器，对应html模板

- data：数据模型

- methods：定义方法

- commputed：定义计算属性，计算本质就是方法，但是方法必须有返回值，计算属性可以像数据模型一样使用。如果计算属性的依赖项没有改变，直接从缓存中命中

- watch：监听，方法名是要监听的数据模型名称，message(newVal，oldVal){}

- 钩子函数：created，在对象初始化之后执行，通常在created中初始化数据

- components：局部组件

  - 全局组件：

  ```javascript
  Vue.component("组件名",{
      template: "html模板",
      data（）{
      	return {
      		数据模型
  		}
  	},
  	methods,watch
  })
  ```

  - 局部组件:

  ```javascript
  const 组件对象名={同上}
  ```

  - 组件通信：

    - 父向子通信：
      - 父自定义属性，属性名随便写，属性值是要传递的数据模型
      - 子通过props接收，参数名是自定义属性的属性名["属性名"]{属性名：{type default required}}
    - 子向父通信：
      - 父自定义事件，事件名随便写，属性值是要传递的方法
      - 定义事件调用子自己的方法，子的方法中通过this.$emit("自定义事件名")

#### 3、路由：vue-router   npm install vue-router   --save

- 引入vue-router组件
- 实例化vue-router：

```javascript
const router = new VueRouter({
    routes: [
        {
            path: 路由路径，要以"/"开头
            component：路由组件
        },
        {}
    ]
})
```

- 引入到vue实例中：通过router

```javascript
<router-link to="/login"></router-link>
<router-view/>: 锚点
```













