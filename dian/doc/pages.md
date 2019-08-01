## 页面构成
> 管理系统页面,大致都相似,主要分为,头部,左侧菜单,中间页面展示三部分,重点主要都集中在`layout`组件中




## layout(重点代码分析)

> 页面说明

- 该页面是由多个组件组成的,主要作用是一个容器组件;
- 分为`level-header`头,`Menu`菜单,`TabsNav`导航栏,`AppMain`页面主体 这四个组件构成

<details>
<summary>代码</summary>

```js
<template>
    <div class="app-wrapper" :class="{hideSidebar:!isOpean}">
        <div class="content">
            <level-header></level-header>
            <div class="section">
                <Menu class="sidebar-container menu"></Menu>
                <div class="content" :class="{'close':!isOpean}">
                    <div class="main-container" ref="mainContainer">
                        <div class="main-nav">
                            <TabsNav></TabsNav>
                            <div class="main-tips">
                                <Prompt :isShow.sync="isPageTips">
                                    <div v-html="pageTips"></div>
                                </Prompt>
                            </div>
                        </div>
                        <div class="main-content">
                            <Dashboard></Dashboard>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Menu from './Menu'; //菜单
import Prompt from './Prompt'; //提示信息
import LevelHeader from './LevelHeader'; //header
import Dashboard from './Dashboard.vue'; //页面主体
import TabsNav from './TabsNav.vue'; //nav导航栏
import { mapGetters } from 'vuex';

export default {
    name: 'layout',
    data () {
        return {
            pageTips: '',
            isPageTips: false
        };
    },
    components: {
        Menu,
        LevelHeader,
        Dashboard,
        Prompt,
        TabsNav
    },
    methods: {
        getTips () {
            this.pageTips = this.$route.meta.pageTips || '暂无'; //页面辅助提示信息
        }
    },
    watch: {
        $route () {
            this.getTips();
        }
    },
    created () {
        this.getTips();
    },
    computed: {
        ...mapGetters([  //用于主体部分,计算margin-left
            'isOpean'
        ])
    }
};
</script>
```

</details>


## TabsNav(重点代码分析)

> 页面说明

- nav导航栏,只显示leval=2的数据;

<details>
<summary>代码</summary>

```js
<template>
    <div class="tabs-nav">
        <el-tabs v-model="activeName" type="card"  @tab-remove="removeTab" v-contextmenu:contextmenu>
            <el-tab-pane :key="item.name" v-for="item in tempNavs" :label="item.label" :name="item.name" :closable="item.closeable">
                <span slot="label"> <yl-icon class="tabs-tip" className="icon-chanpinzhongxin" v-if="item.meta.leval === 2"></yl-icon> {{item.label}}</span>
            </el-tab-pane>
        </el-tabs>
        <v-contextmenu ref="contextmenu">
            <v-contextmenu-item @click="removeAll()">关闭其他</v-contextmenu-item>
            <v-contextmenu-item @click="removeAll(true)">关闭全部</v-contextmenu-item>
        </v-contextmenu>
    </div>
</template>
<script>
import { mapGetters, mapMutations } from 'vuex';
import BASE from '@/utils/constant/index.js';
export default {
    data () {
        return {
            disabledTabs: ['/app/hello'],
            activeName: ''
        };
    },
    computed: {
        //tempNavs: navs数组
        //activeNav :高亮的名称
        ...mapGetters(['tempNavs', 'activeNav'])
    },
    watch: {
        activeNav (val) {
            this.activeName = val;
        },
        activeName (val) {
            let temp = this.tempNavs.filter(item => item.name === val)[0] || {};
            if (val && this.$route.name !== val) {
                this.$router.push({
                    name: val,
                    query: temp.query,
                    params: temp.params
                });
                this.SETACTIVENAV(val);
            }
        },
        $route () {
            this.addViewTags();
        }
    },
    methods: {
        ...mapMutations(['SETACTIVENAV', 'REMOVENAV', 'SETTEMPNAVS']),
        //关闭全部/关闭其他
        removeAll (isAll = false) {
            //关闭其他,排除自身和首页
            let otherMenu = this.tempNavs.filter(item => item.name === this.activeName || item.closeable === false); 
            //关闭全部
            let homeMenu = this.tempNavs.filter(item => item.closeable === false);
            if (isAll) {
                this.SETTEMPNAVS(homeMenu);
                this.$router.push(BASE.defaultUrl);
            } else {
                this.SETTEMPNAVS(otherMenu);
            }
        },
        //单个删除
        removeTab (val) {
            this.REMOVENAV({
                closeName: val
            });
        },
        //获取带name的router
        generateRoute () {
            if (this.$route.name) {
                return this.$route;
            }
            return false;
        },
        //添加nav
        addViewTags () {
            const route = this.generateRoute();
            if (!route) {
                return false;
            }
            let label = route.query.label || route.meta.name;
            this.SETTEMPNAVS({ name: route.name, label: label, query: route.query, params: route.params, meta: route.meta, closeable: !this.disabledTabs.includes(route.path) });//SETTEMPNAVS--->store
        }
    },
    mounted () {
        this.activeName = this.activeNav;
        this.addViewTags();
    }
};
</script>
```

</details>

## Menu

> 页面说明

- 获取了store中leftMenu的数据,循环遍历出左侧的菜单
- 菜单子级目录是右侧展开的模式
- 当鼠标hover到菜单时,触发mouseenter,获取数据,触发函数,获取当前菜单模块的高度,计算展开菜单显示的位置