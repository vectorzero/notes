## 下载nodejs （win64位，版本8.9以上）
- [下载node.js](http://nodejs.cn/download/)
- 安装nodejs

## 安装淘宝镜像 cnpm
`npm install -g cnpm --registry=https://registry.npm.taobao.org`

## 全局安装 vue-cli 3
`cnpm i -g @vue/cli`

`vue --version`

## 创建一个项目
`vue create vue-cli-three`

选择 *Manually select features*

继续回车，用‘↑’、‘↓’和空格键来选取需要的特性

`Babel`,`Router`,`Vuex`,`Css Pre-processors`,`Linter/Formatter`

*Use history?* *y*

*Less*

*ESlint + AirBnb confg*

*Lint on save*

一路回车

## 启动项目
`cd vue-cli-three`

`npm run serve`

浏览器打开： [http://localhost:8081/ ](http://localhost:8081/)