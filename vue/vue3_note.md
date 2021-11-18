# Vue3 笔记

## 1. 基础

## 2. 深入组件

## 3. 过渡&动画

## 4. 可复用&组合

### 4.1 组合式 API

#### 4.1.1 介绍

##### 1. 什么是组合式 API

例子：假设应用中有个显示用户仓库列表的视图。除此之外，具有搜索和筛选功能。代码可能如下：

```javascript
// src/components/UserRepositories.vue
export default {
  components:{
    RepositoriesFilter,
    RepositoriesSortBy,
    RepositoriesList
  },
  props:{
    user:{
      type:String,
      required:true
    }
  },
  data(){
    return {
      repositories:[],
      filters:{},
      searchQuery:''
    }
  },
  computed:{
    filteredRepositories(){},
    repositoriesMatchingSearchQuery(){}
  },
  watch:{
    user:'getUserRepositories'
  },
  methods:{
    getUserRepositories(){
      // get repository by this.user
    },
    updateFilters(){}
  },
  mounted(){
    this.getUserRepositories()
  }
}
```

该组件的职责（或者说功能？）

- 从假定的 API 获取 user 的仓库，并在用户有任何更改时刷新。
- 利用 searchQuery 字符串搜索仓库
- 使用 filters 对象筛选仓库

使用 data/computed/methods/watch 组件选项来组织逻辑，当组件变得大型时。碎片化的组件使得理解和维护组件变得困难。选项的分离掩盖了潜在的问题。

将同一个逻辑关注点相关代码收集在一起会更好。这正是组合式 API 能做到的事。

##### 2. 组合式 API 基础

使用组合式 API 的位置称为 setup。

######  setup 选项

setup 选项在组件创建之前执行，一旦props被解析，就作为组合式 API 的入口。

**注意**：setup 中应避免使用 this ，因为在 setup 在不会找到组件实例。同理，setup 中无法使用 data/computed/methods。setup 的调用发生在它们被解析之前。

setup 接收 props 和 context 作为参数。setup 返回的所有内容都暴露给组件的其余部分（computed、methods、生命周期钩子等等）及组件的模板。

添加 setup 到组件中：

```javascript
export default {
  components:{...},
  props:{
    user:{
      type:String,
      required:true
    }
  },
  setup(props){
    console.log(props) // {user:''}
    return {
      // 返回内容用于组件其余部分
    }
  }，
	// 其余部分
}
```

提取第一个逻辑关注点：

- 从假定的外部 API 获取 user 的 repo，并在 user 改变时进行刷新

从最明显的部分开始：

- 仓库列表
- 更新仓库列表的function
- 返回仓库 list 和 function ，便于组件其余选项访问

```javascript
// src/components/UserRepositories.vue  'setup' function
import {fetchUserRepositories} from '@/api/repositories'
// 组件内
setup(props){
  let repositories =[]
  const getUserRepositories = async ()=>{
    repositories = await fetchUserRepositories(props.user)
  }
  return {
    repositories,
    getUserRepositories
  }
}
```

但是此时还无法生效， repositories 是非响应式的。即从用户的角度，repositories 始终为[]。

###### 带 ref 的响应式变量

ref 函数使任何响应式变量在任何地方起作用。

```javascript
import {ref} from 'vue'
const counter = ref(0)
```

ref 接收参数，将其包裹在带 value property 的对象中，并返回，使用该 value property 访问或更改响应式变量的值。

```javascript
import {ref} from 'vue'
const counter = ref(0)
console.log(counter) // {value:0}
console.log(counter.value) // 0
counter.value++
console.log(counter.value) // 1
```

Note: 即 ref 为我们的值创建了一个响应式引用。

创建响应式 repositories

