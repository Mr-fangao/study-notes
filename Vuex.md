## 第一步，了解Vuex

### 🤯 想象一个场景

如果你的项目里有很多页面（组件/视图），页面之间存在多级的嵌套关系，此时，这些页面假如都需要共享一个状态的时候，此时就会产生以下两个问题：

- 多个视图依赖同一个状态
- 来自不同视图的行为需要变更同一个状态

### 🤪 动动你的小脑袋你就会想到解决以上方法的方案:

- 对于第一个问题，假如是多级嵌套关系，你可以使用父子组件传参进行解决，虽有些麻烦，但好在可以解决；对于兄弟组件或者关系更复杂组件之间，就很难办了，虽然可以通过各种各样的办法解决，可实在很不优雅，而且等项目做大了，代码就会变成屎山，实在令人心烦。
- 对于第二个问题，你可以通过父子组件直接引用，或者通过事件来变更或者同步状态的多份拷贝，这种模式很脆弱，往往使得代码难以维护，而且同样会让代码变成屎山。

### 😇 此时，既然思考到了这里，如果换一种思路呢：

- 把各个组件都需要依赖的同一个状态抽取出来，在全局使用单例模式进行管理。
- 在这种模式下，任何组件都可以直接访问到这个状态，或者当状态发生改变时，所有的组件都获得更新。

### 👶 这时候，Vuex诞生了！

这就是 Vuex 背后的基本思想，借鉴了 Flux、Redux。与其他模式不同的是，Vuex 是专门为 Vue 设计的状态管理库，以利用 Vue.js 的细粒度数据响应机制来进行高效的状态更新。

### 😨 接着，你就会看到下面这张官网的vuex使用周期图（看不懂没关系）：

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d7b77fdcf29d410eb74f4872dd9c82fe~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

### 🤩 什么时候应该用vuex呢？

- 这个问题因人而异，如果你不需要开发大型的单页应用，此时你完全没有必要使用vuex，比如你的页面就两三个，使用vuex后增加的文件比你现在的页面还要多，那就没这个必要了。
- 假如你的项目达到了中大型应用的规模，此时您很可能会考虑如何更好地在组件外部管理状态，Vuex 将会成为自然而然的选择。

### 🤔 对于vuex的简单介绍就到这里，接下来，我们一起用起来吧！

## 第二步，安装

进入项目，在命令行中输入安装指令，回车

```
npm install vuex --save
```

然后配置 vuex，使其工作起来：在src路径下创建store文件夹，然后创建index.js文件，文件内容如下：

```js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
    state: {
        // 定义一个name，以供全局使用
        name: '张三',
        // 定义一个number，以供全局使用
        number: 0,
        // 定义一个list，以供全局使用
        list: [
            { id: 1, name: '111' },
            { id: 2, name: '222' },
            { id: 3, name: '333' },
        ]
    },
});

export default store;
复制代码
```

修改main.js：

```js
import Vue from 'vue';
import App from './App';
import router from './router';
import store from './store'; // 引入我们前面导出的store对象

Vue.config.productionTip = false;

new Vue({
    el: '#app',
    router,
    store, // 把store对象添加到vue实例上
    components: { App },
    template: '<App/>'
});
复制代码
```

最后修改App.vue：

```html
<template>
    <div></div>
</template>

<script>
    export default {
        mounted() {
            // 使用this.$store.state.XXX可以直接访问到仓库中的状态
            console.log(this.$store.state.name); 
        }
    }
</script>
复制代码
```

此时，启动项目`npm run dev`，即可在控制台输出刚才我们定义在store中的name的值。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/160e64c624e941bdb850c0ce175444e0~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

- 🤖 官方建议1：

官方建议我们以上操作this.$store.state.XXX最好放在计算属性中，当然，我也建议你这么使用，这样可以让你的代码看起来更优雅一些，就像这样：

```js
export default {
    mounted() {
        console.log(this.getName); 
    },
    computed:{
        getName() {
            return this.$store.state.name;
        }
    },
}
复制代码
```

此时可以得到和上面一样的效果。

- 🤖 官方建议2：

是不是每次都写this.$store.state.XXX让你感到厌烦，你实在不想写这个东西怎么办，当然有解决方案，就像下面这样：

```html
<script>
import { mapState } from 'vuex'; // 从vuex中导入mapState
export default {
    mounted() {
        console.log(this.name);
    },
    computed: {
        ...mapState(['name']), // 经过解构后，自动就添加到了计算属性中，此时就可以直接像访问计算属性一样访问它
    },
}
</script>
复制代码
```

