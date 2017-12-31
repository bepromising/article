> [使用教程（一）--- entry，devtool，output，resolve][1]
> [使用教程（二）--- module.loaders][2]
> [使用教程（三）--- plugins][3]
> ##### **我的 *Webpack* 版本是 *3.10.0***


## DevServer （[官方的文档][4]）

- 在开发模式下，***DevServer*** 提供虚拟服务器，让我们进行开发和调试。

- 而且提供实时重新加载。简直美滋滋。大大减少开发时间。

- 它不是 *webpack* 内置插件哦，要安装！！！

```
// 安装
npm install webpack-dev-server --save-dev

// 在 package.json 配置下，方便使用。
"scripts": {
    "dev": "webpack-dev-server" // 运行命令会自动在node_modules文件夹找 webapck-dev-server模块。
 }

// webpack.config.js 配置一下 devServer
devServer: {
    clientLogLevel: 'warning',
    historyApiFallback: true,
    hot: true,
    compress: true,
    host: 'localhost',
    port: 8080
  }
```
1. 如果没在 *package.json* 配置且还是局部安装，你就要在命令行输入 <code>node_modules/.bin/webpack-dev-server</code>。若你 *package.json* 配置好了，在命令行输入<code>npm run dev</code>。

2. 下面说说 *devServer* 配置中每一项有什么用。

### Hot （[文档][5]）

- 热模块更新作用。即修改或模块后，保存会自动更新，页面不用刷新呈现最新的效果。

- 这不是和 *webpack.HotModuleReplacementPlugin （HMR）* 这个插件不是一样功能吗？是的，不过请注意了，<code>***HMR* 这个插件是真正实现热模块更新的**</code>。而 *devServer* 里配置了 *hot: true* , *webpack*会自动添加 *HMR* 插件。所以模块热更新最终还是 *HMR* 这个插件起的作用。

### host （[文档][6]）

- 写主机名的。默认 *localhost*

### prot （[文档][7]）

- 端口号。默认 *8080*

### historyApiFallback （[文档][8]）

- 如果为 ***true*** ，页面出错不会弹出 *404* 页面。

- 如果为 *{...}* , 看看一般里面有什么。
    - **rewrites**
    ```
    rewrites: [
        { from: /^\/subpage/, to: '/views/subpage.html' },
        {
          from: /^\/helloWorld\/.*$/,
          to: function() {
              return '/views/hello_world.html;
          }
        }
    ]
    // 从代码可以看出 url 匹配正则，匹配成功就到某个页面。
    // 并不建议将路由写在这，一般 historyApiFallback: true 就行了。
    ```
    - **verbose**：如果 *true* ，则激活日志记录。
    - **disableDotRule**： 禁止 *url* 带小数点 <code>.</code> 。
    
### compress ([文档][9])
    
- 如果为 *true* ，开启虚拟服务器时，为你的代码进行压缩。加快开发流程和优化的作用。

### contentBase （[文档][10]）

- 你要提供哪里的内容给虚拟服务器用。这里最好填 **绝对路径**。
    ```
    // 单目录
    contentBase: path.join(__dirname, "public")
    
    // 多目录
    contentBase: [path.join(__dirname, "public"), path.join(__dirname, "assets")]
    ```

- 默认情况下，它将使用您当前的工作目录来提供内容。

### Open （[文档][11]）

- *true*，则自动打开浏览器。

### overlay ([文档][12])

- 如果为 *true* ，在浏览器上全屏显示编译的errors或warnings。默认 *false* （关闭）

- 如果你只想看 *error* ，不想看 *warning*。

```
overlay：{
    errors：true，
    warnings：false
}
```

### quiet （[文档][13]）

- *true*，则终端输出的只有初始启动信息。 *webpack* 的警告和错误是不输出到终端的。

### publicPath （[文档][14]）

- 配置了 *publicPath*后， <code>*url* = '主机名' + '*publicPath*配置的' +
 '原来的*url.path*'</code>。这个其实与 *output.publicPath* 用法大同小异。
- *output.publicPath* 是作用于 *js, css, img* 。而 *devServer.publicPath* 则作用于请求路径上的。
```
// devServer.publicPath
publicPath: "/assets/"

// 原本路径 --> 变换后的路径
http://localhost:8080/app.js --> http://localhost:8080/assets/app.js
```

### proxy ([文档][15])

- 当您有一个单独的API后端开发服务器，并且想要在同一个域上发送API请求时，则代理这些 *url* 。看例子好理解。

```
  proxy: {
    '/proxy': {
		target: 'http://your_api_server.com',
		changeOrigin: true,
		pathRewrite: {
			'^/proxy': ''
		}
  }
```

1. 假设你主机名为 *localhost:8080* , 请求 *API* 的 *url* 是 *http：//your_api_server.com/user/list*

2. ***'/proxy'***：如果点击某个按钮，触发请求 *API* 事件，这时请求 *url* 是<code>http：//localhost:8080**/proxy**/user/list</code> 。

3. ***changeOrigin***：如果 *true* ，那么 <code>http：//localhost:8080/proxy/user/list 变为 http：//your_api_server.com/proxy/user/list</code> 。但还不是我们要的 *url* 。

4. ***pathRewrite***：重写路径。匹配 */proxy* ，然后变为<code>''</code> ，那么 *url* 最终为 <code>http：//your_api_server.com/user/list</code> 。

### watchOptions （[文档][16]）

- 一组自定义的监听模式，用来监听文件是否被改动过。

```
watchOptions: {
  aggregateTimeout: 300,
  poll: 1000，
  ignored: /node_modules/
}
```
1. **aggregateTimeout**：一旦第一个文件改变，在重建之前添加一个延迟。填以毫秒为单位的数字。

2. **ignored**：观察许多文件系统会导致大量的CPU或内存使用量。可以排除一个巨大的文件夹。

3. **poll**：填以毫秒为单位的数字。每隔（你设定的）多少时间查一下有没有文件改动过。不想启用也可以填<code>false</code>。


----------
## ~~完结，希望大家喜欢！~~ 并未完结，敬请期待！

  [1]: https://segmentfault.com/a/1190000012334562
  [2]: https://segmentfault.com/a/1190000012351195
  [3]: https://segmentfault.com/a/1190000012367082
  [4]: https://webpack.js.org/configuration/dev-server/#devserver-hot
  [5]: https://webpack.js.org/configuration/dev-server/#devserver-hot
  [6]: https://webpack.js.org/configuration/dev-server/#devserver-host
  [7]: https://webpack.js.org/configuration/dev-server/#devserver-port
  [8]: https://github.com/bripkens/connect-history-api-fallback
  [9]: https://webpack.js.org/configuration/dev-server/#devserver-compress
  [10]: https://webpack.js.org/configuration/dev-server/#devserver-contentbase
  [11]: https://webpack.js.org/configuration/dev-server/#devserver-open
  [12]: https://webpack.js.org/configuration/dev-server/#devserver-overlay
  [13]: https://webpack.js.org/configuration/dev-server/#devserver-quiet-
  [14]: https://webpack.js.org/configuration/dev-server/#devserver-publicpath-
  [15]: https://webpack.js.org/configuration/dev-server/#devserver-proxy
  [16]: https://webpack.js.org/configuration/dev-server/#devserver-watchoptions-