```javascript
// src/components/UserRepositories.vue 
import {fetchUserRepositories} from '@/api/repositories'
import {ref} from 'vue'
export default {
	components:{RepositoriesFilters,RepositoriesSortBy,RepositoriesList},
	props:{
		user:{
			type:String,
			required:true
		}
	},
	setup(props){
		const repositories = ref([])
		const getUserRepositories = async () => {
			repositories.value = await fetchUserRepositories(props.user)
		}
    
    return {
      repositories,
      getUserRepositories
    }
	},
  data(){
    return {
      filters:{},
      searchQuery:''
    }
  },
  computed:{
    filteredRepositories(){},
    repositoriesMatchingSearchQuery(){}
  },
  watch:{
    user:'getUserRepositories'
  },
  mounted(){
    this.getUserRepositories()
  }
}
```

###### 在 setup 内注册生命周期钩子

组合式 API 上的生命周期钩子前有 on。例如: mounted-->onMounted。

这些函数接受一个回调作为参数，当钩子被组件调用时，该回调执行。

```javascript
import {fetchUserRepositories} from '@/api/repositories'
import {ref,onMounted} from 'vue'

setup(props){
  const repositories = ref([])
  const getRepositories = async ()=>{
    repositories.value = await fetchUserRepositories(props.user)
  }
  onMounted(getRepositories)
  return {
    repositories,
    getRepositories
  }
}
```

###### watch 响应式更改

从 vue 导入 watch 函数

watch 接受 3 个参数

- 响应式引用或 getter 函数
- 回调
- 配置项（可选）

```javascript
import {ref,watch} from 'vue'
const counter = ref(0)
watch(counter,(newValue,oldValue)=>{
  console.log('New value is:'+counter.value+' '+newValue)
})
```

示例：

```javascript
import {fetchUserRepositories} from '@/api/repositories'
import {ref,onMounted,watch,toRefs} from 'vue'
setup(props){
  const {user} = toRefs(props) // toRefs 创建对 props 的 user property 的响应式引用
  const repositories = ref([])
  const getUserRepositories = async ()=>{
    repositories.value = await fetchUserRepositores(user.value)
  }
  onMounted(getUserRepositories)
  watch(user,getUserRepositories)
  return {
    repositories,
    getUserRepositories
  }
}
```

setup 顶部的 toRefs 是确保侦听器根据 user prop的变化做出反应。

###### 独立的 computed 属性

使用从 Vue 导入的 computed 函数，在 Vue 组件外部创建计算属性。

```javascript
import {ref,computed} from 'vue'
const counter = ref(0)
const twiceTheCounter = computed(()=> counter.value*2)

counter.value++
console.log(counter.value) // 1
console.log(twiceTheCounter.value) // 2
```

computed 被传递了第一个参数，是类似 getter 的回调函数，输出只读的响应式引用。 为了访问该计算变量的 **value** ，需要使用 .value property 。

在 setup 中建立搜索功能。

```javascript
import {fetchUserRepositories} from '@/api/repositories'
import {ref,onMounted,watch,toRefs,computed} from 'vue'
setup(props){
  const {user} = toRefs(props)
  const getRepositories = async ()=>{
    repositories.value = await fetchUserRepositories(user.value)
  }
  onMounted(getRepositories)
  watch(user,getRepositories)
  const searchQuery = ref('')
  const repositoriesMatchSearchQuery = computed(()=>{
    return repositories.value.filter(repository =>{repositorey.name.include(searchQuery.value)})
  })
  return {
    repositories,
    getUserRepositories,
    searchQuery,
    repositoriesMatchingSearchQuery
  }
}
```

将上述代码提取到一个独立的 **组合式函数** 中。创建 useUserRepositories 函数

```javascript
// src/composable/useUserRepositories.js
import { fetchUserRepositories } from '@/api/repositories'
import { ref,onMounted,watch} from 'vue'
export default function useUserRepositories (user){
  const repositories = ref([])
  const getUseRepositories = async ()=>{
    repositories.value = await fetchUserRepositories(user.value)
  }
  onMounted(getUserRepositories)
  watch(user,getUserRepositories)
  return {
    repositoreis,
    getUserRepositories
  }
}
```

