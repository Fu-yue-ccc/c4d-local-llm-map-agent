# C4D 本地大模型 Agent 技能：SIAS 校园地点地图生成 Agent

本仓库是「C4D 本地大模型 Agent 技能」挑战的交付包。

## 交付物一览

| 挑战要求 | 文件 | 说明 |
| --- | --- | --- |
| *Agent技能* | `Student_C4D_Agent技能/` | 可复用 Agent 技能包：`SKILL.md` + `scripts/sias_map_agent.py` + `references/` |
| *demo* | `Student_C4D_demo_map.html` | 离线模式生成的交互式地图产物（12 个地点 / 10 个类别） |
| *AI日志* | `Student_C4D_AI日志.md` | AI 协作日志（20 轮） |
| *AAR* | `Student_C4D_AAR复盘.md` | 七维 AAR 复盘 + 对照 rubric 自评 |

## 其它文档

- `Student_C4D_教学说明.md` — 安装与使用教学（三平台步骤、FAQ、截图要求与检查清单）
- `Student_C4D_拿来说明.md` — 拿来说明（模型选型、运行方式、验收口径）
- `Student_C4D_方案设计.md` — 方案设计
- `Student_C4D_验证报告.md` — 离线验证结果

## 怎么跑

前提：Python 3.8+、Ollama，`pip install requests folium`。

```
cd Student_C4D_Agent技能/scripts
python3 sias_map_agent.py --offline --locations 12 --output Student_C4D_demo_map.html
```

默认模型 `gemma4:e4b`；在线模式需要本地 Ollama 与已下载的 Gemma 4 模型。

## 本地运行截图

`Student_C4D_output_screenshots/` 存放本地真机运行截图（含模型名称、设备信息、推理过程与地图结果）。
截图要求见 `Student_C4D_output_screenshots/README_截图说明.md`，由学生本人在本地设备补齐后放入该目录。
