# cloudflared-helper (English)

Cloudflare configuration and port-forwarding documentation repository for LCMD scenarios.

## Entry Points

- [Global docs index](docs/README.md)
- [English docs index](docs/EN/README.md)
- [中文文档索引](docs/ZH/README.md)

## Recommended Reading Order

1. [Cloudflare Configuration Guide and Port Forwarding Techniques (main flow)](docs/EN/guides/cloudflare-setup-and-port-forwarding.md)
2. [Cloudflare Forwarding MySQL / PostgreSQL (TCP scenario)](docs/EN/guides/cloudflare-mysql-postgresql.md)
3. [Local Area Network Port Forwarding Tutorial](docs/EN/guides/lan-port-forwarding.md)

## FAQ: Tunnel Degradation

If tunnel degradation occurs, set the Cloudflare app environment variable on the client and restart:

```bash
PROTOCOL=auto
```

![tunnel-downgrade-1](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/d0a9869a26cb4696a424e6f9cfe26493.png?imageSlim)
![tunnel-downgrade-2](https://lzc-playground-1301583638.cos.ap-chengdu.myqcloud.com/guidelines/395/ae0ae1a7a8f84b48d282c6d96218a70f.png?imageSlim)
