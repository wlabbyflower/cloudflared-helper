# CloudFlare 配置指南和端口转发技巧（本攻略出现两个域名不影响您的正常使用）

[TOC]

> **需求背景：用户需把微服内的应用通过域名托管和端口转发的方式，实现公网访问微服内的应用，实现下图界面**（可通过自己的域名进行访问微服务内的应用）

![image-20250313172017688](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313172017827.png?imageSlim) 

## 具体实现

## 1、登录 CloudFlare（网址：https://www.cloudflare-cn.com/enterprise/（国内）(https://www.cloudflare.com/zh-cn/（国际））

- 在微服上打开应用商店——> 下载 CloudFlared UI 并打开——> 点击右上角切换语言“中文简体”——>进行网站登录

  ![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/ecafb15f-0546-418c-ad89-b517c0d3c617.png "image.png")

- 通过网站进入到CloudFlare 的登录页面——> 点击右上角切换语言“中文简体”——> 这里有 CloudFlare 账号就登录，没有需要注册一个

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

- 回到您注册域名的平台（比如阿里云腾讯云等这里展示阿里具体流程），找到域名管理界面——> 修改注册资料——> 把 DNS 服务器换成上一步复制的域名——> 确定提交

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/99b9210f-c199-4744-8b2b-1c47d259b922.png "image.png")

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/d59b9a25-da0b-41ad-b74b-2352325cca03.png "image.png")

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/46f9aa33-c1be-4ec8-9daa-66a4a3dfcfa5.png "image.png")

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/eb9760ec-2c40-4fd2-b5d0-af0250340e4b.png "image.png")

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/8f0c05c4-6e7e-44aa-a6d1-f66fe0bf69f5.png "image.png")

### 2、等待生效

- 更换之后请耐心等待生效时，各个域名平台检测时效性不同，可以用命令测试是否生效

```bash
#在Linux上
dig NS 域名
#如果一直出不来可以更换一下主机的DNS;如：223.5.5.5；8.8.8.8等
#在ANSWER SECTION处产生新配的DNS就证明成功了
#在Windows上
nslookup -qt=ns 你的域名
```

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313112407970.png?imageSlim" alt="image-20250313112407897" style="zoom:50%;" /> 

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/e30d6e29-7567-4e6d-943d-57784e634cbb.png "image.png")

### 3、托管完成

- 点击“概况”页面刷新后，出现“好消息！Cloudflare 正在保护您的站点”就证明托管成功了

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313113203312.png?imageSlim" alt="image-20250313113203216" style="zoom: 33%;" />  

## 3、配置转发

### 1、微服关联 Cloudflare

#### 1、创建隧道

- 在 Cloudflare 账户主页侧边导航栏打开“Zero Trust”

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/afdcbcc0-40f9-41bc-9ea4-23043a2e8201.png "image.png")

- 点击“网络”——> Tunnel——> 创建隧道

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/499f1df1-4b4a-4d75-b20a-4975805329ea.png "image.png") 

- 隧道类型选择“Cloudflared”

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313141220799.png?imageSlim" alt="image-20250313141220741" style="zoom: 50%;" /> 

- 为隧道命名——> 保存隧道

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/38f2ca09-9b5f-4e71-839e-0dc9a26241ac.png "image.png")

#### 2、连接隧道

- 选择 Debin——> 复制左边“如果您的计算机上已安装 cloudflared：”内容

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/63008548-e826-464d-a721-03660478bed8.png "image.png")

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/12057a8c-abcd-4077-a0c7-82c1bc60178e.png "image.png")

- 粘贴到微服的这个窗口，点击保存——> 启动

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/f861a3c9-c3d9-460e-a53c-b83fa7fce1af.png "image.png")

- 再次点击侧边导航栏“Tunnel”可以看出隧道连接状态正常

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/616da8f7-4930-48e4-a80c-20c3010a8829.png "image.png")

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/3d6282b5-2a14-4532-91fd-16801887f76b.png "image.png")

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/17559db1-86f6-46e3-b463-4cd8c15714b8.png "image.png")

`先完成配置应用的转发`

### 2、配置应用转发

#### 1、获取应用端口配置转发

- 打开微服——> 下载“局域网端口转发工具”应用“——> 打开——> 添加映射规则——> 开始填写
- **（需注意！！！一个应用包含前端后端等其他组件，用户需要选择需要映射出去的服务端口）**

```ABAP
注意事项：
右侧：
局域网出口类型：选择“微服虚拟网卡（仅微服应用容器内可访问）”
微服虚拟网卡：“host.lzcapp”
出口地址：默认
端口：尽量与需要映射的端口一致
转发目标类型：微服应用
微服应用：选择需要映射的应用（这一有个注意点：这个下拉应用选项中需要选Running的）server tcp 端口（这三个根据自身服务来）

添加完成之后点击“测速连接”确保能正常——>创建
```

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/e367025e-94ff-4cb5-8c50-1352168298e6.png "image.png")

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/1729a2a5-7a7d-4fa6-bf48-7c28f4aed89a.png "image.png")

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/f14b8565-4fb0-4090-b365-acc08baa2e2e.png "image.png")

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

- 回到 cloudfalre 的 web 页面——> 输入URL

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/e7cfdef2-ad22-4c14-8207-2edece94d0ee.png "image.png")

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/1290/8660156b-0a71-4025-bebc-a3232d08a267.png "image.png")

## 4、测试访问

- 分别在手机端、电脑端、命令行端输入 alist.wabby.xyz（测试时输入自己定义的主机名），都能正常访问了

<img src="https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/20250313170458825.png?imageSlim" alt="image-20250313170458661" style="zoom: 33%;" /> 

## 5、其他注意事项

为确保系统安全，避免暴露高风险的直连后端端口，应严格控制端口映射和转发策略。对于长期未使用的端口，应及时取消其映射或转发配置，以降低潜在的安全风险，防止因端口暴露而导致的未经授权访问或网络攻击，从而保障系统的稳定性和数据安全。