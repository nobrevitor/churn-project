# Projeto End-to-End de Churn de Clientes – Olist

## Visão Geral

Este projeto tem como objetivo construir uma solução **end-to-end de dados** para análise e predição de **churn de clientes**, utilizando uma arquitetura moderna e escalável baseada no padrão **Medallion (RAW → BRONZE → SILVER → GOLD)**.

O projeto foi dividido em **três grandes frentes**, que se conectam entre si:

1. **Engenharia de Dados** (Databricks + Spark SQL)
2. **Análise de Dados** (Power BI)
3. **Ciência de Dados** (Machine Learning + MLflow)

A base de dados utilizada é o **Olist (e-commerce brasileiro)**, permitindo trabalhar com dados reais, relacionais e de grande volume, adequados para uso com **Spark SQL**.

---

## Tecnologias Utilizadas

### Engenharia de Dados

* Databricks (Free Edition)
* Apache Spark (Spark SQL)
* Unity Catalog (camada lógica de metadados)
* Git (versionamento via Git folder)

### Análise de Dados

* Power BI
* SQL analítico

### Ciência de Dados

* Python
* PySpark
* Scikit-learn
* MLflow (experimentos e tracking)
* Streamlit (deploy conceitual do modelo)

---

## Arquitetura de Dados

O projeto segue a **Medallion Architecture**, amplamente utilizada em ambientes de dados modernos.

### RAW

* Representa os dados conforme ingeridos da fonte
* No Databricks Free Edition, os dados são carregados diretamente como tabelas tabulares no schema `workspace.default`
* Não há regras de negócio, joins ou transformações semânticas

Exemplos de tabelas:

* `olist_customers_dataset`
* `olist_orders_dataset`
* `olist_order_items_dataset`
* `olist_payments_dataset`

---

### BRONZE

* Camada de **padronização técnica**
* Correção de tipos
* Padronização de nomes de colunas
* Seleção de colunas relevantes
* Nenhuma regra de negócio aplicada

Schema:

* `workspace.olist_bronze`

Exemplos:

* `bronze_customers`
* `bronze_orders`
* `bronze_payments`

---

### SILVER

* Camada de **entidades de negócio**
* Aplicação de regras de negócio
* Joins entre tabelas
* Definição de granularidade

Schema:

* `workspace.olist_silver`

Exemplos:

* `silver_customers`
* `silver_orders_enriched`

---

### GOLD

* Camada de **consumo analítico e modelagem**
* KPIs
* Features para Machine Learning
* Fonte única para Power BI e MLflow

Schema:

* `workspace.olist_gold`

Exemplos:

* `gold_churn_kpis`
* `gold_churn_features`

---

## Definição de Churn

Neste projeto, o churn é definido de forma **temporal e objetiva**.

> Um cliente é considerado churn se **não realizar nenhuma nova compra dentro de um período de 90 dias após sua última compra**.

### Critérios técnicos

* Base temporal: `order_purchase_timestamp`
* Janela de inatividade: 90 dias
* Churn:

  * `1` → cliente churn
  * `0` → cliente ativo

Essa definição é utilizada de forma **consistente**:

* Na engenharia de dados (SILVER / GOLD)
* Nos KPIs do Power BI
* No treinamento do modelo de Machine Learning

---

## Projeto 1 – Engenharia de Dados

Objetivo:
Criar uma base confiável, reprocessável e escalável para suportar análises e modelos de churn.

Principais atividades:

* Ingestão dos dados do Olist
* Padronização na camada BRONZE
* Criação de entidades na camada SILVER
* Geração de tabelas analíticas e features na camada GOLD

Toda a lógica é implementada utilizando **Spark SQL**, organizada em notebooks versionados via Git.

---

## Projeto 2 – Análise de Dados

Objetivo:
Analisar o comportamento de churn dos clientes e identificar os principais fatores associados.

Atividades:

* Conexão do Power BI à camada GOLD
* Construção de KPIs de churn
* Análise por:

  * tempo
  * estado
  * frequência de compra
  * valor gasto
* Criação de dashboard interativo

---

## Projeto 3 – Ciência de Dados

Objetivo:
Construir um modelo preditivo capaz de estimar a probabilidade de churn de um cliente.

Atividades:

* Feature engineering a partir da camada GOLD
* Treinamento de modelos de Machine Learning
* Comparação entre algoritmos tradicionais e redes neurais simples
* Tracking de experimentos com MLflow
* Deploy conceitual do modelo utilizando Streamlit

---

## Organização do Repositório

```
olist-churn-project/
│
├── README.md
├── docs/
│   └── arquitetura_medallion.md
├── databricks/
│   ├── 01_raw/
│   ├── 02_bronze/
│   ├── 03_silver/
│   └── 04_gold/
├── powerbi/
└── ml/
```

---

## Próximos Passos

* Implementar checks de qualidade de dados
* Criar snapshots temporais para churn
* Simular orquestração com múltiplos notebooks
* Evoluir o deploy do modelo para API

---

## Autor

Vitor Nobre
Projeto desenvolvido para fins de estudo, portfólio e aprofundamento em engenharia de dados, análise e ciência de dados.
**Vitor Nobre**
Projeto desenvolvido para fins de estudo, portfólio e aprofundamento em engenharia de dados, análise e ciência de dados.
