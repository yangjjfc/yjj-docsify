<!--
 * @Author: yangjj
 * @Date: 2019-08-13 09:02:12
 * @LastEditors: yangjj
 * @LastEditTime: 2019-08-17 16:12:37
 * @Description: file content
 -->
## 移动端适配
* 使用viewport做适配,并解决兼容问题
* 安装postcss-import : 主要功有是解决@import引入路径问题,可以让你很轻易的使用本地文件、node_modules或者web_modules的文件
* 安装postcss-url : 比如图片文件、字体文件等引用路径的处理。
* 安装autoprefixer : 添加前缀
* postcss-aspect-ratio-mini : 主要用来处理元素容器宽高比
* postcss-px-to-viewport : 主要用来把px单位转换为vw、vh、vmin或者vmax这样的视窗单位
* postcss-write-svg : 插件主要用来处理移动端1px的解决方案
* postcss-cssnext : 该插件可以让我们使用CSS未来的特性，其会对这些特性做相关的兼容性处理
* cssnano : 主要用来压缩和清理CSS代码,由于cssnext和cssnano都具有autoprefixer,事实上只需要一个，所以把默认的autoprefixer删除掉，然后把cssnano中的autoprefixer设置为false。
* postcss-viewport-units : 插件主要是给CSS的属性添加content的属性,解决兼容问题,自动插入content
```js
img {//解决postcss-viewport-units带来的副作用
    content: normal !important;
}
```

* vw有些机型不兼容,还是需要用过`viewport-units-buggyfill`
* `viewport-units-buggyfill.js`和`viewport-units-buggyfill.hacks.js`。你只需要在你的HTML文件中引入这两个文件
* 在HTML文件中调用viewport-units-buggyfill
```js
<script>
    window.onload = function () {
        window.viewportUnitsBuggyfill.init({
            hacks: window.viewportUnitsBuggyfillHacks
        });
    }
</script>
```

[如何在Vue项目中使用vw实现移动端适配](https://www.jianshu.com/p/1f1b23f8348f)
[使用Flexible实现手淘H5页面的终端适配rem自适应布局-移动端自适应必备](https://blog.csdn.net/qq_16664643/article/details/52267562)
[再聊移动端页面的适配](https://blog.csdn.net/qq_21729177/article/details/79466951)