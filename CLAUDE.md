# CLAUDE.md

本文件为 Claude Code 在此仓库中工作时提供指导。

## 项目概述

质谱成像空间分辨率计算器 - 用于测量质谱成像（MSI）中空间分辨率的网页工具。通过分析扫描线上的响应强度数据，计算信号从峰值的20%上升到80%所需的距离（μm）。

## 架构

单文件HTML应用（`index.html`），包含内嵌CSS和JavaScript。无需构建过程或外部依赖。

**核心组件：**
- **数据输入**：接收两列数据（保留时间单位min，响应强度）
- **画布可视化**：绘制响应曲线，支持交互式提示框
- **计算引擎**：查找最小/最大值，计算20%-80%阈值，计算空间分辨率
- **峰值检测**：根据用户指定的峰值位置（左/右），识别离选定最大值最近的最小值

## 工作流程

1. 用户粘贴保留时间和强度数据
2. 输入扫描速度（μm/s）以将时间转换为距离
3. 点击"绘制图表"绘制曲线
4. 选择峰值位置（最大值在最小值左边或右边）
5. 确认最小值/最大值（可编辑）
6. 点击"确认并计算"计算空间分辨率
7. 结果显示20%和80%响应点之间的距离

## 关键变量

- `distances`：距离数组，单位μm（由保留时间 × 60 × 扫描速度计算）
- `intensities`：响应强度值数组
- `highlightMaxIdx`、`highlightMinIdx`：当前计算中使用的最大/最小值点的索引
- `peakPosition`：用户选择（"right"或"left"），决定在哪一侧搜索最小值

## 重要实现细节

- **最小/最大值查找**：使用用户输入值查找对应索引，而非全局极值
- **交点计算**：仅在选定的最小值和最大值之间搜索，避免多个峰值干扰
- **画布坐标**：Y轴使用公式 `canvas.height - padding - (value / maxInt) * height`
- **标注更新**：绘制新数据时重置 `highlightMaxIdx` 和 `highlightMinIdx`；在 `confirmValues()` 中更新
- **提示框**：悬浮显示距离（μm）和强度；点击复制强度到剪贴板

## 部署

部署在 Vercel。GitHub 仓库：`https://github.com/finezhuxy-hub/MSI-spatial-resolution-calculator`

推送更改以触发自动重新部署：
```bash
git add index.html && git commit -m "message" && git push
```

## 常见修改

- **添加新计算指标**：扩展 `confirmValues()` 和结果显示部分
- **改变可视化**：修改 `draw()` 和 `drawWithLines()` 函数
- **调整UI布局**：编辑 `<style>` 部分的CSS
- **修改提示框行为**：更新脚本部分的鼠标事件监听器
