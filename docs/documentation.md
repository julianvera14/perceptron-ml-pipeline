# Perceptron ML Pipeline — Project Overview

## 1. Business Problem

Wanderbricks is a fictional vacation-rental platform provided as a Databricks sample dataset. When a guest cancels a booking, the property sits empty and the host loses revenue. Operations wants to **predict which bookings are likely to be cancelled** so hosts can plan calendars, apply smarter overbooking policies, and target retention offers.

## 2. Aim

Build a **binary classification model** that predicts booking cancellation:

* **Label:** `cancelled` = 1, `completed` = 0. Bookings with `confirmed`/`pending` status are excluded — their outcome is unknown at prediction time.
* **Model users:** hosts and the operations team, via batch scoring.

## 3. Success Criteria

* **Primary metric:** AUC-ROC ≥ 0.80 on a held-out test set
* **Secondary:** Recall of cancellations ≥ 0.70 (missing a cancellation is costlier than a false alarm)
* **Operational:** batch inference completes on 72K rows in minutes; the model is loaded by **alias** (Champion) from Unity Catalog, never by hardcoded version

## 4. Dataset

Source: **`samples.wanderbricks.bookings`** (Databricks-provided sample catalog, read-only Delta Sharing).

## 5. ML Lifecycle → Repo Mapping

| ML lifecycle stage | Where it lives |
| --- | --- |
| Business Problem / Success Criteria | `docs/` (this document) |
| Data Collection | `01-data-prep` — copy from samples into `workspace.perceptron_dev.raw_bookings` |
| Data Preprocessing / Feature Engineering | `01-data-prep` → `cleaned_bookings`, `features_bookings` |
| Model Training (MLflow tracking) | `02-train` |
| Model Evaluation + Registry (Champion alias) | `03-evaluate` |
| Model Deployment (batch inference) | `04-inference` |
| Model Monitoring / retraining | `05-scheduled` + `tests` |

## 6. Environment

* **Dev schema:** `workspace.perceptron_dev` (the `_dev` suffix follows the dev/prod separation convention; a production stage would use a separate catalog, e.g. `workspace.perceptron_prod`)
* **Tables (created by the pipeline, not pre-declared):** `raw_bookings` → `cleaned_bookings` → `features_bookings` → `predictions`
