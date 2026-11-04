## 2. A Refatoração (O Conserto da View)

Para corrigir a falha e garantir a precisão dos dados consumidos pelo dashboard da diretoria, refatoramos a View utilizando a cláusula `CREATE OR REPLACE VIEW` e a sintaxe de junção otimizada (`USING`).

### Script SQL Refatorado:

```sql
-- Refatoração da View Financeira para o Ano Fiscal de 2004
CREATE OR REPLACE VIEW view_relatorio_financeiro AS
SELECT
    c.customerName,
    o.orderNumber,
    SUM(od.quantityOrdered * od.priceEach) AS valor_total
FROM
    customers c
INNER JOIN 
    orders o USING (customerNumber)
INNER JOIN 
    orderdetails od USING (orderNumber)
WHERE
    o.status = 'Shipped'
    AND YEAR(o.orderDate) = 2004
GROUP BY
    o.orderNumber,
    c.customerName;
```

### Comandos de Validação e Inspeção

1. **Inspeção de Objetos no Banco (`SHOW FULL TABLES`):**
   Para confirmar se o objeto foi criado como uma *virtual table* (`VIEW`) no catálogo do MySQL, executa-se:
   ```sql
   SHOW FULL TABLES IN classicmodels WHERE Table_type = 'VIEW';
   ```

2. **Teste de Auditoria Anti-Erros:**
   Para atestar que nenhum pedido cancelado ou fora de 2004 permanece na View:
   ```sql
   SELECT 
       v.orderNumber,
       v.customerName,
       v.valor_total,
       o.status,
       o.orderDate
   FROM 
       view_relatorio_financeiro v
   JOIN 
       orders o USING (orderNumber)
   WHERE 
       o.status != 'Shipped' 
       OR YEAR(o.orderDate) != 2004;
   ```
   * **Resultado Esperado:** **0 linhas retornadas.**