然后搜索：

```javascript
// src/composable/useRepositoryNameSearch.js
import { ref, computed} from 'vue'
export default function useRepositoryNameSearch (repositories) {
  const searchQuery = ref('')
  const repositoriesMatchingSearchQuery = computed(()=>{
    return repositories.filter((repository)=>{
      return repository.name.includes(searchQuery.value)
    })
  })
  return {
    searchQuery,
    repositoriesMatchingSearchQuery
  }
}
```

在组件中使用这两个单独的功能模块：

```javascript
// src/components/UserRepositories.vue
import useUserRepositories from '@/composables/useUseRepositories'
import useRepositoryNameSearch from '@/composables/useRepositoryNameSearch'
import {toRefs} from 'vue'
export default {
  components:{},
  props:{
    user:{
      type:String,
      required:true
    }
  },
  setup(props){
    const {user} = toRefs (props)
    const { repositories, getUserRepositories } = useUserRepositories(user)
    const { searchQuery, repositoriesMatchingSearchQuery } = useRepositoryNameSearch(repositories)
    return {
      repositories: repositoriesMatchingSearchQuery, // 返回的仓库是过滤后的，不关心未过滤的仓库？
      getUserRepositories,
      searchQuery
    }
  },
  data(){
    return {
      filters:{} // 过滤条件
    }
  },
  computed:{
    filteredRepositores(){}, // 当过滤条件改变时 更新过滤后的仓库？
  },
  methods:{
    updateFilters(){ // update filters
      
    }
  }
}
```

迁移过滤模块：

```javascript
// src/components/UserRepositories.vue
import useUserRepositories from '@/composables/useUseRepositories'
import useRepositoryNameSearch from '@/composables/useRepositoryNameSearch'
import useRepositoryFilter from '@/composables/useRepositoryFilter'
import {toRefs} from 'vue'
export default {
  components:{},
  props:{
    user:{
      type:String,
      required:true
    }
  }, 
  setup(props){
    const {user} = toRefs (props)
    const { repositories, getUserRepositories } = useUserRepositories(user)
    const { searchQuery, repositoriesMatchingSearchQuery } = useRepositoryNameSearch(repositories)
    const { filters,updateFilters,filteredRepositories } = useRepositoryFilter(repositoriesMatchingSearchQuery)
    return {
      repositories: filteredRepositories, // 返回的仓库是过滤后的，不关心未过滤的仓库？
      getUserRepositories,
      searchQuery,
      filters,
      updateFilters
    }
  }
}
```

该例子只是触及组合式 API 表面，了解更多，阅读深入指南。

#### 4.1.2 Setup

##### 1. 参数

setup 接受两个参数:

- props
- context

######  Props

setup 中 props 是响应式的，传入新 props 时，它将被更新。

```javascript
// Book.vue
export default {
  props:{
    title:String
  },
  setup(props){
    console.log(props.title)
  }
}
```

注意： props 是响应式的，使用 ES6 语法解构时，会消除 prop 的响应性。

使用 toRefs 函数，使 prop 解构。

```javascript
// Book.vue
import { toRefs } from 'vue'
setup(props){
	const { title } = toRefs ( props )
	console.log( title.value )
}
```

若 title 可选，没有传入 title 时。toRefs 则不会为 title 创建 ref。 需要使用 toRef 替代它。

```javascript
// Book.vue
import { toRef } from 'vue'
setup(props){
  const title = toRef(props,'title')
  console.log( title.value )
}
```

######  Context

第二个参数： context。context: 普通 JavaScript 对象。 作用：暴露其他可能在 setup 中有用的值。

