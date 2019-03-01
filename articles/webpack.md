# webpack.config.js 参数详解
`webpack.config.js`文件通常放在项目的根目录中，它本身也是一个标准的`Commonjs`规范的模块。
```
var webpack = require('webpack'); 
module.exports = { 
    entry: [ 
        'webpack/hot/only-dev-server', 
        './js/app.js' 
    ], 
    output: { 
        path: './build', filename: 'bundle.js' 
    }, 
    module: { 
        loaders: [ 
            { 
                test: /\.js?$/, 
                loaders: ['react-hot', 'babel'], 
                exclude: /node_modules/ 
            }, { 
                test: /\.js$/, 
                exclude: /node_modules/, 
                loader: 'babel-loader'
            }, { 
                test: /\.css$/, 
                loader: "style!css" 
            }, {
                test: /\.less/,
                loader: 'style-loader!css-loader!less-loader'
            } 
        ] 
    }, 
    resolve:{ 
        extensions:['','.js','.json'] 
    }, 
    plugins: [ new webpack.NoErrorsPlugin() ] 
};
```
## 1.entry

`entry`可以是个字符串或数组或者是对象。
当`entry`是个字符串的时候，用来定义入口文件：
```
entry: './js/main.js'
```
当`entry`是个数组的时候，里面同样包含入口js文件，另外一个参数可以是用来配置webpack提供的一个静态资源服务器，`webpack-dev-server`。`webpack-dev-server`会监控项目中每一个文件的变化，实时的进行构建，并且自动刷新页面：
```
entry: [
    'webpack/hot/only-dev-server',
    './js/app.js'
]
```
当`entry`是个对象的时候，我们可以将不同的文件构建成不同的文件，按需使用，比如在我的hello页面中只要\引入hello.js即可：
```
entry: {
    hello: './js/hello.js',
    form: './js/form.js'
}
```
## 2.output
`output`参数是个对象，用于定义构建后的文件的输出。其中包含`path`和`filename`：
```
output: {
    path: './build',
    filename: 'bundle.js'
}
```
当我们在`entry`中定义构建多个文件时，`filename`可以对应的更改为`[name].js`用于定义不同文件构建后的名字。
## 3.module
关于模块的加载相关，我们就定义在`module.loaders`中。这里通过正则表达式去匹配不同后缀的文件名，然后给它们定义不同的加载器。比如说给`less`文件定义串联的三个加载器（！用来定义级联关系）：
```
module: {
    loaders: [{ 
            test: /\.js?$/, 
            loaders: ['react-hot', 'babel'], exclude: /node_modules/ 
        },{ 
            test: /\.js$/, 
            exclude: /node_modules/, 
            loader: 'babel-loader'
        },{ 
            test: /\.css$/, 
            loader: "style!css" 
        },{ 
            test: /\.less/, 
            loader: 'style-loader!css-loader!less-loader'
        }
    ]
}
```
此外，还可以添加用来定义`png`、`jpg`这样的图片资源在小于10k时自动处理为`base64`图片的加载器：
```
{ test: /\.(png|jpg)$/,loader: 'url-loader?limit=10000'}
```
给`css`和`less`还有图片添加了`loader`之后，我们不仅可以像在`node`中那样`require` `js`文件了，我们还可以`require  css`、`less`甚至图片文件：
```
require('./bootstrap.css');
require('./myapp.less');
var img = document.createElement('img');
img.src = require('./glyph.png');
```
但是需要知道的是，这样`require`来的文件会内联到 `js bundle`中。如果我们需要把保留`require`的写法又想把`css`文件单独拿出来，可以使用下面提到的`[extract-text-webpack-plugin]`插件。
在上面示例代码中配置的第一个`loaders`我们可以看到一个叫做`react-hot`的加载器。我的项目是用来学习react写相关代码的，所以配置了一个`react-hot`加载器，通过它，可以实现对`react`组件的热替换。我们已经在`entry`参数中配置了`webpack/hot/only-dev-server`,所以我们只要在启动`webpack`开发服务器时开启`–hot`参数，就可以使用`react-hot-loader`了。在`package.json`文件中这样定义：
```
"scripts": {
    "start": "webpack-dev-server --hot --progress --colors",
    "build": "webpack --progress --colors"
}
```
## 4.resolve
webpack在构建包的时候会按目录的进行文件的查找，resolve属性中的extensions数组中用于配置程序可以自行补全哪些文件后缀：
```
resolve:{
    extensions:['','.js','.json']
}
```
然后我们想要加载一个js文件时，只要require(‘common’)就可以加载common.js文件了。
## 6.externals
当我们想在项目中require一些其他的类库或者API，而又不想让这些类库的源码被构建到运行时文件中，这在实际开发中很有必要。此时我们就可以通过配置externals参数来解决这个问题：
```
externals: {
    "jquery": "jQuery"
}
```
这样我们就可以放心的在项目中使用这些API了：var jQuery = require(“jquery”);

## 7.context
当我们在require一个模块的时候，如果在require中包含变量，像这样：
```
require("./mods/" + name + ".js");
```
那么在编译的时候我们是不能知道具体的模块的。但这个时候，webpack也会为我们做些分析工作：

1.分析目录：’./mods’；  
2.提取正则表达式：’/^.*.js$/’；

于是这个时候为了更好地配合wenpack进行编译，我们可以给它指明路径，像在cake-webpack-config中所做的那样（我们在这里先忽略abcoption的作用）：
```
var currentBase = process.cwd();
var context = abcOptions.options.context 
        ? abcOptions.options.context : path.isAbsolute(entryDir) 
        ? entryDir : path.join(currentBase, entryDir);
```