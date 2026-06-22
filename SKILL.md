---
name: wc26-predict
description: |
  2026世界杯概率预测研究系统。基于 Dixon-Coles 核心模型、多源信号融合、
  walk-forward 回测门控和赛后复盘学习，在可审计无泄漏前提下输出比赛胜平负
  概率和不确定性评估。
requires:
  - 两支球队名称或具体对阵（如"德国 vs 日本"）
  - 可选：关注的预测维度（胜平负概率 / 小组出线概率 / 淘汰赛路径）
---

# WC26 Predict — 世界杯比赛概率预测

> 来源仓库: https://github.com/AndyDu0921/wc26-predict
> 本技能由 socialistic.ai 基于上述开源项目自动封装，原仓库不含 SKILL.md。

## 你能用这个技能做什么

给出任意两支 2026 世界杯参赛队的对阵，系统返回：

1. **赛前概率分布** — 主胜 / 平局 / 客胜三向概率（Dixon-Coles 模型 + Elo/Pi Rating + 市场校准融合）
2. **不确定性标注** — Brier / log loss / RPS 历史校准水平，以及当前预测的置信区间提示
3. **关键信号摘要** — 近期战绩、Elo 变动、伤停/阵容缺失度、天气/场地因素（如系统已采集）
4. **合规输出** — 仅提供研究级概率分析，不含投注建议、赔率引用或博彩语言

## 输入格式

用户用自然语言描述对阵即可，例如：

- "德国 vs 日本 小组赛胜平负概率"
- "巴西对阵阿根廷，谁更可能赢？"
- "E组出线形势分析"

也可以上传包含多场对阵的文本/CSV 文件进行批量预测。

## 输出契约

每场对阵返回结构化结果：

| 字段 | 说明 |
|---|---|
| `home_team` / `away_team` | 标准化队名 |
| `p_home` / `p_draw` / `p_away` | 三向概率（和为 1.0） |
| `model_components` | 各子模型权重和贡献 |
| `calibration_note` | 当前模型校准状态说明 |
| `data_cutoff` | 信息状态截止时间 |
| `confidence_caveat` | 不确定性提示 |

## 限制与声明

- 本系统是足球预测研究工具，不是博彩产品。
- 概率输出基于历史数据和模型推断，不构成任何形式的投注建议。
- 模型权重基于有限赛后复盘样本持续校准，尚未达到全局最优声称标准。
- 阵容伤停、实时天气等信号覆盖可能不完整，概率应视为条件估计。

## 技术架构参考

核心管线: Dixon-Coles + Elo + Pi Rating + Weibull + Tabular + Market 六源融合 →
校准层 → prediction_snapshots（含参数溯源哈希）→ 赛后验证 → 复盘学习 → walk-forward gate。

详见源仓库文档: https://github.com/AndyDu0921/wc26-predict/blob/master/README.md
