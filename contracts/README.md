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
