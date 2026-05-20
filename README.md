# bio_commands

为 **Claude Code** 与 **OpenAI Codex CLI** 双生态设计的生信外包项目工作流命令/技能集，覆盖**项目脚手架 → 分析审计 → 图表审查 → 报告撰写 → 交付打包**全流程。

> 同一仓库同时维护两种形态：Claude Code 的 plugin（`/bio:*` 斜杠命令）和 Codex 的 skill（`$bio-*` 自然语言触发），逻辑保持一致。

## 包含内容

| 功能 | Claude Code 命令 | Codex Skill | 用途 |
|------|------------------|-------------|------|
| 项目脚手架 | `/bio:project-init` | `$bio-project-init` | 生成 plan.md / AGENTS.md / 标准目录 |
| 一次性审计 | `/bio:check`        | `$bio-check`        | 对照 plan.md 审计现有结果 |
| 审计修复循环 | `/bio:audit-fix`  | `$bio-audit-fix`    | 最多 5 轮 audit-fix 一体化 |
| 图表批量审查 | `/bio:fig-review` | `$bio-fig-review`   | SCI 发表标准检查 |
| 出版级绘图 | `/bio:sci-fig`      | `$bio-sci-fig`      | 纯白底/300dpi/色盲友好 |
| Word 报告 | `/bio:report`        | `$bio-report`       | 中文 .docx 交付报告 |
| AI 痕迹清理 | `/bio:ai-clean`    | `$bio-ai-clean`     | 清除元数据/套话/AI 注释 |
| 一键打包 | `/bio:deliver`        | `$bio-deliver`      | 审计 + 清理 + Windows 兼容 ZIP |
| 审计核心 prompt | （subagent: `bio-result-auditor`） | `$bio-result-auditor` | 被以上若干命令/skill 调用 |

---

## 安装 — Claude Code

### 方式 A：作为单一 plugin

```
/plugin install youngfly93/bio_commands
```

### 方式 B：作为 marketplace

```
/plugin marketplace add youngfly93/bio_commands
/plugin install bio@bio_commands
```

### 方式 C：本地开发

```bash
git clone https://github.com/youngfly93/bio_commands ~/.claude/plugins/local/bio
```

随后在 `~/.claude/plugins/local/.claude-plugin/marketplace.json` 中注册（参考本仓库的 `.claude-plugin/marketplace.json`）。

---

## 安装 — Codex CLI

Codex skill 位于本仓库的 `codex/skills/`，每个 skill 是独立目录。安装即"复制到 `~/.codex/skills/`"：

```bash
git clone https://github.com/youngfly93/bio_commands /tmp/bio_commands
cp -r /tmp/bio_commands/codex/skills/bio-* ~/.codex/skills/
```

下次启动 codex 即可在自然语言中触发，例如：

```
> 帮我对照 plan.md 检查现有结果      # codex 会识别并调用 $bio-check
> $bio-deliver                       # 显式调用 skill
> 这些图能不能投稿                    # 触发 $bio-fig-review
```

> Codex 的 skill 是由 description 中的关键词被模型识别后调用的，所以无需特殊配置。如要让 skill 出现在 codex 的 launcher 列表，可在 `~/.codex/config.toml` 把它们配成 marketplace（详见下方"高级：注册为 marketplace"）。

### 高级：注册为 Codex marketplace

如希望多机器统一管理或多人共享，可在 `~/.codex/config.toml` 中追加：

```toml
[marketplaces.bio_commands]
source_type = "git"
source = "https://github.com/youngfly93/bio_commands.git"
```

然后 codex 重启即可拉取（具体语义以你的 codex-cli 版本为准；旧版可能需要本地路径模式 `source_type = "local"`）。

---

## 典型工作流（两套触发方式等价）

```
# 1. 新项目
/bio:project-init        或    "帮我建一个新生信项目脚手架"  → $bio-project-init
   ↓ 填写 plan.md

# 2. 执行分析（自行运行脚本）

# 3. 阶段性自审
/bio:check               或    "对照 plan 检查结果"           → $bio-check

# 4. 反复修复直到通过
/bio:audit-fix           或    "审计并修复所有问题"           → $bio-audit-fix

# 5. 图表把关
/bio:fig-review          或    "图表批量审查"                  → $bio-fig-review

# 6. 生成报告
/bio:report              或    "生成 Word 报告"               → $bio-report

# 7. 一键打包交付
/bio:deliver             或    "打包交付"                      → $bio-deliver
```

---

## 仓库结构

```
bio_commands/
├── .claude-plugin/                    # Claude Code 入口
│   ├── plugin.json
│   └── marketplace.json
├── commands/                          # 8 个 /bio:* 命令
│   ├── project-init.md
│   ├── check.md
│   ├── audit-fix.md
│   ├── fig-review.md
│   ├── sci-fig.md
│   ├── report.md
│   ├── ai-clean.md
│   └── deliver.md
├── agents/
│   └── bio-result-auditor.md          # Claude Code subagent
├── codex/                             # OpenAI Codex CLI 入口
│   └── skills/
│       ├── bio-project-init/{SKILL.md, agents/openai.yaml}
│       ├── bio-check/...
│       ├── bio-audit-fix/...
│       ├── bio-fig-review/...
│       ├── bio-sci-fig/...
│       ├── bio-report/...
│       ├── bio-ai-clean/...
│       ├── bio-deliver/...
│       └── bio-result-auditor/...     # 独立 skill，被其他 skill 引用
├── LICENSE
└── README.md
```

---

## 适用项目类型

- Bulk RNA-seq
- 单细胞 RNA-seq
- 多组学整合
- 比较基因组学（含 OrthoFinder / CAFE5 / PAML 流程）
- IP-MS 蛋白质组学
- 自定义流程

## License

MIT
