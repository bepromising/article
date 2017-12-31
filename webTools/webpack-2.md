> 第一篇讲了**Webpack**的安装，以及配置文件的 *entry*，*devtool*，*output*，*resolve*。这篇接着说配置文件的 **<code>module</code>**。
想看看第一篇的朋友可以点 **[这里][1]**。


----------


### **module** ([官方的文档][2])

```
  module: {
    rules: [
      { 
        test: /\.js$/, 
        loader: 'babel-loader' 
      }, { 
        test: /\.(png|jpg|gif|svg)$/,
        loader: 'file-loader',
        options: { name: '[name].[ext]?[hash]'}
      }
    ]
  }
```
1. **module**：模块受到影响的选项。我个人理解就是要打包的模块会受到它配置的影响而发生一些可控的变化。常见的就是里面的<code>rules</code>，主要说也是这一部分。

2. **rules**：顾名思义，定规则的，怎么定呢？看有 *test*，*正则表达式*，那么要打包的模块是要进行匹配啊，匹配做啥？匹配的模块就要按照<code>**Loader**</code>的规则进行变化了。

3. **loader**：通过使用不同的 *loader*，*webpack* 有能力调用外部的脚本或工具，实现对不同格式的文件的处理，比如说分析转换 *scss* 为 *css*，或者把下一代的JS文件（ES6，ES7)转换为现代浏览器兼容的JS文件。


### **babel-loader** （[官网地址][3]）
- 一个js的编译器，可将 *es6+* 转为*es5* ，从而在现有的浏览器运行。

- 它十分强大，可看 [阮一峰老师的babel教学][4]，到时我也会写一遍详细的 *babel* 使用文章。

```
  {
    test: /\.js$/,
    loader: 'babel-loader',
    exclude: /node_modules/
  }
``` 
1. 每一个规则都有 <code>test：正则表达式</code>，上图是匹配  ***.js*** 。
2. **exclude**：这规则不包含哪个模块的意思，值可以是 *正则*，也可以是 *包含路径的字符串数组*。有不包含当然也有include（包含）。
3. **babel** 有自己的配置文件 ***.babelrc***，*test* 匹配了，就执行配置文件（*.babelrc*）里的规则。

```
// .babelrc 的内容。
{
  "presets": [ // 设定转码规则
    "env",     
    "stage-2" 
    "stage-3" 
  ],
  "plugins": ["transform-runtime"]
}
```
1. *preset* 只是编译了语法，那新的API就可能没办法编译了，这时候需要 *plugins* 了。
    - **env**： 包含 *es2015*，*es2016*，*es2017* 的转码规则 <code>npm install babel-preset-env --save-dev</code>
    - **stage-2**： *es7* 第二版本的转码规则 <code>npm install babel-preset-stage-2 --save-dev</code>
    - **stage-3**: *es7* 第三版本的转码规则 <code>npm install babel-preset-stage-2 --save-dev</code>

2. *babel-polyfill* 虽然支持编译某些新的API，但还有不支持的API，那 *plugins* 提供了辅助的作用，*transform-runtime* 就是其中一个。

### **css-loader** 和 **style-loader**

- **css-loader**： 很多时候会用到 *require*，*import*，*@import* 导入 *css* ，那么就要用到 *css-loader* 进行转换。（[官方文档][5]）
- **style-loader**： 通过注入 *<style>* 标签将 *CSS* 添加到 *DOM*。（[官方文档][6]）
- 这两者一般都是配合在一起用的。

```
  {
    test: /\.css$/,
    use: [ 'style-loader', 'css-loader' ]
    // 也可以写成 loader: 'style!css' 或 loader: [ 'style-loader', 'css-loader' ]
  }
```

### **url-loader** （[官方文档][7]）

- 它的作用就类似 *file-loader*（下面会说），但是在文件大小<code>**低于**</code>指定的限制时（单位 *bytes*）可以返回一个以 *base64* 的方式引用。

```
  {
    test: /\.(png|jpg|gif)$/,
    use: [
      {
        loader: 'url-loader',
        options: { 
          limit: 8192, 
          name: 'images/[hash:8].[name].[ext]'
        }
      }
    ]
  }
```
1. ***use*** 里包含 *loader* 这种写法也可以，这样写是为了给每个 *loader* 都可以添加自身的 *options*。

2. ***options*** 是 *url-loader* 的自身配置，就像 *babel* 也有自己的配置一样。

3. ***limit***：代表图片打包限制，这个限制并不是说超过了就不能打包，而是指当图片大小**<code>小于</code>**限制时会自动转成 *base64* 码引用。上例中大于 *8192* 字节的图片正常打包，小于*8192* 字节的图片以 *base64* 的方式引用。

4. ***name***: 打包到指定目录下会生成 *images* 的文件夹，且原图的 *name*（名字）前带有 *8* 位 *hash* 值。 最后的 *.[ext]* 意思是原来什么扩展名就是什么扩展名。

5. 如果文件大小**<code>大于</code>**限制，将转为使用 *file-loader*，所有的查询参数也会透传过去。如果没有 *file-loader* 就肯定不会传过去咯。

### **file-loader** ([官方文档][8])

- 默认情况下，生成的文件的文件名是文件内容的 *MD5* 散列，具有所需资源的原始扩展名。

```
  {
    test: /\.(png|jpg|gif|svg)$/,
    loader: 'file-loader',
    options: {
      name: '[name].[ext]?[hash]'
    }
  }
```
1. ***name***： 这里的 *name* 跟上面一样的意思。不过它的 *hash* 放后面。
    例如：*file.png* => *file.png?e43b20c069c4a01867c31e98cbce33c9*。

2. ***[hash]***： 这里的是内容的哈希值，默认为十六进制编码的 md5。


### 最后说下

- ***loader*** 当然不止这些，我只是列举了一些常见的。例如 *vue* 和 *react* 的 *loader* 等等。
- 每个 *loader* 都有他们自身的 ***options*** ，我也是列举一些常见的。
- 同样 ***module*** 除了 *rules* 也有其他的属性。
- 上面都是常见，不常见的呢。作为程序猿，**要去读文档**，**要去读文档**，**要去读文档**！！！ 这是基本技能咧，哈哈，每个**官网**我都有贴的。


----------
#### [使用教程（三）--- plugins][9]


  [1]: https://segmentfault.com/a/1190000012334562
  [2]: http://webpack.github.io/docs/configuration.html#module
  [3]: https://babeljs.io/
  [4]: http://www.ruanyifeng.com/blog/2016/01/babel.html
  [5]: https://www.npmjs.com/package/css-loader
  [6]: https://github.com/webpack-contrib/style-loader
  [7]: https://www.npmjs.com/package/url-loader
  [8]: https://www.npmjs.com/package/file-loader
  [9]: https://segmentfault.com/a/1190000012367082