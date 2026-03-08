# Cloudflare Forwarding MySQL/PostgreSQL

**Note:** 

This tutorial requires two Cloudflare threads to run continuously on the machine connecting to MySQL/PostgreSQL. If this is unacceptable for you, you may skip it.   

The tutorial is applicable not only to MySQL/PostgreSQL, but to any situation where traffic needs to be forwarded via a TCP port. (Here uses MySQL as an example.)

## 1. Preparation & Requirements

(1) Ensure Cloudflared is configured successfully and the connection status is normal.

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image.png)

(2) Ensure LCMD has a MySQL/PostgreSQL service or other TCP service running.

(3) Then you need to confirm whether cloudFlare connects to the service via host.lzcapp or localhost.

## 2. How can I confirm the address of my Cloudflare connection to LCMD TCP service?

(1) host.lzcapp: port 
If the TCP service is provided via the LPK application service without exposing the corresponding port through Ingress, you need to forward the port to the virtual network card (host.lzcapp) using port forwarding tool. The service must then be accessed via **host.lzcapp**.

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%201.png)

(2) [localhost/127.0.0.1](http://localhost/127.0.0.1) 

1. LPK application, but the port is already exposed through the ingress.
2. Started via Dockge and ports exposed through port mapping.

## 3. Important Notes on Cloudflare Setup

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%202.png)

### 4. Configurations Required on the Device Connecting Service

(1) Install and start Cloudflare main process

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%203.png)

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%204.png)

(2) Enable the local traffic distribution service

> cloudflared access tcp --hostname domain --url tcp://127.0.0.1:port
> 

The domain name needs to be the same as the one just published in Cloudflare.

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%205.png)

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%206.png)

(3)Test connectivity

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%207.png)

Note: Connections must be made via 127.0.0.1, not via a domain name.