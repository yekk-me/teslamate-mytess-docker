<!-- README.md -->

# 🇨🇳 TeslaMate-CN-Docker

[![Docker Version](https://img.shields.io/badge/docker-%3E%3D%2020.10-brightgreen)](https://docs.docker.com/)
[![Map Proxy](https://img.shields.io/badge/map-proxy-brightgreen)](https://openstreetmap.org)
[![GitHub stars](https://img.shields.io/github/stars/gococonut/teslamate-cn-docker?style=social)](https://github.com/yourname/teslamate-cn-docker)

> 中国大陆可用的 TeslaMate 容器化方案 Teslamate 汉化 Teslamate 中文| TeslaMate Docker Solution for Mainland China

> [镜像 Repository](https://github.com/gococonut/teslamate)

## 🌟 特性 Features

* ✅ ​**完整汉化** - 界面全面中文化（汉化中）
* 🗺️ ​**地图访问** - 内置代理服务器解决 OpenStreetMap 访问问题
* 🗺️ ​**百度地图** - 支持百度地址逆地址解析，车辆位置信息更精准
* 🐳 ​**一键部署** - Docker Compose 一键部署

## 🚀 快速启动 Quick Start


### 云服务器一键部署

#### 准备工作

1. 购买腾讯云服务器
推荐购买腾讯云新加坡轻量应用服务器：  
配置: 1核2GB，99元/年  
地区: 新加坡（海外节点，访问 Tesla API 更稳定）
购买链接: 腾讯云活动
服务器规格建议: 
内存: 2GB 以上  
存储: 40GB 以上 
系统：Ubuntu 24.04 LTS
2. 购买域名
在腾讯云购买域名： 
    - 进入腾讯云控制台 
3. 选择"域名注册" 
    - 搜索并购买一个便宜的域名（如 .top、.xyz 等后缀）
    - 完成实名认证
4. 配置 DNS 解析
    - 添加 A 记录
    - 在腾讯云 DNS 解析控制台：
        - 主机记录: teslamate（或您喜欢的子域名）
        - 记录类型: A
        - 记录值: 您的服务器公网 IP
        - TTL: 600

#### 一键部署脚本

我们提供了一个交互式安装脚本，可以自动完成所有配置：
登录腾讯云控制台，找到您购买的服务器 
远程登录  免密登录
执行以下命令根据提示操作即可

```sh
bash -c "$(curl -sSL https://s.mytesla.cc/install.sh)"
```

脚本功能特性
* 自动安装 Docker 和 Docker Compose
* 交互式配置环境变量
* 自动生成安全密码
* 配置 SSL 证书（Let's Encrypt）
* 基础认证保护
* 使用 Mytesla 优化镜像
* 支持百度地图精确定位
* 自动启动服务
* 支持更新服务
* 支持备份以及恢复数据

#### Mytesla UI推荐

在成功部署 TeslaMate 后，强烈推荐您使用 [Mytesla UI](https://mytesla.cc)， 现代化新数据看板日常使用可以替换掉 grafana：

Mytesla UI 特色功能
* **实时车辆监控**  电池状态实时显示  充电进度跟踪 位置信息监控
* **数据分析**  详细的行驶数据分析  能耗统计报告  充电费用统计  峰谷用电充电自动计费
* **通知提醒**  充电完成通知  行程完成通知  周期数据统计  更新提醒


### 本地内网部署步骤 Deployment

```bash
# 克隆仓库
git clone https://github.com/gococonut/teslamate-cn-docker.git
cd teslamate-cn-docker

# 配置环境变量
cp .env.example .env
nano .env  # 修改环境变量

# 启动服务
docker-compose up -d
```
