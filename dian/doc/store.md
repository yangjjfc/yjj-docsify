## 状态管理(vuex)

> 一个项目中需要用到很多全局的数据,比如登入用户的信息,权限列表树,数据字典的数据等等,还有一些数据多个组件用到也可以在状态管理里面是实现。在我看来状态管理就是相当于一个全局变量，函数，可以让各个组件都能调用。（store,mutations,action）

> 由于一个项目中需要管理很多状态,所以项目中的状态管理 vuex 也是划分了多个 modules,分类去管理,这样也方便查看使用

## app-module(重点代码分析)

> SETTEMPNAVS 函数解释

- 该函数主要用于添加 nav 栏显示的状态的数据处理
- 在页面初始化的时候 会在`TabsNav`组件中调用改函数,进行 nav 的初始化 (嗯哼~,好像不需要也可以)

<details>
<summary>vue-router代码</summary>

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

- 该函数主要用于添加 nav 栏显示的状态的数据处理
- 在页面初始化的时候 会在`TabsNav`组件中调用改函数,进行 nav 的初始化 (嗯哼~,好像不需要也可以)

<details>
<summary>vue-router代码</summary>

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
REMOVENAV: (state, obj) => {
    let tabs = state.tempNavs;
    if (tabs.length === 1) {
        return;
    }
    let { closeName, targetName } = { closeName: state.activeNav, ...obj };
    // 算出下一个激活router name
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
    state.tempNavs = tabs.filter(tab => tab.name !== closeName);
    let cacheName = reverseComponentName(closeName);
    for (const i of state.cachedViews) {
        if (i === cacheName) {
            const index = state.cachedViews.indexOf(i);
            state.cachedViews.splice(index, 1);
            break;
        }
    }
}
```

</details>
