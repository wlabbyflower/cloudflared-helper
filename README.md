CloudFlare 配置指南和端口转发技巧
[cloudflared-helper](https://github.com/wlabbyflower/cloudflared-helper/blob/main/cloudflared-helper.md)

如果遇到隧道降级

![d0a9869a26cb4696a424e6f9cfe26493](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/d0a9869a26cb4696a424e6f9cfe26493.png?imageSlim)

这个需要在客户端修改CloudFlare的环境变量，修改完成，重启CloudFlare应用即可

```
PROTOCOL=auto
```

![ae0ae1a7a8f84b48d282c6d96218a70f](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/ae0ae1a7a8f84b48d282c6d96218a70f.png?imageSlim)
