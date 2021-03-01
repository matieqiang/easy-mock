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

## 用pm2守护启动
为什么需要使用PM2
因为node.js 是单进程，进程被杀死后整个服务就跪了，所以需要进程管理工具，但是pm2 远远不止这些。

PM2 是一个带有负载均衡功能的 Node 应用的进程管理器。 当你要把你的独立代码利用全部的服务器上的所有 CPU，并保证进程永远都活着，0 秒的重载， PM2 是完美的。

npm install pm2

添加pm2 启动配置文件

vim pm2.config.js 

```js

module.exports = {
  apps : [
      {
        name: "easy-mock",
        script: "./app.js",
        watch: true,
        instance_var: 'INSTANCE_ID',
        env: {
            "PORT": 3000,
            "NODE_ENV": "production"
        }
      }
  ]
}
```
pm2 start pm2.config.js --env production

### pm2 常用命令
```bash
npm install pm2 -g     # 命令行安装 pm2
pm2 start app.js -i 4 #后台运行pm2，启动4个app.js
                              # 也可以把'max' 参数传递给 start
                              # 正确的进程数目依赖于Cpu的核心数目
pm2 start app.js --name my-api # 命名进程
pm2 web                # 运行健壮的 computer API endpoint (http://localhost:9615)
pm2 list               # 显示所有进程状态
pm2 ls                 # 显示所有进程状态
pm2 show 0			       # 显示某个应用的详细信息
pm2 monit              # 监视所有进程
pm2 logs               # 显示所有进程日志
pm2 log 0 	           # 查看 0 应用的日志
pm2 stop all           # 停止所有进程
pm2 restart all        # 重启所有进程
pm2 reload all         # 0秒停机重载进程
pm2 stop 0             # 停止指定的进程，0 是应用 id
pm2 restart 0          # 重启指定的进程，0 是应用 id
pm2 startup            # 产生 init 脚本 保持进程活着，startup 是指系统boot, 开机进程自启动
pm2 unstartup          # 禁用开机进程自启动
pm2 delete 0           # 杀死指定的进程，0 是应用 id，会删除该应用
pm2 delete all         # 杀死全部进程，会删除所有应用

```

