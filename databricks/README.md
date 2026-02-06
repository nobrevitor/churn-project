# Engenharia de Dados — Base Analítica RFV

Este módulo do projeto é dedicado à **engenharia de dados**, com foco na construção de uma base analítica confiável para segmentação de clientes utilizando a metodologia **RFV (Recência, Frequência e Valor)**.

O objetivo principal desta etapa é transformar dados transacionais brutos em uma **camada analítica pronta para consumo**, servindo de base para análises exploratórias, segmentação de clientes e modelos analíticos futuros.

---

## Objetivo da Engenharia

- Construir uma base de dados limpa e confiável
- Consolidar dados de pedidos e pagamentos
- Definir corretamente a entidade **cliente**
- Aplicar regras de negócio para garantir consistência
- Criar uma **view analítica RFV** agregada por cliente

Esta etapa antecede qualquer análise ou modelagem, garantindo qualidade, governança e confiabilidade dos dados.

---

## Arquitetura de Dados

O projeto segue uma abordagem em camadas:

- **Bronze**: dados brutos
- **Silver**: dados tratados e padronizados
- **Gold**: dados agregados e analíticos

---

## Definição da Chave de Cliente

Um ponto crítico do projeto foi a escolha correta da chave de agregação.

Apesar de existir o campo `customer_id`, observou-se que:
- um mesmo cliente pode possuir múltiplos `customer_id`
- cada novo pedido pode gerar um novo identificador

Por esse motivo, foi utilizado o campo **`customer_unique_id`**, que representa de forma consistente o cliente ao longo do tempo.

Essa decisão foi essencial para:
- evitar duplicidade de clientes
- calcular corretamente RFV
- eliminar vieses analíticos

---

## Métricas RFV

A base analítica calcula, por cliente:

- **Recência (R)**  
  Número de dias desde a última compra

- **Frequência (F)**  
  Quantidade de pedidos realizados pelo cliente

- **Valor (V)**  
  Valor total gasto pelo cliente

Essas métricas são posteriormente classificadas em **A, B e C**, permitindo a criação de segmentos como `AAA`, `ABC`, `CCC`, etc.

---

## Regras de Negócio Aplicadas

- Consideração apenas de pedidos entregues para cálculo financeiro
- Agregações realizadas exclusivamente por `customer_unique_id`  

Essas regras garantem que os indicadores reflitam o comportamento real de compra dos clientes.

---

## Tecnologias Utilizadas

- **Databricks**
- **Spark SQL**
- Modelagem Analítica
- Views materializadas

---

## Próximas Etapas

Com a base analítica construída, o projeto segue para:

1. Modelagem de previsão de churn e clusterização 
2. Dashboard interativo Power BI para visualização e comunicação dos resultados  

---

## Observação

Esta pasta representa a **primeira etapa** de um projeto maior, pensado de forma modular para separar claramente:
- Engenharia de Dados
- Análise de Dados
- Machine Learning
