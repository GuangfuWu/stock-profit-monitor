# 📈 自动盯盘与盈亏推送助手 (Stock Profit Monitor)

[![Deploy to Privora](https://img.shields.io/badge/Deploy%20to-Privora-5aa4ff?style=for-the-badge&logo=appveyor)](https://privora.cn/use-cases/track-portfolio-risk)
[![License](https://img.shields.io/badge/license-MIT-green?style=for-the-badge)](#-license)
[![ClawHub Skill](https://img.shields.io/badge/ClawHub-privora--cn--quant-blueviolet?style=for-the-badge)](https://clawhub.ai/guangfuwu/skills/privora-cn-quant)

![Demo](./assets/lg-data-demo.gif)

作为一个打工人，每天盯着炒股软件看太心累了，一不小心还会错过止盈止损点。

这是一个基于数据流编排的**轻量级自动化盯盘工具**——A 股 / 港股持仓盈亏自动监控 + 阈值触发 Webhook 推送（微信 / 飞书 / 钉钉）。底层由 [Privora](https://privora.cn/market) 数据后端 + Serverless 调度撑起，**零服务器运维**。

---

## ✨ 核心特性

- **📊 极简看板**：直观展示当日盈亏、累计盈亏、个股涨跌幅、行业归因
- **🚨 自定义告警**：单股跌穿 MA20 / 单日跌幅 > 阈值 / 进入龙虎榜 / 财报公告日 都能触发 Webhook
- **🇨🇳🇭🇰 A 股 + 港股全覆盖**：底层 `stock_day` 表统一 8500+ 股票，按 ticker 自动路由（`000001` / `00700.HK`）
- **🔒 持仓字段级加密**：持仓数量 / 成本价 / 交易价格密文存 DB，per-tenant DEK 隔离，DBA 也读不到明文
- **☁️ 零代码 / 零运维**：不用自己购买服务器挂脚本、不用处理 cron、不用应对上游限流和网页改版
- **⚡ 1 分钟一键部署**：Fork 模板 → 填自选股 + Webhook → 上线

## 🚀 快速开始（1 分钟极速体验）

[一键 Fork 模板 → privora.cn/market](https://privora.cn/use-cases/track-portfolio-risk?utm_source=github&utm_campaign=stock-profit-monitor&utm_content=quick-start)

1. 点击上方链接进入 [Privora 控制台](https://privora.cn/market?utm_source=github&utm_campaign=stock-profit-monitor)
2. 注册账号（**免费 + 免注册可先预览**）
3. 在 Marketplace 找到 `Stock Profit Monitor` 模板
4. 一键 Fork → 填自选股代码 + Webhook URL → 运行
5. 你的专属盯盘机器人就上线了，开盘自动跑 + 触发阈值自动推送

## 🛠️ 定制与二次开发

如果你需要接入更多通知渠道（企业微信群机器人 / 个人邮件 / 短信）或修改计算逻辑（按行业归因 / 持有天数加权 / 多账户合并），本模板完全支持在 Privora 画布中拖拽修改 Processor 节点——也可以用 Python 脚本节点直接写策略：

```python
from lg_utils import get_portfolio_positions, send_webhook

positions = get_portfolio_positions(asset_class="stock")
for p in positions:
    if p.daily_return < -0.05:  # 跌幅 > 5%
        send_webhook(channel="feishu", text=f"⚠️ {p.stock_num} 今日跌 {p.daily_return*100:.2f}%")
```

详见 [Privora Python 工具库文档](https://privora.cn/user-guide?utm_source=github&utm_campaign=stock-profit-monitor&utm_content=customization)。

## 🤖 让 AI Agent 帮你盯盘（进阶玩法）

Privora 提供 Bearer Token 接口，让 Claude / ChatGPT / Hermes / OpenClaw 任何支持外挂工具的 AI Agent **直接读你的持仓 + 调阈值告警**：

```bash
# 1. 注册 Token
# 2. 装 ClawHub skill：
openclaw skills install @guangfuwu/privora-cn-quant
# 3. 让 Claude / Hermes 直接用自然语言操作
```

详见 [Privora AI Agent 接入文档](https://privora.cn/features/agent-investment-analysis-api?utm_source=github&utm_campaign=stock-profit-monitor&utm_content=agent) 或 [ClawHub skill 页面](https://clawhub.ai/guangfuwu/skills/privora-cn-quant)。

## 📚 相关资源

- 🏠 [Privora 主页](https://privora.cn/market?utm_source=github&utm_campaign=stock-profit-monitor)
- 📊 [使用场景：跟踪持仓风险](https://privora.cn/use-cases/track-portfolio-risk?utm_source=github&utm_campaign=stock-profit-monitor)
- 🔔 [功能：实时风险监控](https://privora.cn/features/portfolio-risk-monitoring?utm_source=github&utm_campaign=stock-profit-monitor)
- 📰 [功能：新闻 / 公告监控](https://privora.cn/features/news-announcement-monitoring?utm_source=github&utm_campaign=stock-profit-monitor)
- 🤖 [功能：AI Agent 接入 API](https://privora.cn/features/agent-investment-analysis-api?utm_source=github&utm_campaign=stock-profit-monitor)
- 🧠 [功能：AI 持仓分析](https://privora.cn/features/ai-portfolio-analysis?utm_source=github&utm_campaign=stock-profit-monitor)
- ❓ [常见问题 FAQ](https://privora.cn/faq?utm_source=github&utm_campaign=stock-profit-monitor)
- 🛠 [ClawHub Skill: privora-cn-quant](https://clawhub.ai/guangfuwu/skills/privora-cn-quant)

## 🤝 反馈与贡献

- 用得不爽？想要新能力？欢迎提 [Issue](https://github.com/GuangfuWu/stock-profit-monitor/issues)
- 觉得好用？请去 [ClawHub 给个 ⭐ star](https://clawhub.ai/guangfuwu/skills/privora-cn-quant)，让更多散户找到这个工具

## 📄 License

MIT License — 自由使用、修改、分发。
