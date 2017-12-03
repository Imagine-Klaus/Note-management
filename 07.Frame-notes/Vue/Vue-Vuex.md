# Vuex 学习笔记

## 一、简单使用
### 1.1 创建一个store.js仓库文件
```js
    import Vue from 'vue'
    import Vuex from 'vuex'

    Vue.use(Vuex)
    
    //用于存储状
    //态
    const state = {
        count: 10
    }
    
    //在此定义改变状态中数据的方法
    const mutations = {
        add(state) {
            state.count++
        },
        reduce(state) {
            state.count--
        }
    }

    export default new Vuex.Store({
        state, mutations
    })
```

## 二、state访问状态对象
首先`import store from 'xx/store`

如果要访问仓库中的数据
    
- 1.Store.state.count (这样写比较麻烦)
- 2.`computed:{count(){return this.$store.state.count}}`
    + 然后`template`中`<span>{{count}}</span>`
- 3.引入mapState
```js
    <template>
        <div>
            <span>{{}}</span>
        </div>
    </template>
    <script type="text/javascript">
        import store from '../xx/store'
        import {mapState} from 'vuex'
        export default {
            store,
            //第一种用法
            computed:{
                count(){
                    return this.$store.state.count
                }
            }
            //第二种用法
            computed:mapState({
                count:state=>state.count
            })

            //第三种
            computed:mapState(['count'])
        }
    </script>
```

## 三、Mutations 修改状态
### 3.1 $store.commit
> Vuex提供了commit方法来修改状态

- $store.commit( '方法名' )
```js
    //在以上store文件中定义了修改mutations的方法

    //在组件中使用方式
    //
    $click="$store.commit( 'add' )"
```

### 3.2 传值
- 在mutations中**所需要传参的方法中再加一个参数**
```js
    mutations = {
        reduce(state, n){
            state.count -= n
        }
    }
```
- 在`commit`的时候传入参数:$store.commit( 'reduce', 2 )

### 3.3 模板获取Mutations中的方法
- `mapMutations(['add', 'reduce'])`
- 在使用的时候就可以直接`@click="add"`
    + 如果传参的话就跟平时一样`$click = add(2)`
```js
    methods:mapMutaions([
    ])
```
        'add', 'reduce'

## 四、getters计算过滤属性
> 可以当做store.js的过滤属性,**在获取数据前的再一次编辑**
- 首先在store文件中定义getter操作的对象
```js
    const getters = {
        count(state) {
            return state.count+=100
        }
    }
```
- 然后在组件中依然是写在computed中
    + 在构造器中,只能有一个computed属性,多个的话只有最后一个才会奏效,所以可以使用**扩展运算符将对象的可遍历属性拷贝进去**
```js
    computed:{
        count() {
            return this.$store.getters.count
        }
    }
```
### 4.1 使用更简洁的方式
- 组件中引入mapGetters
```js   
    import {mapGetters} from 'vuex'

    computed:{
        ...mapState(['count']),
        ...mapGetters(['count'])
    }
```