你甚至可以在解构的时候给它赋别名，取外号，就像这样：

```js
...mapState({ aliasName: 'name' }),  // 赋别名的话，这里接收对象，而不是数组
复制代码
```

### 🤗 至此，安装vuex并且初始化的工作就结束了，此时你可以很轻易的在项目的任意地方访问到仓库里的状态

## 第三步，了解修饰器：Getter

当你看到这里的时候，证明你上一步已经完美的创建好一个vue项目，并且将vuex安装了进去！

好！接下来，我们介绍一个读取操作的 “修饰利器” ---Getter

- 🤨 设想一个场景，你已经将store中的name展示到页面上了，而且是很多页面都展示了，此时产品经理过来找事儿😡：
- 产品经理：所有的name前面都要加上“hello”！
- 我：为什么？
- 产品经理：我提需求还需要为什么吗？
- 我：好，我加！

这时候，你第一想到的是怎么加呢，emm...在每个页面上，使用this.$store.state.name获取到值之后，进行遍历，前面追加"hello"即可。

🤦🏻‍♂️ 错！这样很不好，原因如下：

- 第一，假如你在A、B、C三个页面都用到了name，那么你要在这A、B、C三个页面都修改一遍，多个页面你就要加很多遍这个方法，造成代码冗余，很不好；
- 第二，假如下次产品经理让你把 “hello” 改成 “fuck” 的时候，你又得把三个页面都改一遍，这时候你只能抽自己的脸了...

👏🏻 吸取上面的教训，你会有一个新的思路：我们可以直接在store中对name进行一些操作或者加工，从源头解决问题！那么具体应该怎么写呢？这时候，本次将要介绍的这个Getter利器闪亮登场！

🤡 怎么用呢？不废话，show code！

首先，在store对象中增加getters属性

```js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
    state: {
        name: '张三',
        number: 0,
        list: [
            { id: 1, name: '111' },
            { id: 2, name: '222' },
            { id: 3, name: '333' },
        ]
    },
    // 在store对象中增加getters属性
    getters: {
        getMessage(state) { // 获取修饰后的name，第一个参数state为必要参数，必须写在形参上
            return `hello${state.name}`;
        }
    },
});

export default store;
复制代码
```

在组件中使用：

```js
export default {
    mounted() {
        // 注意不是$store.state了，而是$store.getters
        console.log(this.$store.state.name);
        console.log(this.$store.getters.getMessage);
    }
}
复制代码
```

然后查看控制台：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c3e26092fc444753a0904e527af8ba47~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

没有问题的

🤖 官方建议： 是不是每次都写this.$store.getters.XXX让你感到厌烦，你实在不想写这个东西怎么办，当然有解决方案，官方建议我们可以使用mapGetters去解构到计算属性中，就像使用mapState一样，就可以直接使用this调用了，就像下面这样：

```html
<script>
import { mapState, mapGetters } from 'vuex';
export default {
    mounted() {
        console.log(this.name);
        console.log(this.getMessage);
    },
    computed: {
        ...mapState(['name']),
        ...mapGetters(['getMessage']),
    },
}
</script>
复制代码
```

此时可以得到和之前一样的效果。

当然，和mapState一样你也可以取别名，取外号，就像下面这样：

```js
...mapGetters({ aliasName: 'getMessage' }),  // 赋别名的话，这里接收对象，而不是数组
复制代码
```

🤗 OK，当你看到这里，你已经成功的把Getter用起来了，你也能明白在什么时候应该用到getters，你可以通过计算属性访问（缓存），也可以通过方法访问（不缓存），你甚至可以在getters的方法里面再调用getters方法，当然你也实现了像state那样，使用mapGetters解构到计算属性中，这样你就可以很方便的使用getters啦！

😎 读取值的操作我们有 “原生读（state）” 和 “修饰读（getters）”，接下来就要介绍怎么修改值了！

## 第四步，了解如何修改值：Mutation

🤗 OK！首先恭喜你看到了这里，至此，我们已经成功访问到了store里面的值，接下来我来介绍一下怎么修改state里面的值。

- 说到修改值，有的同学就会想到这样写：

```js
// 错误示范
this.$store.state.XXX = XXX;
复制代码
```

🤪 首先，这里我先明确的说明：这是错误的写法！这是错误的写法！这是错误的写法！

为什么上面是错误的写法？因为这个store仓库比较奇怪，你可以随便拿，但是你不能随便改，我举个例子：

