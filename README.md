# HydrogelPilot

面向水凝胶配方的性能预测与多目标优化工具。

## 简介

HydrogelPilot 是一个基于 SciencePal 平台构建的科研工具：从配方实验数据出发，用机器学习回归预测性能、以多目标 Pareto 优化推荐候选配方。核心计算由 SciencePal 自定义智能体在沙箱中真实执行，Streamlit 前端负责交互与结果展示。

## 功能特性

- **数据上传**：支持 CSV / Excel，自动识别特征列与目标列
- **回归建模**：多类候选模型 + 嵌套交叉验证的诚实评估（区分训练集表现与真实泛化能力）
- **多目标优化**：硬约束下的 Pareto 前沿 + 候选配方推荐 + 不确定性标注
- **正向预测与反向设计**：「配方 → 性能」与「性能 → 配方」双路径
- **AI 全程解读**：数据质检解读、建模解读、候选点评、结构化报告

## 目录结构

```
HydrogelPilot/
├── app.py                    # Streamlit 前端（7 步交互流程 + AI 助手）
├── requirements.txt          # Python 依赖
├── 启动工具.bat               # Windows 一键启动（自动建 venv + 装依赖）
├── pack_demo.py              # 演示数据打包脚本
├── data/
│   └── synthetic/            # 脱敏合成演示数据
└── src/
    ├── demo_modeling.py      # 建模脚本（多候选模型 + 嵌套交叉验证）
    ├── demo_optimization.py  # 多目标 Pareto 优化脚本
    ├── synthetic_data.py     # 合成数据生成器
    ├── sciencepal_client.py  # SciencePal API 客户端
    └── SKILL.md              # SciencePal 技能说明
```

## 快速开始

**方式一（推荐）**：双击 `启动工具.bat`，脚本会自动创建虚拟环境、安装依赖并启动前端。

**方式二（手动）**：

```bash
python -m venv .venv
.venv\Scripts\pip install -r requirements.txt
.venv\Scripts\python -m streamlit run app.py
```

启动后浏览器访问 http://localhost:8501，按「数据上传 → 字段设置 → 目标设定 → 运行 → 结果查看 → 反向设计 → AI 报告」的流程操作即可。

## 运行要求

- Python 3.11+（在 3.13 上测试通过）
- 依赖见 `requirements.txt`

## 数据说明

演示数据为**脱敏合成数据**（特征 `comp_A` / `comp_B`，目标 `m1` / `m2` / `m3`，分别对应杨氏模量、界面阻抗、黏附强度），仅用于演示方法流程，不代表真实实验。

## SciencePal 集成

本工具的核心计算由 SciencePal 平台上的自定义智能体在沙箱中真实执行：前端通过 `src/sciencepal_client.py` 调用 SciencePal API，将数据传入沙箱、运行建模与优化脚本，并接收 AI 解读返回前端展示。技能的完整说明见 `src/SKILL.md`。

> 提示：启用 AI 解读与 SciencePal 联动需在 `.streamlit/secrets.toml` 中配置 `SCIENCEPAL_API_KEY`（该文件不随代码分发）。
