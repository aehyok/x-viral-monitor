# 定时任务：X 博主爆款帖监测

- **时间表**：`CRON_TZ=Asia/Shanghai 4 7-23 * * *`（每天 07:04–23:04 北京时间，每小时一次，含周末）
- **调用的 skill**：[`skill/SKILL.md`](../skill/SKILL.md)
- **结果去向**：有达标帖时推到 Grok Bot 对话；没有达标时不提醒，但仍会归档

## 提示词（原文）

```text
每小时按 skill「X 博主爆款帖监测」扫一轮，并把本轮数据归档到仓库 aehyok/x-viral-monitor。

监测这 12 个 X 账号的最新原创帖（含引用帖，不含回复和纯转发）：@MinLiBuilds、@Lonely__MH、@AYi_AInotes、@gkxspace、@servasyy_ai、@yyyole、@Saccc_c、@xiangxiang103、@gengdaJ、@NFT_Chen、@xiaomovps、@alin_zone。用公开 X 数据工具拉每个账号过去 24 小时内发的帖子及其浏览量（impression_count）。

状态文件（运行时）：/cursor/stores/store-d01e1341-d634-4f09-9918-db1126394e2f/x-monitor/state.json，里面有账号 id、每条帖子上次记录的浏览量和时间（snapshots），以及已推送过的帖子 id（pushed）。阈值：每小时曝光超过 5000。时区：Asia/Shanghai。

判定规则：
- 帖子在 snapshots 里有上次记录：用（本次浏览量 − 上次浏览量）÷ 两次间隔小时数，结果 > 5000 就算达标。
- 第一次见到的帖子：用 浏览量 ÷ max(发帖至今小时数, 1)，结果 > 5000 就算达标。
- 已在 pushed 里的帖子不再重复推送。
每次跑完都把所有 24 小时内帖子的最新浏览量和时间写回 snapshots，把新推送的 id 加进 pushed，并清理 48 小时前的旧记录。写回时保留 state 里已有的 accounts（别覆盖掉新加的账号）。

归档到 GitHub（本机目录 /workspace/x-viral-monitor，远端 aehyok/x-viral-monitor）：
1. 先 git pull --rebase，再把更新后的 state.json 复制到 data/state/state.json。
2. 按北京时间写本轮文件：data/hourly/YYYY-MM-DD/HHMM.md 与同名 .json。JSON 至少包含 run_at、accounts、每条检查过的帖子（id、author、views、rate_per_hour、qualified、already_pushed）、本轮新达标列表。Markdown 用中文写简报：扫了几个号、几条帖、新达标几条、最接近阈值的几条。
3. 若账号列表或本提示词有变更，同步更新 ACCOUNTS.md、README.md 里的账号表和 routine/PROMPT.md。
4. git add、commit（说明写本轮北京时间与达标条数）、push 到 origin。若目录缺失则先 gh 克隆 aehyok/x-viral-monitor。提交身份用仓库本地 git user（grokbot少年 / aehyok@163.com）。push 失败时在结果里说明，不要假装已归档。

有达标帖子时，用中文给用户发一条简洁消息，每条帖子一行：作者、帖子链接（用 markdown，标签写帖子内容的简短概括）、当前浏览量、每小时增量、点赞/转发/书签数，时间按北京时间。可顺带一句「本轮数据已推到 aehyok/x-viral-monitor」。

结果发给这个对话里的用户本人。没有达标帖子时什么都不发，保持安静（仍要完成归档提交）。
```
