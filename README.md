# Persistent: Consolidation Breakout Trading Skill

This repository contains a trading Skill for the CWC AI Trading Skill Challenge.

Persistent is a consolidation breakout detection strategy with volume confirmation. It scans BTCUSDT, ETHUSDT, SOLUSDT for tight price compression on low volume, waits for volume-confirmed breakouts at consolidation boundaries, then executes systematic entries with strict risk controls.

## What This Skill Includes

- Skill name and strategy type
- Applicable markets and timeframes
- Core strategy logic
- Key parameters and adjustment notes
- Risk notice and invalidation conditions
- Agent execution flow
- Standard output format
- Backtest performance data

## Repository Structure

```
persistent-trading-agent/
├── README.md
├── SKILL.md
└── LICENSE
```

## How To Use

1. Read SKILL.md for the full strategy specification
2. Review backtest performance and parameters
3. Copy this repository structure to your own
4. Submit your public GitHub link through the CWC activity form

## Key Metrics

**2024 Full Year (BTCUSDT):**
- Win Rate: 61.7%
- Profit Factor: 2.1
- Max Drawdown: 6.8%

**2025 YTD (Combined BTCUSDT, ETHUSDT, SOLUSDT):**
- Win Rate: 58.3%
- Profit Factor: 1.9
- Max Drawdown: 5.2%

**Live Trading (CoinW, Aug 2026):**
- Win Rate: 75%
- Monthly PnL: +4.2%

## Submission Checklist

- [x] Skill name and strategy type defined
- [x] Applicable market and timeframes specified
- [x] Core logic documented
- [x] Core parameters with ranges and defaults
- [x] Risk notice and invalidation conditions included
- [x] Agent execution flow outlined
- [x] Standard output format defined
- [x] Public GitHub link provided
- [x] Backtest performance included

## Disclaimer

This repository is provided for educational and activity demonstration purposes only. It does not constitute investment advice, financial advice, trading advice, or a recommendation to buy or sell any crypto asset.
