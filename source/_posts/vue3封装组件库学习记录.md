---
title: vue3封装组件库学习记录
date: 2022-11-08 11:47:37
tags: vue3,封装组件
---

### 1.子组件添加v-bind="$attrs"即可将父组件中未通过props传递的属性全部注入绑定到子组件中
``` js
// 父组件
<m-menu
  :data="data1"
  background-color="red"
  :defaultActive="2"
  a="ccc"
></m-menu>
// 子组件
<el-menu
 :default-active="defaultActive"
 :router="router"
 v-bind="$attrs"
>
```

### 2.无限层级菜单：使用tsx实现（因为vue模板不太适合递归渲染生成）
可以增加自定义键名，将后台返回的数据的键名以props的方式传递给子组件以更自由地接收数据
```ts
const data = [{
  a:'a',
  b:'b',
  c:'c'
}];
```
``` js
<m-sub-comp :name="a" :icon="b" :children="c" />
```
子组件中可以直接用props.name等进行其他操作

### 3.城市选择组件，点击对应字母跳转至该字母开头的城市区域（滚动条定位的过程）可以使用`el.scrollIntoView()`方法
```ts
// 先给每个字母对应城市区域添加id
let el = document.getElementById();
if(el) el.scrollIntoView();
```

### 4.父组件给子组件绑定显示或隐藏的值
父组件中：
```vue
<m-son :v-model="visible" />
```
子组件中： 两次监听，一次监听父组件props的值以修改子组件的显示隐藏值，第二次监听子组件显示隐藏值 以分发事件给父组件，父组件以v-model的形式接收
``` js
watch(
  () => props.visible,
  (newVal) => {
    dialogVisible.value = newVal;
  }
);
watch(
  () => dialogVisible.value,
  (val) => {
    // console.log('')
    emits("update:visible", val);
  }
);
```

### 5.全局封装注册图标组件
``` js
import * as Icons from "@element-plus/icons-vue";
// Icons是包含所有图标组件的对象
for (const i in Icons) {
  if (Object.prototype.hasOwnProperty.call(Icons, i)) {
    app.component(`el-icon-${toLine(i)}`,(Icons as any)[i])
  }
}
```

### 6.全局注册所有自定义component组件
#### 6.1.在components文件夹下新增一个index.ts文件，将所有自定义组件引入并遍历向全局注入
```ts
const components:any[] = [
  menu,
];

export default {
  install(app: App) {
    components.map(component => {
      app.use(component)
    })
  }
}
```