🤔 假如你打开微信朋友圈，看到你的好友发了动态，但是动态里有个错别字，你要怎么办呢？你可以帮他改掉吗？当然不可以！我们只能通知他本人去修改，因为是别人的朋友圈，你是无权操作的，只有他自己才能操作，同理，在vuex中，我们不能直接修改仓库里的值，必须用vuex自带的方法去修改，这个时候，Mutation闪亮登场了！

😬 把问题解释清楚之后，我们准备完成一个效果：我们先输出state中的number的默认值0，然后我们在vue组件里通过提交Mutations改变number的默认值0，改成我们想修改的值，然后再输出出来，这样就可以简单练习怎么使用Mutations了。不说废话，上代码。

- 修改store/index.js

```js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
    state: {
        name: '张三',
        number: 0,
    },
    mutations: { // 增加nutations属性
        setNumber(state) {  // 增加一个mutations的方法，方法的作用是让num从0变成5，state是必须默认参数
            state.number = 5;
        }
    },
});

export default store;
复制代码
```

- 修改App.vue

```html
<script>
export default {
    mounted() {
        console.log(`旧值：${this.$store.state.number}`);
        this.$store.commit('setNumber');
        console.log(`新值：${this.$store.state.number}`);
    },
}
</script>
复制代码
```

- 运行项目，查看控制台：

