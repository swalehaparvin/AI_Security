# 🛡️ malicious-url-forecast  
> Real-time **malicious URL detection** + **DoS-attack volume forecasting** in one lightweight repo.

---

## 📌 Overview
This repository gives security teams a **plug-and-play** pipeline that does two things:

1. **Classifies** any URL as *benign*, *malware*, *phishing*, or *defacement* using TF-IDF + Logistic Regression.  
2. **Forecasts** denial-of-service attack **traffic volume** with an ARIMA model.

Everything is **fully reproducible**—download data, train, evaluate, and visualise in <10 min.

---

## 🚀 Quick Start

### 1. Clone & install
```bash
git clone https://github.com/<your-handle>/malicious-url-forecast.git
cd malicious-url-forecast
pip install -r requirements.txt
```

### 2. Download data & train
```bash
python src/train.py
```
This will:  
- pull the [Malicious URLs dataset](https://www.kaggle.com/datasets/sid321axn/malicious-urls-dataset) via `kagglehub`  
- train the URL classifier  
- train the ARIMA forecaster on synthetic DoS logs (or your own CSV)  
- save `model.pkl`, `vectorizer.pkl`, and `arima.pkl` under `artifacts/`

### 3. Run the demo dashboard
```bash
streamlit run app.py
```
Point your browser to `http://localhost:8501` to:  
- paste a URL → instant risk score  
- upload a traffic log → 24-hour DoS forecast plot

---

## 📂 Repository Layout
```
malicious-url-forecast/
├── data/                # git-ignored; datasets land here
├── notebooks/
│   ├── EDA.ipynb        # exploratory data analysis
│   └── Forecast.ipynb   # ARIMA tuning
├── src/
│   ├── train.py         # end-to-end training script
│   ├── predict.py       # single URL or batch inference
│   └── features.py      # custom tokenizer & ARIMA helpers
├── app.py               # Streamlit front-end
├── artifacts/           # saved models
└── tests/               # unit tests with pytest
```

---

## 🧪 Model Details

| Task | Algorithm | Features | Accuracy |
|------|-----------|----------|----------|
| URL classification | Logistic Regression | Character-level TF-IDF (custom `url_cleanse` tokenizer) | ≈ 98 % |
| DoS volume forecast | ARIMA (p,d,q) | Minute-level packet counts | MAPE ≈ 8 % |

---

## 🔍 Example Usage

### CLI single URL check
```python
from src.predict import predict_url
print(predict_url("http://badsite.ru/login.php"))
# → {'label': 'phishing', 'proba': 0.93}
```

### Programmatic forecast
```python
from src.predict import forecast_dos
print(forecast_dos("data/dos_traffic.csv", horizon=24))
# → [2031, 1987, 2200, ...]  # predicted packets per minute
```

---

## 📈 Visuals
<p align="center">
  <img src="assets/conf_mat.png" width="400"/>
  <img src="assets/dos_forecast.png" width="400"/>
</p>

---

## 🤝 Contributing
1. Fork the repo  
2. Create a feature branch (`git checkout -b feature/foo`)  
3. Commit & push (`git commit -m "Add foo"`; `git push origin feature/foo`)  
4. Open a Pull Request  
