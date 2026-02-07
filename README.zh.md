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

### 方式一：官方版本（国际用户）
使用 TeslaMate 官方镜像，适合国际用户使用。

```bash
git clone https://github.com/yekk-me/teslamate-mytess-docker.git
cd teslamate-mytess-docker/script
docker compose up -d
```

**注意**: 官方版本在中国大陆可能遇到地图加载问题。

### 方式二：Mytesla 版本（中国用户推荐）
使用 Mytesla 优化镜像，专门解决中国大陆使用问题。

```bash
git clone https://github.com/yekk-me/teslamate-mytess-docker.git
cd teslamate-mytess-docker/script
docker compose -f docker-compose-with-mytesla.yml up -d
```

**镜像说明**:
- TeslaMate: `mytesla/teslamate:v2.2` （中国优化版，推荐）
- Grafana: `mytesla/grafana:v2.2` （中国优化版，推荐）
- Mytesla 增强组件:
  - `mytesla/auth` - 统一认证服务
  - `mytesla/dash` - 现代化看板界面
  - `mytesla/teslamateapi` - 增强版 API（支持 Mytess 应用）
  - `mytesla/env-adapter` - 环境适配器

**Mytesla 镜像解决的问题**:
- ✓ **地图加载问题**: 内置 OpenStreetMap 代理，解决中国大陆地图无法加载
- ✓ **百度地图支持**: 集成百度地图逆地理编码，位置信息更准确
- ✓ **中文优化**: 界面完整中文化，预配置中文 Grafana 仪表盘
- ✓ **网络优化**: 解决部分服务在中国访问慢的问题

**额外功能**:
- ✓ Traefik 反向代理 + 统一认证系统
- ✓ 现代化的 Mytesla Dash 看板界面（移动端友好）
- ✓ 增强版 TeslaMateAPI（完整支持 Mytess iOS 应用）
- ✓ 简化配置，一个端口统一访问所有服务
- ✓ 生产环境就绪的安全架构

**适用场景**: 中国大陆用户、需要移动端访问、统一认证、现代化界面的用户

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
- **问题反馈:** [GitHub Issues](https://github.com/yekk-me/teslamate-mytess-docker/issues)
- **社区支持:** [Discord](https://discord.com/invite/2DBzQfFPW8)
