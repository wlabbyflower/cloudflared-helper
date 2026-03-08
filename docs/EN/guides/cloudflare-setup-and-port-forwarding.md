# CloudFlare Configuration Guide and Port Forwarding Techniques

> Background: Users want to enable public access to applications in LCMD by using domain hosting and port forwarding as shown in the interface below.
> 

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image.png)

# **Instructions**

## 1. Log in CloudFlared

Download and install CloudFlared from LCMD store, select the language you preferred, click ZeroTrust Dashboard:

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%201.png)

The the login page will appear, sign in if you already have an account; if not, sign up first.

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%202.png)

Select your account. 

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%203.png)

Cloudflare will then prompt you to create a team name, which you can customize according to your business needs. I here clicked "**Cancel and Exit**" on the top-right corner to return to the main dashboard.

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%204.png)

## 2. Domain Hosting

### 2.1 Configurations

Enter your **domain name** pre-prepared in the dashboard —> Click **Continue**

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%205.png)

Click first option **Free** and **Continue.**

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%206.png)

Click **Continue to activation** under ****Review your DNS records page

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%207.png)

Copy two domain names from “Replace your current nameservers with Cloudflare nameservers”.

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%208.png)

Return to the website where you registered your domain, find the Domain Management Interface -> Modify Registration Information -> Modify DNS (paste them from last step) -> Submit

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%209.png)

### 2.2 Take Effect

Please wait patiently for it to take effect after the change. The time it takes for different domain name platforms may vary, you can use a command to test if it takes effect.

```jsx
# on Linux
dig NS domain name
# If you still can't get it, try changing your host's DNS; for example: 223.5.5.5; 8.8.8.8, etc.
# under ANSWER SECTION, two new NS are displayed, success!
```

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%2010.png)

### 2.3. Hosting Completed

Click **Overview**, it displays “ **Your domain is now protected by Cloudflare**”, hosting successfully!

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%2011.png)

## 3. Configure Forwarding

### 3.1 Connect LCMD to Cloudflare

**3.1.1 Create Tunnel**

Click **Networking** ->**Tunnels** -> **Create Tunnel**

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%2012.png)

Enter tunnel name.

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%2013.png)

**3.1.2 Connect Tunnel**

Select **Debian**  -> Copy the token

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%2014.png)

Paste it in LCMD Cloudflared-web window as shown, then click **Start**

![44d53c68a1e8e68e03061198fa246812.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/44d53c68a1e8e68e03061198fa246812.png)

Click **Tunnel** on left sidebar, you can see the status of the tunnel: Healthy.

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%2015.png)

### 3.2 Configure Application Forwarding

**3.2.1 Get Application Port**

Get the port from drop-down menu.

![5037f7345afb30279e99f4191e3e90c0.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/5037f7345afb30279e99f4191e3e90c0.png)

**3.2.2 Configure Forwarding**

Open LCMD client -> download LAN Port Forwarding Tool -> Add Mapping Rules -> Edit Forwarding items.

Note:

```jsx
Left side: 
LAN entry type: Microserver virtual NIC(accessible only within the microserver applicatinn container)
Microserver virtual NIC:  **host.lzcapp**

Right side: 
Forwarding target type: microserver app;
LCMD microserver app: select the application that needs to be mapped(application must be in running status)
server:tcp

Ports on left/right side should be the same.
```

At last, click T**est connectivity** and create.

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%2016.png)

**3.2.3 Accessibility Test for Cloudflare & Forwarding Applications**
How to enable SSH ?

[开启 SSH | 懒猫微服开发者手册](https://developer.lazycat.cloud/ssh.html)

```jsx
lzc-docker ps -a |grep cloudfalre
# Find the containers where your CloudFalre is running.
lzc-docker exec -it cloudlazycatappcloudfalredweb-cloudflaredweb-1 bash
# Enter the CloudFalred container using an interactive Bash shell.
curl -I host.lzcapp:5244
# Accessing LCMD's 5244 port,  -I  only displays the request header information. 200 status code indicates a successful access.
```

![59c2abd08018b677f6fb9280f3135155.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/59c2abd08018b677f6fb9280f3135155.png)

### **3.3 Configure Forwarding Domain Names**

Go back to Cloudflared Web side —> Click the 3-dot icon —> Click **Configure.**

![d85f7b94e8d80ce7661fb432d879cba1.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/d85f7b94e8d80ce7661fb432d879cba1.png)

Then click **Add route** 

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%2017.png)

Then click **Add published application.**

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%2018.png)

```jsx
Subdomain: you can customize it(Application name forwarded is recommended)
Domain: select domain name from drop-down menu.
Service URL: http://host.lzcapp:port (Application port that needs to forward)
```

![b58e8c32a12b0d9a648e40d103323173.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/b58e8c32a12b0d9a648e40d103323173.png)

![b8ad0add2681cb3f5f01b7c3d7ee6ee5.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/b8ad0add2681cb3f5f01b7c3d7ee6ee5.png)

## 4. Test Access

Entering **alist.mhwy.store** on the mobile phone, computer, and command line, all worked correctly.

![bb08feb51cf8f15675eebd2bf688103a.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/bb08feb51cf8f15675eebd2bf688103a.png)

## 5. Other Precautions

To ensure system security and avoid exposing high-risk directly connected backend ports, port mapping and forwarding policies shall be strictly controlled. For ports that have not been used for a long time, their mapping or forwarding configurations shall be promptly cancelled to reduce potential security risks, prevent unauthorized access or network attacks caused by port exposure, and thus ensure system stability and data security.