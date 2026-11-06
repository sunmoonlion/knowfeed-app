# Info Consumer Contract Locks

Knowfeed App does not own the Knowledge ingestion schema. The editable source of
truth remains `knowbase-app/contracts/`.

`knowbase-provider-lock.json` pins the compatible contract major and schema
SHA-256 consumed by Info. Contract CI must download or check out the Knowledge
provider artifact, set `KNOWBASE_ARTIFACT_CONTRACT_PATH`, and run the Info
consumer tests. Updating the lock without a passing provider/consumer test is
not a valid contract upgrade.

## 调用方的锁（供给，SDD 0045、0047）

| 文件 | 真源 | 说明 |
| --- | --- | --- |
| `knowbase-vocabulary-lock.json` | `knowbase-app/contracts/data-request/v1/vocabulary.json` | 需求、渠道、订阅用的词表副本（`app/domain/supply/vocabulary.py`）：细类与大类、定期报告、事项、版本形态，主体编号的几种写法（`subject_kinds`：证券代码、`org:` 机构编号）与机构编号能采哪些细类（`collectable.org`，政策法规、统计数据）；并列检出 knowbase-app 时 `tests/test_knowbase_vocabulary_lock.py` 核锁与真源一致、副本与真源一致 |
| `knowbase-chart-lock.json` | `knowbase-app/contracts/data-request/v1/chart-of-accounts.json` | 财报抽表写进 knowbase 标准化层的科目编码照它（`app/application/supply/processing.py` 的 `FIELD_TO_CODE`）；并列检出 knowbase-app 时 `tests/test_knowbase_chart_lock.py` 核锁与真源一致、编码都在真源里 |
| `knowbase-user-data-lock.json` | `knowbase-app/contracts/user-data/v2/user-data.schema.json` | 供给侧实际发送的事实批、公开引用请求与非公开逐读法回执按提供方 schema 验证；公开引用只发 subject/ref，结构化 basis 留在需求记录及 collected 受益人契约中。非公开 readings 只表示实际就绪种类，不能由 has_readings 推断全齐；旧提供方缺字段时按各受益人的授权补加工本人所需读法。 |

## 提供方的契约（knowfeed 是真源）

| 目录 | 说明 |
| --- | --- |
| `supply-status/v1/` | 需求状态推送 `knowfeed-supply-status/v1`（SDD 0045 第 2.5 节「需求状态只由 knowfeed 通知」）：需求项状态每变一次，knowfeed 经发件箱给提这张单的应用发一条，带这张单的汇总状态与每一项。由后端 `app/domain/supply/status_event.py` 的模型生成：在 `knowfeed-backend/app` 下 `python -m app.domain.supply.schema`；`tests/test_supply_status_db.py` 核文件与模型逐字一致。接收端入口 `POST /api/internal/v1/knowfeed/demand-status`，knowfeed 以服务身份调，关系 `supply-status`，scope `<应用>:supply-status`（`contract-manifest.json` 写着）。knowbench、knowlab 的接收端与它们的锁均已做 |
| `supply-prematch/v1/` | 预切合 `knowfeed-supply-prematch/v1`（SDD 0045 第四节）：确认之前，应用问这几项（描述或网址）这类能取 / 取不到 / 网站待审 / 不满足给的条件、候选渠道与许可档、按队列与限速估的预计时间（秒）；只查来源目录与队列，不问渠道、不看 knowbase。入口 `POST /api/internal/v1/supply/prematch`，同需求接口的关系 `supply-demand`、scope `knowfeed:supply-demand`。应用把回答连同可接受的条件照存进 knowbase 确认的供给条件快照：`supply = {"conditions": {"min_license": "via_expert", "channels": []}, "prematch": <回答>}`，受理时 knowfeed 只核 `conditions`（许可档下限、只用哪几条渠道），候选渠道都不满足的需求项转「无法满足」附 `conditions_unmet`。请求、回答两份 schema 由后端 `app/domain/supply/prematch.py` 的模型生成（同上一条的命令），`tests/test_supply_schedule_db.py` 核文件与模型逐字一致 |

## 持有的锁：各版在用情况（SDD 0052 第三节，2026-11-06 第一次整理）

本仓每份契约只持一版，都在用，没有可退的。退役条件与顺序同 knowbench、knowlab 两仓 `contracts/README.md` 末尾的表。本仓作为提供方出 `supply-prematch/v2`（SDD 0051）时，同一笔提交里写 v1 的退役条件。

| 锁 | 提供方 | 契约 | 状态 |
| --- | --- | --- | --- |
| `knowbase-chart-lock.json` | knowbase | 图表 | 在用 |
| `knowbase-provider-lock.json` | knowbase | `knowfeed-knowbase-artifact/v1` 交付资料 | 在用 |
| `knowbase-user-data-lock.json` | knowbase | `user-data/v2` | 在用 |
| `knowbase-vocabulary-lock.json` | knowbase | 描述词表 | 在用 |
