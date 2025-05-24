
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
**Crie uma medida para calcular o total de vendas.**

```dax
Total_Vendas = SUM('Vendas'[receita])
```

---

### 2. Preço Médio por Categoria
**Crie uma medida que calcule o preço médio dos produtos.**

```dax
Preco_Medio = AVERAGE('Vendas'[preco])
```

---

### 3. Estatísticas do Preço
**Crie medidas para calcular: média, mediana, moda, desvio padrão e variância do preço.**

```dax
Media_Preco = AVERAGE('Vendas'[preco])
Mediana_Preco = MEDIAN('Vendas'[preco])
Moda_Preco = MODE.SNGL('Vendas'[preco])
DesvioPadrao_Preco = STDEV.P('Vendas'[preco])
Variancia_Preco = VAR.P('Vendas'[preco])
```

---

### 4. Year-over-Year (YoY) de Vendas
**Crie uma medida que calcule a variação percentual de vendas em relação ao mesmo período do ano anterior.**

```dax
Vendas_Ano_Anterior = CALCULATE([Total_Vendas], SAMEPERIODLASTYEAR('Vendas'[data_pedido]))
Variação_YoY = DIVIDE([Total_Vendas] - [Vendas_Ano_Anterior], [Vendas_Ano_Anterior])
```

---

### 5. Month-over-Month (MoM) de Vendas
**Crie uma medida que calcule a variação percentual das vendas em relação ao mês anterior.**

```dax
Vendas_Mes_Anterior = CALCULATE([Total_Vendas], PREVIOUSMONTH('Vendas'[data_pedido]))
Variação_MoM = DIVIDE([Total_Vendas] - [Vendas_Mes_Anterior], [Vendas_Mes_Anterior])
```

---

### 6. Média Móvel de 3 Meses
**Crie uma média móvel de vendas com uma janela de 3 meses.**

```dax
Media_Movel_3M = 
    AVERAGEX(
        DATESINPERIOD('Vendas'[data_pedido], MAX('Vendas'[data_pedido]), -3, MONTH),
        [Total_Vendas]
    )
```

---

### 7. Filtro de Vendas Altas
**Crie uma medida para somar todas as vendas acima de R$ 500.**

```dax
Vendas_Acima_500 = 
    CALCULATE(
        [Total_Vendas],
        FILTER('Vendas', 'Vendas'[receita] > 500)
    )
```

---

### 8. Top 5 Produtos por Receita
**Crie uma tabela mostrando os 5 produtos com maior receita.**

✅ Dica: Utilize visual `Tabela` ou `Matriz`, insira `Produto` e `[Total_Vendas]`.  
✅ Aplique `Filtro` de visual: **Top N** → `5` baseado em `[Total_Vendas]`.

---

### 9. Porcentagem do Total de Vendas por Categoria
**Crie uma medida para calcular o percentual que cada categoria representa no total de vendas.**

```dax
%_Categoria = DIVIDE([Total_Vendas], CALCULATE([Total_Vendas], ALL('Vendas'[categoria])))
```

---

### 10. Dias Úteis com Venda
**Crie uma medida que calcule o número de dias com pelo menos uma venda.**

```dax
Dias_Com_Venda = DISTINCTCOUNT('Vendas'[data_pedido])
```

---

# 🟦 **Exercícios SQL**

### 1. Total de Vendas e Preço Médio por Categoria
**Liste o total de vendas e o preço médio por categoria.**

```sql
SELECT categoria,
       SUM(receita) AS total_vendas,
       AVG(preco) AS preco_medio
FROM vendas
GROUP BY categoria;
```

... [Texto continua conforme o conteúdo completo] ...
