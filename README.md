# Projeto End-to-End de Churn de Clientes

## Visão Geral

Este projeto tem como objetivo construir uma solução **end-to-end de dados** para análise e predição de **churn de clientes**, utilizando uma arquitetura moderna e escalável baseada no padrão **Medallion (RAW → BRONZE → SILVER → GOLD)**.

O projeto foi dividido em três grandes frentes, que se conectam entre si:

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
* DAX
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

---

### BRONZE

* Camada de exploração: 
  * Tipos das colunas
  * Valores nulos
  * Valores iconsistentes

Schema:

* `workspace.olist_bronze`

---

### SILVER

* Camada de correção e pré-processamento:
  * Correção dos tipos dos dados
  * Tratando valores nulos
  * Correção de dados inconsistentes

Schema:

* `workspace.olist_silver`

---

### GOLD

* Camada de consumo analítico e modelagem:
  * Dados limpos e organizados
  * Criação RFV
  * Fonte única para Power BI e Machine Leaning

Schema:

* `workspace.olist_gold`

---

## Frente 1 – Engenharia de Dados

Objetivo:
Criar uma base confiável, reprocessável e escalável para suportar análises e modelos de churn.

Principais atividades:

* Ingestão dos dados do Olist
* Exploração na camada BRONZE
* Pré-processamento e tratamento na camada SILVER
* Geração de tabelas analíticas e features na camada GOLD

Toda a lógica é implementada utilizando **Spark SQL**, organizada em notebooks versionados via Git.

---

## Frente 2 – Análise de Dados

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

## Frente 3 – Ciência de Dados

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
├── databricks/
│   ├── 01_raw/
│   ├── 02_bronze/
│   ├── 03_silver/
│   └── 04_gold/
├── powerbi/
├── ml/
└── README.md
```

---

## Próximos Passos

* Implementar checks de qualidade de dados
* Criar snapshots temporais para churn
* Simular orquestração com múltiplos notebooks
* Evoluir o deploy do modelo para API

---

## Autor

**Vitor Nobre Silva**

🔗 [LinkedIn](https://www.linkedin.com/in/vitor-nobre-silva/)

Projeto desenvolvido para fins de estudo, portfólio e aprofundamento em engenharia de dados, análise e ciência de dados.
