We do the comparison on lab-scale Hybrid Rocket Fuels firing data for different ML models. This project uses Leave-One-Out validation: each static-fire test (fuel type / sheet) is held out once, the model is trained on the remaining firings, and performance is measured on the held-out firing.
Models compared under the same LOO scheme: XGBoost, Random Forest, and LightGBM, with MC Dropout used for regression-rate uncertainty.

1) **Institution**: Global Academy of Technology (GAT), Department of Aeronautical Engineering  
2) **Project focus**: Compared experimental tests of biofuel-integrated hybrid fuels, with emphasis on regression rate and blend performance.

## Dataset
The previous dataset is found to be in LabVIEW `.lvm` exports.
Processed firing data are then stored as Excel:

1) **File:** `data/daq_raw_4blends.xlsx`
2) **Sheets:** one sheet per fuel blend (4 blends)

| Column | Role | Meaning |
|--------|------|--------|
| `X_Value` | Input | Time (s) from DAQ |
| `Force` | Input | Load-cell force |
| `Pressure` / `Pressure_0` | Input / target* | Chamber-related pressure channels |
| `Gox` | Input | Oxidizer mass flux (kg/m²·s), burn-average |
| `rdot_mm_s` | Target | Average regression rate (mm/s) |
| `Isp` | Target | Specific impulse |
| `Thrust` | Target | Thrust (from force as applicable) |
| `Cf` | Target | Thrust Co-efficient (from force as applicable) |
| `cstar` | Target | Characteristic velocity |
| `specific density` | Target | Specific Density |


---

## Models compared

| Model | Role |
|--------|------|
| **XGBoost** | Gradient-boosted trees (baseline) |
| **Random Forest** | Bagged decision trees |
| **LightGBM** | Gradient-boosted trees (leaf-wise) |

All models use the **same** train/test split, features, and metrics for a fair comparison.

**Uncertainty:** MC Dropout on a small MLP for regression-rate uncertainty only (not a fourth competing tree model).

---

## Repository structure

```text
.
├── README.md
├── requirements.txt
├── compare_models.ipynb
├── data/
    ├── LVM_data
        |── GAT_S1_04.11.2025.lvm
        |── GAT_S2_04.11.2025.lvm
        |── GAT_S3_12.11.2025.lvm
        └── GAT_S4_12.11.2025.lvm
    ├── LVM_to_XLSX.ipynb
    |── Pressure_vs_time.ipynb
    └── daq_raw_4blends.xlsx
├── models/
    ├── xgboost_model.ipynb
    ├── random_forest_model.ipynb
    └── lightgbm_model.ipynb
└── results/
    └── LightGBM
        |── Best_Fuel_Results_lightgbm_LOO.xlsx
        └── Best_Fuel_Results_lightgbm_LOO.txt
    |── Model Comparison
        |── model_comparison_LOO.csv
        └── model_comparison_LOO.txt
    |── Random Forest
        |── Best_Fuel_Results_lightgbm_LOO.xlsx
        └── Best_Fuel_Results_lightgbm_LOO.txt
    └── XGBoost
        |── Best_Fuel_Results_lightgbm_LOO.xlsx
        └── Best_Fuel_Results_lightgbm_LOO.txt
```

## Related repository
For timestep-level DAQ analysis (full LVM traces), see:  
https://github.com/Kani2718/Hybrid_Rocket_Comparison
