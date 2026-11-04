
## 1. Diagnóstico do Problema 

Após análise detalhada do script fornecido pelo desenvolvedor júnior e da estrutura de dados do banco `classicmodels`, identificamos as falhas lógicas e de filtragem que causaram a distorção milionária nos relatórios financeiros.

### Código Analisado (Dev Jr.):
```sql
CREATE VIEW view_relatorio_financeiro AS
SELECT
   c.customerName,
   o.orderNumber,
   SUM(od.quantityOrdered * od.priceEach) AS valor_total
FROM
   customers c
JOIN
   orders o ON c.customerNumber = o.customerNumber
JOIN
   orderdetails od ON o.orderNumber = od.orderNumber
GROUP BY
   o.orderNumber;
```

### Inconsistências Técnicas Detectadas:

1. **Ausência do Filtro de Status do Pedido (`o.status`):**
   * **Falha:** O código realiza junções entre `customers`, `orders` e `orderdetails`, mas não restringe os registros pelo status da venda.
   * **Impacto:** Pedidos com status `'Cancelled'` (cancelados), `'On Hold'` (retidos por limite de crédito), `'In Process'` ou `'Disputed'` foram contabilizados indevidamente como receita realizada.

2. **Ausência do Filtro Temporável / Ano Fiscal (`o.orderDate`):**
   * **Falha:** A diretoria solicitou o faturamento referente **exclusivamente ao ano fiscal de 2004**. A consulta do estagiário acumulou todo o histórico presente na base.
   * **Impacto:** Os valores apresentavam o faturamento acumulado de múltiplos anos, gerando uma visão ilusória dos lucros de 2004.

3. **Inconformidade de Agrupamento (`GROUP BY`):**
   * **Falha:** Apenas `o.orderNumber` foi inserido no `GROUP BY`, enquanto `c.customerName` foi mantido na projeção sem função de agregação. Para garantir compatibilidade com o padrão ANSI SQL e modos estritos do MySQL (`ONLY_FULL_GROUP_BY`), todas as colunas não agregadas no `SELECT` devem constar na cláusula `GROUP BY`.

---
