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