
# 为什么用webpack



目前这个项目中，各个js，css，img到处乱飞，根本没有章法，得不到妥善的处理，虽然已经有了一些规范，如Commonjs，AMD，但是在我现在这个项目里根本什么都没有用到。。。后端程序员决定一切，我已吐血近亡。

Commonjs，一种同步机制，大概用法如下：
```
var commonModule = require("commonModuleName");
// some code here...
module.export = otherModule;
```

AMD，异步加载模块的机制，大概用法如下：
```
define(["amdModule"], function(amd){
// some code here...
return otherModule;
})
```

ES2015(ES6)也有自己的模块加载机制，ES6的模块机制是官方规定的，然后我们可以用babel(ES6 to ES5的编译器)来使用那些新的特性，上面的提到的模块机制的写法，在es6里大概长下面这个样子
```
import {someModule} from "someModule";
// some code here...
exports otherModule;
```

### es6的一些入门文档 


阮一峰老师的[ECMAScript6入门](http://es6.ruanyifeng.com/#docs/class)
babel相关文档[Learn ES2015](https://babeljs.io/docs/learn-es2015/#modules)

## 功能特性
- 支持CommonJs和AMD模块，意思也就是我们基本可以无痛迁移旧项目。
- 支持模块加载器和插件机制，可对模块灵活定制。特别是我最爱的babel-loader，有效支持ES6。
- 可以通过配置，打包成多个文件。有效利用浏览器的缓存功能提升性能。
- 将样式文件和图片等静态资源也可视为模块进行打包。配合loader加载器，可以支持sass，less等CSS预处理器。
- 内置有source map，即使打包在一起依旧方便调试。


# 开始使用

新建webpack-start项目，接下来会在这个目录里进行相应的学习。

```
npm init // 初始化文件
```
然后一些目录文件

```
webpack-start
	--- src
        --- entry.js
        --- module1.js
    --- index.html
    --- package.json
    --- webpack.config.js
```

## 安装webpack
通过npm将webpack安装到全局
```
npm install webpack -g 
```

## 一个最简的webpack
### webpack配置
webpack是需要进行配置的，我们在使用webpack的时候，会默认`webpack.config.js`为我们的配置文件

```
// webpack.config.js
var path = require("path");

module.exports = {
    entry: './src/entry.js', //演示单入口文件
    output: {
        path: path.join(__dirname, 'out'),  //打包输出的路径
        filename: 'bundle.js',              //打包后的名字
        publicPath: "./out/"                //html引用路径，在这里是本地地址。
    }
};
```

## 编写入口文件
接下来就编写我们的入口文件`entry.js`和第一个模块文件`module1.js`。我们一切从简，里面只用来加载一个Js模块。
```
// entry.js
require("./module1"); // 使用CommonJs来加载模块
```
```
// module1.js
console.log("Hello Webpack!");
```

### 启动webpack

一切准备好后，我们仅需要在项目根目录下，用命令行webpack执行一下即可。
```
// webpack 命令行的几种基本命令

$ webpack // 最基本的启动webpack方法
$ webpack -w // 提供watch方法，实时进行打包更新
$ webpack -p // 对打包后的文件进行压缩，提供production
$ webpack -d // 提供source map，方便调试。
```
webpack成功运行后，我们就可以看到根目录出现了out文件夹，里面有我们打包生成的bundle.js。我们最后通过在index.html里对这个文件引入就可以了。我们可以在控制台看到我们想要的结果，**Hello Webpack !**

###多模块依赖

测试其他两种模块加载机制。
```
// 修改module1.js
require(["./module3"], function(){
    console.log("Hello Webpack!");
});
```
```
// module2.js，使用的是CommonJs机制导出包
module.exports = function(a, b){
    return a + b;
}
```
```
// module3.js，使用AMD模块机制
define(['./module2.js'], function(sum){
    return console.log("1 + 2 = " + sum(1, 2));
})
```

以上仅作为展示，实际使用过程中最好使用es6模块机制。

## loader加载器
到了我最喜欢也是最激动人心的功能了！我们先想想应用场景，前端社区有许多预处理器供我们使用。我们可以使用这些预处理器做一些强大的事情，大家都听过的就是`CoffeeScript`和`Sass`了。我们以前要编译这些预处理器，就是用`gulp`进行编译。但是我们对这些文件处理其实也挺繁琐的，webpack可以一次性解决！

在这里我们用Sass和babel编译ES2015为例子，看一下loader是如何使用的。
### 安装loader	
我们第一步就是先要安装好各个必须的loader，我们直接看看需要通过npm安装什么。
```
npm install style-loader css-loader url-loader babel-loader sass-loader file-loader --save-dev
```
### 配置loader
安装完各个loader后，我们就需要配置一下我们的webpack.config.js，载入我们的loader。

```
// webpack.config.js
var path = require("path");
module.exports = {
    entry: path.join(__dirname, 'src/entry.js'),
    output: {
        path: path.join(__dirname, 'out'),
        publicPath: "./out/",
        filename: 'bundle.js'
    },
    // 新添加的module属性
    module: {
        loaders: [
            {test: /\.js$/, loader: "babel"},
            {test: /\.css$/, loader: "style!css"},
            {test: /\.(jpg|png)$/, loader: "url?limit=8192"},
            {test: /\.scss$/, loader: "style!css!sass"}
        ]
    }
};
```
我们主要看看module的loaders。loaders是一个数组，里面的每一个对象都用正则表达式，对应着一种配对方案。比如匹配到js后缀名就用babel-loader，匹配到scss后缀名的就先用sass，再用css，最后用style处理，不同的处理器通过!分隔并串联起来。这里的loader是可以省略掉`-loader`这样的，也就是原本应该写成`style-loader!css-loader!sass-loader`，当然我们必须惜字如金，所以都去掉后面的东东。

我们仅仅是配置一下，已经是可以直接用ES2015和SASS去写我们的前端代码了。在此之前，我们对src文件夹里再细分成js，css，image三个文件夹，处理好分层。话不多说，赶紧试试。

###稍微复杂的webpack项目

####bebel-loader
```
// js/es6-module.js
class People{
    constructor(name){
        this.name = name;
    }
    sayHi(){
        console.log(`hi ${this.name} !`);
    }
}
module.exports = People;
```
写好模块后，我们直接在entry.js入口文件中引入该模块。
```
// entry.js

// javascript
require('./js/module1');
let People = require('./js/es6-module');
let p = new People("Yika");
p.sayHi();

// css
require('./css/main.scss');
```

哈哈哈，不能再爽！这下子我们可以使用很多优秀的ES6特性去构建大型的web了。
###sass-loader

大家或许注意到了下方的css的require，那就是用来加载Sass样式的。我们通过启动style-loader会将css代码转化到`<style>`标签内，我们看一下里面的内容。
```
// css/main.scss
html, body{
    background: #dfdfdf;
}
```
最后我们打开index.html观察我们所有的结果，首先背景已经是淡灰色的，并且控制台也有我们想要的内容。我们通过查看DOM结构，可以发现head标签里多出了style标签，里面正是我们想要定制的样式。

###关于对图片的打包

我们之前也说，webpack对与静态资源来说，也是看作模块来加载的。CSS我们是已经看过了，那图片是怎么作为模块打包加载进来呢？这里我们可以想到，图片是用url-loader加载的。我们在css文件里的url属性，其实就是一种封装处理过require操作。当然还有一种方式就是直接对元素的src属性进行require赋值。
```
div.img{
    background: url(../image/xxx.jpg)
}

//或者
var img = document.createElement("img");
img.src = require("../image/xxx.jpg");
document.body.appendChild(img);
```

上述两种方法都会对符合要求的图片进行处理。而要求就是在url-loader后面通过query参数的方式实现的，这里就是说只有不大于8kb的图片才会打包处理成Base64的图片。关于query，请看文档：[Query parameters](http://webpack.github.io/docs/using-loaders.html#query-parameters)
```
{test: /\.(jpg|png)$/, loader: "url?limit=8192"}
```

###打包成多个资源文件

我们在开发多页面的站点的时候，还是需要希望能有多个资源文件的。这样我们就可以有效利用缓存提升性能，做到文件按需加载。如何写入口文件，这里就不再赘述了，我们直接看如何对webpack.config.js进行修改。
```
// webpack.config.js
entry: {
    page1: "entry.js",
    page2: "entry2.js"
},
output: {
    path: path.join(__dirname, 'out'),
    publicPath: "./out/",
    filename: '[name].js'
}
```

这里重点关注两个地方，entry属性可以是一个对象，而对象名也就是key会作为下面output的filename属性的`[name]`。当然entry也可以是一个数组，更多用法都可以去webpack的[官方文档](https://webpack.github.io/docs/)进行查看。

当然webpack也考虑到公共模块的利用，我们利用插件就可以智能提取公共部分，以提供我们浏览器的缓存复用。我们只需要在`webpack.config.js`添加下面的代码即可。
```
// 修改添加，webpack.config.js
var webpack = require('webpack');
module.exports = {
    // ....省略各种代码
        plugins: [
            new webpack.optimize.CommonsChunkPlugin('common.js')
        ]
}
```
我们做个小测试，让第二个入口文件也加载我们之前的es6-module.js。然后我们用webpack进行打包，就发现生成的common.js里是有相应代码的。我们需要手动在html上去加载common.js，并且是必须要最先加载。

###独立出css样式

如果我们希望样式通过`<link>`引入，而不是放在`<style>`标签内呢，即使这样做会多一个请求。这个时候我们就要配合插件一起使用啦，我们一起来看看。
```
$ npm install extract-text-webpack-plugin --save-dev
```
安装完插件就要配置`webpack.config.js`了。我们添加以下代码
```
var ExtractTextPlugin = require("extract-text-webpack-plugin");
module.exports = {
    // ...省略各种代码
    module: {
        loaders: [
            {test: /\.js$/, loader: "babel"},
            {test: /\.css$/, loader: ExtractTextPlugin.extract("style-loader", "css-loader")},
            {test: /\.(jpg|png|svg)$/, loader: "url?limit=8192"},
            {test: /\.scss$/, loader: "style!css!sass"}
        ]
    },
    plugins: [
        new webpack.optimize.CommonsChunkPlugin('common.js'),
        new ExtractTextPlugin("[name].css")
    ]
}
```

为了区分开用`<link>`链接和用`<style>`，我们这里以CSS后缀结尾的模块用插件。我们重点关注一下使用了ExtractTextPlugin的模块，在ExtractTextPlugin的extract方法有两个参数，第一个参数是经过编译后通过style-loader单独提取出文件来，而第二个参数就是用来编译代码的loader。

当然，插件也支持所有独立样式打包成一个css文件。增加多一个参数即可。
```
new ExtractTextPlugin("style.css", {allChunks: true})
```
至于怎样加载样式是最佳实践，这个就要自己平时多思考了。多站点多样式的时候，是做到一次性打包加载呢，还是按需加载呢？我这里就建议一项，主页尽量做到最精简，毕竟决定用户存留时间。
> 原文 from [here](http://html-js.com/article/3009)
> Written with [StackEdit](https://stackedit.io/).
