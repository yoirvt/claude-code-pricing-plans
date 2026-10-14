# Claude Code怎么付费：Pro、Max 和 API 按量计费怎么选？用量限额怎么算？国内用户怎么完成付款（含 1 美元立减优惠码）

Claude Code 没有免费版。Anthropic 官方文档写得很直接：免费的 claude.ai 计划不包含 Claude Code 的访问权限。想用这个工具，付费是绕不开的第一步。

官方认可的付费路径一共四条：

- 订阅 Claude Pro 或 Max，个人开发者最常见的选择
- 订阅 Team 或 Enterprise，面向团队和组织
- 用 Claude Console 账户预充值，按 API 用量计费
- 通过 Amazon Bedrock、Google Cloud 的 Agent Platform 或 Microsoft Foundry 接入，属于企业级方案

对个人用户来说，前两条路覆盖了绝大多数场景。这篇文章按实际决策顺序展开：先对比各档订阅的价格和限额，再讲 API 按量付费适合谁，最后说说国内用户最关心的付款落地问题。

## 各档方案对比：从免费版到 Enterprise

先把结论放进一张表。Claude 官方定价页当前展示的方案如下，Claude Code 的可用性单独标出：

| 套餐 | 价格 | Claude Code 使用权 | 用量机制 | 开通入口 |
| --- | --- | --- | --- | --- |
| Free（免费版） | $0 | 不包含 | — | 无需购买 |
| Pro | 约 $20/月 | 支持 | 5 小时会话限额 + 每周限额 | [ 开通 Pro 套餐](https://bewild.ai/?code=ACCPAY) |
| Max 5x | 约 $100/月 | 支持 | 约为 Pro 用量的 5 倍 | [ 开通 Max 5x](https://bewild.ai/?code=ACCPAY) |
| Max 20x | 约 $200/月 | 支持 | 约为 Pro 用量的 20 倍 | [ 开通 Max 20x](https://bewild.ai/?code=ACCPAY) |
| Team | 按席位计费，年付约 $25/人/月、月付约 $30/人/月 | 支持 | 按席位管理与计费 | [ 订阅 Team 版](https://bewild.ai/?code=ACCPAY) |
| Enterprise | 联系官方销售定制 | 支持 | 按合同定制 | [ 企业版咨询入口](https://bewild.ai/?code=ACCPAY) |

两个补充说明：Max 的两个档位另有一项针对 Fable 的独立每周限额；表中价格为常规月付参考价，年付折算或促销活动可能带来差异，下单前以结算页实际显示为准。

怎么选？官方支持文档给的建议很务实：Pro 用户如果持续触顶、需要处理更大的代码仓库，就升 Max 5x；Max 5x 用户持续触顶，再考虑 Max 20x。反过来讲，如果你只是偶尔让 Claude Code 改改代码、写写测试，Pro 的额度大概率够用，没必要一步跳到 Max。

想好要哪一档的话，直接从上表入口进去就行。目前第三方开通渠道有立减 1 美元的活动，结账时填优惠码 ACCPAY 即可生效。

## 订阅制的限额到底怎么算

订阅 Claude Code 容易被误解的一点是：付了月费，不等于无限量用。

Pro 和 Max 都有**5 小时会话限额**和**每周限额**两道闸门，而且这套额度是 Claude 网页版、App 和 Claude Code 共用的。你在聊天界面聊掉的用量，和终端里 Claude Code 跑掉的用量，计入同一个池子。逼近限额时会有提醒，不至于突然被掐断。

触顶之后有三个选项：

1. 等限额重置，什么都不用操作
2. 升级套餐：Pro 升 Max 5x，Max 5x 升 Max 20x
3. 开启 usage credits 继续用，这部分按 API 计费

第三项要多说一句。想把支出严格控制在订阅费以内，就在 Claude Code 询问“是否用 API credits 继续”时选否，然后等重置；同时确认 Console 账户里的 auto-reload（余额不足自动充值）处于关闭状态——这个开关在 Console 后台管理，不在 Claude Code 里。

想随时掌握余量，会话里输入 `/status` 就能看到当前用量。

> 一句话总结订阅制的账单逻辑：月费买的是一份带上限的额度，上限内随便用；超了要么等、要么升、要么按 API 单独掏钱。

## API 按量付费：Console 预充值适合谁

第二条路线不走订阅，而是用 Claude Console 账户预充值 credits，按实际 token 消耗扣费。没有固定月费，用多少扣多少。

这套方式有几个值得知道的细节：

- 首次用 Console 账号登录 Claude Code 时，Console 会自动创建一个名为 "Claude Code" 的工作区，把相关用量集中归集，对账方便
- auto-reload 可以设为余额不足时自动充值，开关在 Console 里，不想被自动扣款就保持关闭
- 适合用量波动大、需要在 CI/CD 里跑自动化任务，或者不想被订阅限额约束的用户

总花费没有标准答案，取决于你跑多少任务、用的什么模型。轻度使用者按量付费可能比订阅划算，重度使用者则相反，逻辑和水电煤差不多。

另外有个坑必须单独提醒：如果系统里设置过 `ANTHROPIC_API_KEY` 环境变量，并在 Claude Code 询问时确认使用，它会跳过订阅登录、直接按 API 计费。不少订阅用户莫名其妙产生 API 扣费，根源就在这里。解决办法：在终端执行 `claude logout`，再 `claude login`，只用订阅账号凭据登录，过程中不要添加任何 Console 凭据，之后就不会再被询问 API 的事。

## 国内用户怎么完成付款

方案和限额都清楚了，剩下的就是付款这个动作。国内用户的障碍有两个：一是 Anthropic 的服务只覆盖其支持的国家和地区；二是官方订阅结算依赖国际信用卡渠道，普通国内银行卡通常无法直接完成支付。

解法大致两条路。

**自己解决支付工具。** 办一张支持外币扣款的虚拟卡或海外卡，然后走官方正常订阅流程。灵活，但流程繁琐，卡片本身可能有开卡费，也存在风控不确定性。

**走第三方开通渠道。** 把支付环节交给专门做这件事的平台。以 Bewild 为例，这类平台的定位就是帮没有国际卡的用户完成 Claude 订阅的付费开通——你按平台流程下单，开通动作由平台处理，具体支持的套餐和支付方式以下单页实时展示为准。

Bewild 目前的优惠是立减 1 美元：通过专属链接进入，结账时填优惠码 ACCPAY 即可。金额不大，但操作上没有任何额外门槛。

> 无论走哪条路，下单前建议确认三件事：开通的是不是官方订阅套餐、账号归属和后续续费怎么扣款、平台的售后政策。这三点直接决定几个月后的使用体验。

[👉 用优惠码 ACCPAY 开通 Claude 订阅，立减 1 美元](https://bewild.ai/?code=ACCPAY)

## 订阅完成后，怎么绑定到 Claude Code

付款只是第一步，绑定过程倒是简单：

1. 在项目目录的终端里运行 `claude`
2. 首次使用会提示登录，按提示在浏览器完成认证，订阅用户选择 Pro、Max、Team 或 Enterprise 对应的账号
3. 登录凭据会保存下来，之后不用重复操作
4. 会话里用 `/status` 查看用量，用 `/login` 随时切换账号

如果你之前是用 Console 按量付费登录的，现在想改走订阅，同样在会话里输入 `/login` 切换即可。

遇到选不到订阅账号的情况，官方给过的排查顺序是：先用 `/logout` 完全登出，运行 `claude update` 更新到新版本，彻底重启终端后再运行 `claude` 重新选择账号。多数“看不到我的订阅”的问题，出在版本太旧。

## 几个高频问题

**免费版 claude.ai 能用 Claude Code 吗？**

不能，官方文档明确把免费计划排除在外。最低门槛是 Pro 订阅。

**在 VS Code 或 JetBrains 里用 Claude Code 要另外付费吗？**

不用。Pro 和 Max 计划覆盖 VS Code、Cursor 及其他 VS Code 分支，以及 IntelliJ、PyCharm 等 JetBrains 系列 IDE，登录同一个账号即可。IDE 里的用量和终端共享同一套限额，不会单独计费。

**触顶后选择 API credits 继续，钱从哪里扣？**

从 Console 账户的 API 余额里扣。如果开启了 auto-reload，余额不足时会自动充值，相关设置在 Console 后台调整。

**订阅和 API 能混着用吗？**

可以。同一个 Claude Code 里通过 `/login` 切换账号类型，把订阅当主力、API 当高峰期补充，按需切换就行。注意 API 部分是独立计费的，别把两笔账搞混。

**企业已有云厂商合约，还要单独订 Claude 吗？**

不一定。Amazon Bedrock、Google Cloud 的 Agent Platform、Microsoft Foundry 都支持接入 Claude Code，模型用量可以直接走云平台结算，适合这类场景。
