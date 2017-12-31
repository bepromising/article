>    **Webpack**是什么,我就不过多介绍了，大家都有耳闻，不过还是配张图让大家体会下。
>    ![图片描述][1]  

 **我的webpack版本是 *3.10.0*** 


- 安装Webpack可以全局安装和局部安装。局部安装的话就最好在安装的当前目录下运行，你硬要在在外部用webpack？那你在命令行要输入安装webpack位置的路径了。
```
npm install --save webpack // 这是局部安装
./node_modules/.bin/webpack --help // 局部安装的使用要带路径
```
哇，要写路径，好麻烦哦，没事，那就全局安装吧。
    npm install -g webpack
现在用webpack一般都写好配置文件的了，<code>webpack.config.js</code>，那么接下来就说这个配置文件主要怎样写。
```
module.exports = {
  devtool: '#eval-source-map', // 这个是打包的方式
  entry: './main.js',          // 入口文件。支持数组形式，将加载数组中的所有模块，但以最后一个模块作为输出，对象形式也一样。
  output: {                    
    path: './js',              // 打包后的文件存放位置
    publicPath: '/dist/',      // 这个下面详说
    filename: 'build.js'       // 打包后的文件名
  },
  resolve: {                   // 查找module的话从这里开始查找，下面详说。
    root: 'D:/webpack-test/src',
    extensions: ['.js', '.json', '.scss'],
    alias: {
        // 下面有例子。
    }
  }
};
```
### **devtool**：打包方式。（[官网的文档][2]）
| devtool选项 |打包速度 | 重建速度 | 是否支持生产模式|
| -----------| --------|
|source-map| - | - | 支持|
|eval-source-map| - | + | 不支持 |
|cheap-module-source-map| 0 | - | 支持 |
|cheap-module-eval-source-map| 0 | ++ | 不支持 |
| cheap-source-map | + | 0 | 支持 |
| cheap-eval-source-map | + | ++ | 不支持 |
| eval | +++ | +++ | 不支持|
从上到下,打包速度越来越快，开发环境一般用<code>eval-source-map</code>，生产环境自行斟酌咯。毕竟打包越快，打包质量也就越差。还有，不知大家发现没，带<code>eval</code>都不支持生产模式哦。
### **publicPath** （[官网的文档][3]）
*它被用来更新内嵌到css、html文件里的url值。*  

上面<code> publicPath: '/dist/' </code>,例如：
```
background-image:url('./test.png) // 路径变为'/dist/.test.png'
path: '/js' // 上面打包后的文件位置，那么路径变为'/dist/js/build.js'

```
**pubilcPath**很重要。在生产模式下如“test.png”文件可能会定位到CDN上并且你的Node.js服务器可能是运行在HeroKu（一个支持多种编程语言的云平台）上边的。一张图片，手动修改下咯，那如果你网站有上百张呢，那`publicPath：'你服务器的ip地址'`，这样省事很多吧。
### **resolve** （[官网的文档][4]）
1.          ***root***：包含您的模块的目录（绝对路径）。
2.    ***extensions***： 加载模块时可忽略的扩展名。
3.    ***alias***：模块别名定义。举些例子：
              ```
              'Abc': '/js/x/y/z/abc.js'  
              // require('Abc'); 相当于 require('/js/x/y/z/abc.js')
              // 如果 require('Abc/index.js'), 这样不行的。
              
              'Abc': './js/x/y/z/abc.js' 
              // 如果该值是一个相对路径，它将相对于包含require的文件。
              // 例如：在test.js中require('Abc'), 那么test.js和abc.js要在同目录下的。  
                
              'Abc': '/js/store'
              // require('Abc') 就相当于 require('/js/store/index.js')
              // require('Abc/other.js') 就相当于 require('/js/store/other.js')
                
              'Abc$': '/js/store'
              // require('Abc') 就相当于 require('/js/store/index.js')
              // 后面带有 $ ,意味着要完全匹配 'Abc'
              // 如果 require('Abc/other.js')，因为没完全匹配Abc，那么加载的是 node_modules下Abc文件夹里的other.js！！！
              ```


----------

#### [使用教程（二）--- module.loaders][5] 
                                                                


  [1]: https://sfault-image.b0.upaiyun.com/805/269/805269060-5a29458ba0338
  [2]: http://webpack.github.io/docs/configuration.html#devtool
  [3]: http://webpack.github.io/docs/configuration.html#output-publicpath
  [4]: http://webpack.github.io/docs/configuration.html#resolve
  [5]: https://segmentfault.com/a/1190000012351195