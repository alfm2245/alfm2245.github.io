---
title: "Building a Customer Segmentation Pipeline with Docker & PostgreSQL"
description: "How I ingested 1.85 million transactions and applied K-Means clustering to uncover customer personas."
pubDate: 2026-09-20
tags: ["data-pipeline", "postgresql", "docker", "machine-learning"]
---

In this project, I developed an end-to-end data processing and analytics pipeline designed to handle large-scale financial transaction data. The goal was to transform raw transaction events into clear, actionable customer personas.

## Architecture Overview

The system architecture consists of three main stages:

1. **Local Containerized Ingestion**: Docker container running PostgreSQL for isolated, reproducible database provisioning.
2. **Feature Engineering & Modeling**: Python script querying aggregated metrics (RFM - Recency, Frequency, Monetary value) and fitting a Scikit-Learn K-Means model.
3. **Executive Visualization**: Interactive Tableau dashboard analyzing cluster revenue contribution vs. database processing load.

```text
[Transaction Stream] ──> [PostgreSQL in Docker] ──> [Scikit-Learn K-Means] ──> [Tableau Dashboard]
```

## Ingesting 1.85 Million Records

When handling nearly 2 million rows locally, optimizing ingestion speed is critical. Using batch insertion and indexing key lookup columns (`customer_id`, `transaction_timestamp`) cut query execution times significantly.

Key metrics aggregated per user:
- Total spend / volume
- Transaction frequency (orders per month)
- Average transaction ticket size

## Extracting Customer Personas

Using Scikit-Learn, we evaluated the optimal number of clusters using inertia elbow curves and silhouette scores, landing on 4 distinct segments:
- **High-Value Loyalists**: Consistent high-ticket transactions.
- **Frequent Low-Ticket Buyers**: High activity with small individual amounts.
- **Occasional Shoppers**: Periodic purchases with average spend.
- **Dormant / At-Risk Users**: Low recency and low frequency.

These personas provide product marketing teams with data-backed targets for personalized retention campaigns.
