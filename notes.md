#### initial decision tree model:

###### Most influential factors:

- 200-day Moving Average (29.8% importance)
- 3-Month Returns (26.9% importance)
- Volatility (23.6% importance)


###### where the model performance was sub-optimal:

- Overall accuracy: 38%
- High class imbalance (452 positive vs 175 negative cases)
- Low recall on positive predictions (27%)


need: 
Address class imbalance
Remove non-contributing features (1-Month Returns and RSI)
Optimize tree parameters

---

#### second run: 
Made model adjustments:
 1. Removed non contributors -- 1Month return and RSI
 2. Added class weight to handle imbalance
 3. modified depth and min samples

still not strong. 39 vs 38 which is slight improvement

###### Buy signals: 68% precision but only 28% recall
###### Sell signals: 26% precision with 66% recall

- MA_200 became more dominant (35.4%)
- Returns_3M remained stable (26.4%)
- Volatility decreased in importance (18.7%)
- MA_50 showed increased significance (12.3%)
---

### third run:
added feature interactions

changes:

- Added interaction features (MA_Ratio, Trend_Strength)
- Shortened prediction window from 21 to 14 days
- Adjusted class weights for better balance
- Modified tree parameters


Feature Importance Shifts:
- MA_Ratio (price trend indicator) became top predictor (27.5%)
- MA_50 showed increased significance (24.0%)
- Returns_3M maintained importance (21.0%)
- New Trend_Strength feature contributed (10.9%)
- Several features showed no impact (Returns_1M, Returns_12M, RSI, MA_200_x_Returns_3M)


Performance Results:

- Overall accuracy at 38%
- Improved sell signal detection (75% recall vs previous 66%)
- Decreased buy signal accuracy (21% recall vs previous 28%)
- Better at identifying market downturns than uptrends

So after introducing ratio-based features the model's ability to detect selling opportunities improved but at the cost of buy signal accuracy. So now I am starting to think that the model might be more useful for risk management than opportunity identification.

---

### Fourth run: A New Model: 
So I shifted the focus to risk detection with drawdown thresholds while also adding more risk features like Drawdown, MA_Ratio, and Volatility. I also implemented combined risk conditions for target identification

Feature importance: 
- Drawdown was dominant predictor (86.4%)
- Long term vol (63 day) second most sig (7.8%)
- Trend_Strength (4.2%) and MA_Ratio (1.6%) as refinement indicators
- Short-term metrics showed minimal impact


Performance Metrics:
- 65% overall accuracy
- Perfect precision (100%) for no-risk situations
- Perfect recall (100%) for risk events
- Low risk prediction precision (9%) indicates over-sensitivity
- Balanced f1-score of 0.76 weighted average


So I can see that the model is good at risk screening but needs refinement to reduce false positives while maintaining its effective risk detection capabilities.

---

### Fifth Run: Optimized Risk Detection Model:
So I refined the model further with balanced feature importance and optimized parameters to achieve better precision without sacrificing too much recall.

Feature importance:
* Drawdown remains primary but more balanced (58.3%)
* Drawdown_Speed maintained significance (25.2%)
* Volatility_Ratio came up as key indicator (16.5%)
* Other features (Volatility_21, Volatility_63, MA_Ratio) 

Performance Metrics:
* overall accuracy (99%)
* Great balance in risk detection:
  * Risk event precision: 81% (improvement from 38%)
  * Risk event recall: 81% (slight decrease from 100% but more reliable due to balance)
* Great balance between false positives and false negatives
* F1-score: 0.81 for risk class

Key Improvements:
1. optimal feature balance with three key indicators
2. refined decision boundaries (Drawdown threshold at -12%)
3. Volatility_Ratio splits (1.202 and 1.261)
4. reduced model complexity

The model is now showing risk detection capabilities with a near-perfect balance between identifying true risks and avoiding false alarms.

---

## Comparison

Evolution and Improvements:

1. Base Model (First Iteration):
- Heavy reliance on MA_200 (29.8% importance)
- Low accuracy (38%)
- High false positive rate
- Poor precision (9%) with high recall

2. Risk-Focused Model (Second/Third Iterations):
- Shifted to drawdown dominance (86.4%)
- Improved accuracy (65%)
- Still high false positive rate
- Enhanced risk detection capabilities

3. Final Optimized Model:
- More balanced feature importance:
  * Drawdown: 68.3% (reduced from 86.4%)
  * Drawdown_Speed: 25.8% (new feature)
  * Volatility_21: 5.9%
- Significantly improved metrics:
  * Overall accuracy: 99%
  * Risk event precision: 38% (4x improvement)
  * Maintained 100% recall for risk events
  * F1-score for risk class: 0.55

improvements:
1. Added Drawdown_Speed for rapid decline detection
2. More stringent risk criteria (15% drawdown threshold)
3. Enhanced feature engineering
4. Reduced false positives while maintaining risk sensitivity