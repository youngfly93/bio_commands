# bio_commands

为 Claude Code 设计的生信外包项目工作流命令集，覆盖**项目脚手架 → 分析审计 → 图表审查 → 报告撰写 → 交付打包**全流程。

## 包含内容

### 8 个斜杠命令（`/bio:*`）

| 命令 | 用途 |
|------|------|
| `/bio:project-init` | 生成项目脚手架（plan.md / CLAUDE.md / 标准目录） |
| `/bio:check` | 对照 plan.md 一次性审计分析结果 |
| `/bio:audit-fix` | 审计-修复一体化循环（最多 5 轮） |
| `/bio:fig-review` | 批量审查图表是否符合 SCI 发表标准 |
| `/bio:sci-fig` | 引导绘制 SCI 级别可视化 |
| `/bio:report` | 配合 `docx` 技能生成中文 Word 报告 |
| `/bio:ai-clean` | 扫描并清除交付文件中的 AI 痕迹 |
| `/bio:deliver` | 一键交付打包（质量审计 → AI 痕迹清理 → Windows 兼容 ZIP） |

### 1 个 Subagent

- **bio-result-auditor** — 生物信息学结果审计专家，被 `/bio:check` 与 `/bio:audit-fix` 调用。

## 安装方式

### 方式 A：作为单一 plugin 安装（推荐）

```
/plugin install youngfly93/bio_commands
```

### 方式 B：作为 marketplace 安装

```
/plugin marketplace add youngfly93/bio_commands
/plugin install bio@bio_commands
```

### 方式 C：本地开发安装

```bash
git clone https://github.com/youngfly93/bio_commands ~/.claude/plugins/local/bio
```

然后在 `~/.claude/plugins/local/.claude-plugin/marketplace.json` 中注册（参考本仓库的 marketplace.json）。

## 典型工作流

```
# 1. 新项目
/bio:project-init
   ↓ 填写 plan.md

# 2. 执行分析（自行运行脚本）

# 3. 阶段性自审
/bio:check

# 4. 反复修复直到通过
/bio:audit-fix

# 5. 图表把关
/bio:fig-review

# 6. 生成报告
/bio:report

# 7. 一键打包交付
/bio:deliver
```

## 目录结构

```
bio_commands/
├── .claude-plugin/
│   ├── plugin.json          # plugin 元信息
│   └── marketplace.json     # marketplace 元信息（自指）
├── commands/                # 8 个 /bio:* 命令
│   ├── project-init.md
│   ├── check.md
│   ├── audit-fix.md
│   ├── fig-review.md
│   ├── sci-fig.md
│   ├── report.md
│   ├── ai-clean.md
│   └── deliver.md
├── agents/
│   └── bio-result-auditor.md   # 审计 subagent
├── LICENSE
└── README.md
```

## 适用项目类型

- Bulk RNA-seq
- 单细胞 RNA-seq
- 多组学整合
- 比较基因组学（含 OrthoFinder / CAFE5 / PAML 流程）
- IP-MS 蛋白质组学
- 自定义流程

## License

MIT
