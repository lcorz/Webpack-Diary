#### 1. `webpack.DefinePlugin`

+ 创建一个在**编译**时可以配置的全局常量
+ `Webpack`是属于`Node`的程序，`Node`环境下的环境变量，`Webpack`可以配置可以灵活读取。

#### 2. `webpack.NamedModulesPlugin`

+ 在热加载时直接返回更新文件名，而不是文件的id
+ 模块就是一个数组，引用也是按照在数组中的顺序引用，新增减模块都会导致序号(`module id`)的变化，导致缓存失效。
+ 当开启`HMR`的时候使用该插件会显示模块的相对路径，建议用于**开发环境**。
+ 关联 [6]

#### 3. `webpack.NamedChunksPlugin`

+ 配置的每个`chunks`命名，原本的`chunks`也是数组，没有姓名
+ 如果`chunk id`不固定，则当`chunks`增加或者减少时会和`module id`一样，都可能会导致`chunk`的顺序发生错乱，从而让`chunk`的缓存都失效

#### 4. `webpack.HotModuleReplacementPlugin`

+ 启动热更替时配置插件

#### 5. `webpack.NoEmitOnErrorsPlugin`

+ 在编译出现错误时，使用 NoEmitOnErrorsPlugin 来跳过输出阶段。这样可以确保输出资源不会包含错误。

#### 6. `webpack.HashedModuleIdsPlugin`

+ 该插件会根据模块的相对路径生成一个四位数的hash作为模块id, 建议用于**生产环境**。

+ 关联 [2]

  ```javascript
  new webpack.HashedModuleIdsPlugin({
    // 散列算法，默认为 'md5'。支持 Node.JS crypto.createHash 的所有功能。
    hashFunction: 'sha256',
    // 在生成 hash 时使用的编码方式，默认为 'base64'。
    hashDigest: 'hex',
    // 散列摘要的前缀长度，默认为 4。
    hashDigestLength: 20
  })
  ```

  

#### 7. `webpack.optimize.ModuleConcatenationPlugin`



#### 8. `webpack.optimize.CommonsChunkPlugin`

+ `webpack3`中，经常使用`webpack.optimize.CommonsChunkPlugin`进行模块的拆分，将代码的公共部分，以及变动较少的框架或者库提取到一个单独的文件中。例如：我们引入的代码框架（`vue/react`）。只要页面加载过一次之后，抽离出来的代码就可以放在缓存中，而不是每次加载页面的都重新加载全部资源。用法如下：

```javascript
module.exports = {
  plugins: [
    // 将node_modules中的代码放入vendor.js中
    new webpack.optimize.CommonsChunkPlugin({
      name: "vendor",
      minChunks: function(module){
        return module.context && module.context.includes("node_modules");
      }
    }),
    // 将webpack中runtime相关的代码放入manifest.js中
    new webpack.optimize.CommonsChunkPlugin({
      name: "manifest",
      minChunks: Infinity
    }),
  ]
}
```

`Tips:`存在配置不够灵活，难以理解的缺点。`minChunks`有时候为数字，有时候为函数，并且如果同步模块和异步模块都引入了相同的`module`并不能将公共部分提取出来，最后打包的生成的`js`还是存在相同的`module`。

#### 9. `webpack.optimize.SideEffectsFlagPlugin`

+ 清除一个大的模块文件中的未使用的代码，这个大的文件模块可以是自定义的，也可以是第三方的（注意：一定要`package.json`文件中添加`"sideEffects"`: false`）

#### 10. `webpack.optimize.OccurrenceOrderPlugin`

+ 按照`chunk`引用次数来安排出现顺序，因为这让经常引用的模块和`chunk`拥有更小的`id`
+ 这样可以实现最优的构建输出

#### 11. `webpack.optimize.FlagIncludedChunksPlugin`

+ 检测并标记模块之间的从属关系

