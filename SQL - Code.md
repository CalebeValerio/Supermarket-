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
