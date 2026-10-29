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
| `knowbase-chart-lock.json` | `knowbase-app/contracts/data-request/v1/chart-of-accounts.json` | 财报抽表写进 knowbase 标准化层的科目编码照它（`app/application/supply/processing.py` 的 `FIELD_TO_CODE`）；并列检出 knowbase-app 时 `tests/test_knowbase_chart_lock.py` 核锁与真源一致、编码都在真源里 |

## 提供方的契约（knowfeed 是真源）

| 目录 | 说明 |
| --- | --- |
| `supply-status/v1/` | 需求状态推送 `knowfeed-supply-status/v1`（SDD 0045 第 2.5 节「需求状态只由 knowfeed 通知」）：需求项状态每变一次，knowfeed 经发件箱给提这张单的应用发一条，带这张单的汇总状态与每一项。由后端 `app/domain/supply/status_event.py` 的模型生成：在 `knowfeed-backend/app` 下 `python -m app.domain.supply.schema`；`tests/test_supply_status_db.py` 核文件与模型逐字一致。接收端入口 `POST /api/internal/v1/knowfeed/demand-status`，knowfeed 以服务身份调，关系 `supply-status`，scope `<应用>:supply-status`（`contract-manifest.json` 写着）。knowbench、knowlab 的接收端与它们的锁还没做 |
