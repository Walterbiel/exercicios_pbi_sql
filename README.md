
# 📊 Exercícios de Power BI (DAX) e SQL com Base de Vendas

Este repositório contém **20 exercícios práticos** utilizando a base `amazon.csv`.  
Os exercícios estão divididos entre:

- ✅ **10 exercícios com DAX no Power BI**
- ✅ **10 exercícios com SQL**

---

## ✅ **Estrutura da Base (Exemplo)**

| Coluna        | Tipo       |
|---------------|------------|
| pedido_id     | INT        |
| cliente_id    | INT        |
| produto       | TEXT       |
| categoria     | TEXT       |
| preco         | NUMERIC    |
| quantidade    | INT        |
| receita       | NUMERIC    |
| data_pedido   | DATE       |

---

# 🟩 **Exercícios Power BI - DAX**

### 1. Total de Vendas

```dax
Total_Vendas = SUM('Vendas'[receita])
```

### 2. Preço Médio por Categoria

```dax
Preco_Medio = AVERAGE('Vendas'[preco])
```

### 3. Estatísticas do Preço

```dax
Media_Preco = AVERAGE('Vendas'[preco])
Mediana_Preco = MEDIAN('Vendas'[preco])
Moda_Preco = MODE.SNGL('Vendas'[preco])
DesvioPadrao_Preco = STDEV.P('Vendas'[preco])
Variancia_Preco = VAR.P('Vendas'[preco])
```

### 4. Year-over-Year (YoY) de Vendas

```dax
Vendas_Ano_Anterior = CALCULATE([Total_Vendas], SAMEPERIODLASTYEAR('Vendas'[data_pedido]))
Variação_YoY = DIVIDE([Total_Vendas] - [Vendas_Ano_Anterior], [Vendas_Ano_Anterior])
```

### 5. Month-over-Month (MoM) de Vendas

```dax
Vendas_Mes_Anterior = CALCULATE([Total_Vendas], PREVIOUSMONTH('Vendas'[data_pedido]))
Variação_MoM = DIVIDE([Total_Vendas] - [Vendas_Mes_Anterior], [Vendas_Mes_Anterior])
```

### 6. Média Móvel de 3 Meses

```dax
Media_Movel_3M = 
    AVERAGEX(
        DATESINPERIOD('Vendas'[data_pedido], MAX('Vendas'[data_pedido]), -3, MONTH),
        [Total_Vendas]
    )
```

### 7. Filtro de Vendas Altas

```dax
Vendas_Acima_500 = 
    CALCULATE(
        [Total_Vendas],
        FILTER('Vendas', 'Vendas'[receita] > 500)
    )
```

### 8. Top 5 Produtos por Receita

✅ Crie uma Tabela ou Matriz com `Produto` e `Total_Vendas`.  
✅ Aplique `Filtro` de visual: **Top N** → `5` baseado em `[Total_Vendas]`.

### 9. Porcentagem do Total de Vendas por Categoria

```dax
%_Categoria = DIVIDE([Total_Vendas], CALCULATE([Total_Vendas], ALL('Vendas'[categoria])))
```

### 10. Dias Úteis com Venda

```dax
Dias_Com_Venda = DISTINCTCOUNT('Vendas'[data_pedido])
```

---

# 🟦 **Exercícios SQL**

### 1. Total de Vendas e Preço Médio por Categoria

```sql
SELECT categoria,
       SUM(receita) AS total_vendas,
       AVG(preco) AS preco_medio
FROM vendas
GROUP BY categoria;
```

### 2. Mediana e Moda do Preço

**Mediana:**

```sql
SELECT PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY preco) AS mediana_preco
FROM vendas;
```

**Moda:**

```sql
SELECT preco, COUNT(*) AS ocorrencias
FROM vendas
GROUP BY preco
ORDER BY ocorrencias DESC
LIMIT 1;
```

### 3. Desvio Padrão e Variância do Preço

```sql
SELECT STDDEV_POP(preco) AS desvio_padrao,
       VAR_POP(preco) AS variancia
FROM vendas;
```

### 4. Year-over-Year (YoY)

```sql
WITH vendas_ano AS (
    SELECT EXTRACT(YEAR FROM data_pedido) AS ano,
           SUM(receita) AS total_vendas
    FROM vendas
    GROUP BY ano
)
SELECT v1.ano, 
       v1.total_vendas,
       v2.total_vendas AS vendas_ano_anterior,
       (v1.total_vendas - v2.total_vendas) / v2.total_vendas AS variacao_yoy
FROM vendas_ano v1
LEFT JOIN vendas_ano v2 ON v1.ano = v2.ano + 1;
```

### 5. Month-over-Month (MoM)

```sql
WITH vendas_mes AS (
    SELECT DATE_TRUNC('month', data_pedido) AS mes,
           SUM(receita) AS total_vendas
    FROM vendas
    GROUP BY mes
)
SELECT mes,
       total_vendas,
       LAG(total_vendas) OVER (ORDER BY mes) AS vendas_mes_anterior,
       (total_vendas - LAG(total_vendas) OVER (ORDER BY mes)) / LAG(total_vendas) OVER (ORDER BY mes) AS variacao_mom
FROM vendas_mes;
```

### 6. Média Móvel de 3 Meses

```sql
WITH vendas_mes AS (
    SELECT DATE_TRUNC('month', data_pedido) AS mes,
           SUM(receita) AS total_vendas
    FROM vendas
    GROUP BY mes
)
SELECT mes,
       total_vendas,
       AVG(total_vendas) OVER (
           ORDER BY mes ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
       ) AS media_movel_3m
FROM vendas_mes;
```

### 7. Criação de uma View de Resumo de Vendas

```sql
CREATE VIEW resumo_vendas AS
SELECT produto,
       categoria,
       SUM(quantidade) AS total_vendido,
       SUM(receita) AS receita_total
FROM vendas
GROUP BY produto, categoria;
```

### 8. Filtro de Vendas Acima de R$ 500

```sql
SELECT *
FROM vendas
WHERE receita > 500;
```

### 9. Top 10 Produtos Mais Vendidos

```sql
SELECT produto,
       SUM(quantidade) AS total_vendido
FROM vendas
GROUP BY produto
ORDER BY total_vendido DESC
LIMIT 10;
```

### 10. Clientes com mais de 5 Compras

```sql
SELECT cliente_id,
       COUNT(*) AS total_compras
FROM vendas
GROUP BY cliente_id
HAVING COUNT(*) > 5;
```

---

# ✅ **Conclusão**

Este conjunto de exercícios aborda diversas funcionalidades fundamentais de **Power BI** e **SQL**, explorando estatísticas, comparativos temporais e funções avançadas como **Window Functions**, **CTE** e **Views**.

---

> **Autor:** Walter Gonzaga  
> **Consultoria:** WGG Digital Solutions
