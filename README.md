# Prediksi Kunjungan Klinik — MLflow Tracking

Hasil training 6 model × 8 layanan (48 parent run + 120 child run grid search = **168 run**) tersimpan di MLflow.

## Lihat Hasil (tanpa training ulang)

```bash
mlflow ui --backend-store-uri sqlite:///mlflow.db
```

lalu buka http://localhost:5000 di browser.

- `mlflow.db` → metadata run (params, metrics, tags)
- `mlruns/` → artifacts: model `.pkl` per window, `prediksi.csv`, plot aktual vs prediksi

## Install Dependency

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Jalankan Notebook (opsional, training ulang)

```bash
# salin data input dulu (ada di folder ../Dataset/)
cp ../Dataset/data_harian_kunjungan_2025.csv .
jupyter lab  # → buka modeling_kunjungan.ipynb → Restart & Run All
```

## Struktur Run

| Tag | Keterangan |
|---|---|
| `tipe` | baseline / statistical / ml / tuning |
| `peran` | run (utama) = parent, child = combo tuning |

Naive, Seasonal Naive, SARIMA, LinReg → 1 run per model × layanan.
Ridge & XGBoost → parent run + child run per combo gridsearch, best dipilih dari MAE rata-rata 5 window.