![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ec40d56e76e349ad935ee50ef1211cd8~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

- 🤡 以上是简单实现mutations的方法，是没有传参的，如果我们想传不固定的参数怎么办？接下来教你解决
- 修改store/index.js

```js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
    state: {
        name: '张三',
        number: 0,
    },
    mutations: {
        setNumber(state) {
            state.number = 5;
        },
        setNumberIsWhat(state, number) { // 增加一个带参数的mutations方法
            state.number = number;
        },
    },
});

export default store;
复制代码
```

- 修改App.vue

```html
<script>
export default {
    mounted() {
        console.log(`旧值：${this.$store.state.number}`);
        this.$store.commit('setNumberIsWhat', 666);
        console.log(`新值：${this.$store.state.number}`);
    },
}
</script>
复制代码
```

- 运行项目，查看控制台：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0241f71bc47048178b80dff9cf32df8f~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

没有问题！

- 注意：上面的这种传参的方式虽然可以达到目的，但是并不推荐，官方建议传递一个对象进去，这样看起来更美观，对象的名字你可以随意命名，但我们一般命名为payload，代码如下：
- 修改store/index.js

```js
mutations: {
    setNumber(state) {
        state.number = 5;
    },
    setNumberIsWhat(state, payload) { // 增加一个带参数的mutations方法，并且官方建议payload为一个对象
        state.number = payload.number;
    },
},
复制代码
```

- 修改App.vue

```html
<script>
export default {
    mounted() {
        console.log(`旧值：${this.$store.state.number}`);
        this.$store.commit('setNumberIsWhat', { number: 666 });  // 调用的时候也需要传递一个对象
        console.log(`新值：${this.$store.state.number}`);
    },
}
</script>
复制代码
```

- 此时可以得到和之前一样的效果，并且代码更加美观！

```
😱 这里说一条重要原则：Mutations里面的函数必须是同步操作，不能包含异步操作！（别急，后面会讲到异步）
😱 这里说一条重要原则：Mutations里面的函数必须是同步操作，不能包含异步操作！（别急，后面会讲到异步）
😱 这里说一条重要原则：Mutations里面的函数必须是同步操作，不能包含异步操作！（别急，后面会讲到异步）
```

好的，记住这个重要原则，我们再说一个小技巧：

- 🤖 官方建议：就像最开始的mapState和mapGetters一样，我们在组件中可以使用mapMutations以代替this.$store.commit('XXX')，是不是很方便呢？

```html
<script>
import { mapMutations } from 'vuex';
export default {
    mounted() {
        this.setNumberIsWhat({ number: 999 });
    },
    methods: {   // 注意，mapMutations是解构到methods里面的，而不是计算属性了
        ...mapMutations(['setNumberIsWhat']),
    },
}
</script>
复制代码
```

- 此时可以得到和之前一样的效果，并且代码又美观了一点！
- 当然你也可以给它叫别名，取外号，就像这样：

```js
methods:{
    ...mapMutations({ setNumberIsAlias: 'setNumberIsWhat' }),   // 赋别名的话，这里接收对象，而不是数组
}
复制代码
```

- 🤔 OK，关于Mutation的介绍大致就是这样，另外你也掌握了在定义mutations方法的时候有无参数应该怎么写；并且听取了官方建议，使用mapMutations解构到你的组件内部的methods里，这样你就可以很简单的使用mutations方法啦！
- 🤪 上面提到，Mutations只能进行同步操作，所以，我们马上开始下一节，看看使用Actions进行异步操作的时候应该注意什么！

## 第五步，了解异步操作：Actions

😆 OK！本节我们来学习使用Actions，Actions存在的意义是假设你在修改state的时候有异步操作，vuex作者不希望你将异步操作放在Mutations中，所以就给你设置了一个区域，让你放异步操作，这就是Actions

😛 我们直接上一个代码

- 修改store/index.js

```js
const store = new Vuex.Store({
    state: {
        name: '张三',
        number: 0,
    },
    mutations: {
        setNumberIsWhat(state, payload) {
            state.number = payload.number;
        },
    },
    actions: {   // 增加actions属性
        setNum(content) {  // 增加setNum方法，默认第一个参数是content，其值是复制的一份store
            return new Promise(resolve => {  // 我们模拟一个异步操作，1秒后修改number为888
                setTimeout(() => {
                    content.commit('setNumberIsWhat', { number: 888 });
                    resolve();
                }, 1000);
            });
        }
    }
});
复制代码
```

- 修改App.vue

```js
async mounted() {
    console.log(`旧值：${this.$store.state.number}`);
    await this.$store.dispatch('setNum');
    console.log(`新值：${this.$store.state.number}`);
},
复制代码
```

- 运行项目，查看控制台：

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d76d3befa96146fd8615d61edc134c92~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

🤓 看了例子，是不是明白了，action就是去提交mutation的，什么异步操作都在action中消化了，最后再去提交mutation的。

😼 当然，你可以模仿mutation进行传参，就像下面这样：

- 修改store/index.js

```js
actions: {
    setNum(content, payload) {   // 增加payload参数
        return new Promise(resolve => {
            setTimeout(() => {
                content.commit('setNumberIsWhat', { number: payload.number });
                resolve();
            }, 1000);
        });
    },
}
复制代码
```

- 修改App.vue

```js
async mounted() {
    console.log(`旧值：${this.$store.state.number}`);
    await this.$store.dispatch('setNum', { number: 611 });
    console.log(`新值：${this.$store.state.number}`);
},
复制代码
```

- 运行项目，查看控制台

![img](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/106313d086c042628651e5f593be35c2~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

没有任何问题！

- 🤖 官方建议1：你如果不想一直使用this.$store.dispatch('XXX')这样的写法调用action，你可以采用mapActions的方式，把相关的actions解构到methods中，用this直接调用：

```html
<script>
import { mapActions } from 'vuex';
export default {
    methods: {
        ...mapActions(['setNum']),   // 就像这样，解构到methods中
    },
    async mounted() {
        await this.setNum({ number: 123 });  // 直接这样调用即可
    },
}
</script>
复制代码
```

当然，你也可以取别名，取外号，就像下面这样：

```js
...mapActions({ setNumAlias: 'setNum' }),   // 赋别名的话，这里接收对象，而不是数组
复制代码
```

- 🤖 官方建议2：在store/index.js中的actions里面，方法的形参可以直接将commit解构出来，这样可以方便后续操作：

```js
actions: {
    setNum({ commit }) {   // 直接将content结构掉，解构出commit，下面就可以直接调用了
        return new Promise(resolve => {
            setTimeout(() => {
                commit('XXXX');  // 直接调用
                resolve();
            }, 1000);
        });
    },
},
复制代码
```

- 🤠 OK，看到这里，你应该明白action在vuex的位置了吧，什么时候该用action，什么时候不用它，你肯定有了自己的判断，最主要的判断条件就是我要做的操作是不是异步，这也是action存在的本质。当然，你不要将action和mutation混为一谈，action其实就是mutation的上一级，在action这里处理完异步的一些操作后，后面的修改state就交给mutation去做了。

## 第六步，总结

🤗 好！大致对vuex的讲解就到这里了，看到这里你肯定对vuex不陌生了，你会安装它，配置它，读取state的值，甚至修饰读(Getter)，然后你会修改里面的值了(Mutation)，假如你有异步操作并且需要修改state，那你就要使用Action，这样，你就可以在你的项目中用起来vuex啦！加油吧！🤔


作者：三年没洗澡
链接：https://juejin.cn/post/6928468842377117709
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。