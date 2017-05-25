# webpack学习笔记
## demo-1
根据文档创建了一个目录来学习webpack

初步尝试添加了jquery，和自己的JS文件

并用webpack的ugilifyJsPlugin对bundle.js文件进行了压缩

- `export`和`import`命令
语法见ES6学习笔记

- npm scripts
为了省去每次运行时输入一长段命令，可以在scripts里添加`"build": "webpack"`

运行时输入`npm run build`即可

npm script中除npm默认命令字段外，其他scripts中自定义的字段运行时必须使用命令`npm run <name>`

- .gitignore
git push时输入命令`echo '/node_modules/' > .gitignore`

可以避免node_modules文件夹上传到git上

- __dirname 和 './' 的区别
[What is the difference between __dirname and ./ in node.js?](https://stackoverflow.com/questions/8131344/what-is-the-difference-between-dirname-and-in-node-js)

简单来说，__dirname是执行的JS文件所在目录的绝对路径

而'./'是运行node命令时目录的绝对路径

比如有文件是如下路径：
```
/dir1
  /dir2
    pathtest.js
```
运行：
```
cd /dir1
node dir2/pathtest.js
```
此时__dirname 和 './' 分别代表：
```
. = /dir1
__dirname = /dir1/dir2
```
