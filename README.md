# Repwise

A local-first adaptive training, recovery, nutrition engine.

Repwise is an open-source, local-first fitness and nutrition system that uses a C++ core, Python-trained machine learning models, Lua-defined progression rules, and a CLI/TUI interface to help users understand their training, recovery, and diet data while keeping ownership of their personal data.

## Project Goals

- **Import personal fitness and nutrition logs**
  - workouts
  - sleep
  - readiness or subjective wellness
  - body metrics
  - meals or ingredients
  - optional HRV / resting heart rate

- **Store data locally**
  - SQLite database
  - CSV/JSON export
  - no cloud dependency

- **Compute training and recovery metrics**
  - training load
  - volume load
  - acute:chronic workload ratio
  - rolling averages
  - recovery score
  - readiness score

- **Predict readiness or training tolerance**
  - baseline model first
  - then a 1D CNN or convolutional time-series model
  - export model to ONNX
  - run inference from C++

- **Suggest the next training session**
  - Lua-defined progression rules
  - configurable thresholds
  - examples: deload, strength, mobility, rest, aerobic conditioning

- **Suggest simple meal plans**
  - macro targets
  - budget constraints
  - inventory constraints
  - recipe scaling
  - simple optimizer or greedy planner

- **Provide CLI and TUI interfaces**
  - CLI for scripting and analysis
  - TUI for dashboard/demo
  - Qt deferred

- **Include scientific evaluation**
  - train/validation/test split
  - baseline comparisons
  - error metrics
  - ablation study
  - model card
  - limitations section

## Structure
```text
Repwise/
├── core/     # C++ core: data, metrics, planning, Lua runtime
├── ml/       # Python: feature engineering, training, evaluation
├── plugins/  # Lua rules and user-defined progression logic
├── ui/
│   ├── cli/  # CLI11 command-line interface
│   ├── tui/  # FTXUI terminal dashboard
│   └── qt/   # optional later
├── data/     # SQLite, CSV, JSON, Parquet exports
├── docs/
├── tests/
└── benchmarks/
```

## Pipeline
```text
CSV / manual logs / exported device data
              ↓
C++ ingestion and normalization
              ↓
SQLite local database
              ↓
C++ feature extraction and metrics
              ↓
Python model training and evaluation
              ↓
ONNX model export
              ↓
C++ ONNX inference
              ↓
Lua progression rules
              ↓
TUI / CLI dashboard
              ↓
CSV/JSON export, optional anonymized community sharing
```

## Repwise Everyday Use Case

### Daily logging template

Create a simple CSV:
```csv
date,sleep_min,sleep_quality,hrv,resting_hr,soreness,fatigue,mood,readiness,training_load,activity
2026-01-01,430,7,55,54,4,5,7,72,38,strength
2026-01-02,400,6,52,55,5,6,6,65,0,rest
2026-01-03,475,8,61,52,3,4,8,78,45,running
```

### Weekly workflow

- **Daily**
  - log sleep, readiness, soreness, training session
- **Weekly**
  - analyze last 28 days
  - check acute:chronic ratio
  - review suggested plan
  - generate meal plan
  - export data

## Repwise Stretch Goals

- Garmin/Strava/CSV importers
- HRV and sleep integrations
- federated learning
- community recipe sharing
- mobile logger
- Qt dashboard
- Bayesian uncertainty
- survival analysis for injury risk
- multi-objective meal optimizer
- coach plugin marketplace
- local model personalization
- encrypted backup
- community challenge pack

## Dependencies

Repwise is built as a layered system: a C++ core, a Lua plugin layer,
a Python ML/evaluation layer, and CLI/TUI interfaces. Qt is intentionally
deferred to a later milestone.

### Build & tooling

| Dependency | Purpose | Source / Port | Notes |
|---|---|---|---|
| C++20 compiler | Compile the core | GCC / Clang / MSVC | C++20 only; avoid C++23 for portability |
| CMake | Configure & build | system | modern `find_package` style |
| Ninja | Build generator | system | faster than Make |
| vcpkg | C++ package manager | `vcpkg install ...` | single manifest via `vcpkg.json` |

### C++ core

| Dependency | Purpose | vcpkg port | Notes |
|---|---|---|---|
| SQLite | local-first storage | `sqlite3` | user-owned data, no cloud |
| nlohmann/json | config & export | `nlohmann-json` | settings, plugin metadata, JSON export |
| fmt | formatting | `fmt` | clean output formatting |
| spdlog | logging | `spdlog` | structured logs for debugging |
| Eigen | linear algebra | `eigen3` | Kalman filter, feature transforms |
| Catch2 | unit tests | `catch2` | correctness & regression tests |
| Google Benchmark | performance | `google-benchmark` | ingest/inference benchmarks |
| ONNX Runtime | C++ inference | `onnxruntime` | runs Python-trained models in C++ (CPU) |

### Lua plugin layer

| Dependency | Purpose | Source / Port | Notes |
|---|---|---|---|
| Lua | progression rules & configs | `lua` (5.4) | user-editable rules, community packs |
| sol2 | Lua↔C++ binding | `sol2` | safe, clean embedding API |

### Python ML / evaluation layer

| Dependency | Purpose | Install | Notes |
|---|---|---|---|
| Python 3.11 | training & evaluation | venv | pin for reproducibility |
| NumPy | numerical arrays | `pip install numpy` | |
| pandas | data handling | `pip install pandas` | CSV import, feature engineering |
| scikit-learn | baselines & metrics | `pip install scikit-learn` | linear, gradient boosting, eval |
| SciPy | signal/stats | `pip install scipy` | smoothing, filters, stats |
| PyTorch | 1D CNN training | `pip install torch --index-url .../cpu` | CPU wheel unless GPU configured |
| ONNX | model export | `pip install onnx` | PyTorch → ONNX bridge |
| Matplotlib | evaluation plots | `pip install matplotlib` | readiness trends, residuals |

### UI layer (one-month scope)

| Dependency | Purpose | vcpkg port | Notes |
|---|---|---|---|
| CLI11 | command-line interface | `cli11` | header-only, robust parsing |
| FTXUI | terminal dashboard | `ftxui` | TUI for demo & daily use |

### Deferred (post-PoC)

| Dependency | Purpose | vcpkg port | Notes |
|---|---|---|---|
| Qt | desktop dashboard | `qt` | skipped for v0.1 to protect scope |

### Reproducibility

- C++ deps pinned via `vcpkg.json`.
- Python deps pinned via `requirements.txt`.
- ONNX model versioned in `ml/models/` with a model card.