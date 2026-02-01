# Probability Density Functions

## 📋 Overview

This project implements a roll-number-parameterized non-linear transformation and learns parameters of a Gaussian-like probability density function from India Air Quality Data (NO2 feature).

## 🎯 Objectives

1. **Transform NO2 data** using a roll-number-specific non-linear function
2. **Learn PDF parameters** (λ, μ, c) using statistical estimation techniques
3. **Validate and visualize** the learned probability density function

## 📊 Mathematical Formulation

### Step 1: Data Transformation
Transform each NO2 value `x` to `z` using:

```
z = Tr(x) = x + ar × sin(br × x)
```

Where:
- `ar = 0.05 × (r mod 7)`
- `br = 0.3 × (r mod 5 + 1)`
- `r` = your university roll number

### Step 2: PDF Parameter Estimation
Learn parameters for the probability density function:

```
p̂(z) = c × exp(-λ(z - μ)²)
```

Where:
- **λ (lambda)**: Controls the width/variance of the distribution
- **μ (mu)**: Location parameter (mean/center)
- **c**: Normalization constant

## 🚀 Quick Start

### Prerequisites

```bash
pip install numpy pandas scipy matplotlib seaborn
```

### Download Dataset

1. Visit: [https://www.kaggle.com/datasets/shrutibhargava94/india-air-quality-data](https://www.kaggle.com/datasets/shrutibhargava94/india-air-quality-data)
2. Download `data.csv`
3. Place it in your working directory

### Using Google Colab

1. Open the notebook
2. Add the Dataset
3. Modify the `ROLL_NUMBER` variable in the first code cell
4. Run all cells sequentially

## 🔧 Implementation Details

### Three Parameter Estimation Methods

The code implements three different estimation techniques:

#### 1. Maximum Likelihood Estimation (MLE) - **Recommended**
- Uses optimization to find parameters that maximize the likelihood of observed data
- Most statistically rigorous approach
- Implemented using `scipy.optimize.minimize`

#### 2. Curve Fitting to Histogram
- Fits the PDF curve to the empirical histogram
- Provides standard errors for parameters
- Implemented using `scipy.optimize.curve_fit`

#### 3. Method of Moments
- Matches theoretical moments to sample moments
- Simpler but less accurate
- Uses analytical formulas

### Key Functions

```python
# Transformation function
def transform(x, ar, br):
    return x + ar * np.sin(br * x)

# PDF model
def pdf_model(z, lambda_val, mu, c):
    return c * np.exp(-lambda_val * (z - mu)**2)

# Negative log-likelihood (for MLE)
def negative_log_likelihood(params, z):
    lambda_val, mu, c = params
    pdf_values = pdf_model(z, lambda_val, mu, c)
    return -np.sum(np.log(pdf_values))
```

## 📊 Output Interpretation

### Visualization

The code generates a comprehensive 4-panel plot:

1. **Original NO2 Distribution**: Histogram of raw data
2. **Transformation Function**: Shows how x maps to z
3. **Fitted PDF**: Histogram overlaid with learned PDF
4. **Q-Q Plot**: Validates model fit quality

### Parameter Validation

The code validates that:
- The PDF integrates to approximately 1 (proper normalization)
- Parameters are positive and reasonable
- The fit quality is acceptable (via Q-Q plot)

## 📝 Example Output

For roll number `12345`:

```
Roll Number: 12345
ar = 0.05 × (12345 mod 7) = 0.150000
br = 0.3 × (12345 mod 5 + 1) = 0.300000

Learned PDF Parameters (MLE):
  λ (lambda) = 0.002145
  μ (mu)     = 38.547291
  c          = 0.026891

PDF Equation:
  p̂(z) = 0.026891 × exp(-0.002145 × (z - 38.547291)²)
```

## ✅ Submission Checklist

Before submitting your parameters:

- [ ] Verified your roll number is correct
- [ ] Confirmed ar and br calculations
- [ ] Checked that PDF integrates to ~1
- [ ] Reviewed visualization plots
- [ ] Recorded all three parameters (λ, μ, c)
- [ ] Noted which estimation method was used (recommend MLE)

## 🔍 Troubleshooting

### Common Issues

**Q: I get "Dataset not found" error**
- A: Download `data.csv` from Kaggle and use correct file path
- The code will use simulated data if the file is missing (for testing only)

**Q: Optimization doesn't converge**
- A: Try different initial parameters
- Increase the number of iterations
- Check that your data doesn't have extreme outliers

**Q: PDF doesn't integrate to 1**
- A: This is expected for non-normalized PDFs
- The parameter `c` acts as a scaling factor, not necessarily a normalization constant
- The assignment asks you to learn `c`, not force it to normalize

**Q: Parameters seem unreasonable**
- A: Compare with all three methods (MLE, Curve Fitting, Moments)
- Check data preprocessing (NaN removal, outliers)
- Visualize the fit to ensure it looks reasonable

## 📚 Theoretical Background

### Why This PDF Form?

The PDF `p(z) = c × exp(-λ(z - μ)²)` is a Gaussian-like distribution:

- **Gaussian PDF**: `(1/√(2πσ²)) × exp(-(z-μ)²/(2σ²))`
- **Our PDF**: `c × exp(-λ(z-μ)²)`

Relationship: `λ = 1/(2σ²)` where σ is the standard deviation

This form is:
- **Flexible**: Can model many unimodal distributions
- **Tractable**: Easy to work with mathematically
- **Interpretable**: Parameters have clear meanings

### Why Transform the Data?

The non-linear transformation:
```
z = x + ar × sin(br × x)
```

- Makes the problem **unique to your roll number**
- Introduces **non-linearity** for interesting analysis
- Tests understanding of **probability transformations**
- Simulates real-world feature engineering

## 🎓 Learning Outcomes

After completing this assignment, you will understand:

1. **Probability Density Functions**: How to model and work with continuous distributions
2. **Parameter Estimation**: Multiple techniques (MLE, Curve Fitting, Moments)
3. **Data Transformation**: Non-linear transformations and their effects
4. **Statistical Validation**: How to verify model quality
5. **Python Implementation**: Practical coding skills for statistical analysis

## 📖 Additional Resources

- **Scipy Optimization**: https://docs.scipy.org/doc/scipy/reference/optimize.html
- **Maximum Likelihood**: https://en.wikipedia.org/wiki/Maximum_likelihood_estimation
- **Probability Distributions**: https://en.wikipedia.org/wiki/Probability_distribution

## 🤝 Support

If you encounter issues:

1. Check the troubleshooting section above
2. Review the code comments for detailed explanations
3. Verify your input data and roll number
4. Compare outputs from different estimation methods

## 📄 License

This code is provided for educational purposes. Feel free to modify and extend it for your learning.
