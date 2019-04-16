# NPM的使用

`devDependencies`是只会在开发环境下依赖的模块，生产环境不会被打入包内。通过`NODE_ENV=developement`或`NODE_ENV=production`指定开发还是生产环境。
`而dependencies`依赖的包不仅开发环境能使用，生产环境也能使用。其实这句话是重点，按照这个观念很容易决定安装模块时是使用`--save`还是`--save-dev`。

---

当你为你的模块安装一个依赖模块时，正常情况下你得先安装他们（在模块根目录下`npm install module-name`），然后连同版本号手动将他们添加到模块配置文件`package.json`中的依赖里（`dependencies`）。

`-save`和`-save-dev`可以省掉你手动修改`package.json`文件的步骤。   
`spm install module-name -save` 自动把模块和版本号添加到dependencies部分   
`spm install module-name -save-dve` 自动把模块和版本号添加到`devdependencies`部分

至于配置文件区分这俩部分， 是用于区别开发依赖模块和产品依赖模块， 以我见过的情况来看 `devDepandencies`主要是配置测试框架， 例如jshint、mocha。