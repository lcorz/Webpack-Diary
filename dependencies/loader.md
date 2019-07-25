## 关于loader

#### 1. `css-loader`

+ 解析css文件

#### 2. `style-loader`

+ 将`css-loader`解析出来的样式以`style`标签的形式插入`html`

#### 3. `postcss-loader`

+ 作为一个平台，提供一个`css`预处理工具，可以添加许多插件对`css`进行预处理，例如（`autoprefixer`等）
+ 关于`postcss`的操作可以单独提取出来，成为`postcss.config.js`

#### 4. `autoprefixer`

+ 自动添加浏览器前缀

#### 5. `postcss-import`

+ 处理`css`中的`@import`语法，例如

  ```css
  /* can consume `node_modules`, `web_modules` or local modules */
  @import "cssrecipes-defaults"; /* == @import "../node_modules/cssrecipes-defaults/index.css"; */
  @import "normalize.css"; /* == @import "../node_modules/normalize.css/normalize.css"; */
  
  @import "foo.css"; /* relative to css/ according to `from` option above */
  
  @import "bar.css" (min-width: 25em);
  
  body {
    background: black;
  }
  ```

  will give you:

  ```css
  /* ... content of ../node_modules/cssrecipes-defaults/index.css */
  /* ... content of ../node_modules/normalize.css/normalize.css */
  
  /* ... content of css/foo.css */
  
  @media (min-width: 25em) {
  /* ... content of css/bar.css */
  }
  
  body {
    background: black;
  }
  ```

  

#### 6. `postcss-url`

+ 处理`css`中`url`，例如：`background: url('image/path')`

#### 7. `sass-loader`/`less-loader`

+ 处理`sass`和`less`文件

#### 8. `extract-text-webpack-plugin`(`webpack3`)

+ 它会将所有`required` 的 `*.css` 模块**抽取**到分离的 `css` 文件。 所以你的样式将不会内联到 `JS bundle`，而是在一个单独的`css` 文件。

#### 9. `optimize-css-assets-webpack-plugin`(`webpack3`)

+ **生产环境**对`css`代码进行**压缩和优化**
+ 配合`extract-text-webpack-plugin`一起使用时，解决其分离出`css`重复的问题

#### 10. `mini-css-extract-plugin`(`webpack4`)

+ 将`css`单独**提取**出来成为单独的文件（主要用于**生产环境**）

+ 不支持`HMR`（热更替）

+ 该插件应该只用在`production`配置中，并且在`loaders`链中不使用`style-loader`

  ```javascript
  const MiniCssExtractPlugin = require('mini-css-extract-plugin');
  const devMode = process.env.NODE_ENV !== 'production';
  
  module.exports = {
    plugins: [
      new MiniCssExtractPlugin({
        filename: devMode ? '[name].css' : '[name].[hash].css',
        chunkFilename: devMode ? '[id].css' : '[id].[hash].css',
      })
    ],
    module: {
      rules: [
        {
          test: /\.(sa|sc|c)ss$/,
          use: [
            devMode ? 'style-loader' : MiniCssExtractPlugin.loader,
            'css-loader',
            'postcss-loader',
            'sass-loader',
          ],
        }
      ]
    }
  }
  ```

  

#### 11. `vue-style-loader`

+ 动态的将`vue`中的`css`样式以`style`标签的形式注入到文档中
+ 包含在`vue-loader`的依赖项中，并默认使用，不用手动配置

#### 12. `vue-loader`

+ 主要作用就是提取
+ 解析和转换`.vue`文件，提取其中逻辑代码`script`、样式代码`style`，以及`HTML`代码`template`，再将其分别交给对应的`Loader`去处理

#### 13. `vue-template-compiler`

+ 在构建阶段，将`vue-loader`提取出来的`template`模版编译成对应可执行的`js`代码（即，将模版变为`render`函数）
+ 预先编译好`template`模版相对于在浏览器中编译性能更好

#### 14. `url-loader`/`file-loader`

