# cloudflared-helper（中文）

Cloudflare 配置与端口转发文档仓库（懒猫微服场景）。

## 文档入口

- [文档总索引](docs/README.md)
- [中文文档索引](docs/ZH/README.md)
- [English Docs Index](docs/EN/README.md)

## 推荐阅读顺序

1. [Cloudflare 配置指南和端口转发技巧（主流程）](docs/ZH/guides/cloudflare-setup-and-port-forwarding.md)
2. [Cloudflare 转发 MySQL / PostgreSQL（TCP 场景）](docs/ZH/guides/cloudflare-mysql-postgresql.md)
3. [局域网端口转发教程（入口/目标地址说明）](docs/ZH/guides/lan-port-forwarding.md)

## 常见问题：隧道降级

如果遇到隧道降级，可在客户端调整 Cloudflare 应用环境变量并重启：

```bash
PROTOCOL=auto
```

![tunnel-downgrade-1](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/d0a9869a26cb4696a424e6f9cfe26493.png?imageSlim)
![tunnel-downgrade-2](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/ae0ae1a7a8f84b48d282c6d96218a70f.png?imageSlim)
