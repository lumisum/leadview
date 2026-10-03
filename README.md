# 前沿观 · 科技动态（leadview）

前沿科技动态与深挖报告站，GitHub Pages（Jekyll）：
https://lumisum.github.io/leadview/

## 结构（参考 wulai 仓库的做法）

- `_config.yml`：站点配置（title / baseurl / 描述）
- `_layouts/default.html`：全站唯一布局（内联样式，无外部依赖）
- `index.md`：首页，手工维护报告卡片列表（发新报告时加一张卡片）
- `reports/<日期-拼音slug>/report.md`：每篇报告一个目录，front matter 必备 layout / title / date / permalink

## 发布流程

1. 新报告写入 `reports/<日期-slug>/report.md`，permalink 设为 `/reports/<日期-slug>/`
2. 在 `index.md` 顶部加一张报告卡片
3. 在本文件下方时间线加一行
4. 一次提交推送到 main 分支，Pages 自动构建

## 报告时间线

| 日期 | 报告 |
|---|---|
| 2026.10.03 | [用光互连替代电线连接 AI 芯片与内存芯片](reports/2026-10-03-ai-memory-optical-interconnect/report.md) |

## 标注约定

＝公司新闻稿/官网/高管访谈，未经独立复现；＝独立媒体、拆解、学术与行业来源；＝有第三方证据的实际出货或部署。报告只梳理事实与逻辑链，不构成投资建议。
