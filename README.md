# x-ui

支持多协议多用户的 xray 面板

# 功能介绍

- 系统状态监控
- 支持多用户多协议，网页可视化操作
- 支持的协议：vmess、vless、trojan、shadowsocks、dokodemo-door、socks、http
- 支持配置更多传输配置
- 流量统计，限制流量，限制到期时间
- 可自定义 xray 配置模板
- 支持 https 访问面板（自备域名 + ssl 证书）
- 支持一键SSL证书申请且自动续签
- 更多高级配置项，详见面板

# 安装&升级

```
bash <(curl -Ls https://raw.githubusercontent.com/ctrlyyy/x-ui-j/master/install.sh)
```

## 手动安装&升级

1. 首先从 https://github.com/vaxilu/x-ui/releases 下载最新的压缩包，一般选择 ``amd64``架构
2. 然后将这个压缩包上传到服务器的 “/root/”目录下，并使用 “root”用户登录服务器

> 如果你的服务器 CPU 架构不是 ``amd64``，自行将命令中的 ```````替换为其他架构

```
cd /root/
rm x-ui/ /usr/local/x-ui/ /usr/bin/x-ui -rf
解压x-ui-linux-amd64.tar.gz文件
chmod +x x-ui/x-ui x-ui/bin/xray-linux-* x-ui/x-ui.sh
cp x-ui/x-ui.sh /usr/bin/x-ui
cp -f x-ui/x-ui.service /etc/systemd/system/
mv x-ui/ /usr/local/
systemctl 重新加载守护进程配置
systemctl 启用 x-ui
systemctl重启x-ui
```

## 使用Docker安装

> 此 docker 教程与 docker 镜像由[Chasing66](https://github.com/Chasing66)提供

1. 安装Docker

```外壳
curl -fsSL https://get.docker.com | sh
```

2. 安装x-ui

```外壳
创建目录 x-ui 并进入 x-ui 目录
docker run -itd --网络=主机
    -v $PWD/db/:/etc/x-ui/
    -v $PWD/cert/:/root/cert/ \
    --name x-ui --重启=除非停止
    enwaiax/x-ui:最新
```

> 构建自己的镜像

```外壳
docker build -t x-ui .
```

##SSL证书申请

> 此功能与教程由[FranzKafkaYu](https://github.com/FranzKafkaYu)提供

脚本内置SSL证书申请功能，使用该脚本申请证书，需满足以下条件:

- 知晓Cloudflare 注册邮箱
- 知晓Cloudflare全局API密钥
- 域名已通过Cloudflare解析到当前服务器

获取Cloudflare Global API Key的方法:
    ![](media/bda84fbc2ede834deaba1c173a932223.png)
    ![](媒体/d13ffd6a73f938d1037d0708e31433bf.png)

使用时只需输入 '域名', '邮箱', 'API KEY'即可，示意图如下：
        ![](媒体/2022-04-04_141259.png)

注意事项:

- 该脚本使用DNS API进行证书申请
-默认使用Let's Encrypt作为CA方
- 证书安装目录为/root/cert目录
- 本脚本申请证书均为泛域名证书

## Tg机器人使用（开发中，暂不可使用）

> 此功能与教程由[FranzKafkaYu](https://github.com/FranzKafkaYu)提供

X-UI支持通过Tg机器人实现每日流量通知，面板登录提醒等功能，使用Tg机器人，需要自行申请
具体申请教程可以参考[博客链接](https://coderfan.net/how-to-use-telegram-bot-to-alarm-you-when-someone-login-into-your-vps.html)
使用说明:在面板后台设置机器人相关参数，具体包括

- Tg机器人代币
- Tg机器人聊天ID
- Tg机器人周期运行时间，采用crontab语法  

参考语法：
- 30 * * * * * //每一分的第30秒进行通知
- @小时      //每小时通知
- @每天       //每天通知（凌晨零点整）
- @每8小时    //每8小时通知  

TG通知内容：
- 节点流量使用
- 面板登录提醒
- 节点到期提醒
- 流量预警提醒  

更多功能规划中...
##建议系统

- CentOS 7+
- Ubuntu 16+
- Debian 8+

# 常见问题

## 从 v2-ui 迁移

首先在安装了 v2-ui 的服务器上安装最新版 x-ui，然后使用以下命令进行迁移，将迁移本机 v2-ui 的 `所有 inbound 账号数据`至 x-ui，`面板设置和用户名密码不会迁移`

> 迁移成功后请 `关闭 v2-ui`并且 `重启 x-ui`，否则 v2-ui 的 inbound 会与 x-ui 的 inbound 会产生 `端口冲突`

```
x-ui v2-ui
```

## issue 关闭

各种小白问题看得血压很高

## Stargazers over time

[![Stargazers over time](https://starchart.cc/vaxilu/x-ui.svg)](https://starchart.cc/vaxilu/x-ui)
