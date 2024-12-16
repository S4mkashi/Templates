---
盈亏: 
交易质量: 
操盘质量: 
Asset: 
IsDayTrade: false
EnterDate: 
EntryTimeFrame: 
Action: 
D1: 
H1: 
M5: 
ChanceOfSuccess: 
TradeEntryPrice: 10
StopPrice: 10
SL: 10
Risk%: 
InitialR:R: 
ExitDate: 
离场策略: 
WinLoss: 10
"% Return": 
潜在损失: 
High: 10
Low: 10
Exit: 5
Duration: 
ReviewDate: 2024-12-16
tags:
  - TradingJournal
---

```dataview
TABLE 
choice(
    Action[0] = "Long", 
    max(0, (Exit - TradeEntryPrice) / (High - TradeEntryPrice)), 
    max(0, (TradeEntryPrice - Exit) / (TradeEntryPrice - Low))
) AS "SwingPercentage",
choice(
    Action[0] = "Long",
    choice(
        (TradeEntryPrice - Low) <= 0,
        0,
        (TradeEntryPrice - Low) / (TradeEntryPrice - StopPrice)
    ),
    choice(
        (High - TradeEntryPrice) <= 0,
        0,
        (High - TradeEntryPrice) / (StopPrice - TradeEntryPrice)
    )
) AS "MAE" ,
choice(
    WinLoss >= 0, 
    WinLoss / SL,
    choice(
        Action = "Long",
        (High - TradeEntryPrice) / -1*WinLoss,
        (TradeEntryPrice - Low) / -1*WinLoss
    )
) AS "Actural R:R"
WHERE file.name = this.file.name
