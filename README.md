# API Observability with PySpark

## Overview
This project implements a distributed API observability system designed to monitor latency, schema consistency, and availability across multiple regions. Using PySpark, the system scales horizontally to handle high-volume monitoring with efficiency.

## Problem
APIs in distributed environments may behave inconsistently across regions, leading to performance issues, silent data failures, and unreliable data pipelines.

## Solution
A scalable monitoring system that executes parallel API requests, tracks performance metrics in real-time, and validates response structures against expected schemas.

## Architecture
The system uses a distributed approach to ensure scalability and security.

```mermaid
graph TD
  subgraph "Security & Source"
    AKV[Azure Key Vault] -- Secrets --> Spark
    APIs[Target APIs - Multi-Region]
  end

  subgraph "PySpark Distributed Engine"
    Spark[PySpark Cluster]
    Spark --> Executor[ThreadPoolExecutor]
    Executor --> Req[Parallel API Requests]
    Req --> Val[Schema & Latency Validation]
    Val --> Enrich[Metadata Enrichment]
  end

  subgraph "Storage (Silver Layer)"
    Enrich --> ADLS[Azure Data Lake Storage Gen2]
    ADLS --> Parquet[(Parquet Files)]
  end

  style Spark fill:#f96,stroke:#333,stroke-width:2px
  style ADLS fill:#0078d4,stroke:#fff,color:#fff
  style AKV fill:#0078d4,stroke:#fff,color:#fff
