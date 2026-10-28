# BMS 网关 · 网页控制台

ESP32-S3 + ML307C 4G 网关的浏览器控制台。纯静态单文件，不需要后端，直接托管即可访问。

## 目录

```
index.html      入口页（选版本）
v2/index.html   完整版 v1.9.41  (899 KB，MQTT 库内置，断网也能打开界面)
v3/index.html   轻量版 v1.8.31  (494 KB，MQTT 库从 CDN 加载)
.nojekyll       告诉 GitHub Pages 不要用 Jekyll 处理
```

## 部署到 GitHub Pages

1. 登录 GitHub，右上角 **+** → **New repository**
2. 仓库名填 `bms-web`，可见性选 **Public**（免费账号只有公开仓库能开 Pages）
3. 创建后点 **uploading an existing file**，把本目录里的 `index.html`、`v2/`、`v3/`、`.nojekyll` 一起拖进去，点 **Commit changes**
4. 进仓库 **Settings** → 左侧 **Pages** → Source 选 **Deploy from a branch** →
   Branch 选 **main** / 目录选 **/(root)** → **Save**
5. 等 1~2 分钟，访问 `https://<你的用户名>.github.io/bms-web/`

> 拖文件夹时保持目录结构不变，`index.html` 必须在仓库根目录，否则入口页打不开。

## 关于线路

| 线路 | 地址 | 说明 |
|---|---|---|
| 免费线（出厂默认） | `wss://broker.emqx.io:8084/mqtt` | 公共 broker，免密、零成本，数据公开可见 |
| 服务线 | `wss://8.163.68.117/mqtt` | 自建 EMQX，需要账号密码 |

页面会按当前协议自动选通道：https 部署时服务线自动用 `wss://8.163.68.117/mqtt`（443，走 Nginx `/mqtt`），
本机 `file://` 打开时用 `ws://8.163.68.117:8084/mqtt`。**不需要手工改地址。**

## 使用前提

- **蓝牙直连**：需要 Chrome / Edge（Web Bluetooth API），iOS Safari 不支持
- **4G 远程**：网关必须在线（4G 通 + MQTT 连上），页面显示「在线设备 0 台」时命令发不出去
- **HTTPS 页面**：浏览器禁止明文 `ws://`，所以公网部署只能用 `wss://`

## 注意

本仓库为公开仓库时，页面里的服务器地址与 MQTT 主题结构（`bms/<mac12>/cmd|data|evt`）会被公开。
免费线是公共 broker 且无鉴权，知道 MAC 的人可以发命令。如果不希望公开，请改用私有仓库（需 GitHub Pro
才能开 Pages），或直接部署到自己的服务器。
