从入口文件开始， 构建<mark>依赖图</mark>

## 面对的问题
### 1. 文件管理变得复杂
大量的JS和CSS文件，手动管理和链接它们很困难
你的html 可能用到很多css， js， image 等资源。 你需要把这些资源依赖加入到html中。 
当这些资源<mark>文件过多</mark>， 且<mark>路径很深</mark>的时候 如 /assets/images/game/people/...  就比较麻烦
### 2.请求过多 
需要多次<mark>请求依赖</mark>
请求一个html ， 它里面的每一个链接的JS和CSS文件都需要浏览器发起一个HTTP请求。这导致加载速度变慢。
例如，如果你的HTML文件链接了100个JavaScript文件，那么浏览器需要发起100个HTTP请求。

### 3. 依赖管理困难
依赖的文件必须放在被依赖的文件之前。
例如： script2.js依赖于script1.js，  那么顺序是
```js
<script src="script1.js"></script>
<script src="script2.js"></script>
```

**为了解决这些问题，Webpack应运而生。**

Webpack是一个模块打包器，它可以扫描你的应用程序，找到所有的依赖关系，并将它们打包成一个或多个优化和压缩的文件。这解决了上述的问题，同时还带来了一些新的优势：

    优化和压缩：Webpack通过删除不需要的空格、注释和代码，以及通过其他技术（如代码分割）来缩小文件大小，从而加快页面加载速度。
    模块管理：Webpack支持各种模块系统，如CommonJS、AMD和ES6模块。这意味着你可以使用你喜欢的模块系统，Webpack将处理好剩下的事情。
    加载器和插件：Webpack有一个强大的加载器和插件系统，可以做很多不同的事情，如转换ES6到ES5，编译Sass到CSS，等等



## 主要功能

1. **模块打包**：Webpack 可以处理各种类型的模块，包括 JavaScript、CSS、图片、字体等，并将它们打包成一个或多个优化的文件。
2. **代码分割**：Webpack 支持将代码分割成多个模块，这样可以实现按需加载或惰性加载，从而提高应用程序的性能。
3. **加载器**：Webpack 使用加载器处理非 JavaScript 的模块。例如，`babel-loader` 可以将 ES6+ 代码转换为 ES5 代码，`css-loader` 可以将 CSS 代码转换为 JavaScript 模块，而 `url-loader` 和 `file-loader` 可以处理图片和字体文件。
4. **插件系统**：Webpack 提供了一个强大的插件系统，可以用来扩展其功能。例如，`html-webpack-plugin` 可以生成 HTML 文件，`mini-css-extract-plugin` 可以将 CSS 代码从 JavaScript 代码中分离出来，而 `optimize-css-assets-webpack-plugin` 可以优化和压缩 CSS 代码。
5. **热模块替换**：Webpack 的 Hot Module Replacement（HMR）功能允许在运行时替换、添加或删除模块，而无需完全刷新页面。
6. **Tree Shaking**：Webpack 可以通过静态分析移除 JavaScript 中未被引用的代码，从而减小最终包的大小。
7. **开发服务器**：`webpack-dev-server` 提供了一个简单的 Web 服务器，并提供热更新的能力。

## 常用命令

- `webpack`：启动 Webpack，并打包项目。
- `webpack --watch`：启动 Webpack，并监听文件的改变。当检测到文件改变时，Webpack 会自动重新打包。
- `webpack-dev-server`：启动一个简单的 Web 服务器，并提供热更新的能力。
- `webpack --config`：指定配置文件的路径，例如 `webpack --config config/myconfig.js`。
- `npx webpack`：如果你没有全局安装 Webpack，但在项目的 `devDependencies` 中安装了 Webpack，你可以使用 `npx` 来运行 Webpack。
