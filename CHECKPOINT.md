# 2026-11-02 第二轨 Wind 通用查询与年报证据候选

基线 `ab901b6390a30994a776651273778f5dbba284e3`，子仓先推 GitHub，再钉本仓。

- knowfeed-backend: `a15665a3443e2b2d3c99a2e079fc1ba70fd96936`
- knowfeed-admin-frontend: `a290cde1ffdf1abc3acc2885f23503057a8034f7`

可选 source_query 契约/消费锁同步，既有固定专家和巨潮原件路径保留。财务、公告年报等能力按官方七服务域发现；供应商文本/结构化回执不冒充 PDF 或页码。测试、初始失败和剩余门见子仓 CHECKPOINT 与 sunmoonun 的 Wind-W0评估续记。当前未生产启用，发布仍由 qwen 走 Jenkins。

#52 已发布修复在 knowbase/knowbench 子仓成为祖先；源码与已验证候选相同。实际第二轨演示还待正式服务配置、真实作者/方法与数据读工具链验证，不把取数探针当成业务通过。

以下是之前历史：


## 2026-11-02 巨潮采单交付修复（luna，候选未发布）

原采单 3ee2184d-63c5-4b64-be75-bf7206fb293b 已在 08:29:27+08 由本人确认。失败在交付：facts 原样行 label 为空触发整批 422；公开 references 多传 basis，被提供方 extra=forbid 拒绝。巨潮原件已留存，不能归因为用户未确认或外网未采到。

抽表沿现有唯一跨行科目匹配保留 source_label，空数字行只在可唯一匹配时关联真实原文；其余留诊断、不编科目名。processor_version 升为 periodic-statements-2；公开引用仍保存需求 ref，按已发布 user-data/v2 契约发送 subject/ref。新增 consumer lock 与实际 HTTP body 的 schema 验证（此前 MockTransport 一律 200 掩盖契约漂移）。顺带修复 26 条既有类型错误，没有放宽检查规则。

真实已留存的 2026Q1 PDF 只读复核：此前 244 raw/97 standard，44 个空 label 校验错误；修复后 219 raw/97 standard、14 条未匹配空标签诊断，提供方 schema 零错误，全部标准化数值/期间/页码保持一致。临时证据 /tmp/luna-live-facts-payload.json、/tmp/luna-live-facts-repaired.json；原文和金融数值不提交仓库。

状态通知关系仍未启用；knowbench 有带退避的按单号查询兜底（最长 5 分钟），本轮界面必须展示最近核对时间，不能把跳过通知当成取数完全没启动。修复经 Jenkins 正式发布，不能热改 Pod/重写用户确认。已失败需求不能由管理端 open-job requeue 恢复；工作台补齐同一任务「重新核对采单 → 本人确认」的正常路径，保留范围与历史、新一轮幂等键防止重放失败旧单。

最终验证：1033 passed、13 skipped（/tmp/luna-supply-repair-tests-final.txt）；ruff 全通过，pyright 0 errors（/tmp/luna-supply-pyright-final.txt）。后端提交 bb85408f20c78dbda0301315dfd3de0bbfc7d955 已推送 GitHub fix/supply-delivery。2026-11-02 所有者将发版交给 qwen，luna 负责交付候选与继续开发；交接目录为 qwen/sunmoonun/docs/总纲/发版，本工位不触发发布。

## 第二轨独立开发：feat/reading-receipts（未发布）

在上述发布候选之后独立开发，fix/supply-delivery 分支未移动。后端 8f4ee39a297ccd47d2cecb74a968d0291380cc57 按实际就绪读法补加工、逐受益人领授权、校验回执归属，保留部分成功并隔离写入时撤权。搭配 knowbase-app 09ab4ce4a398f7b3fa7963d471665bfacb4e7ff2（后端 a69436bc440624e87f81974f1ced05f6302505cd）兼容新增的 readings/renditions；consumer lock 已更新，既有 provider schema 定义不变。

完整回归 1043 passed / 13 skipped；最终逐读法累积及中途撤权补充后，所有受影响四个测试文件 47 passed；ruff/pyright 通过。证据 /tmp/luna-readings-feed-full-final.txt、/tmp/luna-readings-feed-revocation.txt。具体验证先后与剩余并发/版本恢复边界见后端 CHECKPOINT，不宣称整个非公开链验收完成。

本轮不接 Wind、不改部署源、不启动 Jenkins。后续发布由 qwen 按完整差异评审；knowbase 候选还包括先前未发布的 WP-3，不得当作仅本次两字段的改动。


## 2026-11-03 #58 后采集等待与 PDF 正文修复候选

仅更新 backend 至 df981b29762a2156f8879d832c1eb7cbe23d856c；web/admin/其它契约锁保持 #58。
说明与验证见子仓 CHECKPOINT 及统一发版交接第11节。没有自动重跑旧任务/旧采单。

## 2026-11-04 勾稽权益抽取修复候选

从发布基线b9464a9出发，仅推进backend至c4df6c87fd607669bdd0acd183939b88f4c3f511，web/admin及契约锁不变。三份Q1原件回放恢复12条归母权益/总权益事实；新加工版本periodic-statements-3。250项回归通过，详情见子仓CHECKPOINT。发版后须正式补加工旧原件生成新读法，部署不会自动修正旧表；不得覆盖既有委托钉版或自动发起模型重试。
