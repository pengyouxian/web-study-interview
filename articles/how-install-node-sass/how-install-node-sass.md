# node-sass的安装，以及出现的问题及解决方法

### node-sass

在安装node-sass之前，我先介绍一下什么是node-sass。node-sass是一个项目依赖，在一个项目中在使用sass语法的时候，必须通过sass-loader来解析sass，从而使sass语法变成浏览器能够识别的CSS语法，而node-sass模块就是对sass-loader的支持模块，所以不安装node-sass，sass-loader就不能正常工作。

### node-sass安装过程中问题的解决

![image-20210709104713769](./img/image-20210709104713769.png)

如上图，在pull前端开源项目代码之后，如果依赖了node-sass，经常会遇到如上`gyp....` 开头的信息提示，其中有提到需要python某版本的依赖。

这个问题并不是npm网络代理导致的，所以即便使用`cnpm`代替`npm`来执行安装操作，一样会遇到这样的问题，所以重点还是要通过安装python来解决，或者可以通过手动安装node-sass来解决。

### 方案一：安装python的步骤

- 打开 WEB 浏览器访问https://www.python.org/downloads/windows/

![image-20210709105511419](./img/image-20210709105511419.png)

- 在下载列表中选择Window平台安装包

![image-20210709105633232](./img/image-20210709105633232.png)

- 下载后，双击下载包，进入 Python 安装向导，安装非常简单，你只需要使用默认的设置一直点击"下一步"直到安装完成即可。

![image-20210709105754961](./img/image-20210709105754961.png)

- 建议勾选这两项，其中一个是关于环境变量的(如果出现Disable path length limit的字样，建议点击)

![image-20210709105848770](./img/image-20210709105848770.png)

- 安装成功！

  ### 再次安装node-sass：

  ![image-20210709110040996](./img/image-20210709110040996.png)

依旧会跑一大堆脚本，输出一大堆信息，只看最后的结果：

![image-20210709110156693](./img/image-20210709110156693.png)

如图，便是安装成功了！

### 方案二：手动下载安装node-sass

适用于方案一无法解决，依然报错的情况：

1. 浏览器访问[Releases · sass/node-sass (github.com)](https://github.com/sass/node-sass/releases)下载对应的`.node`版本，假设下载的路径为`src/xxxx.node`

2. 配置临时环境变量

   打开cmd，输入`set SASS_BINARY_PATH=src/xxxx.node`(如果是PowerShell需要将等号换成空格)

3. `npm i -g node-sass`，如果前面安装失败，可以尝试`npm i -g node-sass --force`

4. 命令行输入`npm view node-sass version`能输出成功表示安装成功！

   #### 可能遇到的情况：

   在第3步，依然报错，但不是提示已存在安装，可能是npm版本的问题，`npm i npm@6.14.11 -g`，本次安装是将npm版本从7.x降低到6.x解决的。

   

   

===================end====================
