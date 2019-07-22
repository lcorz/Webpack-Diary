### 使用说明

该文件主要被一下工具使用：

- `Autoprefixer`
- `Babel`
- `post-preset-env`
- `eslint-plugin-compat`
- `stylelint-unsupported-browser-features`
- `postcss-normalize`

所有的工具将自动的查找当前工程规划的目标浏览器范围，前提是你在前端工程的 `package.json `里面增加如下配置：

```javascript
{
  "browserslist": [
    "last 1 version",
    "> 1%",
    "maintained node versions",
    "not dead"
  ]
}
```

或者，在工程的根目录下存在`.browerslistrc`配置文件（内容如下）：

```json
# 注释是这样写的，以#号开头
last 1 version
> 1%
maintained node versions
not dead
```

