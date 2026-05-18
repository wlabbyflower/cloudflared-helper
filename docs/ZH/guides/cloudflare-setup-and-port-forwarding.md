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
