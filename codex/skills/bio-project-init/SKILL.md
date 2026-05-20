---
name: bio-project-init
description: >-
  为生信项目生成完整脚手架（plan.md、CLAUDE.md、execution_log、标准目录）。
  触发条件：用户说"项目初始化"、"建项目脚手架"、"新建生信项目"、"plan.md 模板"，
  或当前目录是空的/只有数据文件但缺乏 plan.md 时使用。
  不适用于：已存在完整 plan.md 的项目、纯执行任务、与脚手架无关的咨询。
---

# 生信项目脚手架

直接执行，不要只做计划。

## 步骤 1：收集项目信息

询问用户以下信息（有默认值的直接用默认值，用户可覆盖）：

1. **项目名称**（中文，如"心衰转录组分析"）
2. **分析类型**：bulk_rnaseq | scrnaseq | multi_omics | comparative_genomics | ip_ms | custom
3. **物种**：human | mouse | 其他
4. **项目目录**（默认当前目录）

如果当前目录已有 plan.md 或数据文件，可以从中推断类型和物种，直接确认即可。

## 步骤 2：创建标准目录结构

```bash
mkdir -p results figures scripts reports delivery data
```

目录用途：
- `data/` — 原始/中间数据（不入交付）
- `results/` — 分析结果表格（xlsx/csv/tsv）
- `figures/` — 可视化图表（png/pdf/svg）
- `scripts/` — 分析脚本（R/Python/Shell）
- `reports/` — 中间报告与笔记
- `delivery/` — 最终交付物（由 $bio-deliver 生成）

## 步骤 3：生成核心模板文件

### 3.1 `plan.md`（分析计划）

```markdown
# 项目分析计划：<项目名称>

## 1. 项目背景
- 数据来源：?
- 实验设计：?
- 物种 / 参考基因组版本：<species> / ?

## 2. 分析目标
1. ?
2. ?

## 3. 分析步骤
| 序号 | 步骤 | 输入 | 输出 | 工具 / 关键参数 |
|------|------|------|------|----------------|
| 1    | 数据质控 | ? | results/01_qc/ | ? |
| 2    | ?    | ?    | ?    | ? |

## 4. QC 断言
- sample_total = ?
- min_reads_per_sample = ?
- 其他关键阈值：?

## 5. 交付物清单
- [ ] 分析报告（.docx）
- [ ] 关键图表（figures/）
- [ ] 结果表格（results/）
- [ ] 分析代码（scripts/）
```

> 填写时把所有 `?` 替换为具体值；针对不同分析类型补充对应步骤。

### 3.2 `AGENTS.md`（代理行为约定，对应 Claude Code 的 CLAUDE.md）

```markdown
# Agent 工作约定

## 项目类型
<type>（<species>）

## 通用约束
- 严格按 plan.md 执行；任何偏离需更新 plan.md
- 所有结果写入 results/、figures/、reports/ 三个目录
- 脚本统一放 scripts/，命名 NN_步骤名.{R,py,sh}
- 提交前用 $bio-check 自审

## 数据约束
- 不修改 data/ 下的原始文件
- 中间数据写入 data/intermediate/

## 命名规范
- 结果文件：`{Step}_{Description}.{ext}`，如 `01_qc_summary.xlsx`
- 图表：`fig{N}_{Description}.png`
```

### 3.3 `execution_log.md`

```markdown
# 执行日志

| 日期 | 步骤 | 脚本 | 输入 | 输出 | 状态 | 备注 |
|------|------|------|------|------|------|------|
|      |      |      |      |      |      |      |
```

### 3.4 `numeric_reference.tsv` 与 `report_claims.tsv`

```tsv
key	value	source_file	note
```

两张表用于报告写作时校对数值。

## 步骤 4：引导填写

展示生成的 plan.md，标注需要填写的 `?` 占位符，帮助用户逐项填写。

## 步骤 5：验证

- 确认所有 `?` 占位符已替换为具体值
- 确认目录结构齐全
- 输出"项目脚手架就绪，可以开始执行"
