现在前端项目中不能使用使用Sass或Stylus等预处理器，是一个很不爽的事情，而svelte目前官方暂时真的没将这一块纳入初始模板，那么我们就只能自己动手了。

可以直接使用我集成好的模板：
```
# for Rollup
npx degit "KeiferJu/svelte-sass-template" my-app

cd my-app

npm install
npm run dev & open http://localhost:5000
```



# 正文

以下文章以集成saas（scss）为例，其他的可以自己研究[svelte-preprocess](https://github.com/kaisermann/svelte-preprocess)这个开源项目。

## 一. 集成saas预处理器
### 1. 安装依赖模块

```
npm i -D svelte-preprocess node-sass
```

### 2. 修改配置文件



```
// rollup.config.js

// ...
import sveltePreprocess from 'svelte-preprocess'
const preprocess = sveltePreprocess({
  scss: {
    includePaths: ['src'],
  },
  postcss: {
    plugins: [],
  },
});

export default {
  ...,
  plugins: [
    svelte({
    // ...
      preprocess: preprocess,
   // ...
    })
  ]
}
```

#### 3. 使用方法
现在，你只要将lang=“scss”添加到样式标签上，就可以解析saas语法了
```
<style lang="scss">
  $color: red;
  div {
    color: $color;
  }
</style>
```

## 二. js中直接引入css
平时我们的操作中，可能需要在js中直接引入css或者scss文件，例如在main.js中引入一些第三方框架或者自己定义的主题文件。

直接引入肯定是不行的。
```
[!] Error: Unexpected token (Note that you need plugins to import files that are not JavaScript)
```

这是就需要借助`rollup-plugin-postcss`插件:

```
npm install rollup-plugin-postcss -D
```

配置`rollup.config.js`:

```
export default {
    ......
	plugins: [
	    //...
		postcss({
			plugins: []
		}),
	    //...
	]
};
```


### 三. vs code修正
我们完全可以正常使用scss了，但是有一个麻烦，vscode对这样的表示方式认为是错的。
首先，从VS Code中安装“ Svelte”扩展。
接下来，在项目的顶层创建一个svelte.config.js
```
const sveltePreprocess = require('svelte-preprocess');
module.exports = {
  preprocess: sveltePreprocess({
    scss: {
      includePaths: ['src'],
    },
    postcss: {
      plugins: [],
    },
  }),
};
```
