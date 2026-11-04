## 3. Síntese Teórica: Segurança e Abstração

Em alinhamento com a literatura técnica, uma *Database View* é uma **tabela virtual (*virtual table*)** baseada na consulta `SELECT` de uma ou mais tabelas, não fazendo parte do esquema físico da base. 

A requisição do gerente financeiro por uma View — em vez da distribuição de scripts `SELECT` manuais — fundamenta-se nas seguintes vantagens arquiteturais:

### 3.1. Segurança de Dados (Restrição de Acesso)
- **Controle de Acesso em Nível de Colunas/Linhas:** As Views adicionam **camadas extras de segurança**. O administrador do banco (DBA) pode conceder privilégios de consulta (`GRANT SELECT`) apenas sobre a View `view_relatorio_financeiro`, impedindo que usuários não autorizados acessem as tabelas base (`customers`, `orders`, `orderdetails`).
- **Proteção de Informações Sensíveis:** Informações restritas contidas na tabela `customers` (como `creditLimit`, contatos e dados bancários) ficam ocultas da equipe financeira ou analistas de relatórios.

### 3.2. Complexidade de Abstração e Consistência
- **Simplificação de Consultas Complexas:** A View oculta a complexidade dos múltiplos `JOINs`, agregações (`SUM`), junções relacionais e funções temporais (`YEAR`). O usuário final necessita apenas executar um comando simples:
  ```sql
  SELECT * FROM view_relatorio_financeiro;
  ```
- **Consistência da Lógica de Negócios:** Centraliza a regra de negócio ("faturamento = pedidos *Shipped* em 2004") em um único objeto no catálogo do SGBD. Caso a regra mude no futuro, atualiza-se apenas a View, garantindo que todas as aplicações e dashboards integrados continuem recebendo os dados corretos sem alteração em seus códigos-fonte (habilidade de compatibilidade e manutenibilidade).

---

## Referências

DEVMEDIA. MySQL – Trabalhando com Views. Disponível em: https://www.devmedia.com.br/mysql-trabalhando-com-views/8724. Acesso em: 3 nov. 2026.

BARROS, Thiago Medeiros. Aula 03 - BD - ER e Abstração. YouTube, 17 out. 2020. Disponível em: https://www.youtube.com/watch?v=DV0ftFMmZdQ. Acesso em: 3 nov. 2026.
