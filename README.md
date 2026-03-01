# cloudflared-helper

Cloudflare 配置与端口转发文档仓库（懒猫微服场景）。

本仓库仅保留中文文档。

## 文档导航

### 核心指南
- [Cloudflare 配置指南和端口转发技巧（主流程）](docs/guides/zh/cloudflared-helper.md)
- [Cloudflare 转发 MySQL / PostgreSQL（TCP 场景）](docs/guides/zh/cloudflare-mysql-postgresql.md)
- [局域网端口转发教程（入口/目标地址说明）](docs/guides/zh/lan-port-forwarding.md)

### 索引与结构
- [docs 文档索引](docs/README.md)

## 常见问题：隧道降级

如果遇到隧道降级，可在客户端调整 Cloudflare 应用环境变量并重启：

```bash
PROTOCOL=auto
```

![tunnel-downgrade-1](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/d0a9869a26cb4696a424e6f9cfe26493.png?imageSlim)
![tunnel-downgrade-2](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/ae0ae1a7a8f84b48d282c6d96218a70f.png?imageSlim)
