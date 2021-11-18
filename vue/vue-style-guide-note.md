# Style guide 

## 1. 规则类别

### A 必要的（务必遵守的）

### B 强烈推荐 （能改善绝大多数工程中的可读性和开发体验，即使违反了，项目也能运行）

### C 推荐的

有多个处于同等价值的备选项，可以选择任意一项。推荐使用一个默认项。

### D 谨慎使用

Vue 中的某些特性被过度使用会造成代码难以维护，甚至变成 bug 的来源。

## 2. 优先级A

#### 1. 组件名为多个单词

组件名应该始终由多个单词组成，除了根组件 App，transition、component 这样的自定义组件。

好处：避免与现有或未来 HTML 元素产生冲突，HTML 元素都是单个单词。

eg:

```javascript
Todo // bad
todo // bad
todo-item // good
TodoItem // good
```

#### 2. prop 定义

prop 定义尽量详细。

好处：

- 不正确的 prop ，会被 Vue 告警，以帮助开发者捕获潜在的错误来源。

eg:

```javascript
props:{
  status:String
} // good
props:{
  status:{
    type:String,
    required:true,
    validator:(value)=>{
      return [
        'syncing',
        'synced',
        'version-conflict',
        'error'
      ].includes(value)
    }
  }
} // great
```

#### 3. 为 v-for 设置 key

始终使用 key 配合 v-for。

好处：便于维护内部组件及其子树的状态。即使对于元素，维持可预测的行为也是一种好的做法。（此处提到了动画中的对象固化。对象固化是什么意思？[对象固化](https://bost.ocks.org/mike/constancy/)） 好处详解：[例子](https://cloud.tencent.com/developer/article/1676293)

```vue
<ul>
  <li v-for='todo in todos' :key='todo.id'>
    {{todo.text}}
  </li>
</ul> // good
```

#### 4. 避免 v-for 和 v-if 一起使用

永远不要在一个元素上同时使用 v-if 和 v-for。 记住是 **永远**

什么样的情况两者可能会一起使用？

- 对列表中项目进行过滤。

  ```vue
  <li v-for="user in users" v-if='user.isActive'>
  	
  </li>
  // 这种情况下，请将 users 替换为 activeUsers (计算属性)
  // vue3 中，v-if 的优先级比 v-for 高。 这里将抛出错误，v-if 先执行，但是访问不到 user
  ```

- 避免渲染应该被隐藏的列表。

  ```vue
  <li v-for="user in users" v-if="shouldShowUser">
  
  </li>
  // 这里的整个 li 列表本意是被隐藏，应该将 v-if 提到容器元素上
  ```

#### 5. 为组件样式设置作用域

对于某个应用，样式在顶层 App 组件和布局组件中可以是全局的，但是在其他所有组件中都应该是有作用域的。

这条规则适用于单文件组件。 

作用域可通过 scoped attribute 、Css Modules（[css modules](https://vue-loader.vuejs.org/guide/css-modules.html)  一个基于 class 的，类似 [BEM](http://getbem.com/) 的策略）、或者其它的库/约定来实现。

对于组件库来说，倾向使用基于 class 的策略，而不是 scoped attribute。这样的好处：覆写内部样式更容易，使用了人类可理解的 class 名称，没有太高的选择器优先级，不太会导致冲突。

