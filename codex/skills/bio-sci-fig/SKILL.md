---
name: bio-sci-fig
description: >-
  绘制 SCI 发表级别的可视化图表，要求纯白背景、无网格线、配色舒服、文字可读。
  触发条件：用户说"画 SCI 级别的图"、"出版级图表"、"sci-fig"、"画一张高质量的图"、
  或要求把已有 ggplot/matplotlib 图按发表标准重画时使用。
  不适用于：批量审查（用 $bio-fig-review）、纯探索性绘图、网页/海报场景。
---

# SCI 级别可视化

## 硬性要求

- 背景：纯白（不要灰色、不要透明）
- 网格线：移除或设为极淡（alpha < 0.1）
- 分辨率：300 DPI 以上（保存时显式指定）
- 字体：英文优先 Arial/Helvetica，中文优先 SimHei/PingFang
- 配色：色盲友好（建议 viridis、Set2、自定义 6-8 色调色板）
- 字号：图内文字相对比例合理，缩放后仍可读
- 坐标轴：标签 + 单位
- 图例：放在不遮挡数据的位置，必要时使用 "facet" 拆分
- 统计标注：`*` (p<0.05), `**` (p<0.01), `***` (p<0.001), `ns`

## R / ggplot2 推荐起手式

```r
library(ggplot2)
theme_sci <- theme_classic(base_size = 12, base_family = "Arial") +
  theme(
    panel.background = element_rect(fill = "white", color = NA),
    panel.grid = element_blank(),
    axis.line  = element_line(color = "black", linewidth = 0.4),
    axis.ticks = element_line(color = "black", linewidth = 0.4),
    legend.position = "right"
  )

ggsave("figures/fig1.png", plot = p, width = 6, height = 4, dpi = 300, bg = "white")
```

## Python / matplotlib 推荐起手式

```python
import matplotlib.pyplot as plt
plt.rcParams.update({
    "figure.facecolor": "white",
    "axes.facecolor":   "white",
    "axes.grid":        False,
    "savefig.dpi":      300,
    "savefig.bbox":     "tight",
    "font.family":      "Arial",
    "font.size":        12,
})
```

## 工作流

1. 询问用户绘图意图：数据类型、想表达的对比、目标期刊（如已知）
2. 检查源数据列名与分组结构
3. 选择最合适的图型（柱/箱/小提琴/散点/热图/UMAP/火山）
4. 生成代码并保存到 `figures/`
5. 输出图供用户视觉检查
6. 若不满足上述硬性要求，迭代修复
