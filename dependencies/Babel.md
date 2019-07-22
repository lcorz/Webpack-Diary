## 依赖插件

#### 1. `babel-core`

+ 将`js`代码分析为`AST`（抽象语法树），方便各个插件分析语法和处理

#### 2. `babel-cli`

+ 通过命令行对`js`文件进行转码的工具

#### 3. `babel-node`

+ 随`babel-cli`一起安装
+ 在命令行中输入`babel-node`会启动一个支持`ES6`的`js`环境；
+ 还可用来执行`js`文件，与`node`命令类似，只不过会在执行过程中进行`babel`转译。执行过程中会产生缓存，导致内存占用过量，官方并不建议这么做；

#### 4. `babel-loder`

+ 转译`js`文件
+ 该版本为`8.X`时，要求`babel`版本为`7.X`

#### 5. `babel-polyfill`

+ 垫片

+ 作用：用已经存在的语法和`api`实现一些浏览器还没实现的`api`，对浏览器的一些缺陷进行修补；

+ 存在的原因：`babel`的转译只是语法层面的，例如箭头函数、结构赋值、`class`等，对一些新增`api`及全局函数（如：`Promise`）无法进行转译，这时就需要在代码里引入`babel-polyfill`包；

+ 使用方法：在入口处引用，通过`require`或`import`方式，如果是`webpack`则在文件入口数组中引用：

  ```javascript
  require("babel-polyfill");
  或
  import "babel-polyfill";
  或
  module.exports = {
    entry: ["babel-polyfill", "./app/js"]
  };
  ```

+ **缺点**：引入后与项目代码一同编译到生产环境，我们不一定使用所有`ES6+`语法，使得打出来的包体积增大。而且它是通过向全局对象和内置对象的`prototype`上添加方法来实现的，会污染全局变量（故出现了`babel-runtime`）

#### 6. `babel-runtime`

+ 不会污染全局变量和内置对象原型（不会改写一些实例方法，如`Object`和`Array`原型链上的方法）

+ 按需加载，例如：环境不支持`Promise`，则在项目中引用如下代码来获取：

  ```javascript
  import Promise from 'babel-runtime/core-js/promise'
  ```

+ **缺点**：在实际开发过程中，我们会引入很多`ES6 api`，每一个都这样加载进来也很不方便。而且每个文件都重复引用会造成代码冗余（故出现了`babel-plugin-transform-runtime`）

#### 7. `babel-plugin-transform-runtime`

+ 依赖`babel-runtime`；
+ 在多个文件中重复使用的方法实际上`polyfill`只有一份；
+ 分析`AST`中是否有引用`babel-runtime`中的垫片，如果有，就会在自动在当前模块顶部插入我们需要的`polyfill`，这些`polyfill`就在`babel-runtime`包里（`core-js 、regenerator`等 `poiiyfill`）；
+ 自动引入`helpers`（类似于工具包）

#### 8. `babel-preset-env`

+ 插件预设
+ 预设是指一系列插件的集合，通过该插件能灵活决定加载哪些插件和`polyfill`

#### 9. `babel-helper-vue-jsx-merge-props`

+ 预设`babel-template`函数，提供给`vue,jsx`等使用

#### 10. `babel-plugin-syntax-jsx`

+ 支持jsx编译

#### 11. `babel-plugin-transform-vue-jsx`

## 使用`.babelrc`配置规则

```javascript
{
    "presets": [
		[
			"@babel/preset-env",
			{
				"modules": false，
                // 支持运行环境
                "targets": {
					"browsers": [
						"> 1%",
						"last 2 versions",
						"not ie <= 8"
					]
				}
			}
		]
	],
	"plugins": [
		"@babel/plugin-syntax-jsx",
		"@babel/plugin-transform-runtime"
	]
}
```

+ 在`babel7.X`版本中`stage-*`已弃用
+ 关于支持运行环境可单独提出在`.browserslistrc`