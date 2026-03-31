# Pipeline de Engenharia de Dados — Olist E-commerce

Projeto de engenharia de dados end-to-end utilizando o dataset público da Olist, maior plataforma de e-commerce do Brasil. O objetivo é construir um pipeline ETL completo, desde a ingestão dos dados brutos até análises de negócio com Python e SQL.

---

## Objetivo

Construir um pipeline de dados estruturado em camadas (Data Lake), aplicar boas práticas de qualidade de dados e responder perguntas de negócio relevantes sobre o comportamento de compras, satisfação de clientes e logística.

---

## 📊 Perguntas de Negócio Respondidas

- Qual o volume de pedidos ao longo do tempo?
- Quais estados compram mais e têm melhor satisfação?
- O tempo de entrega impacta a nota de satisfação do cliente?
- Qual o método de pagamento mais usado e o ticket médio de cada um?
- Qual o tempo médio de aprovação de pedido por método de pagamento?

---

## Arquitetura do Projeto

```
data_lake/
├── raw/                  # Dados originais sem modificação (CSV)
├── processed/            # Dados limpos e tipados por tabela (Parquet)
│   ├── customers/
│   ├── orders/
│   ├── order_items/
│   ├── order_payments/
│   └── order_reviews/
└── curated/              # Tabela analítica final unindo todos os datasets (Parquet)
    └── olist_curated.parquet
```

A arquitetura segue o padrão de camadas utilizado em ambientes de produção (Data Lakehouse), onde cada camada tem uma responsabilidade clara:

- **Raw**: dados brutos, nunca modificados
- **Processed**: dados limpos, tipados e validados
- **Curated**: tabela analítica pronta para consumo

---

## 🔄 Pipeline ETL

O pipeline é composto por funções específicas de transformação para cada tabela, evitando tratamentos genéricos que poderiam descartar dados válidos.

**Etapas por tabela:**
- Remoção de duplicatas
- Validação de campos obrigatórios com `dropna(subset=[...])`
- Conversão de tipos (datas, strings)
- Filtros de regras de negócio (ex: preço > 0)
- Padronização de nomes de colunas

**Camada Curated:**
- Join entre todas as tabelas usando `order_id` e `customer_id`
- Agregações de itens e pagamentos por pedido
- Cálculo de tempo de entrega em dias
- Filtro apenas para pedidos com status `delivered`

---

## Qualidade dos Dados

Antes do pipeline, foram aplicadas validações de negócio:

- Nenhum pedido entregue antes de ser comprado
- Nenhum pedido aprovado antes da compra
- Nenhum preço ou frete negativo
- Valores nulos contextualizados (ex: campos opcionais em reviews não são tratados como erro)

---

## 🔍 Análises e Insights

**Tempo de entrega vs. Satisfação**
Pedidos entregues em até 7 dias têm nota média significativamente maior do que pedidos com mais de 30 dias de espera — confirmando que a logística é um fator crítico na experiência do cliente.

**Volume por Estado**
SP, RJ e MG concentram a maior parte dos pedidos, refletindo a distribuição populacional e econômica do Brasil.

**Aprovação por Método de Pagamento**
Boleto leva em média 1.8 dias para aprovação, enquanto cartão de crédito é aprovado em menos de 0.2 dias — diferença explicada pelo fluxo de compensação bancária do boleto.

---

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Uso |
|---|---|
| Python | Linguagem principal |
| Pandas | Manipulação e transformação de dados |
| DuckDB | Consultas SQL analíticas em memória |
| Matplotlib | Visualizações |
| Parquet | Formato de armazenamento colunar |
| Google Colab | Ambiente de desenvolvimento |
| GitHub | Versionamento |

---

## Dataset

Dataset público disponível no Kaggle: [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

Tabelas utilizadas:
- `olist_customers_dataset.csv`
- `olist_orders_dataset.csv`
- `olist_order_items_dataset.csv`
- `olist_order_payments_dataset.csv`
- `olist_order_reviews_dataset.csv`

---

## Como Executar

1. Acesse o notebook pelo botão **Open in Colab** no topo do arquivo
2. Execute as células na ordem — o pipeline baixa os dados automaticamente do repositório
3. Todas as dependências já estão disponíveis no ambiente Colab, exceto DuckDB:

```bash
pip install duckdb
```

---

## Próximos Passos

- Orquestração do pipeline com **Apache Airflow**
- Migração para armazenamento em nuvem (**AWS S3** ou **Google Cloud Storage**)
- Em cenários com maior volume de dados, o pipeline seria migrado para **PySpark**
- Dashboard interativo com **Streamlit** ou **Looker Studio**

---

## Autor

Desenvolvido como projeto de portfólio durante o 5º semestre de Ciência da Computação.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-blue?style=flat&logo=linkedin)](www.linkedin.com/in/thiagomotagomes)
[![GitHub](https://img.shields.io/badge/GitHub-black?style=flat&logo=github)](https://github.com/th900)

