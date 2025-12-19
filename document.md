# Logit vs Probit Models: No-Bullshit Edition

You want to predict **yes/no outcomes** (will the lifter make it? will the student pass?). Both models do this. The difference? **How they squeeze your data into probabilities between 0 and 1.**

---

## The Weightlifting Example (Concrete AF)

You're predicting: **Will this lifter successfully snatch 200 kg?**

- **Y = 1** → Success (lift complete)
- **Y = 0** → Failure (dropped it, bombed out)
- **X** → Attempted weight (kg), training age, whatever

You get a formula like:
```
Score = -5 + 0.02 × (weight in kg)
```

**Problem**: This "score" can be anything (-∞ to +∞). You need a **probability** (0 to 1).

**Solution**: Run it through a **link function** that squashes it into 0–1 range.

---

## Logit (Logistic Regression)

**Link function**: Logistic curve (sigmoid)

```
P(success) = 1 / (1 + e^(-score))
```

### What This Means:
- If score = 0 → P = 50%
- If score = +5 → P ≈ 99%
- If score = -5 → P ≈ 1%

### Why Use It:
- **Odds ratios** are natural: "Each 10 kg heavier = 0.3× the odds of success"
- Fatter tails = better when extreme outcomes (0% or 100%) happen more than you'd expect from a normal curve
- Standard in economics, medicine, most applied work

### Interpretation:
Coefficients = **log-odds**. If β₁ = -0.05, then:
- 10 kg heavier → odds multiply by e^(-0.5) ≈ 0.6 (40% worse odds)

---

## Probit

**Link function**: Inverse of the standard normal CDF (bell curve)

```
P(success) = Φ(score)
```
(where Φ = that cumulative normal distribution you learned in stats 101)

### What This Means:
- Score = "how many standard deviations above/below average"
- If score = 0 → P = 50%
- If score = 1.96 → P = 97.5%

### Why Use It:
- **Assumes normal distribution** underneath (like many natural phenomena)
- Lighter tails = less probability in extreme zones
- Common in bioassay, psych/social sciences where "latent normality" makes sense

### Interpretation:
Coefficients = **z-score changes**. If β₁ = -0.03:
- 10 kg heavier → decreases z-score by 0.3 standard deviations

---

## Side-by-Side: The Real Difference

| **Feature**          | **Logit**                          | **Probit**                          |
|----------------------|------------------------------------|-------------------------------------|
| **Curve shape**      | S-curve (sigmoid)                  | S-curve (normal CDF)                |
| **Tails**            | Fatter (more extreme outcomes)     | Thinner (fewer extreme outcomes)    |
| **Coefficients mean**| Log-odds                           | Z-scores                            |
| **Interpretation**   | "Multiply the odds by X"           | "Shift probability by X std devs"   |
| **When to use**      | Default choice, easy interpretation| Assuming underlying normality       |
| **Predictions**      | **Basically identical** for probabilities 20–80% | **Basically identical** for probabilities 20–80% |

---

## The Punchline

**For 95% of real-world data**, logit and probit give you **the same predictions**. The curves overlap almost perfectly in the middle (where most data lives).

**When it matters**:
- **Extreme probabilities** (near 0% or 100%): Logit's fatter tails can fit better if you have lots of "certain" outcomes
- **Interpretation**: Logit = easier to explain with odds. Probit = cleaner if you think in z-scores
- **Field conventions**: Economists love logit. Some biostatisticians prefer probit

**Bottom line**: Pick logit unless you have a specific reason (like "my thesis advisor demands probit" or "I genuinely believe this follows a normal distribution"). Either way, you'll be fine.

---

## Visualization (If You Need It)

Both curves look like this stretched "S":

```
P(success)
   1.0 |           ________
       |         /
   0.5 |        /  ← Both curves nearly identical here
       |       /
   0.0 |______/
       |_____|_____|_____|
         -5    0    5
              Score (η)
```

Logit has slightly thicker tails (approaches 0/1 slower). Probit hugs them faster. **You won't notice unless you zoom way in.**

---

## Quick Reference: Math Formulas

### Logit Model
```
Linear predictor: η = β₀ + β₁X₁ + β₂X₂ + ...
Link function: P(Y=1) = 1 / (1 + exp(-η))
Log-odds: log(P/(1-P)) = η
```

### Probit Model
```
Linear predictor: η = β₀ + β₁X₁ + β₂X₂ + ...
Link function: P(Y=1) = Φ(η)
Where Φ is the standard normal CDF
```

---

## Code Examples

### Python (using statsmodels)

```python
import statsmodels.api as sm

# Logit
logit_model = sm.Logit(y, X).fit()
print(logit_model.summary())

# Probit
probit_model = sm.Probit(y, X).fit()
print(probit_model.summary())
```

**TL;DR**: Both predict binary outcomes with S-curves. Logit = fatter tails + odds ratios. Probit = normal distribution + z-scores. **Pick logit by default.** Results will be 99% the same anyway.

---

## Questions?
Bruh for fuck sakes, buy me some cat food damn it: evilmathcat@yandex.com
