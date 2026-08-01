[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![Orchestration: Dagster](https://img.shields.io/badge/orchestrator-Dagster-red.svg)](https://dagster.io/)
[![Engine: DuckDB](https://img.shields.io/badge/engine-DuckDB-yellow.svg)](https://duckdb.org/)
[![Format: Parquet](https://img.shields.io/badge/format-Apache%20Parquet-green.svg)](https://parquet.apache.org/)
[![ML: XGBoost](https://img.shields.io/badge/ml-XGBoost-orange.svg)](https://xgboost.readthedocs.io/)
[![License: MIT](https://img.shields.io/badge/license-MIT-lightgrey.svg)](LICENSE)

# Introduction

An end-to-end, modular Data Lakehouse and Machine Learning pipeline for historical weather ingestion, feature engineering, automated backtesting, and localized forecasting. 

This project demonstrates production-grade Data Engineering principles: decoupled storage/compute, schema-enforced columnar storage, automated orchestration DAGs, and tracking ML experiment lineage.

---

## 🏗️ Architecture Overview

```text
[ External APIs / Reanalysis ]
   (ECMWF ERA5 / Open-Meteo)
              │
              ▼
   ┌──────────────────────┐
   │ Ingestion Pipeline   │  <-- Automated Dagster Assets
   └──────────┬───────────┘
              │ (Raw JSON / GRIB2)
              ▼
   ┌──────────────────────┐
   │  Bronze Layer (Raw)  │  <-- Local Object Storage / Parquet
   └──────────┬───────────┘
              │ (Clean, Deduplicate, Cast Types)
              ▼
   ┌──────────────────────┐
   │ Silver Layer (Clean) │  <-- DuckDB + Apache Parquet
   └──────────┬───────────┘
              │ (Rolling Aggregations, Lags, Weather Indices)
              ▼
   ┌──────────────────────┐
   │ Gold Layer (Features)│  <-- Feature Store Tables
   └──────────┬───────────┘
              │
      ┌───────┴────────┐
      ▼                ▼
┌──────────────┐  ┌─────────────────────────┐
│ ML Training  │  │ Interactive Dashboards  │
│  (XGBoost)   │  │ (Streamlit / Metabase)  │
└──────┬───────┘  └─────────────────────────┘
       │
       ▼
┌──────────────┐
│ Model Registry│  <-- MLflow Lineage & Artifacts
└──────────────┘
