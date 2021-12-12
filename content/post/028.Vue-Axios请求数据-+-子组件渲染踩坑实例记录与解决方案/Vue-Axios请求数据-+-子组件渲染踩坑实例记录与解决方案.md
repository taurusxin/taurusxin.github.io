---
title: "Vue Axios请求数据 + 子组件渲染踩坑实例记录与解决方案"
categories: [ "程序人生","前端技术" ]
tags: [  ]
draft: false
slug: "vue-issue-1"
date: "2020-11-09 09:37:00"
---

今日用 Vue 做前端学习时，踩到了一个大多数人都踩过的坑，特此记录。
代码如下：

**parent.vue**
```html
<template>
    <div>
        <hello :data="message"></hello>
    </div>
</template>

<script>
export default {
    components: {
        Hello
    },
    data() {
        return {
            message: ''
        }
    },
    created() {
        axios.get('/static/data.json').then((response) => {
            this.message = response.data.message
        })
    }
}
</script>
```
**child.vue**
```html
<template>
    <div>
        <h1>{{ message }}</h1>
    </div>
</template>

<script>
export default {
    name: 'Hello',
    props: ['message'],
    created: {
        console.log(this.message)
    }
}
</script>
```
乍一看没啥问题，父组件created钩子进行axios请求，子组件拿到时在控制台输出数据。
但是控制台却报错了，message undefined
查了半天，生命周期也查过了，没有问题。
原因十分简单，就是父组件在进行axios请求时，子组件还没有拿到message数据
所以在进行console.log的时候 数据自然不能输出
报错 undefined
当然，在子组件的视图中，能够正常显示。
因为数据请求十分快速，又因为是Vue的双向数据绑定，所以更新渲染视图那一步就容易被忽略。

## 总结
异步编程很容易踩坑，尤其是前端的做 XHR 请求的时候，十分容易没拿到数据就急着往视图上面渲染。
解决方案可以在视图上加一个v-if

**child.vue**
```html
    <h1 v-if="message.length != 0">{{ message }}</h1>
```
第二个解决方案就是用 Vuex 管理全局状态。

**vuex / index.js**
```js
import Vue from 'vue'
import Vuex from 'vuex'
import axios from 'axios'
Vue.use(Vuex) 
export default new Vuex.Store({
    state: {  
        message: ''
    },
    mutations: {
        changeData(data) {
            this.message = data
        }
    },
    actions: {
        getDataAction({ commit }) {
            axios.get('/static/data.json')
                .then((res) => {
                    commit('changeData', res.data)
                })
                .catch((error) => {
                    console.log(error)
                })

        }
    }
})
```
**child.vue**
```html
<template>
  <div>
    <h1 v-if="this.message.length != 0">{{ message }}</h1>
  </div>
</template>

<script>
    import {mapState, mapMutations} from 'vuex'
    export default {
        created(){  
            this.$store.dispatch('getDataAction') // 实例创建时调用异步方法
        },
        computed:{
            ...mapState({
                message: ''
            })
        },
        methods:{
            changeData(data){
              this.$store.commit('changeData',data)
            }
        }
    }
</script>
```