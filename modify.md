# install

## nodejs install
官方说> 8.9.4,但是我用 node 12.x安装失败，用node 8.9.4安装成功

官网下载8.9.4
https://nodejs.org/zh-cn/download/releases/

## redis install 
docker run -d --name redis605 -h 192.168.10.132 -p 6379:6379 redis
## mongodb install 
docker run -d --name mongo4023 -h 192.168.10.132 -p 27017:27017 -v /data/mongo/mongodata mongo:4.0.23

## 问题处理 1 
### /__webpack_hmr 404
webpack.base.config.js 增加entry array
```js
module.exports = {
  entry: [
    'webpack-hot-middleware/client?path=http://' + config.fe.host + ':' + config.fe.port + '/__webpack_hmr'
  ],
```

### performance 
webpack.base.config.js 调整这两项的值到3M
```js
  performance: {
    maxEntrypointSize: 3000000,
    maxAssetSize: 3000000,
    hints: isProd ? 'warning' : false
  },
```
## 配置文件
增加两个 local.json,production.json
