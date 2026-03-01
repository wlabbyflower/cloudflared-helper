> 注意⚠️：本教程最后需要在连接Mysql/PostgreSql的机器上常驻两个ChloudFlare进程，如觉不可接受的本教程将不适用于，避免浪费您的时间。
> 本教程适用范围不仅仅是Mysql/PostgreSql，适用所有需要通过TCP端口转发流量的情况。（本教程以Mysql例）



## 🔧 准备工作
- 确保CloudFlare配置成功，并且连接状态是正常的。
- 首先确保微服上有mysql/PostgreSql服务或者其他TCP服务。
- 需要确认cloudFlare通过host.lzcapp连接服务还是通过localhost连接服务。
![图片.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/1ca971a0-cce5-4084-b6c8-438bb820fe6b.png?imageSlim "图片.png")
### 如何确认自己的CloudFlare连接微服tcp服务的地址是什么

#### 1️⃣ host.lzcapp:端口号 连接
- 如果是tcp服务是通过LPK应用服务提供的，且没有通过ingress暴露对应的端口。此时需要通过“端口转发”将端口转发到“虚拟网卡”（host.lzcapp）上。此时就需要通过“host.lzcapp”连接服务。
![图片.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/a1a946a7-2680-4b8f-9603-8887a745efd7.png?imageSlim "图片.png")

#### 2️⃣ localhost/127.0.0.1 连接
- 1. LPK应用，但是端口已经通过ingress暴露出来了的。
- 2. 通过dockge启动并通过port暴露端口的。
![图片.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/82186c43-b9be-47bd-85c5-eca95fde7291.png?imageSlim "图片.png")

## 🚀 CloudFlare配置需要注意的地方
![图片.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/e6231871-a50a-4746-9a68-c9a3ac897a08.png?imageSlim "图片.png")

## 🚀 连接服务的机器上需要添加的配置
#### 1️⃣ 安装并开启CloudFlare 主进程
![图片.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/e33516d6-2c16-43f4-83aa-f20cfaf981e9.png?imageSlim "图片.png")
![图片.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/39734d5d-181e-4f7b-ab3a-75a93c1c3fdb.png?imageSlim "图片.png")
#### 2️⃣ 本地开启流量分发服务
``` shell
cloudflared access tcp --hostname 域名 --url tcp://127.0.0.1:端口号
```
> 域名需要和cloudFlare中的一致
> ![图片.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/0df6de3b-cc81-49c5-8918-37f9b5a7c197.png?imageSlim "图片.png")
> ![图片.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/70787aa3-a1e2-458e-ba13-4c01be4f6f85.png?imageSlim "图片.png")
#### 3️⃣ 测试连接
![图片.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/bc3760a5-2f69-412b-b3ba-a7ae7d1b2bc3.png?imageSlim "图片.png")
> 注意 ⚠️： 连接必须通过127.0.0.1去连接，不能够通过域名去连接。