# DSRP-Airflow-Proyecto_Final
Implementación de un a orquestación en Apache Airflow para el curso Airflow de la especialización de Data Engineering del Instituto Data Science Research Peru

# ✈️ Pipeline de Tráfico Aéreo (OpenSky) con Airflow y Arquitectura Medallón

Este proyecto implementa un pipeline de datos ELT (Extract, Load, Transform) de extremo a extremo que ingesta datos de tráfico aéreo en vivo desde la API de **OpenSky Network**, los procesa siguiendo la **Arquitectura Medallón** (Bronce, Silver, Gold) y los modela para ser consumidos por un dashboard en **Power BI**.

El pipeline está completamente orquestado con **Apache Airflow**, utilizando la sintaxis moderna de TaskFlow API (decoradores).

## 🚀 Objetivo

El objetivo es capturar "fotos" (snapshots) del estado global del tráfico aéreo a intervalos regulares (cada hora) para construir un modelo de datos histórico que permita analizar patrones de vuelo, densidad de tráfico y métricas de rendimiento en un dashboard interactivo.

## 🏛️ Arquitectura

El flujo de datos sigue la Arquitectura Medallón, asegurando la trazabilidad, calidad y reprocesabilidad de los datos.

```mermaid
graph TD
    A[OpenSky API (states/all)] -->|1. Ingesta (JSON)| B(Capa RAW)
    B -->|2. Consolidación (Parquet)| C(Capa BRONZE)
    C -->|3. Limpieza y Enriquecimiento| D(Capa SILVER)
    D -->|4. Modelado (Star Schema)| E(Capa GOLD)
    E -->|5. Visualización| F(Power BI)

    subgraph Airflow DAG (elt_medallon_opensky_states)
        B
        C
        D
        E
    end