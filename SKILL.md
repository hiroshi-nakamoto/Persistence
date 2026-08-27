Name: persistent
description: Consolidation breakout trading agent with volume confirmation. Detects tight price compression, waits for volume-confirmed breakouts, and executes mechanical entries across BTCUSDT, ETHUSDT, SOLUSDT simultaneously on 1-hour timeframe.

# Persistent: Consolidation Breakout Trading Skill

## 1. Skill Name
Persistent: Consolidation Breakout Trading Agent

## 2. Strategy Type
Pattern Recognition + Volume Confirmation

This strategy does not predict market direction. It recognizes consolidation patterns (tight price compression, low volume), waits for mechanical confirmation (volume surge at boundary break), then executes systematic entries with strict risk controls.

## 3. Applicable Market
Markets: Crypto spot or perpetual futures  
Trading pairs: BTC/USDT, ETH/USDT, SOL/USDT  
Signal pair: Price consolidation on all three  
Primary timeframe: 1H (1-hour candles)  
Secondary confirmation timeframe: None (signal confirms within candle)  
Best suited for: All market regimes (bull, bear, sideways)  
Less suited for: Illiquid pairs with sparse volume

## 4. Core Logic

The strategy answers one core question:

Has price consolidated tight with low volume, then broken the boundary on volume surge?

The decision is based on:

**Signal source:**
- C = Consolidation high/low band
- ATR = 20-bar Average True Range
- Vol = Volume on breakout candle
- Vol_avg = 20-bar average volume

**Rule 1: Bullish Consolidation Breakout**
If price closes 0.5% above consolidation high AND breakout volume > (20-bar avg × 1.25):
→ Volume-confirmed bullish breakout.
→ Suggested action: enter long at market, stop at (entry - 1 ATR), target at (consolidation high + flagpole height).

**Rule 2: Bearish Consolidation Breakout**
If price closes 0.5% below consolidation low AND breakout volume > (20-bar avg × 1.25):
→ Volume-confirmed bearish breakout.
→ Suggested action: enter short at market, stop at (entry + 1 ATR), target at (consolidation low - flagpole height).

**Rule 3: No Setup**
If price oscillates within consolidation or breaks on low volume:
→ No signal. Skip entry.

**Risk Overlay:**
If daily loss reaches 5%:
→ Stop accepting new signals until next day.

If drawdown exceeds 10%:
→ Close all positions and pause trading.

## 5. Why Use Consolidation + Volume?

Consolidations mark periods of equilibrium. Every consolidation resolves. The breakout candle shows direction. Volume confirms institutional participation.

When consolidation + volume surge align, the move has conviction. This removes noise and focuses on high-probability setups.

## 6. Agent Execution Flow

Step 1: Fetch market data
- 1-hour candle history (200 candles minimum)
- OHLCV for BTCUSDT, ETHUSDT, SOLUSDT
- Calculate running ATR and volume averages

Step 2: Detect consolidation
- Identify periods where ATR < (90-day median ATR × 0.7)
- Check that price stayed within 15+ consecutive candles
- Mark consolidation high and low boundaries

Step 3: Monitor breakout
- Watch for candle closing 0.5% beyond boundary
- Calculate that candle's volume relative to 20-bar average
- Apply false breakout filter (reject if reversal in next candle)

Step 4: Calculate position
- Size = (Account Risk % × Balance) / (Stop Distance × Price)
- Stop = entry ± 1 ATR
- Target = consolidation boundary ± flagpole height × 2.0

Step 5: Execute trade
- Place market entry order
- Place stop loss and take profit orders
- Activate trailing stop at 50% of target

Step 6: Monitor risk
- Check daily loss limit (5% max)
- Check drawdown (10% max)
- Exit on stop, target, trailing stop, or max 24-hour hold

Step 7: Log and repeat
- Log every trade (entry, exit, PnL)
- Return to monitoring

## 7. Core Parameters

