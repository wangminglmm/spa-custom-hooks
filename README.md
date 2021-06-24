
<div align="center">
<img src="https://pubser-res.zhenai.com/other/temp/202105/24/16494052710198.png?imageMogr2/thumbnail/436x"/>

[![license](https://img.shields.io/badge/license-%20MIT-blue.svg)](https://github.com/1977474741/vue-custom-hooks/blob/main/LICENSE) [![license](https://img.shields.io/npm/v/vue-custom-hooks?color=red)](https://www.npmjs.com/package/vue-custom-hooks) [![license](https://img.shields.io/bundlephobia/min/vue-custom-hooks)](https://www.npmjs.com/package/vue-custom-hooks) [![license](https://img.shields.io/github/last-commit/1977474741/vue-custom-hooks)](https://github.com/1977474741/vue-custom-hooks/commits/main) [![license](https://img.shields.io/github/stars/1977474741?style=social)](https://github.com/1977474741)

**简体中文 | [English](./README.EN.md)**
</div>

- [vue-custom-hooks 是什么？](#head1)
- [ 它有什么用？](#head2)
- [ 常见应用场景](#head3)
- [ 使用示例](#head4)
- [ 注册参数说明](#head5)
    - [ 注册CustomHook](#head6)
    - [ diyHooks对象说明](#head7)
- [ 钩子使用规则](#head8)
- [ 已经内置的原生钩子](#head9)
- [ Demo二维码](#head10)
- [ 进群交流](#head11)

## <span id="head1">vue-custom-hooks 是什么？</span>
- 一个可以定制vue组件钩子的东西，你可以注册全局的异步任务，自己定义钩子的触发条件，满足条件时即可自动执行页面里相关的钩子。
- 支持和vue的原生钩子created，mounted等随意搭配使用。
- 支持传统h5、uni-app、wepy、mpvue（理论上支持所有基于vue的框架）。

## <span id="head2"> 它有什么用？</span>

用简单优雅的方式解决业务页面里需要同时监听多个全局状态的问题

## <span id="head3"> 常见应用场景</span>
````javascript
export default {
    name: 'Home',
    onCreatedLogin(){
        //登录成功（拿到token） && 页面初始化完成
        //Tips：适用于某页面发送的请求依赖token的场景
    },
    onCreatedUserInfo(){
        //页面初始化完成 && 获取用户信息完成
        //Tips：适用于页面初始化时需要用到用户信息去做判断再走页面逻辑的场景
    },
    onMountedUserInfo(){
        //dom渲染完成 && 获取用户信息完成
        //Tips：适用于首次进入页面需要在canvas上渲染头像的类似场景
    },
    onMountedShow(){
        //dom渲染完成 && 页面显示
        //Tips：适用于需要获取组件或者dom，并且每次页面显示都会执行的场景
    },
}
````

## <span id="head4"> 使用示例</span>
```javascript
//第一步，安装插件：
npm install vue-custom-hooks

//第二步，入口文件里注册插件：
import CustomHook from 'vue-custom-hooks';
Vue.use(CustomHook ,{
     'UserInfo':{
        name:'UserInfo',
        watchKey: 'userinfo',
        deep: true,
        onUpdate(val){
            //userinfo里含有nickName则表示命中此钩子
            return !!val.nickName;
        }
    }
},store)

//第三步，业务页面里使用插件（任何页面都可以使用，耦合度低，重复性代码少）：
onMountedUserInfo(){
    //可以渲染canvas了
    renderCanvas();
}
```

## <span id="head5"> 注册参数说明</span>
#### <span id="head6"> 注册CustomHook</span>
````javascript
import store from './store'
import CustomHook from 'vue-custom-hooks';
Vue.use(CustomHook,diyHooks,store)
````

#### <span id="head7"> diyHooks对象说明</span>
````javascript
{
    //1.注册属性监听钩子
    //UserInfo，钩子单名，首字母大写
    'UserInfo':{
        //name，钩子全称，监听属性的话可以和上面的key一致，必填
        name:'UserInfo',
        //watchKey要监听的store里的属性名（相当于$store.state.userinfo），属性监听钩子模式必填
        watchKey: 'userinfo',
        //是否默认命中,非必填
        hit: false,
        //deep是否深度监听，非必填
        deep: true,
        //onUpdate属性改变时执行的callback，用来决定是否要命中此钩子，非必填，缺省值相当于返回了!!val
        onUpdate(val){
            //这里表示userinfo里含有nickName则命中此钩子。注意不可以异步return
            return !!val.nickName;
        }
    },
    
    //2.注册事件监听钩子
    //BeforeMount，钩子单名，首字母大写
    'BeforeMount':{
        //name，原生钩子名，用来命中此钩子，必填
        name:'beforeMount',
        //destroy，相反的钩子名，用来取消命中，事件监听钩子必填
        destroy:'destroyed',
        //是否默认命中,非必填
        hit: false
    }
}
````

## <span id="head8"> 钩子使用规则</span>
````javascript
`on{UserInfo}{BeforeMount}{Login}{Position}...` //所有注册好的钩子都可以随意搭配，排列顺序不影响钩子执行，都是 && 的关系
````

## <span id="head9"> 已经注册好的原生钩子</span>
````javascript
Launch、Created、Load、Attached、Show、Mounted、Ready
//↓↓↓如需其他的钩子可自行注册↓↓↓（如果当前框架的某钩子和其对应的相反钩子跟如下配置不一致也需要手动注册，比如wepy有created但没有destroyed）
````
- [已注册的钩子详细配置](https://github.com/1977474741/vue-custom-hooks/blob/main/lib/vue-custom-hooks/hooks.js)
- [ diyHooks对象说明](#head7)

## <span id="head10"> Demo二维码</span>
![left image description here](https://pubser-res.zhenai.com/other/temp/202103/20/16460141027094.png?imageMogr2/thumbnail/200x200)

## <span id="head11"> 进群交流</span>
![left image description here](https://pubser-res.zhenai.com/other/temp/202103/20/17024414117439.png?imageMogr2/thumbnail/203x203)

如果有什么好的建议欢迎提issues或者提pr

喜欢的话点个star

