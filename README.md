# 🛒 Supermarket Sales & Performance Analytics Pipeline

![Status](https://img.shields.io/badge/STATUS-COMPLETED-brightgreen)
![Python](https://img.shields.io/badge/PYTHON-3.10%2B-blue)
![Pandas](https://img.shields.io/badge/PANDAS-ETL-150458)
![SQL](https://img.shields.io/badge/SQL-DUCKDB%20%2F%20MOTHERDUCK-FFF000)
![Looker Studio](https://img.shields.io/badge/BI-LOOKER%20STUDIO-4285F4)

> **Desafio Empresarial Real:** Projeto end-to-end simulando uma demanda da Diretoria de Operações para estruturar um pipeline completo de análise de vendas. O fluxo contempla a ingestão de dados brutos de transações em Excel, higienização em Python/Pandas, modelagem analítica via SQL/MotherDuck e publicação de um Dashboard Executivo no Looker Studio.

---

## 🎯 1. O Problema de Negócio (Business Case)

> 📨 **Mensagem da Diretoria de Operações**  
> **De:** Diretoria de Vendas & Operações  
> **Para:** Calebe Valério (Analytics Engineer)  
> **Assunto:** Diagnóstico do Histórico de Vendas das Filiais  
>
> Bom dia, Calebe!  
>
> Precisamos de um diagnóstico claro sobre o desempenho de vendas da nossa rede de supermercados ao longo do ano. Nossos dados brutos estão consolidados em planilhas de transações, mas não temos visibilidade sobre o faturamento real por filial, a sazonalidade dos meses e a diferença entre os produtos que mais faturam versus os que mais vendem em volume.  
>
> Sua missão é criar um pipeline automatizado de dados para limpar e tratar essa base, estruturar consultas analíticas em banco de dados e construir um painel executivo e interativo para a tomada de decisão.  
>
> **Requisitos do Pipeline:**
> 1. Ingerir o histórico transacional bruto (Excel/CSV).
> 2. Tratar datas, formatar valores monetários e validar duplicidades via Python (Pandas).
> 3. Modelar e criar Views SQL no MotherDuck/DuckDB para consumo de BI.
> 4. Desenvolver um Dashboard no Looker Studio com indicadores de faturamento, filiais e produtos.

---

## ⚙️ 2. Pipeline de Dados & Arquitetura

1. **Ingestão & ETL (Python / Pandas):** Carregamento dos dados brutos, conversão de tipos temporais (`datetime`), tratamento de caracteres e validação de consistência dos valores monetários.
2. **Armazenamento & Modelagem (SQL / MotherDuck):** Criação da tabela fato e views de agregação temporal e dimensional no DuckDB/MotherDuck.
3. **Business Intelligence (Looker Studio):** Conexão das Views SQL diretamente na ferramenta de visualização para construção dos KPIs e gráficos executivos.


---

## 🐍 3. Amostra dos Dados Tratados (Pandas DataFrame)

| id_venda | data_registro | produto | quantidade | preco_unitario | valor_total | filial | metodo_pagamento |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `1001` | `2025-01-15` | Picanha (kg) | 2 | 85.00 | 170.00 | Matriz - Centro | Cartão de Crédito |
| `1002` | `2025-01-16` | Leite Integral 1L | 10 | 4.50 | 45.00 | Filial - Zona Sul | PIX |
| `1003` | `2025-02-01` | Arroz 5kg | 3 | 28.00 | 84.00 | Filial - Zona Norte | Cartão de Débito |
| `1004` | `2025-02-10` | Cerveja Harboe | 24 | 3.50 | 84.00 | Filial - Zona Sul | Dinheiro |

---

De: Calebe Valério (Analytics Engineer)

Para: Diretoria de Vendas & Operações

Assunto: Diagnóstico Estratégico de Vendas e Faturamento

Prezados, apresento os resultados da análise consolidada de vendas, construída após a higienização da base transacional e modelagem SQL:
Volume Geral de Receita: A rede acumulou um Faturamento Total de R$ 42.450,41 distribuído em 1.000 transações únicas ao longo do período analisado.
Comportamento Temporal: A análise de evolução mensal revela picos relevantes de vendas em Agosto e Outubro, enquanto meses como Março e Junho registraram retração. Essa oscilação sazonal permite um planejamento otimizado de estoque e compras preventivas.
Participação por Filial: A distribuição de receita entre as unidades é extremamente equilibrada:
Filial Zona Sul: Lidera com 34,9% do faturamento.
Filial Zona Norte: Representa 34,1% das vendas.
Matriz Centro: Responde por 31,0% da receita total.
Mix de Produtos (Faturamento vs. Volume): Identificou-se uma distinção clara entre produtos de alta margem (ex: Picanha e Corte Bovino, que lideram em faturamento financeiro em R$) e produtos de alto giro de estoque (ex: Leite e Pão, que dominam em quantidade de unidades vendidas).
Interatividade Operacional: O relatório foi equipado com controles de filtro por filial e metodo_pagamento, permitindo análises segmentadas dinâmicas

## 💾 4. Análise e Views SQL (MotherDuck / DuckDB)

```sql
-- 1. View de Faturamento Mensal (Evolução Temporal)
CREATE OR REPLACE VIEW my_db.vw_faturamento_mensal AS
SELECT 
    STRFTIME(CAST(data_registro AS TIMESTAMP), '%Y-%m') AS mes_ano,
    ROUND(SUM(valor_total), 2) AS faturamento_total
FROM my_db.fato_vendas
GROUP BY mes_ano
ORDER BY mes_ano ASC;

-- 2. View de Desempenho por Filial
CREATE OR REPLACE VIEW my_db.vw_desempenho_filial AS
SELECT 
    filial,
    COUNT(DISTINCT id_venda) AS total_transacoes,
    ROUND(SUM(valor_total), 2) AS faturamento_total
FROM my_db.fato_vendas
GROUP BY filial
ORDER BY faturamento_total DESC;

-- 3. View de Ranking de Produtos (Faturamento vs. Volume)
CREATE OR REPLACE VIEW my_db.vw_ranking_produtos AS
SELECT 
    produto,
    SUM(quantidade) AS unidades_vendidas,
    ROUND(SUM(valor_total), 2) AS faturamento_total
FROM my_db.fato_vendas
GROUP BY produto
ORDER BY faturamento_total DESC;
```

---

<img width="1877" height="1414" alt="IMG_0999" src="https://github.com/user-attachments/assets/234d65f5-287f-48d3-9f2b-48ed59a38439" />



---


## 📌 6. Considerações Finais & Recomendações Futuras

### 💡 Conclusão Executiva
Este projeto entregou uma solução completa de dados, transformando dados brutos de transações em uma estrutura analítica confiável e de fácil consulta. A integração entre **Python**, **SQL (MotherDuck)** e **Looker Studio** garante que a diretoria tenha acesso em tempo real a métricas consolidadas, permitindo identificar rapidamente quais produtos sustentam a receita e quais filiais demandam ações operacionais.

---
