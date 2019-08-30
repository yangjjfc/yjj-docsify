## 状态管理(vuex)

> 一个项目中需要用到很多全局的数据,比如登入用户的信息,权限列表树,数据字典的数据等等,还有一些数据多个组件用到也可以在状态管理里面是实现。在我看来状态管理就是相当于一个全局变量，函数，可以让各个组件都能调用。（store,mutations,action）

> 由于一个项目中需要管理很多状态,所以项目中的状态管理 vuex 也是划分了多个 modules,分类去管理,这样也方便查看使用

## app-module(重点代码分析)

> SETTEMPNAVS 函数解释

- 该函数主要用于添加 nav 栏显示的状态的数据处理
- 在页面初始化的时候 会在`TabsNav`组件中调用改函数,进行 nav 的初始化 (嗯哼~,好像不需要也可以)

<details>
<summary>SETTEMPNAVS代码</summary>

```js
 state: {
    activeNav: '', //激活name
    tempNavs: [], // 临时导航
    cachedViews: [], // 缓存Views
    auth: {},
    iconlist: [],
    leftMenu: []
}
// 添加或设置tab
let reverseComponentName = (str) => str.replace(/(\/|\.)/g, '');
SETTEMPNAVS: (state, item) => {
  if (item instanceof Array) {
    state.tempNavs = item; // 导航栏
    state.cachedViews = item.filter(data => data.meta.cache).map(data => reverseComponentName(data.name)); //缓存组件名称,去除'/''.'这种字符
    return;
  }
  let indexNo = 0, //当前选中的编号
    notInclude = true;
  state.tempNavs.forEach((data, index) => {
    data.name === state.activeNav && (indexNo = index); //当前选中的编号,添加tempNavs使用
    // 已包含
    if (data.name === item.name) { //当遇到相同的name,则进行合并,主要是详情页部分,url相同,但参数不同这些情况
      Object.assign(data, item);
      notInclude = false;
    }
  });
  if (notInclude) { //如果是新的导航栏,需要添加到当前选中栏的后面,indexNo + 1登录之后默认会跳转到首页,就会有个首页导航栏
    state.tempNavs.splice(indexNo + 1, 0, { ...item });
  }
  state.activeNav = item.name; //重新初始化激活的name
  let cacheName = reverseComponentName(item.name);//缓存组件名称,去除'/''.'这种字符
  if (item.meta.cache && !state.cachedViews.includes(cacheName)) {
    state.cachedViews.push(cacheName); //添加缓存组件名称,keep-alive组件使用
  }
};
```

</details>

> REMOVENAV 函数解释

- 该函数主要用于关闭nav页，`TabsNav.vue`主要在这个文件中使用
- 可用`this.removeNav`方法实现详情页跳转回列表页 this.removeNav({targetName:this.sourceName});
- 还有自动推算下个激活nav的功能

<details>
<summary>REMOVENAV代码</summary>

```js
 state: {
    activeNav: '', //激活name
    tempNavs: [], // 临时导航
    cachedViews: [], // 缓存Views
    auth: {},
    iconlist: [],
    leftMenu: []
}
// 添加或设置tab
let reverseComponentName = (str) => str.replace(/(\/|\.)/g, '');
//obj={
// targetName:'', 需要跳转的router nam
// closeName:'',  需要移除的router nam，默认上次激活的activeNav
// }
REMOVENAV: (state, obj) => {
    let tabs = state.tempNavs; //获取tempNavs保存的数据
    if (tabs.length === 1) { //只有一条时退出，保证首页不被删除
        return;
    }
    let { closeName, targetName } = { closeName: state.activeNav, ...obj };
    // 算出下一个激活router name
    //作用就是删除当前激活的tab时会做一个计算，把当前的下一个或前一个作为下个激活的tab
    if (closeName === state.activeNav && !targetName) {
        tabs.forEach((tab, index) => {
            if (tab.name === closeName) {
                let nextTab = tabs[index + 1] || tabs[index - 1];
                if (nextTab) {
                    targetName = nextTab.name;
                }
            }
        });
    }
    targetName && (state.activeNav = targetName);
    state.tempNavs = tabs.filter(tab => tab.name !== closeName); //过滤
    let cacheName = reverseComponentName(closeName);
    for (const i of state.cachedViews) { //去除当前页面的缓存
        if (i === cacheName) {
            const index = state.cachedViews.indexOf(i);
            state.cachedViews.splice(index, 1);
            break;
        }
    }
}
```
</details>



## user-module(重点代码分析)

> 该模块主要是用户数据的一些处理，比如用户的登录数据，token,login,logout的一些操作
* 这里说下登录,退出的一个流程
    1. 当用户打开网站到login页面是，会请求接口`currentUser`,得到临时的`token`
    2. 然后用户输入用户名,密码(处理加密),验证码,调用`login`接口
    3. login接口在store-->app中,当登入正常后,会把userInfo存到对应的store中
    4. 当用户退出时,也会调用`logout`接口,会清空所有的store数据,和sessionStorage中的数据
    5. 清空后会跳转到login页面,然后会做一次页面刷新,保证store数据全部清空


> currentUser 函数解释


- 用户登入保存数据
- 获取store数据都采用了`promise`方法,当有数据时直接`resolve`当前数据,没用时请求接口,然后commit去保存数据,在`resolve()`,异常就`reject`出去;

<details>
<summary>currentUser代码</summary>

```js
 state: {
     userInfo: { //用户信息
        avator: '',
        userName: ''
    }, // 用户信息
    menuList: null, // 用户权限菜单
    routers: null, // 路由
    buttons: null, // 按钮权限
    collectList: []// 员工常用菜单
}

//refresh是是否强制去请求,获取最新的数据,默认false
currentUser ({ commit, state }, refresh = false) {
    return new Promise((resolve, reject) => {
        //先判断是否有数据,有就resolve,没就请求
        state.userInfo && state.userInfo.enterpriseNo && !refresh
            ? resolve(state.userInfo) // 判断是否需要去请求
            : Http('currentUser', {
                token: state.userInfo ? state.userInfo.token : ''
            }).then(
                result => {
                // 获取token 获取登录信息
                    let user = result.data || {};
                    commit('CHANGEUSER', user);
                    resolve(user);
                },
                err => {
                    reject(err);
                }
            );
    });
}
```
</details>

##  permission-module
> 这个模块是权限菜单处理的,在权限篇里面基本都介绍过,就不展开了,具体看`权限篇`和实际的代码吧~

##  其他-module
> 会根据页面的需要,把需要多个页面共享的数据提取出来,放到store中,会建立单独的module;