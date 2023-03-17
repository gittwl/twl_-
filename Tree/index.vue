<template>
  <!-- 可以绑定style  :style="cssVals"-->
  <div class="main">
    <div v-if="data" v-for="(item, index) in data">
      <div class="container">
        <span class="switch" @click="switch_($event, index)" v-if="item.children"></span>
        <span v-else style="width: 30px;"> </span>
        <input type="checkbox" ref="parent" @click="updateDisplay($event, index)" :id="item.id"><label :for="item.id">
          <span class="checkbox">
            <span class="check"></span>
          </span>
          {{ item.label }}</label>
      </div>
      <Tree v-if="item.children" ref="child" :data-index="index" :data="item.children" class="child" />
    </div>
  </div>
</template>
<script>
export default {
  name: 'Tree',
  props: {
    data: {
      type: Array
    }
  },
  data() {
    return {
      //使用传递父组件传递来的数据
      prop: this.data,
      currentAll: []
    }
  },
  methods: {
    //展开和关闭第一层子选项
    switch_(e, index) {
      const classLists = e.srcElement.classList
      const childsClassLists = this.$refs.child[index].$el.classList
      if (classLists.length > 1) {
        classLists.remove('open_close')
        childsClassLists.remove('child_open_close')
      } else {
        classLists.add('open_close')
        childsClassLists.add('child_open_close')
      }
    },
    //获取勾选选项实现联动Tree联动
    updateDisplay(e, index) {
      //当前节点联动子节点
      const current = e.target
      const currentParentI = this.$el.getAttribute('data-index')//子组件传入了自定义属性父节点的索引index，找到父节点
      if (this.prop[index].children) {
        let currentChilds = this.$refs.child[index].$el.querySelectorAll('input')//当前节点的所有
        for (const item of currentChilds) {
          if (current.checked) {
            item.checked = true
          } else {
            item.checked = false
          }
        }
      }
      //递归联动父节点-根节点
      this.getParents(this)
    },
    //递归获取当前当前节点的所有父级节点
    getParents(that) {
      if (that.$parent.prop) {
        const currentParentI = that.$el.getAttribute('data-index')//子组件传入了自定义属性父节点的索引index，找到父节点
        const currentAll = that.$el.querySelectorAll('input')//获取当前根节点的所有节点
        const currentParent =that.$parent.$refs.parent[currentParentI]
        if (this.$parent.prop) {
          const Arr = []
          for (const item of currentAll) {
            Arr.push(item.checked)
          }
          if (Arr.every(value => value === false)) {
            currentParent.checked = false
            currentParent.indeterminate = false;
          } else if (Arr.every(value => value === true)) {
            currentParent.checked = true
            currentParent.indeterminate = false;
          } else {
            currentParent.checked = false;
            currentParent.indeterminate = true;
          }
        }
        this.getParents(that.$parent)
      }
    }
  }
}
</script>
<style scoped lang='less'>
@import url('./reset.css');
.container {
  display: flex;
  align-items: center;
  // justify-content:center;
  font-size: 30px;
  padding: 30px;
  &:hover {
    background: #a5a5a5;
  }
  .switch {
    width: 30px;
    height: 30px;
    background-image: url('data:image/jpeg;base64,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');
    background-repeat: no-repeat;
    background-size: contain;
    background-position: center center;
  }
  .open_close {
    transform: rotate(90deg);
  }
}
.child {
  display: none;
  padding-left: 30px;
}
.child_open_close {
  display: block;
}
</style>