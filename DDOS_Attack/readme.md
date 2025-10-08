# DoS Attack Detection using Autoregressive Models

This project implements autoregressive models (ARIMA, SARIMA) for detecting Denial-of-Service (DoS) attacks by analyzing network traffic patterns and identifying statistical anomalies.

## Overview

Autoregressive models analyze temporal patterns in network traffic to detect deviations that may indicate a DoS attack. Since DoS attacks typically cause sudden, abnormal traffic surges, ARIMA can model normal behavior and flag statistically significant outliers.

## Methodology

### 1. Model Normal Traffic
- Train ARIMA on historical traffic data (e.g., packets/sec)
- Example: `ARIMA(order=(5,1,1))` learns from past 5 timesteps (`p=5`), applies differencing (`d=1`), and accounts for noise (`q=1`)

### 2. Forecast & Detect Anomalies
- Predict the next traffic value using the trained model
- If actual traffic exceeds the prediction by a threshold (e.g., `3× standard deviation`), flag as a DoS attack

```python
predicted = model.forecast()[0]
if actual > predicted + threshold:
    trigger_alert()
    print("DoS DETECTED!")
```

### 3. How It Works
- DoS attacks cause sudden deviations from learned patterns
- ARIMA adapts to baseline traffic without requiring static thresholds
- Can be combined with secondary checks (IP reputation, packet inspection) to reduce false positives

## Production Considerations

- **Online Learning**: Update model in real-time to adapt to changing traffic patterns
- **Hybrid Models**: Combine ARIMA with LSTM or other deep learning approaches
- **Rolling Windows**: Use sliding window approach for continuous model retraining

## Challenges & Solutions

| Challenge | Solution |
|-----------|----------|
| False positives | Combine with ML classifiers (e.g., Random Forests) to filter benign spikes |
| Slow adaptation to new attacks | Retrain model incrementally or use online learning with rolling windows |

## References

The following research papers informed this implementation:

1. "Attack based DoS attack detection using multiple classifier"
2. "DDoS Attack Detection Method Based on Network Abnormal Behavior in big data environment"

## Usage

```python
# Example usage
model = ARIMA(training_data, order=(5,1,1))
model_fit = model.fit()
forecast = model_fit.forecast(steps=1)
```

## Requirements

- Python 3.x
- statsmodels (for ARIMA implementation)
- pandas, numpy
- scikit-learn (for additional classifiers if used)

## Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues for suggestions and improvements.