```javascript
// Book.vue
export default {
  setup(props,context){
    console.log(context.attrs) // Attribute (非响应式对象，等同于$attrs)
    console.log(context.slots) // 插槽 (非响应式对象，等同于 $slots)
    console.log(context.emit) // 触发事件(方法,等同于 $emit)
    console.log(context.expose) // 暴露公共 property (函数)
  }
}
```

可以对 context 使用 ES6 解构。

```javascript
// Book.vue
export default {
  setup(props,{attrs,slots,emit,expose}){
    // ...
  }
}
```

attrs、slots: 有状态的对象，随组件本身的更新而更新。即应该避免对它们进行解构，应当始终以 attrs.x 或者 slots.x 方式引用 property。attrs、slots 的 property 是非响应式的。 若根据 attrs 或 slots 的更改应用副作用，则应该在 onBeforeUpdate 生命周期钩子执行操作。

##### 2. 访问组件的 property

执行 setup 时，组件实例还未创建，故只能访问以下 property

- props 
- attrs
- slots
- emit

##### 3. 结合模板使用

setup 返回对象时，该对象的 property 及传递给 setup 的 props 参数中的 property 就都可以在模板中访问到（后半句话什么意思？未使用 setup 时 props 的 property 依旧能访问到呀？）

```vue
// Book.vue
<template>
	<div>
    {{ collectionName}}--{{readersNumber}}--{{book.title}}
  </div>
</template>
<script>
	import { ref,reactive} from 'vue'
  export default {
    props:{
      collectionName:String
    },
    setup(props){
      const readersNumber = ref(0)
      const book = reactive({title:'Vue 3 Guide'})
      // 暴露出去
      return {
        readersNumber,
        book
      }
    }
  }
</script>
```

注意：setup 中返回的 refs 在模板中是被自动浅解包的，故不用使用 .value。

##### 4.  使用渲染函数

setup 可以返回渲染函数，该函数可以直接使用在同一作用域中声明的响应式状态。（同一作用域。。。指哪个作用域呢）

```javascript
// Book.vue
import { h,ref,reactive } from 'vue'
export default {
  setup(){
    const readersNumber = ref(0)
    const book = reactive({title:'Vue 3 Guide'})
    // 渲染函数中 需要显式使用 ref 的value
    return ()=> h('div',[readersNumber.value,book.title])
  }
}
```

返回渲染函数时将阻止返回其他任何东西。所以此时不能将该组件的方法通过模板 ref 暴露给父组件。 通过模板 ref 暴露给父组件？

使用 expose 解决这个问题。给 expose 传递对象，其中定义的 property 就可以被外部组件实例访问。 例子？

```javascript
import {h,ref} from 'vue'
export default {
  setup(props,{expose}){
    const count = ref(0)
    const increment = ()=> ++count.value
    expose({
      increment
    })
    return () => h('div',count.value)
  }
}
```

此时，increment 方法可以通过父组件的模板 ref 访问。 (此时父组件从能访问子组件所有的属性和方法变成了只能访问 increment 方法，即只能访问通过 expose 抛出去的属性)

##### 5. 使用this

setup() 内部，this 并不是该活跃实例的引用。setup 是在解析其他组件选项之前被调用的。所以 setup() 内部的 this 的行为与其他选项中的 this 完全不同。

#### 4.1.3 生命周期钩子

在生命周期钩子前加上 on 来访问组件的生命周期钩子。

| **选项式 API ** | **Hook inside setup** |
| --------------- | --------------------- |
| beforeCreate    | Not needed*           |
| created         | Not needed            |
| beforeMount     | onBeforeMount         |
| mounted         | onMounted             |
| beforeUpdate    | onBeforeUpdate        |
| updated         | onUpdated             |
| beforeUnmount   | onBeforeUnmount       |
| unmounted       | onUnmounted           |
| errorCaptured   | onErrorCaptured       |
| renderTracked   | onRenderTracked       |
| renderTriggered | onRenderTriggered     |
| actived         | onActived             |
| deactivated     | onDeactivated         |
|                 |                       |
|                 |                       |

