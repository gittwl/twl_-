### Tree组件

#### 使用方式

将树形结构测试数据data写入data.js

```js
export const data =  [{
  id: 1,
  label: '一级 1',
  children: [{
    id: 4,
    label: '二级 1-1',
    children: [{
      id: 10,
      label: '三级 1-1-1'
    }, {
      id: 11,
      label: '三级 1-1-2'
    }]
  },
  {
    id: 5,
    label: '二级 1-2',
  }
]
}, {
  id: 2,
  label: '一级 2',
  children: [{
    id: 6,
    label: '二级 2-1'
  }, {
    id: 7,
    label: '二级 2-2'
  }]
}, {
  id: 3,
  label: '一级 3',
  children: [{
    id: 8,
    label: '二级 3-1'
  }, {
    id: 9,
    label: '二级 3-2'
  }]
}]
```



在想要使用Tree组件的index.vue中

```vue
<template>
  <div >
      <Tree :data="data"/>
  </div>
</template>
<script>
import Tree from '../Tree/index.vue'
import {data} from './data.js'
export default{
  name: 'Index',
  data() {
    return {
      data:data
    }
  },
  components:{
    Tree
  }
}
</script>
<style scoped lang='less'>

</style>
```

