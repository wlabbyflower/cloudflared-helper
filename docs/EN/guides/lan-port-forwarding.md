# Local Area Network Port Forwarding Tutorial

# Local Area Network Port Forwarding Tutorial

## 1. Download and Install

Install LAN Port Forwarding Tool from LCMD store.

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image.png)

> Device access -> entry address -> port forwarding-> visit target address
Always remember **the address on the left** is always accessed.
Port range: 1~65535 (ports above 1024 is preferred, avoid commonly used protocol ports and occupied ports). Always remember test connectivity before creation.
> 

## 2. Configuration Options & Usage Explanation

### 2.1 Entry Address

The 5 options below will be explained one by one.

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%201.png)

**(1) Microserver physical NIC (accessible only within the LAN where the microserver is located)**

Access is restricted to the local area network where the LCMD is located, external networks are prohibited from direct connection. This is suitable for service calls and data transfer within a local area network.

Use case: 

1. **via Microserver physical NIC ——>visit ——> Microserver app**

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%202.png)

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%203.png)

**b. Microserver physical NIC ——>visit ——> Microserver client** (The side at the target address needs to log in to LCMD.)

This allows you to access the device (with LCMD client logged in) for a remote desktop (this is a Python example).

![e2e0c9062afc60af60251257791d1f9a.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/e2e0c9062afc60af60251257791d1f9a.png)

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%204.png)

**c. Microserver physical NIC ——>visit ——> Other network address**

The device within LCMD local area network or LCMD 127.0.0.1 service (127.0.0.1 service itself refers to a **mainframe** with **network_mode: host**, or a container deployed in dockge).

This can be used for remote desktop for devices within same LCMD LAN, or LCMD 127.0.0.1 service (the example shows the service deployed on dockge)

![d580cac63dc39e556dfee685660ee831.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/d580cac63dc39e556dfee685660ee831.png)

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%205.png)

**(2）Microserver virtual NIC (accessible only within the microserver application container)**

Suitable for communication within LCMD application containers, inaccessible directly from the outside.
Provides an isolated network environment to ensure the security of communication between containers.
**Applications in the LCMD  enjoy network isolation by default, similar to the isolation between Docker containers. This option enables forwarding configuration if two applications need to communicate with each other.**

Use case: 

1. One application **——>host.lzcapp:port ——> another application**

![5037f7345afb30279e99f4191e3e90c0.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/5037f7345afb30279e99f4191e3e90c0.png)

**（3）Microserver domain (accessible only after logging in to the microserver client)**

Access via domain name and is restricted to users already logged into LCMD client. This provides authentication-based access control to enhance security.

**Use case:** 

1. **log in the client ——> visit username.heiyu.space:port ——>Microserver client (the side on the target address must log in LCMD client)**

This can be used for remote desktop, allowing access to the device with LCMD client logged(this is a Python example).

 

![63d5dc200035d9200404924db3d91272.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/63d5dc200035d9200404924db3d91272.png)

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%206.png)

**Use case:** 

b. log in the client ——>username.heiyu.space:port ——> other network address: devices within LCMD LAN or LCMD 127.0.0.1 service (127.0.0.1 service itself refers to a mainframe with network_mode: host, or a container deployed in dockge).

This can be used for a remote desktop, access devices within same LCMD LAN or LCMD 127.0.0.1 service.

![dccb6a7b164dccce46d667db7d1cb8fb.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/dccb6a7b164dccce46d667db7d1cb8fb.png)

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%207.png)

**(4) Microserver client (accessible only locally by the mapped client)**

The service is only mapped to the client locally and cannot be accessed by other devices or users. This is suitable for scenarios requiring localized services to protect data privacy.

This forwards the LCMD applications , other client server ports, and other address server ports to a specific device, from which they can be accessed using 127.0.0.1.

Use case:

1. Log in the client → access 127.0.0.1: port → microserver app

![eca3db899f2fcf9655bdfec0536d468a.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/eca3db899f2fcf9655bdfec0536d468a.png)

![7f693c748caf801561cf3d0e8ce77c0b.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/7f693c748caf801561cf3d0e8ce77c0b.png)

**（5）Microserver wildcard address**

Binds to all network interfaces, allowing access from any address.

This is typically used in scenarios involving open services to protect data privacy.

This option can replace all the above configurations except for **microservice client (accessible only locally to mapped clients)**.

### 2.2 Target Address

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%208.png)

**(1) Microserver app**

**Steps:**

Select **apps** (the applications that are already running in LCMD)
Select **Service** (service name in manifest.yml)
Select **Service Protocol** (Default TCP/UDP)
Select the **port** from the dropdown menu.

**Main Purpose:** 

Application service exposure and access.

**Notes:** 

The application needs to be in a running state. If the application is running but not listed, please refresh the page as circled in red.

If you don't know which port the application is forwarding to, you can click the small question mark to find the port.

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%209.png)

**(2) Microserver client**

**Steps:**

Select client network address (hostname)
Select service protocol (default TCP/UDP)
Select the port
(Optional) Add remarks

**Main purpose:** 

Client debugging and access

This can be used for remote access, provided that the other device is also logged into the LCMD client.

![image.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/image%2010.png)

**(3) Other Network Address** 

**Steps:**

Enter the target network address. 

Select the service protocol (default TCP/UDP). 

Enter the port.

(Optional) Add remarks.

**Main purpose:** 

General network forwarding (e.g., dockge: 127.0.0.1 + port in compose,  for networks that LCMD can communicate with: using IP address + service port). 

Connectivity test is mandatory. 

This can be used for remote access to services on devices within the same local network as LCMD. 

dockge syntax.

![e4effd0a2ae23782c738168aeb7e1597.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/e4effd0a2ae23782c738168aeb7e1597.png)

Same LAN as LCMD

![245d7117abf02ebcf5d9a455995614b9.png](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/245d7117abf02ebcf5d9a455995614b9.png)

## Conclusion

Port forwarding can be implemented in various ways, with flexible combinations of entry addresses and target addresses. Please remember always verify connectivity before saving the rules.