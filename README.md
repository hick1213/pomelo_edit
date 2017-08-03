## 介绍

本仓库主要是修改了pomelo的分布式部署的一些功能，根据官方文档是不支持在多平台异构的机器上进行部署的，主要原因是因为在starter.js中的命令行存在一些绝对化路径的问题，现已经修改。

###	使用

首先将本工程下的node_module下的starter.js拷贝到对应的工程对应的目录中，覆盖原来的js，然后在对应的工程目录中按照config下的servers.json进行配置，和官方不同的是这里增加了一个cmd的参数。

```json
  "connector": [
    {
      "id": "connector-server-1", 
      "host": "xxxx", 
      "port": 4050, 
      "clientPort": 3050, 
      "frontend": true,
      "cmd":"cd \"chatofpomelo-websocket/game-server\" && \"node\" \"app\""
    }
]
```
该参数主要功能是用于指定对应的服务器的目录和相关的执行命令，可以自定义更改。

在slaver服务器上需要在master.json中配置对应的远端的服务器的地址

```json
"development":{
    "id":"master-server-1",
    "host":"127.0.0.1",   <= 这里的地址在slaver服务器上要改为master服务器的地址，端口也是一样的
    "port":3005
},

"production":{
    "id":"master-server-1",
    "host":"127.0.0.1",
    "port":3005
}
```

### 原理

首先要保证主slaver服务器之间可以通过SSH的同用户免密码登录，这个部分可以自行google，然后要在slaver服务器上安装pomelo的相关依赖，确保slaver服务器可以运行pomelo。

`运行原理：通过ssh到slaver服务器上，然后通过命令模式启动从服务器的相关服务`

### Tips

如果是在内网中测试，另外一台机器是在外网的，可能需要用到tcp穿透技术，这里可以用到一些工具，测试使用的工具是ngrok,该工具可以提供一定的转发功能