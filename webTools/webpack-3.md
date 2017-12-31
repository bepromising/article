> [使用教程（一）--- entry，devtool，output，resolve][1]
> [使用教程（二）--- module.loaders][2]
> **Webpack版本** *3.10.0*

## Plugins ([官方的文档][3])
- 上一篇讲到的 *loader* 是被用来转换某些类型的模块，它则可以用来做更广泛的任务。
- 如：模板编译输出，捆绑优化，定义类似环境变量等等。

```
plugins: [
    new webpack.DefinePlugin({
      'process.env': require('../config/dev.env')
    }),
    new webpack.HotModuleReplacementPlugin(),
    new HtmlWebpackPlugin({
      filename: 'index.html',
      template: 'index.html',
      inject: true
    }),
  ]
```

1. **plugins**: 数组形式包住一个个 *plugin* 实例。

2. 每个**插件实例**是一个具有 *apply* 属性的 *js* 对象 (<code>Function.prototype.apply</code>)。
    ```
  const webpack = require('webpack');
  const configuration = require('./webpack.config.js');
  let compiler = webpack(configuration);

  function HelloWorldPlugin (options) {
    // do something...
  }
  
  // this 指向了 compiler。
  HelloWorldPlugin.prototype.apply = function (compiler) {
    // compiler.plugin(),可看作绑定事件，打包时会触发事件。
    compiler.plugin('run', function() { 
        console.log('hello world!')
    });
  };
    ```
    
3. 每个插件都有自身 *options*（配置）。

4. 下面说下常见的 ***plugin*** 。

### webpack.DefinePlugin ([官方的文档][4])

- *webpack.xxxxPlugin* 这种插件是 *webpack* 的内置插件。

- *DefinePlugin*：它的作用是定义**全局常量**，是**常量**。即在模块用它定义的**全局常量**，那么你就不能改变它的值啦。用法例子：

    ```
    //webpack.config.js 
    Plugins: [
        new webpack.DefinePlugin({
            '_ABC_': false
        })
    ]
    
    // 在某个要打包的js模块里
    if （_ABC_）{
        console.log('_ABC_是 true，才看得见这输出');
    }
    ```
    
### webpack.HotModuleReplacementPlugin ([官方的文档][5])

- 它是热模块更换（HMR） 。在应用程序运行时交换，添加或删除**模块**，而无需完全重新加载。

- 它的options：

| 配置属性 | 数据类型 | 作用 |
| ------ | ------ | ----- |
| multiStep | Boolean | 如果 *true* 插件将分两步建立。首先编译常更新的模块，然后编译剩余的普通资源。|
| fullBuildTimeout | Number | *multiStep* 为 *true*，启用两步之间的延迟。|
| requestTimeout | Number | 下载某些东西的超时时间 |
<code>官方讲，这些 *options* 可能会被**弃用**</code> ，就 <code>new webpack.HotModuleReplacementPlugin()</code> 就可以了。

### webpack.NoEmitOnErrorsPlugin ([官方的文档][6])

- 它的作用：跳过编译时出错的代码并记录，使编译后运行时的包不会发生错误。

### webpack.optimize.DedupePlugin ([官方的文档][7])

- 项目中会有很多模块，有些模块之间存在相同的依赖，那么它的作用是把重复的依赖删除掉。
- 例如 A 模块和 B 模块都依赖*lodash.js*，那么就留在一个 *lodash.js* ，多余的删除。

### webpack.ProvidePlugin ([官方的文档][8])

- 会自动加载模块，不用 *import / require* 导入都可用。

```
Plugins: [
    $: 'jquery'
]
// 不用导入 jquery ，都可用 $。
// 可以理解为在全局就 import $ from 'jquery' 或 const $ = require('jquery')
```

### HtmlWebpackPlugin --第三方插件 （[官方的文档][9]）

- 根据你提供的 ***html* 模板** 生成 *html*。

- 因为是第三方插件，需要安装，导入使用。

```
// 安装
npm install --save-dev html-webpack-plugin

// 导入
const HtmlWebpackPlugin = require('html-webpack-plugin');

// 使用
 new HtmlWebpackPlugin({
  filename: 'index.html',
  template: 'index.html',
  inject: true
  ... // 其他配置，可看下面。
 })
```

1. **filename**: 生成的文件名。可以带路径。

2. **template**：*html* 模板路径。

3. **inject**：*'true'* | *'head'* | *'body'* | *'false'* 。
    - *true*：将所有资源（ *js*，*css* 等等）注入模板，*js* 资源在注入 *html* 里 *body* 底部。
    - *body*: 将所有 *js* 资源注入 *html* 里 *body* 底部。
    - *head*：将 *js* 资源放在  *html* 里 *head* 。
    
4. **hash**： *'true'* | *'false'*。 如果为 *true* ，*webpack* 打包后，所有 *js* 和 *css* 都会带有 *hash* 值。这对缓存清除非常有用。

5. **minify**： *{...}* | *'false'* 。
 *{...}*：通过 *[html-minifier][10] * 的选项对象，来减小打包后文件的大小。在生产环境一般用到。看看里面一般用到什么吧。
    - collapseWhitespace：*'true'* | *'false'* 。
     可以理解为如果 *true* ，则减少*html* 中节点与节点之间的空白区域。用于优化 *html* 。
    - *removeComments*： 如果 *true* ，则去掉 *html* 里的注释。
    - *removeAttributeQuotes*：如果 *true* ，则尽可能地去掉 *html* 里标签元素属性的引号，有些特定属性则不会。（ <code>请注意，HTML规范建议始终使用引号。谨慎使用这选项</code> ）
        ```
        <p id="abc" title="hello world"></P>
        转换为：
        <p id=abc title="hello world"></p>
        // 可见有些特定的属性是不会去掉引号的。
        ```
        
6. **chunksSortMode**：*'none' | 'auto' | 'dependency' |'manual' | {function} * 。
  作用：对注入 *html* 中的 *js* 模块进行排序。默认：*'auto'* (自动排序)  
    - *auto*： 默认，自动排序。 
    - *none*： 不排序 。
    - *dependency*： 按依赖排序。
    - *manual*： 手动排序。
    - *{function}*： 按你传入函数的功能进行排序。  
    
7. 以上是常见的，还有其他的，想了解可以看下[官方文档][11]哦。


---------

***plugins*** 就讲到这里了，以上都是一些常见常用的插件，喜欢的朋友可以点波赞，谢谢啦。

## [使用教程四][12]


  [1]: https://segmentfault.com/a/1190000012334562
  [2]: https://segmentfault.com/a/1190000012351195
  [3]: https://webpack.js.org/concepts/plugins/
  [4]: https://webpack.js.org/plugins/define-plugin/
  [5]: https://webpack.js.org/plugins/hot-module-replacement-plugin/
  [6]: https://webpack.js.org/plugins/no-emit-on-errors-plugin/
  [7]: https://webpack.github.io/docs/list-of-plugins.html#dedupeplugin
  [8]: https://webpack.js.org/plugins/provide-plugin/
  [9]: https://www.npmjs.com/package/html-webpack-plugin
  [10]: https://github.com/kangax/html-minifier#options-quick-reference
  [11]: https://www.npmjs.com/package/html-webpack-plugin
  [12]: https://segmentfault.com/a/1190000012383015