---
title: vue3简单上手
date: 2022-06-14 21:28:22
tags: vue3
categories:
- vue
---


### compostionApi
能够将同一个逻辑关注点相关代码收集在一起 == 函数式编程

``` js
export default {
  setup(props,context) {
    // 使用 `toRefs` 创建对 `props` 中的 `user` property 的响应式引用
    const { user } = toRefs(props)
    // ref
    const another = ref([]);
    //...reactive computed需要导入
    const state = reactive({
      a:0,
      b:computed(() => a * 2)
    });

    const getUserRepositories = async () => {
    repositories.value = await fetchUserRepositories(props.user)
  }
    // 生命周期的监听 注意create相关已移除，之前create中的代码在setup函数中直接写即可
    onMounted(getUserRepositories)；

    return {
      state,another
      // 在template中使用的时候 用state.a
    }
  }
}
```

主要作用可以把同一模块的代码合并到一个单独的js文件中，再引入进来，实现模块的隔离
单独的js文件 export default function func1 返回想要的变量或方法即可

``` js
// 拆分后的一部分示例模块
import { fetchUserRepositories } from '@/api/repositories'
import { ref, onMounted, watch } from 'vue'

export default function useUserRepositories(user) {
  const repositories = ref([])
  const getUserRepositories = async () => {
    repositories.value = await fetchUserRepositories(user.value)
  }

  onMounted(getUserRepositories)
  watch(user, getUserRepositories)

  return {
    repositories,
    getUserRepositories
  }
}
// 在主文件中import进来 在setup中 取出js文件return的值
const { repositories, getUserRepositories } = useUserRepositories(user)
```

### 计算属性
```ts
// 计算属性的函数中如果只传入一个回调函数，表示是get
setup() {
  const user = reactive({
    firstName:"f",
    lastName:"l"
  });
  // 返回的是一个ref类型的对象
  const fullName1 = computed(()=>{
    return  user.firstName+ '_' + user.lastName
  })
  // 如果是get set都有的话，传对象里面有两个方法
  const fullName2 = computed({
    get(){
      return  user.firstName+ '_' + user.lastName
    },
    set(val: string){
      const names = val.split("_");
      user.firstName = names[0];
      user.lastName = names[1];
    }
  })
}
```

### 监视属性watch
```ts
// 初始化的时候默认没有执行
watch(user, ({firstName,lastName})=>{
  fullName3 = firstName+"_"+lastName
}，{immediate:true,deep:true})
```
#### watchEffect
```ts
// 初始化的时候默认执行，不需要配置immediate
watchEffect(()=> {
  fullName3 = user.firstName+"_"+user.lastName
})
// 当使用watch监视非响应式数据的时候，需要改成箭头函数的形式如下
watch([()=>user.firstName,()=>user.lastName],()=>{})
```

### toRefs
toRefs可以把一个响应式对象转换成普通对象，该对象的每个peoperty都是一个ref
```ts
setup() {
  const state = reactive({
    name:'a',
    age:1
  });

  setInterval(()=> {
    toRefs(state).name.value += "=="//注意使用value
  })

  return {
    ...state//这样的话模板中直接使用name和age数据不会响应式变化
    ...toRefs(state)//这样传递出来的对象属性就都是响应式的了
  }
}
```

### shallowReactive和shallowRef
仅最外层属性是双向绑定的，不是深层监视（不常用）

### readonly和shallowReadonly
readonly是深度的只读，对象内所有属性都只读
shallowReadonly是浅只读的，对象中还包含对象，嵌套对象就无效了

### toRaw和markRaw 
toRaw把代理对象变成普通对象，数据变化，页面内容不变
markRaw标记的对象数据，从此以后都不能成为代理对象，就是不能变化了 （？）

### toRef
为源响应式 对象上的某个属性创建一个ref对象，二者内部操作的是同一个数据值，更新时二者是同步的
ref与toRef区别：拷贝了一份新的数据值单独操作，更新时互相不影响
应用：当要将某个prop的ref传递给复合函数时，toRef很有用（复合函数的参数要求是ref格式）

### customRef
```ts
function useDebouncedRef<T>(value: T,delay = 200) {
  let timeoutId: number;
  // track trigger固定写法
  return customRef((track,trigger) => {
    return {
      get() {
        track();
        return value;
      },
      set(newValue: T) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => {
          value = newValue
          trigger()
        }, delay);
      },
    }
  })
}
```