**Tip**: setup 是围绕 beforeCreate 和 created 生命周期钩子运行的。在这些钩子中的代码都应该直接在 setup 中编写。

这些钩子函数接受一个回调函数，钩子被组件调用时将会被执行。

```javascript
// Book.vue
export default {
  setup(){
    // mounted
    onMounted(()=>{
      console.log('Component is mounted!')
    })
  }
}
```

#### 4.1.4 Provide / Inject

组合式 API 中能使用 provide / inject。但两者都只能在当前活动实例的 setup() 期间调用。

##### 1.  设想场景

MyMap 组件为 MyMarker 组件提供用户位置。

```vue
// MyMap.vue
<template>
	<MyMarker></MyMarker>
</template>
<script>
	import MyMarker from './MyMarker.vue'
  export default {
    components:{
      MyMarker
    },
    provide:{
      location:'North Pole',
      geolocation:{
        longitude:90,
        latitude:135
      }
    }
  }
</script>
```

```vue
// MyMarker.vue
<template>

</template>
<script>
	export default {
    inject:['location','geolocation']
  }
</script>
```

##### 2. 使用 Provide

provide 函数接受两个参数：

- name (String 类型)
- value

重构 MyMap 组件

```vue
// MyMap.vue
<template>
</template>
<script>
	import {provide} from 'vue'
  import MyMarker from './MyMarker.vue'
  export default {
    components:{
      MyMarker
    },
    setup(){
      provide('loaction','North Pole')
      provide('geolocation',{
        longitude:90,
        latitude:135
      })
    }
  }
</script>
```

##### 3. 使用 inject

子组件中使用 inject。

inject 函数接受两个参数：

- 要 inject 的 property 的name
- default value(可选)

```vue
<script>
	import { inject } from 'vue'
  export default {
    setup(){
      const userLocation = inject('location','The Universe')
      const userGeolocation = inject('geolocation')
      return {
        userLocation,
        userGeolocation
      }
    }
  }
</script>
```

##### 4. 响应性

###### 添加响应性 

添加响应性是什么意思呢？ 响应性？即祖先组件 provide 的项更改了值之后，后代组件 inject 的项会自动更新。

可以使用 ref 和 reactive 增加响应性。

```vue
// MyMap.vue
<template>
	<MyMarker></MyMarker>
</template>
<script>
	import {provide,reactive,ref} from 'vue'
  import MyMarker from './MyMaker.vue'
  export default {
    components:{
      MyMarker
    },
    setup(){
      const location = ref ('North Pole')
      const geolocation = reactive({
        longitude:90,
        latitude:135
      })
      provide('location',location)
      provide('geolocation',geolocation)
    }
  }
</script>
```

这两个 property 有任何修改时，MyMarker 组件也会自动更新！

###### 修改响应式 property

使用响应式 provide / inject 时，建议将修改限制在定义 provide 的组件内部。

例如，更改用户位置时，最好在 MyMap 中执行。

```vue
//  MyMap.vue
<template>
	<MyMarker></MyMarker>
</template>
<script>
  import {provide,reactive,ref} from  'vue'
  import MyMarker from './MyMarker.vue'
  export default {
    components:{
      MyMarker
    },
    setup(){
      const location = ref('North Pole')
      const geolocation = reactive({
        longitude:90,
        latitude:135
      })
      provide('location',location)
      provide('geolocation',geolocation)
      return {
        location
      }
    },
    methods:{
      updateLocation(){
        this.location = 'South Pole'
      }
    }
  }
</script>
```

假如要在被 inject 的组件中改变 provide 的值，应该怎么做呢？ 建议 provide 一个方法，使用此改变响应式 provide。

