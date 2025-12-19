# Logit vs Probit Models: Key Differences

**Logit** and **probit** models predict binary outcomes (yes/no, success/failure). They differ mainly in their **link function**, which converts a linear predictor into a probability between 0 and 1.

### Weightlifting Example
Imagine predicting whether a weightlifter successfully lifts a given weight (e.g., 200 kg snatch).

- **Outcome (Y)** — 1 if successful lift, 0 if failed.
- **Predictor (X)** — Attempted weight in kg (or other factors like training hours, but focus on weight).

The linear predictor might be:  
η = β₀ + β₁ × weight_kg  

(Here, β₁ would be negative: higher weight → lower success probability.)

### Logit Model (Logistic Regression)
- Link function: **Logistic (sigmoid)**  
  P(Y=1) = 1 / (1 + exp(-η))
- Assumes errors follow **logistic distribution**.
- Probability changes most rapidly around 50%.

**Interpretation example**:  
A 10 kg increase in attempted weight multiplies the odds of success by exp(β₁).

<grok-card data-id="914669" data-type="image_card"></grok-card>


### Probit Model
- Link function: **Inverse cumulative normal distribution (CDF of standard normal)**  
  P(Y=1) = Φ(η) where Φ is the standard normal CDF.
- Assumes errors follow **normal distribution**.
- Slightly heavier tails than logistic.

**Interpretation example**:  
A coefficient represents change in z-score for the probability.

<grok-card data-id="8ebb1e" data-type="image_card"></grok-card>


### Visual Comparison
Both curves are S-shaped and very similar in the center, but logit has slightly heavier tails.

<grok-card data-id="e44f57" data-type="image_card"></grok-card>


### Practical Differences
| Aspect              | Logit                              | Probit                              |
|---------------------|------------------------------------|-------------------------------------|
| Link Function       | Logistic sigmoid                   | Normal CDF                          |
| Interpretation      | Odds ratios (exp(β))               | Z-scores/probits                    |
| Tails               | Heavier (better for extreme cases) | Lighter                             |
| Common Use          | General binary outcomes, economics | When assuming normality (e.g., bioassay, some social sciences) |
| Predictions         | Very similar in most datasets      | Very similar in most datasets       |

In practice, for medium probabilities (20–80%), logit and probit give nearly identical results. Choice often depends on field convention or interpretability needs.

### Example Images (Weightlifting Context)

<grok-card data-id="841458" data-type="image_card"></grok-card>



<grok-card data-id="407637" data-type="image_card"></grok-card>


*Images show calibrated kilogram plates used in Olympic weightlifting.*
