##### 1. 入口`entry`的值为什么会有异步函数和同步函数的区分？
##### 2. 关于`bundle/chunk/module`的区别？
+ `module`: 表示一个独立的功能模块,可以是`ESM`模块也可以是`commonJS`或者`AMD`模块
+ `chunk`: 打包过程中被操作的模块（`module`）文件叫做`chunk`
    + 被操作的模块分为三种类型：
        > 项目入口（`entry`）
        
        > 通过`import/require`引入进来的代码
        
        > 通过`splitChunks`拆分出来的代码
        
    + `chunk`和`module`是一对一的关系，但是当一个文件`import/require`其他模块时，两者会变为一对多的关系
+ `bundle`: 最后打包后的文件。
    + 一般是和`chunk`一对一
    + 但是当有`splitChunks`拆分出来的`chunk`时，则不是一对一的关系

#### 3. `runtime`和`manifest`

+ 持久化缓存策略
+ `runtime`: 指浏览器运行时，`webpack`用来连接模块化的应用程序的所有代码。主要包含：
  + 模块交互时，连接模块所需的加载和解析逻辑。包括浏览器中的已加载模块的连接，以及懒加载模块的执行逻辑
+ `manifest`: 当编译器开始执行、解析和映射(指`module`到`bundle`的映射)应用程序时，它会保留所有模块(`module`)的详细要点，这个数据集合称为`Manifest`。完成打包并发送到浏览器时，会在运行时通过`Manifest`来解析和加载模块。
+ `Tips:`
  + 打包后，无论选择那种模块语法，那些 `import` 或 `require` 语句现在都已经转换为 `__webpack_require__` 方法，此方法指向模块标识符(`module identifier`)
  + 通过`manifest`中的数据，`runtime`将能够查询模块标识符(`module identifier`)，检索出背后对应的模块

