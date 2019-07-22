## 依赖插件

#### 1. `eslint`

#### 2. `eslint-loader`

+ 使`webpack`支持`eslint`

#### 3. `eslint-friendly-formater`

+ 指定错误的格式规范

#### 4. `babel-eslint`

+ 检查`babel`解析后的代码

#### 5. `eslint-plugin-vue`

+ 对`vue`中的代码进行检查

#### 6.  配置`Eslint`时可指定`Eslint`规范

+ `With error prevention only`
+ `Airbnb`
+ `Standard`: 需要安装第三方规范库`eslint-config-standard`（安装完毕后，手动安装其四个依赖项）
+ `Prettier`

## 使用`.eslintrc`配置规则

#### 1. `root`

+ 限定配置文件的使用范围

+ 为`true`时表示以当前目录为根目录，不再向上查找。如果想要在不同的目录使用不同的`.eslintrc`，则在应在该目录的配置项中添加如下配置，**否则**，它会一直**向上查找**，直到**根目录**为止，如果根目录有配置文件，则使用根目录的配置文件，使得**当前目录配置不生效**。

  ```javascript
  {
      "root": true
  }
  ```

  

#### 2. `extends`

- 指定`Eslint`规范，一般使用第三方的

#### 3. `rules`

在第三方库的基础上：

- 添加默认或第三方库没有的规则；

- 覆盖默认或第三方库规则；

  ```javascript
  {
      "rules": {
          "规则名称": ["错误等级", "处理方式"]
      }
  }
  ```

  

项目中可能出现其他文件也需要格式化规范，如`vue, html, react`等，这时需要引入第三方插件（即，对`plugin`进行配置）

#### 4. `plugins`

+ 第三方插件

#### 5. `parserOptions`

+ 设置解析器选项

  `parser`: 指定解析器

  ```javascript
  {
      "parserOptions": {
          "parser": "babel-eslint"
      }
  }
  ```

  

#### 6. `env`

+ 指定环境

+ 指定环境后，可以使用其全局变量和属性

  ```javascript
  {
      "env": {
          "browser": true,
          "node": true
      }
  }
  ```

  

#### 7. `globals`

+ 指定全局变量

#### 8. `overrides`

+ 对于匹配 `overrides.files` 且不匹配 `overrides.excludedFiles` 的 文件，`overrides.rules` 中的规则会覆盖 `rules` 中的同名规则。

  ```javascript
  {
    "rules": {...},
    "overrides": [
      {
        "files": ["*-test.js","*.spec.js"],
        "excludedFiles": "*.test.js",
        "rules": {
          "no-unused-expressions": "off"
        }
      }
    ]
  }
  ```

  

## 额外配置

#### 1. `.eslintignore`文件

+ 如果有一些文件不想被`Eslint`检测，则在`.eslintignore`文件中配置

  ```javascript
  build/*
  config/*
  test/*
  src/store/*
  src/utils/*
  src/router/*
  src/personalCenter/view/orders/info.vue
  ```

  