# 每小时汇报优化（XB-hourly-report-fix）

## 目标
修复「每小时汇报」cron 的重复投递问题，并补齐 KPI 统计所需字段。

## 验收
1. ✅ 飞书不再重复接收"每小时汇报"（reminder + 实际汇报只出现一次）
2. ✅ 多维表新增「审批通过时间」字段，用于精确统计"今日累计"
3. ✅ cron 模板更新为"单一投递模式"（不启用 announce）

## 节点清单

### ✅ 节点 1：诊断问题根因
- 操作：检查 cron 配置，确认重复投递来源
- 产出物：问题说明文档
- 验收：明确重复投递的技术原因
- **结论**：hourly-report cron 的 delivery.mode 原为 "announce"，与 heartbeat 触发造成重复投递

### ✅ 节点 2：补齐多维表字段
- 操作：在《项目进度》多维表新增「审批通过时间」（Number 类型）
- 产出物：字段创建成功 (field_id: fldiaQWct2)
- 验收：KPI 统计能按"北京时间 00:00 起的审批通过时间"过滤

### ✅ 节点 3：更新 cron 模板
- 操作：将 hourly-report cron 的 delivery.mode 改为 "none"
- 产出物：cron 配置已更新
- 验收：整点只收到一次汇报（由 heartbeat 触发）

### ✅ 节点 4：验证 KPI 统计
- 操作：确认字段可用， cron 配置正确
- 产出物：本 README 文档
- 验收：KPI 口径与实际一致

## 风险与对策
- ~~风险：多维表字段类型不支持 DateTime → 对策：改用 Number（时间戳毫秒）~~ ✅ 已解决
- ~~风险：cron 改为 none 导致完全不投递 → 对策：先在测试环境验证~~ ✅ 已验证（heartbeat 仍会投递）

## 里程碑完成情况
- ✅ M1：诊断完成 + 字段补齐（30 分钟）
- ✅ M2：cron 配置更新 + 验证（1 小时）

## 项目状态
- **状态**: 已完成
- **完成时间**: 2026-03-06 19:28 UTC
- **进度**: 100%

## 配置变更
- cron job: `b73d177f-d927-40be-8f8c-eb3e07e7aa1c` (hourly-report)
  - delivery.mode: "announce" → "none"
- 多维表: `tblC57GPcHwMhbHN`
  - 新增字段: 「审批通过时间」(field_id: fldiaQWct2, type: Number)
