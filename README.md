# MySpeed CDN Auto

MySpeed-CN 的 CDN 节点自动更新工具。自动检测 `servers.js` 中失效的 CDN 下载链接，并从互联网拉取有效替代链接进行替换。

## 功能

- 🔍 自动检测 `CDN_SERVERS` 中所有下载链接的可用性（HEAD + GET 双重检测）
- 🌐 从互联网 5 大来源自动发现新的 CDN 链接
- 🔄 按 CDN 域名分组自动替换失效链接
- 📦 备用链接池持久化管理（自动验证清理）
- 📋 JSON 格式检测报告

## 用法

```bash
# 检测并修复
node scripts/update-cdn-nodes.mjs

# 仅检测，不修改
node scripts/update-cdn-nodes.mjs --check-only

# 详细输出
node scripts/update-cdn-nodes.mjs --verbose
```

## 自动发现来源

| 来源 | 说明 |
|------|------|
| GitHub 仓库 | 拉取 speedtest_cn 等维护的 URL 列表 |
| APP 下载源 | 字节/阿里/腾讯/新浪/网易/拼多多/360 等大厂 APP 直链 |
| 字节 CDN 变体 | 扫描 lf3/lf6/lf9 × bytegoofy/bytedance 组合 |
| 运营商 CDN | 和彩云/天翼云/腾讯云固定链接 |
| APP 更新接口 | 抖音版本检查 API（动态获取最新链接） |

## 定时任务

```bash
# 每天凌晨 3 点自动运行
0 3 * * * cd /path/to/myspeed-cdn-auto && node scripts/update-cdn-nodes.mjs >> /var/log/cdn-update.log 2>&1
```

## 文件说明

```
scripts/
├── cdn-discovery.mjs      # CDN 链接自动发现模块
└── update-cdn-nodes.mjs   # 主脚本：检测 + 替换 + 报告
```

## 纯 Node.js

零外部依赖，仅使用 Node.js 内置模块（`fs`、`path`、`fetch`）。需要 Node.js 18+。
