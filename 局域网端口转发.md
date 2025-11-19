# 局域网端口转发教程




# 局域网端口转发访问流程：

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118115456587.png?imageSlim" alt="image-20251118115456587" style="zoom:33%;" /> 

> 设备访问——>入口地址——>通过端口转发后——>访问目标地址
>
> 始终记住访问的永远是左边的地址
>
> 注意端口范围：1~65535（尽量用1024以上的端口，避免一些常用协议端口，避免转发一些使用过的端口，创建之前一定先检测连通性）

## 各个配置选项说明以及使用方法

## 入口地址

### 1、微服网卡 (仅微服所在局域网可访问)

限制在微服所在的局域网内访问，外部网络无法直接连接

适用于局域网内部的服务调用和数据传输

#### 适用使用举例：

##### 通过微服网卡IP——>访问——>微服应用服务

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118121830837.png?imageSlim" alt="image-20251118121830837" style="zoom: 33%;" /> 

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118121940734.png?imageSlim" alt="image-20251118121940734" style="zoom:50%;" /> 

##### 通过微服网卡IP——>访问——>微服登录的客户端主机上的服务（目标地址的客户端要登录懒猫微服应用）

这个可以用远程桌面，访问登录客户端的设备（示例演示的是一个python的例子）

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118123002780.png?imageSlim" alt="image-20251118123002780" style="zoom:33%;" /> 

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118123220926.png?imageSlim" alt="image-20251118123220926" style="zoom: 50%;" /> 

##### 通过微服网卡IP——>访问——>其他网络地址：微服局域网设备或者微服127.0.0.1的服务（本身127.0.0.1的服务是指：mainframe中有network_mode：host的，或者dockge部署的容器）

这个可以用远程桌面，微服局域网设备或者微服127.0.0.1的服务（示例演示dockge起的服务）

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118130601733.png?imageSlim" alt="image-20251118130601733" style="zoom:33%;" /> 

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118130714411.png?imageSlim" alt="image-20251118130714411" style="zoom:50%;" /> 

### 2、微服虚拟网卡 (仅微服应用容器可访问)

适用于微服应用容器内部的通信，外部无法直接访问

提供隔离的网络环境，确保容器间通信的安全性

**懒猫微服平台中的应用默认享有网络隔离，类似Docker容器间隔离。若需两个应用互访，此选项来实现转发配置。**

#### 适用使用举例：

应用访问——>host.lzcapp:端口——>另一个应用服务

[以Radarr和Jackett为例](https://playground.lazycat.cloud/#/guideline/295)

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250605165748100.png?imageSlim" alt="image-20250409132826584" style="zoom:33%;" /> 

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250605165748091.png?imageSlim" alt="image-20250409140008473" style="zoom: 50%;" /> 

### 3、微服域名 (仅登录微服务户端可访问)

通过域名访问，仅限于已登录微服务客户端的用户

提供基于身份验证的访问控制，增强安全性

#### 适用使用举例：

##### 登录客户端——>访问：$微服名.heiyu.space:端口——>微服登录的客户端主机上的服务（目标地址的客户端要登录懒猫微服应用）

这个可以用远程桌面，访问登录客户端的设备（示例演示的是一个python的例子）

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118132638385.png?imageSlim" alt="image-20251118132638385" style="zoom:33%;" /> 

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118132738392.png?imageSlim" alt="image-20251118132738392" style="zoom:50%;" />  

##### 登录客户端——>访问：$微服名.heiyu.space:端口——>其他网络地址：微服局域网设备或者微服127.0.0.1的服务（本身127.0.0.1的服务是指：mainframe中有network_mode：host的，或者dockge部署的容器）

这个可以用远程桌面，微服局域网设备或者微服127.0.0.1的服务（示例演示微服局域网的Ubuntu的ssh远程）

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118132901093.png?imageSlim" alt="image-20251118132901093" style="zoom:33%;" />  

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118133025952.png?imageSlim" alt="image-20251118133025952" style="zoom:50%;" />  

### 4、微服务户端 (仅映射的客户端本地可访问)

服务仅映射到客户端本地，其他设备或用户无法访问

适用于需要本地化服务的场景，保护数据隐私

这个就相当与把微服应用服务、其他客户端的服务端口、其他地址服务端口，转到某一个设备上面，在那个设备上面使用127.0.0.1即可访问

#### 适用使用举例：

##### 登录客户端——>本地客户端访问127.0.0.1:端口——>微服应用服务、其他客户端的服务端口、其他地址服务端口

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118135539336.png?imageSlim" alt="image-20251118135539336" style="zoom: 33%;" /> 

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118135613205.png?imageSlim" alt="image-20251118135613205" style="zoom: 50%;" /> 

### 5、微服通配地址 (0.0.0.0)

绑定到所有网络接口，允许从任意地址访问

通常用于开放服务的场景，保护数据隐私

这一个配置可以代替以上除了   微服务户端 (仅映射的客户端本地可访问)  的所有配置

## 目标转发地址

#### 1、微服应用

##### 操作过程：

选择应用（映射懒猫已运行的应用服务）

选择服务(mainfest.yml里的service名称)

选择服务协议（默认tcp/udp）

可下拉选择映射的端口

##### 主要用途：

应用的服务暴露与访问

##### 注意事项：

应用需要running状态，如果启动了应用，但是列表里面没有running，可以点一下刷新

如果不知道转发应用那个端口，可以点击端口旁边的小问号，查看具体服务对应的具体端口

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118140517380.png?imageSlim" alt="image-20251118140517380" style="zoom: 33%;" /> 

### 2、微服客户端

##### 操作过程：

选择客户端网络地址（主机名称）

选择服务协议（默认tcp/udp）

选择需要映射的端口

（可选）添加备注

##### 主要用途：

客户端调试与访问

这个可以用作远程访问，前提是对端设备也登陆了懒猫微服客户端

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118140604164.png?imageSlim" alt="image-20251118140604164" style="zoom:33%;" /> 

### 3、其他网络地址

##### 操作过程：

输入目标网络地址

选择服务协议（默认tcp/udp）

输入需要映射的端口

（可选）添加备注

##### 主要用途：

通用网络转发（如：dockge写127.0.0.1+端口compose里的端口、微服能通信的网络：写IP地址+服务端口）

一定要测试连通性

可用用作远程访问微服同局域网的设备上的服务

dockge的写法

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118140845642.png?imageSlim" alt="image-20251118140845642" style="zoom: 33%;" /> 

微服同局域网的写法

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251118140942224.png?imageSlim" alt="image-20251118140942224" style="zoom:33%;" /> 

## 结语

端口转发玩法性很多，入口地址与目标地址可以有很多种搭配方式，规则保存之前一定要测试连通性

