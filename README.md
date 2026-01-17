# ✈️ AeroInsight — Previsão Inteligente de Atrasos de Voos  
### Projeto de Data Science & Machine Learning em Produção

Este projeto apresenta um **sistema completo de previsão de atrasos de voos**, desenvolvido como um **produto de dados real**, integrando **Engenharia de Dados, Ciência de Dados e Machine Learning** em um único pipeline produtivo.

O modelo é consumido diretamente por uma **API de backend**, entregando previsões em tempo real para sistemas externos, simulando um **ambiente real de produção**.

> 🔹 Projeto desenvolvido com foco em **empregabilidade**, **arquitetura de produção** e **boas práticas de Data Science industrial**.

---

## 🎯 Problema de Negócio

Atrasos de voos geram impactos diretos em:

- Custos operacionais de companhias aéreas
- Logística aeroportuária
- Satisfação do cliente
- Planejamento de frota
- Eficiência de rotas
- Alocação de tripulação

### Objetivo do Modelo

Construir um sistema capaz de:

✅ Prever a **probabilidade de atraso significativo (≥ 15 minutos)**  
✅ Utilizar apenas **dados disponíveis antes da decolagem**  
✅ Evitar **vazamento de informação temporal**  
✅ Generalizar para **períodos futuros**  
✅ Operar em **ambiente produtivo**  
✅ Integrar-se via **API**  
✅ Escalar para **grandes volumes de dados**  

---

## 🧠 Formulação do Problema

O problema foi modelado como uma **classificação binária**:

| Classe | Significado |
|------|------|
| `0` | Voo sem atraso relevante |
| `1` | Voo com atraso ≥ 15 minutos |

O output do modelo é uma **probabilidade de atraso**, permitindo integração com regras de negócio, alertas e sistemas de decisão.

---

## 📊 Fontes de Dados Oficiais

Projeto construído **exclusivamente com dados públicos e oficiais**, garantindo:

✔️ confiabilidade  
✔️ reprodutibilidade  
✔️ escalabilidade  
✔️ validade científica  

**Fontes:**

- **ANAC (Brasil)** — dados operacionais de voos comerciais  
- **Bureau of Transportation Statistics (EUA)** — dados oficiais do DOT  
- **ERA5 (ECMWF)** — dados meteorológicos de reanálise climática  
- **OurAirports** — dados geográficos de aeroportos  

---

## 🧩 Arquitetura do Pipeline de Dados

Pipeline desenvolvido seguindo padrões de **engenharia de dados moderna**:

### 🔹 ETL Operacional
- Padronização de bases heterogêneas (ANAC + BTS)
- Criação de features temporais pré-voo
- Definição do target (`delay ≥ 15 min`)
- Processamento incremental

### 🔹 Enriquecimento Geográfico
- Associação espacial por latitude/longitude
- Compatibilidade com códigos **ICAO e IATA**

### 🔹 Enriquecimento Climático
- Integração com dados **ERA5**
- Extração climática em janelas de:
  - `1h antes do voo`
  - `3h antes do voo`
- Variáveis:
  - vento
  - chuva
  - nebulosidade
  - neve
- Matching espacial por ponto mais próximo

### 🔹 Enriquecimento Temporal
- Feriados nacionais (Brasil e EUA)
- Vésperas e pós-feriados
- Finais de semana prolongados
- Contexto do país do aeroporto de origem

### 🔹 Histórico Operacional (Features Estatísticas)
- Taxa de atraso rolling 30 dias por:
  - rota
  - aeroporto
  - companhia aérea
- Janelas móveis com `closed="left"`  
  → **zero vazamento de dados**

### 🔹 Persistência
- Armazenamento em **Parquet**
- Pipeline preparado para **big data**

---

## 🧠 Modelagem Preditiva

### Algoritmo
**Random Forest Classifier**

### Justificativa Técnica
- Captura relações não lineares
- Robustez a ruído
- Baixa sensibilidade à multicolinearidade
- Boa performance em dados heterogêneos
- Estabilidade em produção
- Facilidade de serialização
- Integração simples com APIs

---

## ⏱️ Validação Temporal (Padrão Produção)

Separação de dados baseada em tempo:

| Conjunto | Período |
|------|------|
| Treino | 2023 – 2024 |
| Teste | 2025 |

> 🔹 Simula cenário real de produção  
> 🔹 Evita data leakage  
> 🔹 Garante generalização futura  

---

## 🧰 Stack Tecnológica

<div align="center">

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Engineering-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-Numerical%20Computing-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--Learn-Machine%20Learning-F7931E?style=for-the-badge&logo=scikitlearn&logoColor=white)
![Xarray](https://img.shields.io/badge/Xarray-Climate%20Data-2C5AA0?style=for-the-badge)
![PyArrow](https://img.shields.io/badge/Parquet-Big%20Data-3A3A3A?style=for-the-badge)
![Joblib](https://img.shields.io/badge/Joblib-MLOps-4B8BBE?style=for-the-badge)
![FastAPI](https://img.shields.io/badge/FastAPI-API%20Integration-009688?style=for-the-badge&logo=fastapi&logoColor=white)

</div>

---

## ⚙️ Pipeline de Machine Learning

- `ColumnTransformer` para pré-processamento
- Encoding categórico com:
  - `min_frequency`
  - `max_categories`
  - `handle_unknown="ignore"`
- Pipeline único persistido
- Padronização treino / inferência
- Artefato único de produção

---

## 📈 Avaliação e Calibração

Avaliação realizada em **dados futuros (2025)**

### Métricas:
- Precision
- Recall
- F1-score

### Estratégia de Decisão:
- Análise de trade-off entre:
  - falso positivo
  - falso negativo
- Calibração manual de threshold
- Otimização para **detecção de risco**

> 🔹 Modelo prioriza **identificação preventiva de atrasos**, alinhado a objetivos operacionais.

---

## 📦 Entrega para Produção

O projeto entrega um **artefato único de inferência**, pronto para consumo em produção:

```python
artefato = {
    "pipeline": modelo_treinado,
    "threshold": 0.502
}