```vue
// MyMap.vue
<template>
	<MyMarker></MyMarker>
</template>
<script>
	import {provide,reactive,ref} from 'vue'
  import MyMarker from './MyMarker.vue'
  export default {
    components:{
      MyMarker
    },
    setup(){
      const location = ref('North Pole')
      const geolocation = reactive({
        longitude:90,
        latitude:135
      })
      const updateLocation = ()=>{
        location.value = 'South Pole'
      }
      provide('location',location)
      provide('geolocation',geolocation)
      provide('updateLocation',updateLocation)
    }
  }
</script>
```

```javascript
// 在 MyMarker.vue 组件中接收值
<script>
  import {inject} from 'vue'
	export default {
    setup(){
      const userLocation = inject('location','The Universe')
      const userGeolocation = inject('geolocation')
      const updateUserLocation = inject('updateLocation')
      return {
        userLocation,
        userGeolocation,
        updateUserLocation
      }
    }
  }
</script>
```

如果不允许通过子组件更改 provide 传递的值，可以对 provide 使用 readonly 函数。

```javascript
import { provide,reactive,ref,readonly} from 'vue'
setup(){
  const location = ref('North Pole')
  const geolocation = reactive({
    longitude:90,
    latitude:135
  })
  const updateLocation = ()=>{
    location.value = 'South Pole'
  }
  provide('location',readonly(location))
  provide('geolocation',readonly(geolocation))
  provide('updateLocation',updateLocation)
}
```

#### 4.1.5 模板引用

使用组合式 API 时，响应式引用和模板引用概念是一致的。 （啥意思呢。。？响应式引就是模板式引用，模板式引用也可以变成响应式引用？）为了获得对模板内元素或组件实例的引用，可以像往常一样声明 ref 并从 setup() 返回。

```vue
<template>
	<div ref='root'>
    This is a root element
  </div>
</template>
<script>
	import {ref,onMounted} from 'vue'
  export default {
    setup(){
      const root = ref(null)
      onMounted(()=>{
        // DOM元素将在初始渲染后 分配给 ref,
        console.log(root.value) // <div>This is a root element</div>
      })
      return {
        root
      }
    }
  }
</script>
```

此处，渲染上下文中暴露了 root，通过 ref='root' ，将其绑定到 div 作为其 ref。虚拟 DOM 补丁算法中，如果 VNode 的 ref 键对应渲染上下文中的 ref 。（渲染上下文是什么东西呢？）则 VNode 的相应元素或组件实例将被分配给该 ref 的值。这是在虚拟 DOM 挂载/打补丁过程中执行的，因此模板引用只会在初始渲染后获得赋值。

作为模板引用的 ref 的行为与其他 ref 一样，是响应式的，可以传递到（或从中返回）复合函数中。

##### 1.  JSX 中的用法

```javascript
export default {
  setup(){
    const root = ref(null)
    return ()=>{
      h('div',{ // with 渲染函数
        ref:root
      })
      
      // with JSX
      return ()=> <div ref={root}/>
    }
  }
}
```

##### 2. v-for 中的用法

##### 组合式 API 模板引用在 v-for 内部使用时没有特殊处理。相反，请使用函数引用执行自定义处理。（啥意思哇？）

```vue
<template>
	<div v-for="(item,i) in list" :ref="el=>{if(el) divs[i]=el}">
    {{item}}
  </div>
</template>
<script>
  import {ref,reactive,onBeforeUpdate} from 'vue'
	export default{
    setup(){
      const list = reactive([1,2,3])
      const divs = ref([])
      
      // 确保在每次更新之前重置ref
      onBefroeUpdate(()=>{
        divs.value=[]
      })
      return {
          list,
          divs
 			}
    }
  }
</script>
```

##### 3. 侦听模板引用

侦听模板引用的变更可以替代前面例子中演示使用的生命周期钩子。

但是有一个区别，wath() 和 watchEffect() 在 DOM 挂载或更新之前运行副作用，所以当侦听器运行时，模板引用还未被更新。

