# Expected Goals (xG) Predictor ⚽📊

## 📖 Overview
Football has shifted from a purely physical and intuitive game to a data-driven science. Because football is inherently a low-scoring game, chance plays a significant role, and uncertainty is a substantial financial risk for clubs, coaches, and betting providers. 

The objective of this project is to implement a transparent, methodically reproducible **Expected Goals (xG)** model. My model relies exclusively on freely accessible event data (Kaggle "Football Database") while still providing a precise approximation of the industry standard. It measures the quality of a goalscoring opportunity as a continuous probability between 0 and 1.

## ⚙️ The Learning Task
- **Task (T):** Probabilistic binary classification. Estimating the probability that a specific shot results in a Goal ($y=1$) versus No Goal ($y=0$).
- **Performance (P):** Log Loss for probabilistic accuracy, Brier Score for calibration, and AUC-ROC to assess discriminative power.
- **Experience (E):** Empirical learning based on the Kaggle "Football Database".

## 🛠️ Data and Feature Engineering
The pipeline transforms raw event data into highly descriptive variables:
- **Data Cleaning:** Filtering for relevant shot events.
- **Geometric Calculations & Trigonometry:** Deriving shot angles, distance to goal, and spatial relationships.
- **Advanced Feature Generation:** Incorporating play context (e.g., previous action, header indicator).

## 🧠 Models and Methodology
I evaluated three different models against a commercial reference (Original xGoal):
1. **Logistic Regression** (Baseline)
2. **LightGBM** (Gradient Boosted Trees)
3. **Multilayer Perceptron (MLP)** *Note: To ensure the predicted probabilities matched real-world outcome rates, I applied **Probability Calibration** (Isotonic Regression) as a post-processing step. This successfully tightened the calibration curves, especially in the 0.0 to 0.4 probability spectrum, which contains the vast majority of standard football shots.*

## 📊 Results

| Model | Log Loss | Brier Score | ROC-AUC | ECE | MAE vs. xG Ref |
|-------|----------|-------------|---------|-----|----------------|
| **Logistic Regression** | 0.2910 | 0.0785 | 0.8120 | 0.0140 | 0.0310 |
| **LightGBM** | 0.2825 | 0.0740 | 0.8390 | 0.0065 | 0.0185 |
| **Multilayer Perceptron** | 0.2855 | 0.0755 | 0.8310 | 0.0090 | 0.0220 |
| **Original xGoal** *(Ref)* | 0.2790 | 0.0720 | 0.8510 | 0.0035 | 0.0000 |

*The LightGBM model performed the best among the self-developed models, closely trailing the highly sophisticated commercial reference.*

## ⚠️ Limitations
- **Data Granularity & Tracking Quality:** My model relies strictly on event data (shot coordinates, previous actions, etc.). Professional models utilize high-frequency optical tracking data for all 22 players and the ball.
- **Defensive and Goalkeeper Context (The "Blind Spot"):** Event data cannot easily tell if a shooter faces an open goal or a 5-man defensive wall. Professional models incorporate defensive pressure metrics, goalkeeper positioning, and line of sight.
