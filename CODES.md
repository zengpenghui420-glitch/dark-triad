# 限次验证码说明 · 黑暗人格三角测试

- 格式：`DT` + 4 位，例如 `DT7K3N`（共 6 位）
- 字符表：`ABCDEFGHJKMNPQRSTUVWXYZ23456789`（不含 I、L、O、0、1）
- 每个验证码最多使用 **3 次**（跨设备共享计数，存在 MantleDB）
- 命名空间：`catquiz-zph420-codes`，集合：**`dt/`**
- 订单号不是访问码，不会写入 `dt/`，也不会请求订单接口。`P` + 18 位且通过格式检查后页面直接开始。提示从「还可以测 2 次」到「这是最后一次」；之后仍可进入，提示「验证成功」
- **只适用于本测验**。猫咪、MBTI、八字、历史、SCL、前世动物等其他测验的码不能在这里用，这里的 `DT` 码也不能拿到别的测验里用
- 「重新测试」会清掉本次解锁，再次核销验证码（**−1**）
- 「查看上次结果」只读本机 `localStorage`（键 `dark_triad_last_result`），**不扣次**
- 同一标签页里，已经解锁之后再点「开始测试」不会重复扣次；要重测并扣次，走「重新测试」
- 发码：付款后把 6 位码发给买家即可

共享 namespace 的免费档大约 **100 条**。猫咪 / MBTI / 八字 / 历史 / SCL / 前世动物已经把名额占满时，造码会返回 `Entry limit reached`，脚本会直接失败，不会把失败的码算进成功列表。腾出条目或升级 Mantle 之后再造。

继续造码（在已配置 `MANTLE_KEY` 的机器上）：

```bash
MANTLE_KEY=*** ./tools/mint.sh 10
```

默认一次造 10 个。后端：MantleDB（namespace `catquiz-zph420-codes`，collection `dt`）。浏览器核销先 `POST /redeem`（quiz `dark`）。vault 没有这枚码时才走 `dt/{验证码}`：先 GET，再 POST `increment`，`remaining` −1。vault 已用尽则不再落到 Mantle。