```vue
<template>
	<div ref='root'>
    This is a root element
  </div>
</template>
<script>
  import {ref,watchEffect} from 'vue'
	export default {
    setup(){
      const root = ref(null)
      watchEffect(()=>{ // 这个副作用在 DOM 更新之前运行，因此，模板引用还没有持有对元素的引用。
        console.log(root.value) // null
      })
      return {
        root
      }
    }
  }
</script>
```

侦听器应使用 flush:'post' 选项定义，这会在 DOM 更新后运行副作用，确保模板引用与 DOM 保持一致，并引用正确的元素。

```javascript
watchEffect(()=>{
  console.log(root.value) // <div> This is a root element </div>
},{
  flush:'post'
} )
```

#### 4.1.6 Mixin

##### 	1. 基础

Mixin : 一种灵活的方式，用来分发 Vue 组件中的可复用功能。mixin 对象可包含任意组件选项。

组件使用 mixin 对象时，所有该对象的选项将被混合进该组件本身的选项。

```javascript
// 定义 mixin 
const myMixin = {
  created(){
    this.hello()
  }
  methods:{
  	hello(){
      console.log('hello from mixin')
    }
	}
}

// 定义一个使用此 mixin 对象的应用
const app = Vue.createApp({
  mixins:[myMixin]
})
app.mount('#mixins-basic') // 'hello from mixin'
```

##### 2. 选项合并

组件和 mixin 对象有同名选项时，会以恰当的方式进行合并。

当 property 名冲突时，会以组件自身的数据优先。

```javascript
const myMixin = {
  data(){
    return {
      message:'mixin',
      foo:'abc'
    }
  }
}
const app = Vue.createApp({
  mixin:[myMixin],
  data(){
    return {
      message:'component',
      bar:'def'
    }
  },
  created(){
    console.log(this.$data) // {message:'component',foo:'abc',bar:'def'}
  }
})
```

同名钩子函数合并为一个数组，因此都将调用。此外，mixin 对象的钩子会在组件钩子调用之前调用。

```javascript
const myMixin = {
  created(){
    console.log('mixin created')
  }
}

const app = Vue.createApp({
  mixins:[myMixin],
  created(){
    console.log('com created')
  }
})
// mixin created
// com created
```

值为对象的选项（例如：methods/components/directives）,被合并为同一个对象。键名冲突时，组件对象键值对优先。

```javascript
const myMixin = {
  methods:{
    foo(){
      console.log('foo')
    }
    conflicting(){
  		console.log('from mixin')
		}
  }
}
const app = Vue.createApp({
  mixins:[myMixin],
  methods:{
    bar(){
      console.log('bar')
    }
    conflicting(){
  		console.log('from self')
		}
  }
})
const vm = app.mout('#mixins-basic')
vm.foo() // foo
vm.bar() // bar
vm.conflicting() // from self
```

##### 3. 全局 Mixin

可以为 Vue 应用程序全局应用 mixin:

```javascript
const app = Vue.createApp({
  myOption:'hello'
})
// 为自定义的选项 myOption 注入一个处理器
app.mixin({
  created(){
    const myOption = this.$options.myOption
    if(myOption){
      console.log(myOption)
    }
  }
})
app.mount('#mixins-global') // hello
```

Mixin 可以进行全局注册。一旦使用全局 mixin,它将影响每一个之后创建的组件。

```javascript
const app = Vue.createApp({
  myOption:'hello'
})
app.mixin({
  created(){
    const myOption = this.$options.myOption
    if(myOption){
      console.log(myOption)
    }
  }
})
// 将 myOption 也添加到子组件
app.component('test-component',{
  myOption:'hello from component'
})
app.mount('#mixins-global')
// hello
// hello from component
```

大多数情况下，只应当应用于自定义选项，就像上面的示例一样。推荐将其作为插件发布。

##### 4. 自定义选项合并策略 TODO

##### 5. 合并 TODO

#### 4.1.7 自定义指令

##### 1. 简介

