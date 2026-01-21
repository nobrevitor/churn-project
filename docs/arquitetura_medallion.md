# Arquitetura Medallion – Projeto Olist Churn

## Objetivo da Arquitetura

Esta arquitetura foi projetada para suportar um **pipeline end-to-end de dados**, garantindo:

* rastreabilidade dos dados
* separação de responsabilidades
* reprocessamento confiável
* reutilização dos dados para análise e ciência de dados

O projeto adota o padrão **Medallion Architecture**, amplamente utilizado em ambientes modernos de dados e nativamente alinhado ao Databricks.

---

## Visão Geral

A arquitetura é composta por quatro camadas lógicas:

```
Fonte (CSV – Olist)
        ↓
RAW  →  BRONZE  →  SILVER  →  GOLD
```

Cada camada possui um propósito bem definido e evita acoplamento excessivo entre ingestão, transformação e consumo.

---

## RAW – Camada de Ingestão

### Descrição

A camada RAW representa os dados **como foram disponibilizados pela fonte**, sem aplicação de regras de negócio ou transformações semânticas.

No contexto do **Databricks Free Edition**, os arquivos CSV do Olist são ingeridos diretamente como **tabelas tabulares** no schema padrão:

```
workspace.default
```

Essa limitação da ferramenta não compromete a arquitetura, pois a RAW continua cumprindo seu papel conceitual.

### Características

* Dados originais da fonte
* Sem joins
* Sem filtros
* Sem métricas
* Estrutura próxima ao fornecedor

### Exemplos de Tabelas

* `clientes`
* `pedidos`
* `itens_pedido`
* `pagamentos`

---

## BRONZE – Padronização Técnica

### Descrição

A camada BRONZE é responsável por **preparar tecnicamente os dados**, tornando-os consistentes e reutilizáveis para camadas superiores.

Nenhuma regra de negócio é aplicada nesta camada.

### Responsabilidades

* Padronização de nomes (snake_case)
* Conversão de tipos (datas, timestamps, numéricos)
* Garantia de schema estável

### Schema

```
workspace.olist_bronze
```

### Exemplos de Tabelas

* `clientes_bronze`
* `pedidos_bronze`
* `itens_pedido_bronze`
* `pagamentos_bronze`

---

## SILVER – Entidades de Negócio

### Descrição

A camada SILVER representa os dados **no contexto do negócio**. Aqui são criadas as entidades que fazem sentido para análise e modelagem.

Nesta camada surgem as primeiras regras de negócio.

### Responsabilidades

* Joins entre tabelas BRONZE
* Criação de entidades consolidadas
* Definição de granularidade
* Aplicação de regras de negócio
* Preparação para análises analíticas

### Schema

```
workspace.olist_silver
```

### Exemplos de Tabelas

* `silver_customers`
* `silver_orders_enriched`

---

## GOLD – Consumo Analítico e Machine Learning

### Descrição

A camada GOLD é orientada ao **consumo final** dos dados, seja para dashboards, KPIs ou modelos de Machine Learning.

Os dados nesta camada são **altamente agregados ou featureados**.

### Responsabilidades

* Criação de KPIs de churn
* Agregações temporais
* Feature engineering
* Fonte única para Power BI e MLflow

### Schema

```
workspace.olist_gold
```

### Exemplos de Tabelas

* `gold_churn_kpis`
* `gold_churn_features`

---

## Definição de Churn na Arquitetura

A definição de churn é aplicada **a partir da camada SILVER**, onde já existe contexto de cliente e pedidos.

Critério adotado:

> Um cliente é considerado churn se não realizar uma nova compra dentro de 90 dias após sua última compra.

Essa definição é propagada para a camada GOLD, garantindo consistência entre:

* engenharia de dados
* análise de dados
* ciência de dados

---

## Organização dos Notebooks

Cada camada possui seus próprios notebooks SQL, versionados via Git:

```
databricks/
├── 01_raw/
├── 02_bronze/
├── 03_silver/
└── 04_gold/
```

Princípios adotados:

* 1 notebook por entidade
* Código reexecutável (idempotente)
* Separação clara de responsabilidades

---

## Considerações Finais

Esta arquitetura foi desenhada considerando:

* boas práticas de engenharia de dados
* limitações do Databricks Free Edition
* clareza para análise e modelagem de churn

O design prioriza **simplicidade, rastreabilidade e escalabilidade conceitual**, permitindo evolução futura para ambientes produtivos com orquestração e monitoramento.
