# Vue中computed和watch的区别

**computed**
> computed是vue的计算属性，它会根据你所依赖的数据动态显示新的计算结果。计算的结果会被缓存，computed的值在getter执行后是会被缓存的，只有当它依赖的属性改变后，下次获取computed的值时才会重新调用对应的getter来计算。


1. 下面是一个比较经典的简单案例


```JS
<template>
  <div class="hello">
      {{fullName}} //computed中的函数无需括号即可调用
  </div>
</template>

<script>
export default {
    data() {
        return {
            firstName: '飞',
            lastName: "旋"
        }
    },
    props: {
      msg: String
    },
    computed: {
        fullName() {
            return this.firstName + ' ' + this.lastName
        }
    }
}
</script>
```


**注意**
>在Vue的 template模板内（{{}}）是可以写一些简单的js表达式的很便利，如上直接计算 {{this.firstName + ' ' + this.lastName}}，因为在模版中放入太多声明式的逻辑会让模板本身过重，尤其当在页面中使用大量复杂的逻辑表达式处理数据时，会对页面的可维护性造成很大的影响，而 computed 的设计初衷也正是用于解决此类问题。


**应用场景**
>适用于重新计算比较费时不用重复数据计算的环境。所有 getter 和 setter 的 this 上下文自动地绑定为 Vue 实例。如果一个数据依赖于其他数据，那么把这个数据设计为computed


**watch**

>watcher则是data的数据监听回调，当依赖的data的数据变化时，执行回调，在方法中会传入newVal和oldVal。可以提供输入值无效，提供中间值 特场景。Vue 实例将会在实例化时调用 $watch()，遍历 watch 对象的每一个属性。如果你需要在某个数据变化时做一些事情，使用watch。


1. 下面是一个比较经典的简单案例


```JS
import Vue from "vue/dist/vue.js";

Vue.config.productionTip = false;

new Vue({
  data(){
    return {
      firstName: '飞',
      lastName: "旋"
  }
  },
  props: {
    msg: String
  },
  computed: {
    fullName() {
      return this.firstName + ' ' + this.lastName
  }
},
watch: {
  firstName(newval,oldval) {
    console.log(newval)  //大
    console.log(oldval) //飞
  }
  },
  template: `
  <div class="app">
    {{fullName}}
  <button @click="setNameFun">click</button>
  </div>
  `,
  methods: {
    setNameFun() {
      this.firstName = "大";
      this.lastName = "熊"
  }
  }
}).$mount("#app");
```


总结： 
1.如果一个数据依赖于其他数据，那么把这个数据设计为computed的  

2.如果你需要在某个数据变化时做一些事情，使用watch来观察这个数据变化
