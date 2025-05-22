# OceanGuide

## 项目简介

OceanGuide 是一个集成了前端、后端、算法与数据处理的综合性项目，旨在为用户提供高效的路径规划与地理信息服务。项目采用微服务架构，后端基于 Python FastAPI，前端采用现代 Web 技术，集成了 TSP（旅行商问题）求解算法，并支持 Docker 部署。

---

## 目录结构

```
OceanGuide/
├── PPT.pptx                # 项目演示PPT
├── README.md               # 项目说明文档
├── 技术文档.pdf            # 技术文档
├── 演示视频.mp4            # 项目演示视频
├── .idea/                  # IDE 配置文件
├── docker/
│   ├── backend.jar         # 后端服务Jar包（如有Java服务）
│   ├── docker-compose.yml  # Docker Compose 配置
│   ├── Dockerfile.backend  # 后端服务Dockerfile
│   └── database/
│       └── poi_data/
│           ├── 相关数据说明.docx
│           └── 数据集/
│   └── frontend/
│       └── kqgis/          # 前端代码
│   └── WorkSpace/
│       ├── Dockerfile.python      # Python服务Dockerfile
│       ├── get_plan.py           # 路径规划API
│       ├── map_recovery_api.py   # 地图恢复API
│       ├── open_server           # 服务器启动脚本
│       ├── plan.log              # 日志文件
│       ├── requirements.txt      # Python依赖
│       ├── test.py               # 测试脚本
│       ├── beijing/              # 北京相关数据
│       └── tsp-solver-master/    # TSP算法库
```

---

## 快速开始

### 1. 环境准备

- 安装 [Docker](https://www.docker.com/)
- 克隆本项目到本地

### 2. 构建与运行

#### 使用 Docker Compose 一键启动

```sh
cd docker
docker-compose up --build
```

#### 单独构建 Python 路径规划服务

```sh
cd docker/WorkSpace
docker build -f Dockerfile.python -t ocean_guide_python .
docker run -p 7780:7780 ocean_guide_python
```

服务启动后，FastAPI 应用将监听在 `http://localhost:7780`。

---

## 主要功能

- 路径规划（TSP 求解）
- 地图数据恢复与处理
- 前端地图可视化
- 多种数据集支持
- Docker 化部署，便于迁移和扩展

---

## 路径规划 API

后端服务基于 FastAPI，主要入口为 [`get_plan.py`](docker/WorkSpace/get_plan.py)。

### 启动命令

```sh
uvicorn get_plan:app --reload --host 0.0.0.0 --port 7780
```

### 示例请求

```http
POST /plan
Content-Type: application/json

{
  "points": [[x1, y1], [x2, y2], ...]
}
```

### 返回结果

```json
{
  "route": [0, 2, 1, ...],
  "distance": 123.45
}
```

---

## TSP 算法库

项目集成了 [`tsp_solver2`](docker/WorkSpace/tsp-solver-master/)，支持 4000 点以内的 TSP 问题求解，算法为贪心近似，适合大规模数据的快速路径规划。

- 主要代码位于 [`tsp_solver/`](docker/WorkSpace/tsp-solver-master/tsp_solver/)
- 支持 Numpy、Matplotlib 可视化
- 可独立运行 demo 或测试

---

## 依赖安装

Python 依赖见 [`requirements.txt`](docker/WorkSpace/requirements.txt)：

```sh
pip install -r requirements.txt
```

---

## 数据与演示

- 数据集位于 [`docker/database/poi_data/`](docker/database/poi_data/)
- 项目演示视频、PPT、技术文档分别为 [`演示视频.mp4`](演示视频.mp4)、[`PPT.pptx`](PPT.pptx)、[`技术文档.pdf`](技术文档.pdf)

---

## 贡献与许可

- 本项目部分代码基于开源协议（如 TSP Solver 为 Public Domain）
- 欢迎提交 Issue 与 Pull Request

---

## 联系方式

- 作者：Wenbin Xu
- 邮箱：<your-email@example.com>
- 相关链接：[TSP Solver 项目主页](https://github.com/dmishin/tsp-solver)
