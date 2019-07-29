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
    <div class="app-wrapper">
        <div class="content">
            <level-header></level-header>
            <div class="section">
                <Menu class="menu"></Menu>
                <div class="content" :class="{'close':!isOpean}">
                    <div class="main-container" ref="mainContainer">
                        <div class="main-nav">
                            <TabsNav></TabsNav>
                        </div>
                        <AppMain class="main-content"></AppMain>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import Menu from './Menu';
import AppMain from './AppMain';
import LevelHeader from './LevelHeader';
import TabsNav from './TabsNav.vue';
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
        AppMain,
        TabsNav
    },
    methods: {

    },
    computed: {
        ...mapGetters([ //用于主体部分,计算margin-left
            'isOpean'
        ]),
        pageCode () { //不知道干嘛用的
            return this.$route.meta.componentUrl || this.$route.name;
        },
        funcNo () {//不知道干嘛用的 
            return this.$route.meta.no || this.$route.query.funcNo;
        }
    },
    mounted () {

    }
};
</script>
```

</details>

