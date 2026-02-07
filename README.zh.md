[English](README.md) | [中文](README.zh.md)

# TeslaMate 部署脚本

> 本项目为 TeslaMate 提供 Docker Compose 部署配置,专门优化用于 Mytess iOS 应用连接。

## 中文文档说明

本项目主要面向国际用户,主要文档为英文版本。

**快速链接:**
- [English README](README.md) - 完整的项目介绍和部署说明
- [部署脚本目录](script/) - Docker Compose 配置文件

## 主要特性

- 一键部署 TeslaMate
- 预配置 TeslaMateAPI (用于 Mytess 应用连接)
- 支持 Mytesla Dash 现代化看板
- Traefik 反向代理和统一认证

## 快速开始

详细部署说明请参阅 [英文文档](README.md)。

### 基础部署
```bash
git clone https://github.com/gococonut/teslamate-cn-image.git
cd teslamate-cn-image/script
docker compose up -d
```

### 包含 Mytesla Dash
```bash
git clone https://github.com/gococonut/teslamate-cn-image.git
cd teslamate-cn-image/script
docker compose -f docker-compose-with-mytesla.yml up -d
```

## Mytess iOS 应用

**Mytess** 是一款原生 iOS 应用,可将您的 TeslaMate 数据转化为可操作的洞察。

### 主要功能
- 智能驾驶洞察 ("金脚"评分、安全距离分析)
- 智能充电成本计算 (地理围栏、峰谷电价)
- 交互式地图模式
- 实时通知 (充电状态、灵动岛支持)
- 隐私优先 (数据保存在您自己的服务器)

**下载:** [App Store](https://apps.apple.com/app/id6757828502) | **官网:** [mytess.net](https://mytess.net)

## 相关链接

- **Mytess iOS 应用:** [mytess.net](https://mytess.net)
- **TeslaMate 官方文档:** [docs.teslamate.org](https://docs.teslamate.org)
- **问题反馈:** [GitHub Issues](https://github.com/gococonut/teslamate-cn-image/issues)
- **社区支持:** [Discord](https://discord.com/invite/2DBzQfFPW8)
