# X 博主爆款帖监测归档

公开仓库，存放 Grok Bot skill「X 博主爆款帖监测」以及每小时扫描结果，欢迎查看和复用。

> 数据来源为 X 公开帖子的浏览、点赞、转发、书签等公开指标，仅做趋势记录，不含任何私信或非公开内容。

## 当前监测账号（11 个）

| 账号 | 主页 | 用户 ID |
| --- | --- | --- |
| @MinLiBuilds | https://x.com/MinLiBuilds | `1679698342283186177` |
| @Lonely__MH | https://x.com/Lonely__MH | `2926712868` |
| @AYi_AInotes | https://x.com/AYi_AInotes | `1974653328274714624` |
| @gkxspace | https://x.com/gkxspace | `1881991469730594816` |
| @servasyy_ai | https://x.com/servasyy_ai | `1998968758422179840` |
| @yyyole | https://x.com/yyyole | `939870716391854082` |
| @Saccc_c | https://x.com/Saccc_c | `1869683226178146304` |
| @xiangxiang103 | https://x.com/xiangxiang103 | `1385766205403713536` |
| @gengdaJ | https://x.com/gengdaJ | `1897545708770840576` |
| @NFT_Chen | https://x.com/NFT_Chen | `1429221486032678914` |
| @xiaomovps | https://x.com/xiaomovps | `1572253121547735040` |

账号增减以定时任务配置为准，同时会同步写入 `data/state/state.json` 的 `accounts` 字段。完整名单也见 [ACCOUNTS.md](ACCOUNTS.md)。

## 规则摘要

- **阈值**：上一小时新增浏览量 **> 5000**
- **窗口**：每天 **07:04–23:04**（北京时间 / Asia/Shanghai），含周末
- **范围**：近 24 小时内的原创帖与引用帖（不含纯回复、纯转发）
- **去重**：已推送过的帖子 id 记在 `pushed`，不再重复提醒

## 目录说明

| 路径 | 内容 |
| --- | --- |
| `skill/SKILL.md` | 可复用的扫描流程（账号、阈值由调用方传入） |
| `routine/PROMPT.md` | 定时任务的时间表和提示词原文 |
| `ACCOUNTS.md` | 当前监测账号列表（中文） |
| `data/state/state.json` | 最新状态：账号 id、浏览量快照、已推送帖子 |
| `data/hourly/YYYY-MM-DD/HHMM.md` | 每小时一轮的中文简报 |
| `data/hourly/YYYY-MM-DD/HHMM.json` | 同一轮的结构化数据 |

## 说明

- 想监测自己关注的博主：把 `skill/SKILL.md` 拿去用，账号列表、阈值和状态文件路径换成你自己的即可。
- 每小时扫描结束后，定时任务会更新 `state.json`，并新增对应的小时文件，再 `git push` 到本仓库。
- 告警消息发到 Grok Bot 对话；无达标时不打扰，但仍会归档本轮数据。
