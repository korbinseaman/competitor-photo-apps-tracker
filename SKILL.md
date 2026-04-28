---
name: competitor-photo-apps-tracker
description: >
  追踪八大手机厂商照片/相册/图库应用的新版本发布、功能更新、社区反馈和重要新闻，生成竞品周报。
  覆盖竞品：Apple 照片、Google Photos、华为 HarmonyOS 图库、小米相册、OPPO 相册、VIVO 相册、荣耀相册、三星相册。
  触发条件：(1) 用户请求照片应用竞品周报/竞品分析，(2) 用户询问某相册APP最新动态，(3) 用户要求追踪竞品照片APP每周动态。
  不适用于：非照片类竞品、非周报频率的请求。
---

# 竞品照片应用动态追踪

追踪 8 大主流照片/相册/图库应用的每周动态，输出结构化竞品分析报告。

## 竞品范围

| # | 竞品 | 平台 |
|---|-----|------|
| 1 | Apple 照片 | iOS / macOS |
| 2 | Google Photos | Android / iOS / Web |
| 3 | 华为 HarmonyOS 图库 | HarmonyOS |
| 4 | 小米相册 | HyperOS / Android |
| 5 | OPPO 相册 | ColorOS / Android |
| 6 | VIVO 相册 | OriginOS / Android |
| 7 | 荣耀相册 | MagicOS / Android |
| 8 | 三星相册 | One UI / Android |

## 追踪维度

1. **新版本/功能更新** — 官网、Changelog、博客、微博、微信公众号、小红书
2. **社区用户反馈** — 微博、小红书、抖音评论区、Reddit、Twitter/X、ProductHunt
3. **重要公开信息** — 发布会、新专利、战略合作、行业新闻

## 研究流程

### 步骤 1：生成研究计划

运行辅助脚本获取所有信息源 URL：

```bash
python3 scripts/research_plan.py
```

输出包含竞品 → 信息源 URL 的 JSON。

### 步骤 2：抓取信息源

每个竞品用 `web_fetch` 抓取 2–3 个关键信息源：
- **第一优先级**：官方博客/新闻页/更新日志
- **第二优先级**：社区论坛（Reddit、微博、产品论坛）
- **第三优先级**：科技媒体（36氪、少数派、9to5Google 等）

若页面加载失败，从 `references/competitor-sources.md` 中换源重试。

### 步骤 3：综合发现

对每个竞品，按以下分类整理发现：
- 🆕 **新功能/更新** — 版本号、功能新增
- 💬 **用户反馈** — 吐槽、好评、功能诉求（提炼主题）
- 📰 **新闻/事件** — 发布会、专利、合作

检查后无发现则标注「本周无重大更新」。

### 步骤 4：生成报告

以 `assets/report-template.md` 为模板，填充：
- `{date}` → `YYYY-MM-DD`（报告日期）
- `{week_start}` / `{week_end}` → 周一至周日
- `{generated_at}` → ISO 时间戳
- `{content_or_none}` → 综合发现内容
- `{sources}` → 信息来源名称 + 链接
- `{summary}` → 1–2 句竞争格局观察

保存到：`workspace/竞品分析/竞品周报-{date}.md`

### 步骤 5：同步至飞书（创建 + 写入内容）

1. 用 `feishu_doc`（action: create）创建飞书文档，获取 `doc_token`
2. **立即用 `feishu_doc`（action: write, doc_token=上一步返回值）将步骤 4 生成的完整 Markdown 报告内容写入文档**
   - ⚠️ 必须确认 write 返回 `success: true` 且 `blocks_added > 0`，否则重试或报错提醒用户
3. 将报告链接发送给用户

## 输出格式规则

- 每个竞品一个独立章节，清晰分隔
- 使用列表而非大段文字
- 每条结论附来源链接
- 「本周观察」限 1–2 句，聚焦整体格局
- 英文来源的关键信息翻译为中文
