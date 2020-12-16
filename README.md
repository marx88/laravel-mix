# Laravel-mix踩坑记
前排提醒：该坑是基于`laravel-mix@V5.0.9`的，官方的`6.*`还是`beta`版，安装方式有变动，尚未测试。

### 安装
---
跟着官网教程来

#### 1. 创建项目根目录
```sh
mkdir my-app && cd my-app
```

#### 2. 初始化项目
```sh
yarn init -y
```

#### 3. 安装laravel-mix依赖
```sh
yarn add -D laravel-mix
```

#### 4. 复制webpack.mix.js
```sh
cp node_modules/laravel-mix/setup/webpack.mix.js ./

# windows 使用下面这行命令
# copy node_modules\laravel-mix\setup\webpack.mix.js webpack.mix.js
```

#### 5. 修改根目录下的webpack.mix.js
```js
let mix = require('laravel-mix');

mix.js('src/app.js', 'dist')
  .sass('src/app.scss', 'dist')
  .setPublicPath('dist'); // 这句用来配置打包目录
```

#### 6. 创建资源文件路径
```sh
mkdir src && touch src/app.{js,scss}

# windows cmd使用下面这行命令
# mkdir src && type nul>src/app.js && type nul>src/app.scss

# windows powershell使用下面这行命令
# mkdir src; New-item src/app.js; New-item src/app.scss
```

#### 7. 编译
```sh
# 官方给的指令
# 如果你成功了，那么恭喜你，该教程完结
# 如果该指令报错：Can't resolve './src'，请看下一节
node_modules/.bin/webpack

# windows 使用下面命令
# node_modules\.bin\webpack
```

### 填坑
---
我按着教程来，是一直报错的，官方也有个`issue`，但是没有解决就`closed`。
填坑，先按官方的操作，直到出现`Can't resolve './src'`错误后，进行下面步骤：

#### 1. 安装其它依赖
```sh
# 如果不是windows可以不用安装cross-env
yarn add -D vue-template-compiler sass-loader@8.* sass resolve-url-loader@3.1.0 cross-env
```

#### 2. 编译
```sh
# 执行完这一步就算成功了，可以开发了。
node_modules/.bin/webpack --config=node_modules/laravel-mix/setup/webpack.config.js

# windows 使用下面命令
# node_modules\.bin\webpack --config=node_modules\laravel-mix\setup\webpack.config.js
```

#### 3. 更改package.json添加scripts
```json
"scripts": {
  "dev": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
  "watch": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --watch --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
  "hot": "cross-env NODE_ENV=development webpack-dev-server --inline --hot --config=node_modules/laravel-mix/setup/webpack.config.js",
  "production": "cross-env NODE_ENV=production node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js"
}
```

#### 4. 开启实时打包
```sh
yarn watch
```
