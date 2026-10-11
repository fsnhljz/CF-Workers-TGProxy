# CF-Workers-TGProxy

Cloudflare Workers 的 Telegram Web/MTProto WebSocket 代理中继（KWS Lanes）。

[`telegram_webproxy.js`](./telegram_webproxy.js) 是一个单文件 Cloudflare Workers 实现：它扮演 Telegram 官方 Web Proxy（td web-proxy）后端，把客户端（Desktop / Android）在隔离 webview 里发起的 WebSocket lanes，直接转接到 **Telegram Web K 自己的 KWS 通道**（`kws{dc}[-1].web.telegram.org/apiws`）。

它不再依赖一套独立的 MTProxy / middle proxy 后端：Worker 在本地解开客户端 MTProto 传输层的 obfuscation，再用自己生成的随机密钥重新封一层，拨 Telegram 官方的 Web K 入口。内层 MTProto 载荷始终由客户端端到端加密，Worker 不接触、也不做 mtp 层加解密。

## 核心能力

