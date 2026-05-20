配合 document-skills:docx 生成中文生信分析 Word 交付报告。

## 前置准备

1. 读取 plan.md 了解项目背景、分析内容和要求
2. 扫描 results/figures 目录收集所有可用的图表和结果
3. 确定报告结构（根据 plan.md 的分析步骤组织章节）

## 报告结构模板

```
1. 项目概述
   1.1 研究背景
   1.2 分析目标
   1.3 数据概况

2. 分析方法
   2.1 数据质控
   2.2 分析流程
   2.3 使用的软件与参数

3. 分析结果
   3.1 [根据 plan.md 的具体分析步骤]
   3.2 [每个步骤配对应图表]
   ...

4. 结论与讨论

附录
   A. 软件版本
   B. 参数配置
```

## 格式要求

- **中文正文**: 宋体（SimSun），小四号（12pt）
- **英文/数字**: Times New Roman，12pt
- **标题**: 黑体（SimHei）
  - 一级标题: 三号（16pt），加粗
  - 二级标题: 四号（14pt），加粗
  - 三级标题: 小四号（12pt），加粗
- **行距**: 1.5 倍
- **页边距**: 上下 2.54cm，左右 3.18cm

## 图表嵌入规则

1. 嵌入前验证图片路径存在：
   ```bash
   ls -la "$img_path"
   ```
2. 处理 `_page1`/`_page2` 变体（PDF 转图片时生成）：优先使用 `_page1`
3. 图表标题格式: `图 X. 描述内容` / `表 X. 描述内容`
4. 图表居中显示
5. 支持 .png/.jpg/.tiff 格式

## 生成后验证

1. **XML 完整性**: 解压 .docx 检查 XML 无语法错误
   ```bash
   python3 -c "
   import zipfile
   z = zipfile.ZipFile('report.docx')
   for f in z.namelist():
       if f.endswith('.xml'):
           z.read(f)  # 能读取即基本完整
   print('XML check passed')
   "
   ```
2. **图片完整性**: 检查 word/media/ 中的图片文件数量与文档引用数量一致
3. **Markdown 残留**: 扫描文档文本中是否有 `**`, `##`, `` ` ``, `- [ ]` 等 markdown 标记残留，发现则清除

## 注意事项

- 使用 document-skills:docx 的能力生成 .docx 文件
- 图表编号连续，与正文引用一致
- 表格使用三线表风格
- 避免 AI 套话（参考 /ai-clean 中的关键词列表）
