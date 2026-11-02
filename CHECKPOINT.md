
## 2026-11-02 巨潮采单交付修复（luna，候选未发布）

原采单 3ee2184d-63c5-4b64-be75-bf7206fb293b 已在 08:29:27+08 由本人确认。失败在交付：facts 原样行 label 为空触发整批 422；公开 references 多传 basis，被提供方 extra=forbid 拒绝。巨潮原件已留存，不能归因为用户未确认或外网未采到。

抽表沿现有唯一跨行科目匹配保留 source_label，空数字行只在可唯一匹配时关联真实原文；其余留诊断、不编科目名。processor_version 升为 periodic-statements-2；公开引用仍保存需求 ref，按已发布 user-data/v2 契约发送 subject/ref。新增 consumer lock 与实际 HTTP body 的 schema 验证（此前 MockTransport 一律 200 掩盖契约漂移）。顺带修复 26 条既有类型错误，没有放宽检查规则。

真实已留存的 2026Q1 PDF 只读复核：此前 244 raw/97 standard，44 个空 label 校验错误；修复后 219 raw/97 standard、14 条未匹配空标签诊断，提供方 schema 零错误，全部标准化数值/期间/页码保持一致。临时证据 /tmp/luna-live-facts-payload.json、/tmp/luna-live-facts-repaired.json；原文和金融数值不提交仓库。

状态通知关系仍未启用；knowbench 有带退避的按单号查询兜底（最长 5 分钟），本轮界面必须展示最近核对时间，不能把跳过通知当成取数完全没启动。修复经 Jenkins 正式发布，不能热改 Pod/重写用户确认。已失败需求不能由管理端 open-job requeue 恢复；工作台补齐同一任务「重新核对采单 → 本人确认」的正常路径，保留范围与历史、新一轮幂等键防止重放失败旧单。

最终验证：1033 passed、13 skipped（/tmp/luna-supply-repair-tests-final.txt）；ruff 全通过，pyright 0 errors（/tmp/luna-supply-pyright-final.txt）。后端提交 bb85408f20c78dbda0301315dfd3de0bbfc7d955 已推送 GitHub fix/supply-delivery。2026-11-02 所有者将发版交给 qwen，luna 负责交付候选与继续开发；交接目录为 qwen/sunmoonun/docs/总纲/发版，本工位不触发发布。
