# CDN + 代理配合使用指南

通过 CDN 隐藏代理服务器真实 IP，提升抗封锁和防御能力。

## 为什么用 CDN

- 隐藏源站 IP: 攻击者无法直接攻击服务器
- 访问加速: CDN 边缘节点就近访问
- DDoS 防护: 吸收恶意流量
- 抗封锁: 伪装成正常网站流量

## Cloudflare 免费套餐

Cloudflare 提供免费 CDN：

1. 注册 Cloudflare
2. 添加你的域名
3. 修改 NS 服务器
4. 等待生效

## 代理 + CDN 方案

### 方案1: WebSocket + CDN

```
用户 > CDN > WebSocket代理 > 目标
     (隐藏真实IP)
```

V2Ray WebSocket 配置：

```json
{
  "inbounds": [{
    "port": 10000,
    "listen": "127.0.0.1",
    "protocol": "vmess",
    "streamSettings": {
      "network": "ws",
      "wsSettings": {"path": "/ray"}
    }
  }]
}
```

Nginx 反代：

```nginx
location /ray {
    proxy_pass http://127.0.0.1:10000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
}
```

## 注意事项

- CDN 只支持 HTTP/2 和 WebSocket，不支持 gRPC
- 部分 CDN 可能影响延迟
- 免费 CDN 节点可能被墙识别

## 常见问题

**CDN 速度慢？** 选择距离你较近的 Cloudflare 节点。

---

推荐工具：

- [Clash for Windows](https://clashforwindows.site/)
- [ClashMI](https://clashmi.site/)
- [FlClash](https://flclash.us/)