+ 图片处理及优化
+ 两者工作原理差不多
+ 两者区别：`url-loader`可设置大小限制。当超过限制时，行为等同于`file-loader`；小于限制时，会将文件以`base64`的形式打进`css`文件，减少请求次数

#### 15. `image-webpack-loader`

+ 优化图片处理

#### 16. `uglifyis-webpack-plugin`

+ **压缩**`js`文件

#### 17. `webpack-bundle-analyzer`

+ 可视化输出`webpack`包的文件大小

#### 18. `clean-webpack-plugin`

+ 清理指定目录，可用于清理上一次打包出来的`dist`目录

+ 在`webpack`配置的`plugin`中添加：

  ```javascript
  new cleanWebpackPlugin([dist])
  ```

#### 19. `rimraf`

+ 强制删除文件或目录
+ 可用于删除`dist`目录

#### 20. `node-notifier`

+ 支持`node`发送跨平台通知

#### 21. `portfinder`

+ 查看空闲端口位置，默认查询8080端口

#### 22. `semver`

+ 语义化版本控制规范

#### 23. `shelljs`

+ 对`node`底层`API`的封装

#### 24. `friendly-errors-webpack-plugin`

+ 更好的在终端看到webpack运行时的错误和警告等信息。提升开发体验。

## 配置项

#### 1. `webpack rules`配置

+ 多个`loader`的解析规则为从右向左

#### 2. 关于`css`和`js`压缩的配置项

+ 可在`webpack`的配置中通过`optimization.minimizer`覆盖`webpack`默认提供的压缩

  ```javascript
  module.exports = {
    optimization: {
      minimizer: [
        new UglifyJsPlugin({
          cache: true,
          parallel: true,
          sourcMap: true
        }),
        new OptimizeCSSAssetsPlugin({}),
      ],
    }
  }
  ```

  

#### 3. `.postcssrc.js`配置文件

```javascript
module.exports = {
  "plugins": {
    "postcss-import": {},      //用于@import导入css文件
    "postcss-url": {},           //路径引入css文件或node_modules文件
    "autoprefixer": {},        // 编辑目标浏览器，使用package.json中的"browserlist"字段，或读取".browserlist"文件配置
    "postcss-aspect-ratio-mini": {},   //用来处理元素容器宽高比
    "postcss-write-svg": { utf8: false },    //用来处理移动端1px的解决方案。该插件主要使用的是border-image和background来做1px的相关处理。
    "postcss-cssnext": {},  //该插件可以让我们使用CSS未来的特性，其会对这些特性做相关的兼容性处理。
    "postcss-px-to-viewport": {    //把px单位转换为vw、vh、vmin或者vmax这样的视窗单位，也是vw适配方案的核心插件之一。
    	viewportWidth: 750,    //视窗的宽度
    	viewportHeight: 1334,   //视窗的高度
    	unitPrecision: 3,    //将px转化为视窗单位值的小数位数
    	viewportUnit: 'vw',    //指定要转换成的视窗单位值
    	selectorBlackList: ['.ignore', '.hairlines'],    //指定不转换视窗单位值得类，可以自定义，可以无限添加
    	minPixelValue: 1,    //小于等于1px不转换为视窗单位
    	mediaQuery: false   //允许在媒体查询中使用px
    },
    "postcss-viewport-units":{}, //给vw、vh、vmin和vmax做适配的操作,这是实现vw布局必不可少的一个插件
    "cssnano": {    //主要用来压缩和清理CSS代码。在Webpack中，cssnano和css-loader捆绑在一起，所以不需要自己加载它。
    	preset: "advanced",   //重复调用
    	autoprefixer: false,    //cssnext和cssnano都具有autoprefixer,事实上只需要一个，所以把默认的autoprefixer删除掉，然后把cssnano中的autoprefixer设置为false。
    	"postcss-zindex": false   //只要启用了这个插件，z-index的值就会重置为1
 	}
  }
}
```

