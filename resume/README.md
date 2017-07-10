# resume

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

# 遇到的坑

要安装 `babel-plugin-transform-runtime` 这个插件   

并且在`.babelrc`文件中配置
```
"plugins": ["transform-runtime"]
```

否则ES6以及更新的一些新特性会用不了
```
    async / await 就会报错  
```