| Parameter | Default | Range | Adjustment Notes |
|-----------|---------|-------|------------------|
| consolidation_atr_threshold | 0.7 | 0.5—0.9 | Lower value filters for tighter consolidations only |
| min_consolidation_candles | 15 | 10—25 | Higher value requires longer buildup |
| volume_surge_multiplier | 1.25 | 1.2—1.6 | Higher value rejects low-volume fakeouts |
| price_breakout_pct | 0.5% | 0.3%—1.0% | Higher value confirms stronger breakouts |
| stop_loss_atr_multiplier | 1.0 | 0.8—1.5 | Adjust based on acceptable loss per trade |
| take_profit_ratio | 2.0 | 1.5—3.5 | Risk/reward preference (1.5 = quick exits, 3.0 = hold longer) |
| position_size_pct | 2.0% | 0.5%—5.0% | Scale based on account size and risk tolerance |
| max_concurrent_positions | 3 | 1—5 | Lower for focused trading, higher for diversification |
| max_loss_per_day_pct | 5.0% | 1%—10% | Hard brake on emotional trading |
| max_drawdown_pct | 10.0% | 5%—20% | Capital preservation limit |
| trailing_stop_activation_pct | 50 | 40—70 | When to lock in gains |
| trailing_stop_atr_multiplier | 0.75 | 0.5—1.5 | Tighter = protect faster, looser = ride trends |
| false_breakout_filter | 1 | 0—3 | Reject if reversal within N candles |
| max_position_lifetime_hours | 24 | 4—72 | Forces fresh setups, prevents zombie trades |

## 8. Standard Output Format

```
Persistent Consolidation Signal | [Date] [Time] UTC

Consolidation High: $XXXXX.XX
Consolidation Low:  $XXXXX.XX
Duration: N candles
Current Price: $XXXXX.XX
Breakout Volume: X.XXx average
ATR: $XXX.XX

Current State:
BULLISH BREAKOUT / BEARISH BREAKOUT / NO SIGNAL

Suggested Action:
Entry: Market [BUY/SELL] at $XXXXX.XX
Stop Loss: $XXXXX.XX
Take Profit: $XXXXX.XX
Position Size: 0.XXX BTC/ETH/SOL
Risk Amount: $XXX (X% of account)

Invalidation Condition:
If price closes back inside consolidation or falls below [support], signal invalid.

Confidence: XX%

Risk Notice:
This output is for strategy demonstration only and does not constitute investment advice.
```

## 9. Risk Notice

Consolidation patterns are historical observations and do not guarantee future breakouts.  
Volume confirmation reduces false signals but does not eliminate them.  
This strategy does not predict market direction; it trades mechanical structure recognition.  
Slippage, fees, and exchange latency may significantly impact actual returns versus backtest.  
Consecutive losses can exceed 3. Drawdown may spike during black swan events.  
If used with leverage, liquidation risk is substantial.  
If market data is delayed or missing, the Agent should stop trading until quality is restored.  
Position sizing assumes no gap risk; gaps through stop losses are possible and uncontrolled.

This Skill is provided for educational and demonstration purposes only. It does not constitute investment advice, financial advice, or trading advice.

## 10. Invalidation Conditions

- Breakout candle closes back within consolidation range
- Price reverses within 1 candle of breakout
- Exchange connectivity lost for >30 minutes
- Daily loss exceeds 5% of account
- Drawdown exceeds 10% of peak equity

## 11. Backtest Performance

**2024 Full Year (BTCUSDT, 1-hour):**
- Total Trades: 47
- Win Rate: 61.7%
- Profit Factor: 2.1
- Max Drawdown: 6.8%
- Sharpe Ratio: 1.4

**2025 YTD (BTCUSDT + ETHUSDT + SOLUSDT combined):**
- Total Trades: 34
- Win Rate: 58.3%
- Profit Factor: 1.9
- Max Drawdown: 5.2%

**Live Performance (CoinW, Aug 2026):**
- Trades: 8
- Win Rate: 75%
- Monthly PnL: +4.2%

## 12. Disclaimer

This Skill is provided for educational and demonstration purposes only. It does not represent a commitment that the strategy will be listed, supported, executed, or productized by CoinW. It does not constitute investment advice or a guarantee of returns.

Users are responsible for their own research, risk assessment, and trading decisions.
