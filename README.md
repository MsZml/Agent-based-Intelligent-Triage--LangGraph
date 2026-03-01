# 医疗智能分诊系统

# 项目介绍
本项目是基于 LangGraph 构建的医疗智能分诊系统，通过大语言模型（LLM）+ 智能体（Agent）+ 工具调用的模式，实现用户医疗问题的意图分析、智能工具调用（如健康档案检索、数值计算）、检索结果相关性评分、查询重写及最终回复生成的全流程自动化分诊。
## 系统核心优势：
基于状态图（StateGraph）实现可视化、可追溯的对话工作流
支持多工具并行调用，提升响应效率
集成 PostgreSQL+pgvector 实现对话状态和用户记忆的持久化存储
适配多类大模型（OpenAI/GPT、阿里通义千问、Ollama 本地模型）
提供 FastAPI 接口和 Gradio 可视化 WebUI，支持多端交互
# 📋 环境要求
Python 3.11（推荐使用 Conda 管理环境）
PostgreSQL 15+（需安装 pgvector 扩展）
Docker（用于快速部署 PostgreSQL）
依赖库：langgraph、langchain-openai、psycopg、chromaDB 等
# 创建环境
conda create -n ATL python=3.11
# 激活环境
conda activate ATL
# 安装项目核心依赖
pip install langgraph==0.2.74
pip install langchain-openai==0.3.6
pip install langchain-community==0.3.19
pip install langchain-chroma==0.2.2

# 数据库相关
pip install psycopg2==2.9.10
pip install langgraph-checkpoint-postgres
pip install psycopg psycopg-pool

# 其他工具
pip install pdfminer.six
pip install nltk==3.9.1
pip install concurrent-log-handler==0.9.25

# WebUI相关（可选）
pip install gradio
pip install fastapi
# 部署 PostgreSQL 数据库（Docker 方式）
准备docker-compose.yml文件，启动 PostgreSQL 服务：

docker-compose up -d
进入 Docker 容器安装 pgvector 扩展：

# 进入容器
docker exec -it <postgres容器名> bash

# 安装依赖
apt update 
apt install -y git build-essential postgresql-server-dev-15 

# 编译安装pgvector
git clone --branch v0.7.0 https://github.com/pgvector/pgvector.git
cd pgvector
make
make install

# 验证安装
ls -l /usr/share/postgresql/15/extension/vector*
安装系统依赖（根据操作系统选择）：
Windows：安装 PostgreSQL 客户端库libpq（推荐直接安装完整 PostgreSQL）
MacOS：brew install postgresql

# 运行项目
python main.py
# WebUI 启动
python webUI.py
### 本地访问 http://127.0.0.1:7860
# 📁 项目结构
```text
医疗智能分诊系统/
├── assets/               # 静态资源（流程图、截图）
├── prompts/              # 提示词模板文件
│   ├── prompt_template_agent.txt      # Agent意图分析模板
│   ├── prompt_template_grade.txt      # 文档评分模板
│   ├── prompt_template_rewrite.txt    # 查询重写模板
│   └── prompt_template_generate.txt   # 回复生成模板
├── chromaDB/             # Chroma向量数据库存储目录
├── output/               # 日志输出目录
├── config.py             # 全局配置类
├── llms.py               # 大模型初始化配置
├── tools.py              # 工具定义与配置
├── vectorSave.py         # RAG向量存储初始化
├── ragAgent.py           # 核心工作流逻辑
├── main.py               # FastAPI接口入口
├── webUI.py              # Gradio WebUI
└── apiTest.py            # API测试脚本
```
# ⚠️ 注意事项
调用通义千问模型时，需设置check_embedding_ctx_length=False，否则会报 400 错误
确保 PostgreSQL 的 pgvector 扩展安装成功，否则向量存储功能无法使用
运行前需配置正确的数据库 URI（config.py中的DB_URI）
大模型 API 密钥需有效，否则无法调用模型服务
#📄 许可证
本项目基于 MIT 许可证 开源。
