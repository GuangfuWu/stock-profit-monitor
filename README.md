# 📈 stock-profit-monitor

> 零代码部署的自动化股票盈亏监控与预警系统

![Demo](./assets/lg-data-demo.gif)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

---

## 简介

**stock-profit-monitor** 是一个面向普通投资者的 **无服务器、全自动** 股票盈亏监控与预警工具。它能够：

- 📊 **自动抓取** 实时/延迟股价（支持美股、港股、A股）
- 💰 **逐日计算** 持仓盈亏（当日盈亏 & 总持仓盈亏）
- 🔔 **阈值预警** — 当盈利或亏损超过设定比例/金额时，通过 Webhook 推送通知
- 📱 **多渠道推送** — 飞书（Feishu）、企业微信（WeCom）、钉钉（DingTalk）三选一，或同时推送

---

## 零代码一键部署

本项目作为 [LG-Data 数据编排平台](https://lg-data.cc/market) 的官方模板发布。

**无需写一行代码**，在 LG-Data 平台完成以下三步即可运行：

1. 打开 [LG-Data 应用市场](https://lg-data.cc/market)，搜索 **stock-profit-monitor** 并点击 **一键导入**。
2. 在可视化流程编辑器中，填写 `config.yaml` 中的股票代码、成本价以及 Webhook 地址。
3. 设置定时触发器（例如：工作日收盘后自动运行），点击 **启动** 即可。

> 💡 LG-Data 是一个支持拖拽式流程编排的低代码数据平台，支持 HTTP 请求、数据转换、条件判断等节点，
> 适合构建各类自动化数据管道。

---

## 功能特性

| 功能 | 说明 |
|------|------|
| 多股票监控 | 支持同时监控多只股票，单独配置成本价和持仓数量 |
| 当日盈亏 | 基于昨收价与今日最新价计算当日浮动盈亏 |
| 总持仓盈亏 | 基于成本价与当前价计算总体盈亏金额与百分比 |
| 盈亏预警 | 可按金额或百分比设置触发阈值 |
| Webhook 推送 | 支持飞书、企业微信、钉钉机器人 |
| 定时执行 | 通过 LG-Data 平台配置 Cron 定时任务，自动在收盘后运行 |

---

## 配置说明

编辑 `template/config.yaml` 填写您的持仓信息：

```yaml
stocks:
  - symbol: AAPL
    name: 苹果
    shares: 100
    cost_price: 150.00
  - symbol: TSLA
    name: 特斯拉
    shares: 50
    cost_price: 200.00

alert:
  profit_threshold_pct: 5.0    # 盈利超过 5% 时推送
  loss_threshold_pct: -3.0     # 亏损超过 3% 时推送

webhook:
  feishu: "https://open.feishu.cn/open-apis/bot/v2/hook/YOUR_TOKEN"
  wecom: ""
  dingtalk: ""
```

---

## 工作流说明

`template/workflow.json` 包含完整的可视化流程定义，主要节点包括：

1. **Trigger（触发器）** — 定时或手动触发
2. **Fetch Price（获取股价）** — HTTP 请求节点，从行情 API 拉取最新价格
3. **Calculate PnL（计算盈亏）** — 数据转换节点，计算当日/总体盈亏
4. **Check Threshold（阈值判断）** — 条件分支节点，判断是否触发预警
5. **Send Alert（发送预警）** — HTTP POST 节点，向 Webhook 地址推送格式化消息

---

## 项目结构

```
stock-profit-monitor/
├── assets/
│   └── lg-data-demo.gif        # 平台演示动图
├── template/
│   ├── config.yaml             # 用户配置文件模板
│   └── workflow.json           # LG-Data 流程定义文件
├── LICENSE
└── README.md
```

---

## 开源协议

本项目采用 [MIT License](./LICENSE) 开源协议，欢迎自由使用与二次开发。                                                                                                   
