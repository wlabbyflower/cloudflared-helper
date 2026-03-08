# CloudFlare 配置指南和端口转发技巧

[TOC]

> **需求背景：用户需把微服内的应用通过域名托管和端口转发的方式，实现公网访问微服内的应用，实现下图界面**

![image-20250313172017688](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313172017827.png?imageSlim) 

## 具体实现

## 1、登录 CloudFlare

- 在微服上打开应用商店——> 下载 CloudFlared 并打开——> 点击右上角切换语言“中文简体”——> 点击底部“ZeroTrust 仪表盘”

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313092316870.png?imageSlim" alt="image-20250313092316818" style="zoom: 33%;" />  

- 这里会出现 CloudFlare 的登录页面——> 点击右上角切换语言“中文简体”——> 这里有 CloudFlare 账号就登录，没有需要注册一个

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313092502039.png?imageSlim" alt="image-20250313092501950" style="zoom: 33%;" />  

- 登录后选择一个账户

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313092802485.png?imageSlim" alt="image-20250313092802438" style="zoom:50%;" /> 

- 这里 CloudFlare 会让您搞一个团队名称，这个根据业务需求自行选择，这里我是直接点击右上角“取消并退出”——> 回到仪表盘主页

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313092937381.png?imageSlim" alt="image-20250313092937325" style="zoom:50%;" /> 

## 2、域名托管

### 1、托管域名配置

- 在仪表盘中输入您事先准备好的域名——> 点击继续

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313094712174.png?imageSlim" alt="image-20250313094712035" style="zoom: 33%;" /> 

- 点击第一个 Free——> 再点继续

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313101203092.png?imageSlim" alt="image-20250313101202977" style="zoom:33%;" /> 

- 继续前往激活——> 继续

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313101326048.png?imageSlim" alt="image-20250313101325938" style="zoom:33%;" /> 

- 在“将您当前的名称服务器替换为 Cloudflare 名称服务器”中的 B 项中的两个域名复制一下（备用）

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313101515942.png?imageSlim" alt="image-20250313101515836" style="zoom: 33%;" />  

- 回到您注册域名的平台，找到域名管理界面——> 修改注册资料——> 把 DNS 服务器换成上一步复制的域名——> 确定提交

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313101950671.png?imageSlim" alt="image-20250313101950604" style="zoom: 33%;" /> 

 

### 2、等待生效

- 更换之后请耐心等待生效时，各个域名平台检测时效性不同，可以用命令测试是否生效

```bash
#在Linux上
dig NS 域名
#如果一直出不来可以更换一下主机的DNS;如：223.5.5.5；8.8.8.8等
#在ANSWER SECTION处产生新配的DNS就证明成功了
```

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313112407970.png?imageSlim" alt="image-20250313112407897" style="zoom:50%;" /> 

### 3、托管完成

- 点击“概况”页面刷新后，出现“好消息！Cloudflare 正在保护您的站点”就证明托管成功了

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313113203312.png?imageSlim" alt="image-20250313113203216" style="zoom: 33%;" />  

## 3、配置转发

### 1、微服关联 Cloudflare

#### 1、创建隧道

- 在 Cloudflare 账户主页侧边导航栏打开“Zero Trust”

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313140851117.png?imageSlim" alt="image-20250313140851055" style="zoom:50%;" />  

- 点击“网络”——> Tunnel——> 创建隧道

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313141058765.png?imageSlim" alt="image-20250313141058692" style="zoom:50%;" /> 

- 隧道类型选择“Cloudflared”

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313141220799.png?imageSlim" alt="image-20250313141220741" style="zoom: 50%;" /> 

- 为隧道命名——> 保存隧道

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313141520534.png?imageSlim" alt="image-20250313141520466" style="zoom:50%;" /> 

#### 2、连接隧道

- 选择 Debin——> 复制左边“如果您的计算机上已安装 cloudflared：”内容

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313142835007.png?imageSlim" alt="image-20250313142834917" style="zoom:50%;" /> 

- 粘贴到微服的这个窗口，点击保存——> 启动

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313142901240.png?imageSlim" alt="image-20250313142901177" style="zoom:50%;" /> 

- 再次点击侧边导航栏“Tunnel”可以看出隧道连接状态正常

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313143255411.png?imageSlim" alt="image-20250313143255321" style="zoom:50%;" /> 

### 2、配置应用转发

#### 1、获取应用端口

下拉端口就可以获取了

![a6aace8f82bbd2374a77235eb09ca5bb](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/202506261623480.png)

#### 2、配置转发

- 打开微服——> 下载“局域网端口转发工具”应用“——> 打开——> 添加映射规则——> 开始填写
- **（需注意！！！一个应用包含前端后端等其他组件，用户需要选择需要映射出去的服务端口）**

```ABAP
注意事项：
左侧：
局域网出口类型：选择“微服虚拟网卡（仅微服应用容器内可访问）”
微服虚拟网卡：“host.lzcapp” 端口：尽量与需要映射的端口一致
右侧：
转发目标类型：微服应用
微服应用：选择需要映射的应用（这一有个注意点：这个下拉应用选项中需要选Running的）server tcp 端口（这三个根据自身服务来）

添加完成之后点击“测速连接”确保能正常——>创建
```

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313160853937.png?imageSlim" alt="image-20250313160853847" style="zoom: 33%;" /> 

#### 3、测试 Cloudflare 应用与转发应用是否正常访问

[微服开启ssh方式](https://developer.lazycat.cloud/ssh.html)

```bash
lzc-docker ps -a |grep cloudfalre
#找出cloudfalre应用运行的容器
lzc-docker exec -it  cloudlazycatappcloudfalredweb-cloudflaredweb-1 bash
#以交互式的 Bash shell，进入cloudfalred的容器
curl -I host.lzcapp:5244
#访问微服的5244端口，-I只显示请求头信息，返回状态码200，证明访问成功
```

![image-20250313162928024](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313162928090.png?imageSlim)

### 4、配置转发域名

#### 1、添加公共主机名

- 回到 cloudfalre 的 web 页面——> 点击隧道后面的“配置”

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313164606608.png?imageSlim" alt="image-20250313164606533" style="zoom: 33%;" /> 

- 点击”已发布应用程序路由“——> 添加已发布应用程序路由

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image-20251008123125863.png?imageSlim" alt="image-20251008123125863" style="zoom: 25%;" /> 

#### 2、公共主机名配置

```bash
子域：可以自定义（一般写转发的应用名称）
域：下拉复选需要的域名
类型：HTTP
URL:host.lzcapp:需要转发的应用服务端口
最后保存主机名
```

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313165105526.png?imageSlim" style="zoom:33%;" /> 

## 4、测试访问

- 分别在手机端、电脑端、命令行端输入 alist.wabby.xyz，都能正常访问了

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313170458825.png?imageSlim" alt="image-20250313170458661" style="zoom: 33%;" /> 

## 5、其他注意事项

为确保系统安全，避免暴露高风险的直连后端端口，应严格控制端口映射和转发策略。对于长期未使用的端口，应及时取消其映射或转发配置，以降低潜在的安全风险，防止因端口暴露而导致的未经授权访问或网络攻击，从而保障系统的稳定性和数据安全。