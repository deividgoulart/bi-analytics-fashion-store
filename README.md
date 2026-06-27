# Projeto em Business Intelligence e Analytics – European Fashion Store

Este repositório contém o código, dados e modelos utilizados no projeto de BI e Analytics
desenvolvido a partir do dataset público [**Multitable Ecommerce European Fashion**](https://www.kaggle.com/datasets/joycemara/european-fashion-store-multitable-dataset),
focado na análise de desempenho de vendas de uma loja de moda europeia.

## 1. Estrutura do repositório

- `data/raw/`: arquivos CSV originais do Kaggle (clientes, vendas, itens de venda, produtos, estoque, campanhas e canais).
- `data/processed/forecast_sales.csv`: série histórica e valores previstos gerados no notebook de Analytics.
- `notebooks/Forecast_sales.ipynb`: notebook em Python/Colab com o código de agregação diária e modelo de regressão linear para previsão de vendas.
- `powerbi/Dashboard_sales.pbix`: arquivo com o modelo dimensional, medidas DAX e dashboards em Power BI.
- `powerquery/queries_m_transforms.pq`: código M das consultas de importação, limpeza e transformação dos dados em Power Query.

## 2. Código fonte de preparação, coleta, limpeza e processamento de dados

A preparação dos dados é dividida em dois blocos principais:

1. **Power Query (Power BI)**  
   - Importação dos arquivos CSV de `data/raw/`.  
   - Conversão de tipos de dados, tratamento de valores nulos e criação de colunas derivadas.  
   - Modelagem dimensional (fato de itens de venda e dimensões de clientes, produtos, campanhas, canais e datas).  
   - O código M das consultas está em: `powerquery/queries_m_transforms.pq`

2. **Google Colab – Análise preditiva**  
   - Leitura do arquivo de itens de venda (`data/raw/dataset_fashion_store_sales_items.csv`).  
   - Agregação do faturamento diário, criação de calendário contínuo (incluindo dias sem venda).  
   - Treinamento de um modelo simples de regressão linear no tempo e geração de previsões para os dias futuros.  
   - Exportação do arquivo `data/processed/forecast_sales.csv`, utilizado posteriormente no Power BI.  
   - Notebook: `notebook/Forecast_sales.ipynb`

## 3. Dashboards (Solução de BI)

Os dashboards foram desenvolvidos em **Power BI Desktop** a partir do modelo dimensional e das medidas DAX definidas no arquivo PBIX:

- **Página 1 – Visão Geral de Vendas**  
  - KPIs principais: Total de vendas, Total de pedidos, Ticket médio, Clientes ativos.  
  - Gráfico de vendas por semana e gráfico de faturamento por categoria, com parâmetro de campo para alternar dimensões de produto.  
  - Tabela de produtos por faturamento.

- **Página 2 – Campanhas & Canais**  
  - Análise de vendas por campanha e canal de venda.  
  - Linha de vendas diárias durante a campanha selecionada.  
  - Tabela-resumo com início, fim, faturamento e número de pedidos por campanha.

- **Página 3 – Analytics & Previsão**  
  - Comparação entre o faturamento dos últimos 7 dias reais e a previsão para os próximos 7 dias.  
  - Gráfico de linha com série histórica de vendas e série prevista, combinando dados reais e valores projetados.

Arquivo do relatório:

- `powerbi/Dashboard_sales.pbix`

## 4. Análise preditiva

A análise preditiva utiliza um modelo de regressão linear simples aplicado sobre a série diária de faturamento:

- O notebook `notebooks/Forecast_sales.ipynb` prepara a série temporal, treina o modelo e gera previsões.  
- As previsões são exportadas para `data/processed/forecast_sales.csv`.  
- Esse arquivo é reimportado no Power BI e relacionado à dimensão de datas.

## 5. Tabelas principais

| Tabela                       | Origem                                         | Descrição                                                                                     |
| ---------------------------- | ---------------------------------------------- | --------------------------------------------------------------------------------------------- |
| customers                    | dataset_fashion_store_customers.csv (Kaggle)   | Dados cadastrais dos clientes da loja (identificador, localização e atributos básicos).       |
| sales            | dataset_fashion_store_sales.csv (Kaggle)       | Informações de cada venda/pedido (ID da venda, cliente, data, canal e campanha associada).    |
| sales_items | dataset_fashion_store_sales_items.csv (Kaggle) | Itens de cada venda: produto vendido, quantidade e valor total da linha.                      |
| products      | dataset_fashion_store_products.csv (Kaggle)    | Catálogo de produtos, incluindo categoria, gênero, cor, tamanho e outras características.     |
| stock            | dataset_fashion_store_stock.csv (Kaggle)       | Informações de estoque por produto, utilizadas para análises de disponibilidade e giro.       |
| campaigns    | dataset_fashion_store_campaigns.csv (Kaggle)   | Campanhas de marketing com nome, data de início e data de término.                            |
| channels      | dataset_fashion_store_channels.csv (Kaggle)    | Canais de venda (por exemplo, loja física e e‑commerce).                                      |
| dim_date                     | Gerada em Power BI (função CALENDAR)           | Dimensão de datas para análises temporais (dia, mês, ano, ano‑mês, dia da semana etc.).       |
| forecast_sales               | data/processed/forecast_sales.csv (Colab)      | Série diária combinando vendas históricas (total_sales) e valores previstos (forecast_sales). |
