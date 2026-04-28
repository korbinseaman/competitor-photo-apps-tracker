# 竞品系统应用动态追踪

> 追踪八大手机厂商任意系统级应用的每周动态，输出结构化竞品分析报告。

## 概述

本 Skill 用于对主流手机厂商的系统级应用（照片、备忘录、设置、相机、日历等）进行竞品分析。覆盖 **Apple、Google、华为、小米、OPPO、VIVO、荣耀、三星** 8 大厂商，按周维度追踪新版本发布、功能更新、社区反馈和行业新闻，自动生成竞品周报。

**核心特点：**
- 🔍 **通用化设计** — 不限于照片应用，支持任意系统应用的竞品分析
- 📊 **多维追踪** — 功能更新、用户反馈、行业新闻三个维度
- 📝 **自动报告** — 结构化 Markdown 报告 + 飞书文档同步
- 🔗 **信息源预置** — 8 厂商 × 2–4 源信息源 + web_search 补充策略

## 竞品厂商

| # | 厂商 | 平台 |
|---|-----|------|
| 1 | Apple | iOS / macOS |
| 2 | Google | Android |
| 3 | 华为 | HarmonyOS |
| 4 | 小米 | HyperOS / Android |
| 5 | OPPO | ColorOS / Android |
| 6 | VIVO | OriginOS / Android |
| 7 | 荣耀 | MagicOS / Android |
| 8 | 三星 | One UI / Android |

## 快速开始

### 前置条件

- Python 3.8+
- OpenClaw 运行环境（提供 `web_fetch`、`web_search`、`feishu_doc` 等工具）

### 执行流程

1. **指定应用名称** — 告诉 AI 要分析的系统应用（如"照片"、"备忘录"、"设置"）
2. **生成研究计划** — 运行脚本获取各厂商信息源和搜索关键词
3. **抓取信息** — `web_fetch` 抓取官方源 + `web_search` 补充搜索
4. **综合分析** — 按 🆕 新功能 / 💬 用户反馈 / 📰 新闻事件分类整理
5. **生成报告** — 保存为 Markdown 文件并同步至飞书文档

## 项目结构

```
skills/competitor-photo-apps-tracker/
├── SKILL.md                        # Skill 主流程定义（核心）
├── README.md                       # 项目说明文档
├── assets/
│   └── report-template.md          # 竞品周报 Markdown 模板
├── references/
│   └── competitor-sources.md       # 各厂商信息源参考 + 搜索策略
└── scripts/
    └── research_plan.py            # 研究计划生成脚本
```

## 使用示例

### 照片/相册竞品分析

```
帮我分析一下照片 APP 在各手机厂商这周的竞品动态
```

### 备忘录竞品分析

```
帮我做一份备忘录 APP 的竞品周报
```

### 设置 APP 竞品分析

```
追踪一下各厂商设置 APP 这周有没有更新
```

## 输出格式

报告按厂商分为 8 个章节，每个章节包含：

```markdown
## Apple 照片

**🆕 系统更新**
- iOS 26.4.2 安全更新，未涉及功能变更

**📰 行业动态**
- Tim Cook 宣布 9 月卸任 CEO...

**信息来源：**
- [Apple 新闻室](https://www.apple.com/newsroom/)
```

末尾附「本周观察」总结整体竞争格局。

## 脚本用法

```bash
# 生成备忘录的研究计划
python3 scripts/research_plan.py --app "备忘录"

# 指定周数
python3 scripts/research_plan.py --app "照片" --week "2026-W17"

# 输出 JSON 格式
{
  "week": "2026-W17",
  "app_name": "备忘录",
  "vendors": {
    "Apple": {
      "platform": "iOS / macOS",
      "base_sources": [...],
      "search_keywords": ["Apple 备忘录 update 2026", ...]
    },
    ...
  }
}
```

## 信息源策略

每个厂商配置了两层信息获取策略：

1. **Base Sources（固定源）** — 官网、社区、官方博客，适用于所有应用分析
2. **Search Keywords（动态搜索）** — 按应用名称自动生成搜索关键词，用于 `web_search` 补充

> 示例：`--app "备忘录"` 会为 Apple 生成搜索关键词 `"Apple 备忘录 update 2026"`

## 已知限制

| 限制类型 | 说明 |
|---------|------|
| 微博/小红书 | 需登录才能查看完整内容，`web_fetch` 可能受限 |
| 华为/小米官网 | 部分页面使用 JS 渲染或跳转拦截，抓取效果有限 |
| 社区论坛 | 部分源（如花粉俱乐部）可能返回 403 |
| Google Blog | JS 渲染页面，`web_fetch` 提取内容较少 |

这些限制已在 `references/competitor-sources.md` 中标注，AI 会自动调整策略。

## 许可证

MIT
