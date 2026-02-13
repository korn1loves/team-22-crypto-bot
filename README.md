# team-22-crypto-bot
# Team 22 - Risk-Aware Multi-Factor Crypto Trading Strategy

## Team Members
- Ilya Bochkarev
- Kornilov Sergei
- Dic Chung
- Egor Rudenko

## Strategy Overview

### LLM Metrics Design

**Extracted Metrics:**
- **sentiment_score** (-1.0 to 1.0): Overall market sentiment from news analysis. Critical for capturing short-term price catalysts from news events.
- **regulatory_risk** (0.0 to 1.0): Assessment of regulatory threats mentioned in news. High values (>0.7) trigger position size reduction to mitigate regulatory shock risk.
- **adoption_signal** (0.0 to 1.0): Signals of institutional adoption, partnerships, or technological breakthroughs. Provides forward-looking growth indicators beyond price action.
- **market_mood** ("bullish"/"bearish"/"neutral"): Qualitative market sentiment classification to cross-validate quantitative sentiment score.
- **confidence** (0.0 to 1.0): LLM's self-assessed confidence in its analysis. Used to weight the influence of news signals in final decision.

**Design Philosophy:** We moved beyond simple sentiment analysis to a multi-dimensional news assessment framework. By extracting regulatory risk and adoption signals separately, we can distinguish between short-term hype (high sentiment but low adoption) and sustainable growth catalysts (moderate sentiment with strong adoption signals). Confidence weighting ensures we don't overreact to ambiguous news.

### Business Logic Design

**Strategy Type:** Hybrid scoring system with dynamic risk management

**Core Decision Framework:**
1. **Weighted Scoring System (100-point scale):**
   - 40% News sentiment (scaled by confidence)
   - 25% RSI momentum (normalized: 70 = neutral, lower = bullish)
   - 20% Bollinger Band position (lower band = buying opportunity)
   - 15% Adoption signals (forward-looking growth indicators)

2. **Decision Rules:**

**Decision Flowchart:**
```mermaid
graph TD
 A[Market Data + News] --> B{RSI > 85?}
 B -->|Yes| C[Sell: Emergency Stop-Loss]
 B -->|No| D[Calculate Weighted Score]
 D --> E{Score > 35 AND<br>RSI < 75 AND<br>Vol < 8%?}
 E -->|Yes| F[Buy: Strong Signal]
 E -->|No| G{Score < -25 OR<br>(RSI > 78 AND Score < 10)?}
 G -->|Yes| H[Sell: Weakness Confirmed]
 G -->|No| I{Volatility > 8%<br>AND |Score| < 40?}
 I -->|Yes| J[Hold: High Volatility]
 I -->|No| K[Hold: Neutral Conditions]
