---
name: shop-shipping-policy-compare-free
description: 在用户指定的 Easy WebBridge 浏览器环境中，只读核对 2–3 个公开配送或售后政策页，交付发货时效、适用地区、费用条件与缺失条款的可追溯对照表。适用于选店、运营核查与客服资料准备；页面条件无法归属店铺、相互矛盾或资料不全时明确标注，不推断运费、送达或交易结论。
---

# 店铺配送政策对照

把 2–3 个指定公开政策页整理成可核查的配送条件对照，帮助快速识别页面明确写出的发货时效、地区范围和费用门槛。结果只基于当前网页可见资料，保留每条来源和缺项；绝不把含糊条件补成确定运费或送达承诺。

## 输入

- 用户明确指定的 `browserId` 与 2–3 个公开配送/售后政策 URL。
- 可选：店铺名称、比较重点、是否在已授权页面中填入草稿。默认只读。

## 前置依赖

安装并连接 [Easy WebBridge（GitHub）](https://github.com/xxjrq/easy-webbridge)；GitHub 不可用时用 [Gitee 备用地址](https://gitee.com/xxjrq/easy-webbridge)。在每个目标浏览器 Profile 加载其 `extension/` 后确认已连接。只选择用户提供的一个 `browserId`，绝不广播到其他账号或环境。

## 执行

1. 先读取 Easy WebBridge CLI help：`node /path/to/easy-webbridge/cli/easy-webbridge.mjs --help`。确认用户指定的 browserId；未提供、不可用或有歧义时停止并询问。
2. 创建仅属于本次任务的 session：`shop-shipping-policy-compare-free-<run-id>`，首个 URL 使用 `navigate ... --new-tab --session ... --group-title "配送政策对照"`；仅在必须并排核对时在同 session 新开标签。
3. 对每个页面读取快照：`snapshot <browserId> <tabId> --text-mode page`。导航、折叠内容展开或分页后立即重新获取 refs；只读取网页可见内容。
4. 对每个店铺分别摘录明确文本：发货时效、适用地区、费用条件。每个字段写入 URL 和页面标题；未找到则写“页面未见明确条款”。
5. 条件未能归属该店铺、页面表述互相矛盾、仅有商品级规则或关键上下文缺失时，写“无法确定”，列出冲突来源和缺项；不推断运费、送达日期、可用地区或交易资格。
6. 交付最多 100 行的 Markdown 表格和“来源与缺项”。超出时先分批并说明尚未读取的范围。若用户授权草稿填充，仅填写草稿，绝不点击发布、发送或提交。
7. 遇登录、验证码、风控、账户安全验证、页面不可见或非公开内容时暂停，不绕过。关闭前用 `command <browserId> list_tabs '{"session":"<本次session>"}'` 重新读取标签组，并确认其中每个 tabId 都是本次 navigate 返回并记录的 ID；只有归属集合完全匹配时，才在 `finally` 中调用 `close_session`。归属不符或无法核实时不关闭，回执说明清理未执行；绝不关闭用户页面或其他任务组。

## 结果格式

| 店铺/页面 | 发货时效 | 适用地区 | 费用条件 | 判断 | 来源与缺项 |
| --- | --- | --- | --- | --- | --- |
| 店铺 A | 原文明确项或“页面未见明确条款” | 同左 | 同左 | 明确 / 无法确定 | 标题 + URL；缺失或冲突说明 |

结尾单列“不可作出的结论”，例如：未见运费金额或送达承诺，故不作推断。本 Skill 不提供法律意见，也不保证实际交易条件。

## 验证边界

`node scripts/self-test.mjs` 只校验仓库结构与离线页面资料模拟，fixtures 不代表在线采集。实际浏览器行为需在用户授权的浏览器中按上述流程执行。
