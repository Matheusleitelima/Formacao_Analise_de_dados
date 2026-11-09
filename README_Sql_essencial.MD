# SQL Essencial – EducaDados

> Material de consulta rápida dos estudos de SQL – Formação Analista de Dados | EducaDados.

## Índice

1. [Fundamentos de Consulta](#1-fundamentos-de-consulta)
2. [Filtros e Operadores](#2-filtros-e-operadores)
3. [Funções de Agregação](#3-funções-de-agregação)
4. [GROUP BY e HAVING](#4-group-by-e-having)
5. [Métricas de Negócio](#5-métricas-de-negócio)
6. [JOINs e Relacionamentos](#6-joins-e-relacionamentos)
7. [Ordem de Execução SQL](#7-ordem-de-execução-sql)
8. [Resumo dos Comandos](#8-resumo-dos-comandos)

---

## 1. Fundamentos de Consulta

### SELECT – Selecionar dados

Seleciona informações de uma tabela.

```sql
-- Selecionar todos os dados
SELECT *
FROM alunos;

-- Selecionar uma coluna específica
SELECT instrutor
FROM cursos;

-- Selecionar várias colunas
SELECT nome, email, cidade
FROM alunos;
```

### DISTINCT – Remover duplicidades

Retorna apenas valores distintos.

```sql
SELECT DISTINCT cidade
FROM alunos;

-- Instrutores sem duplicidade
SELECT DISTINCT instrutor
FROM cursos;
```

### ALIAS (AS) – Renomear colunas e tabelas

Cria nomes temporários para facilitar a identificação.

```sql
-- Alias de coluna
SELECT
    categoria_id AS id_categoria,
    nome_categoria AS categoria
FROM categorias_curso;

-- Alias de tabela
SELECT cat.nome_categoria
FROM categorias_curso AS cat;
```

### ORDER BY – Ordenar resultados

| Sintaxe | Resultado |
|---|---|
| `ASC` | Crescente (padrão) |
| `DESC` | Decrescente |

```sql
SELECT *
FROM alunos
ORDER BY data_cadastro DESC;

-- Ordenar por mais de uma coluna
SELECT *
FROM cursos
ORDER BY preco DESC, nome_curso ASC;
```

---

## 2. Filtros e Operadores

### WHERE – Filtrar registros

```sql
SELECT *
FROM alunos
WHERE uf = 'RJ';

-- Filtrar registros ativos
SELECT *
FROM alunos
WHERE ativo = 1;

-- Comparação numérica
SELECT *
FROM cursos
WHERE preco > 900;
```

**Operadores de comparação:**

| Operador | Significado |
|---|---|
| `=` | Igual |
| `<>` ou `!=` | Diferente |
| `>` | Maior |
| `<` | Menor |
| `>=` | Maior ou igual |
| `<=` | Menor ou igual |

### AND, OR e NOT – Operadores lógicos

```sql
-- AND: todas as condições devem ser verdadeiras
SELECT *
FROM matriculas
WHERE status = 'Ativa'
AND canal = 'TikTok';

-- OR: pelo menos uma condição verdadeira
SELECT *
FROM matriculas
WHERE status = 'Ativa'
OR canal = 'TikTok';

-- NOT: inverte a condição
SELECT *
FROM matriculas
WHERE NOT status = 'Ativa';
```

### BETWEEN – Filtrar intervalos

Utilizado para consultar valores entre dois limites (inclusive).

```sql
SELECT *
FROM acessos
WHERE data_acesso BETWEEN '2025-12-24' AND '2025-12-31';
```

### IN e NOT IN – Filtrar múltiplos valores

```sql
-- IN: valores que pertencem à lista
SELECT *
FROM acessos
WHERE dispositivo IN ('Celular', 'Tablet');

-- NOT IN: valores que não pertencem à lista
SELECT *
FROM acessos
WHERE dispositivo NOT IN ('Celular', 'Tablet');
```

### LIKE – Pesquisar textos

Utilizado para localizar padrões em campos textuais.

| Padrão | Resultado |
|---|---|
| `'%Power BI%'` | Contém Power BI |
| `'Power BI%'` | Começa com Power BI |
| `'%Power BI'` | Termina com Power BI |
| `'_A%'` | Segundo caractere é A |

```sql
SELECT *
FROM cursos
WHERE nome_curso LIKE '%Power BI%';
```

### NULL – Valores ausentes

```sql
-- Identificar valores nulos
SELECT *
FROM cursos
WHERE instrutor IS NULL;

-- Identificar valores preenchidos
SELECT *
FROM cursos
WHERE instrutor IS NOT NULL;
```

**Atenção:** não utilize `= NULL`. Para verificar ausência de valores, utilize `IS NULL` ou `IS NOT NULL`.

### Combinação de filtros

```sql
SELECT *
FROM cursos
WHERE instrutor IS NOT NULL
AND preco > 900
ORDER BY carga_horaria_horas DESC;
```

---

## 3. Funções de Agregação

Utilizadas para realizar cálculos e gerar indicadores.

| Função | Finalidade | Exemplo |
|---|---|---|
| `SUM()` | Soma | `SUM(valor_total)` |
| `MAX()` | Maior valor | `MAX(valor_total)` |
| `MIN()` | Menor valor | `MIN(valor_total)` |
| `AVG()` | Média | `AVG(valor_total)` |
| `COUNT()` | Contagem | `COUNT(id_pedido)` |

### SUM() – Soma

```sql
SELECT SUM(valor_total) AS total_vendas
FROM pedidos;
```

### MAX() – Maior valor

```sql
SELECT MAX(valor_total) AS maior_pedido
FROM pedidos;
```

### MIN() – Menor valor

```sql
SELECT MIN(valor_total) AS menor_pedido
FROM pedidos;
```

### AVG() – Média

```sql
SELECT AVG(valor_total) AS ticket_medio
FROM pedidos;
```

### COUNT() – Contagem

```sql
-- Contar registros
SELECT COUNT(*) AS total_registros
FROM pedidos;

-- Contar valores não nulos
SELECT COUNT(id_pedido) AS total_pedidos
FROM pedidos;

-- Contar valores únicos
SELECT COUNT(DISTINCT id_cliente) AS total_clientes
FROM pedidos;
```

**Diferença importante:**

- `COUNT(*)`: conta todas as linhas.
- `COUNT(coluna)`: conta valores não nulos.
- `COUNT(DISTINCT coluna)`: conta valores únicos não nulos.

### Combinação de agregações

```sql
SELECT
    MAX(data_pedido) AS pedido_mais_recente,
    MIN(data_pedido) AS pedido_mais_antigo,
    SUM(valor_total) AS total_vendas,
    AVG(valor_total) AS ticket_medio
FROM pedidos;
```

---

## 4. GROUP BY e HAVING

### GROUP BY – Agrupar dados

Agrupa registros com valores iguais para permitir cálculos por categoria.

```sql
-- Total de despesas por descrição
SELECT
    descricao,
    SUM(valor_usd) AS total_despesas
FROM despesas
GROUP BY descricao
ORDER BY total_despesas DESC;
```

### WHERE + GROUP BY

O `WHERE` filtra os registros antes do agrupamento.

```sql
SELECT
    metodo_pagamento,
    SUM(valor) AS total_pago
FROM pagamentos
WHERE status_pagamento = 'aprovado'
AND data_pagamento BETWEEN '2025-01-01' AND '2025-12-31'
GROUP BY metodo_pagamento
ORDER BY total_pago DESC;
```

### HAVING – Filtrar agrupamentos

O `HAVING` filtra os resultados depois do agrupamento.

```sql
SELECT
    metodo_pagamento,
    SUM(valor) AS total_pago
FROM pagamentos
WHERE status_pagamento = 'aprovado'
GROUP BY metodo_pagamento
HAVING SUM(valor) > 200000000
ORDER BY total_pago DESC;
```

**WHERE x HAVING**

| WHERE | HAVING |
|---|---|
| Filtra registros | Filtra grupos |
| Antes do GROUP BY | Depois do GROUP BY |
| Filtra colunas e condições individuais | Permite filtrar agregações |

---

## 5. Métricas de Negócio

Consultas utilizadas para responder perguntas empresariais e gerar indicadores.

### Quantidade de pedidos e clientes

```sql
SELECT
    COUNT(id_pedido) AS total_pedidos,
    COUNT(DISTINCT id_cliente) AS total_clientes
FROM pedidos;
```

### Quantidade de pedidos por status

```sql
SELECT
    status,
    COUNT(id_pedido) AS total_pedidos
FROM pedidos
GROUP BY status
ORDER BY total_pedidos DESC;
```

### Faturamento e ticket médio por status

```sql
SELECT
    status,
    COUNT(id_pedido) AS quantidade_pedidos,
    SUM(valor_total) AS valor_pedidos,
    AVG(valor_total) AS ticket_medio
FROM pedidos
GROUP BY status;
```

### Análise por moeda

```sql
SELECT
    codigo_moeda,
    COUNT(id_pedido) AS quantidade_pedidos,
    SUM(valor_total) AS valor_pedidos,
    AVG(valor_total) AS ticket_medio
FROM pedidos
GROUP BY codigo_moeda;
```

**Atenção:** não some valores de moedas diferentes sem realizar a conversão para uma moeda de referência.

### Receita mensal em reais (BRL)

```sql
SELECT
    YEAR(data_pedido) AS ano,
    MONTH(data_pedido) AS mes,
    COUNT(id_pedido) AS quantidade_pedidos,
    SUM(valor_total) AS receita_total
FROM pedidos
WHERE codigo_moeda = 'BRL'
GROUP BY
    YEAR(data_pedido),
    MONTH(data_pedido)
ORDER BY receita_total DESC;
```

### Mês com maior quantidade de pedidos

```sql
SELECT
    YEAR(data_pedido) AS ano,
    MONTH(data_pedido) AS mes,
    COUNT(id_pedido) AS quantidade_pedidos
FROM pedidos
WHERE codigo_moeda = 'BRL'
GROUP BY
    YEAR(data_pedido),
    MONTH(data_pedido)
ORDER BY quantidade_pedidos DESC;
```

---

## 6. JOINs e Relacionamentos

Os JOINs permitem combinar informações de diferentes tabelas utilizando colunas relacionadas.

### PK – Primary Key (Chave Primária)

Identifica exclusivamente cada registro de uma tabela.

Características:
- Não permite valores duplicados.
- Não permite valores NULL.
- Identifica cada registro.

### FK – Foreign Key (Chave Estrangeira)

Coluna que estabelece um relacionamento com uma chave primária ou chave única de outra tabela.

**Exemplo:**

Tabela `clientes`:

| id_cliente (PK) | nome |
|---|---|
| 1 | João |
| 2 | Maria |

Tabela `pedidos`:

| id_pedido (PK) | id_cliente (FK) | valor |
|---|---|---|
| 101 | 1 | 250 |
| 102 | 1 | 180 |
| 103 | 2 | 350 |

**Relacionamento:** um cliente pode possuir vários pedidos (1:N).

### INNER JOIN

Retorna somente os registros que possuem correspondência nas duas tabelas.

```sql
SELECT
    c.nome,
    p.id_pedido,
    p.valor
FROM clientes AS c
INNER JOIN pedidos AS p
    ON c.id_cliente = p.id_cliente;
```

### LEFT JOIN

Retorna todos os registros da tabela à esquerda e os correspondentes da tabela à direita. Quando não há correspondência, os campos da direita retornam NULL.

```sql
SELECT
    c.nome,
    p.id_pedido
FROM clientes AS c
LEFT JOIN pedidos AS p
    ON c.id_cliente = p.id_cliente;
```

### RIGHT JOIN

Retorna todos os registros da tabela à direita e os correspondentes da tabela à esquerda.

```sql
SELECT
    c.nome,
    p.id_pedido
FROM clientes AS c
RIGHT JOIN pedidos AS p
    ON c.id_cliente = p.id_cliente;
```

### FULL OUTER JOIN

Retorna todos os registros das duas tabelas, correspondentes ou não.

```sql
SELECT
    c.nome,
    p.id_pedido
FROM clientes AS c
FULL OUTER JOIN pedidos AS p
    ON c.id_cliente = p.id_cliente;
```

---

## 7. Ordem de Execução SQL

Ordem lógica de processamento de uma consulta:

1. `FROM` – Identifica a tabela.
2. `JOIN` – Relaciona tabelas.
3. `WHERE` – Filtra registros.
4. `GROUP BY` – Agrupa dados.
5. `HAVING` – Filtra agrupamentos.
6. `SELECT` – Seleciona colunas e cálculos.
7. `DISTINCT` – Remove duplicidades.
8. `ORDER BY` – Ordena resultados.
9. `LIMIT` – Limita a quantidade de registros (quando suportado).

**Exemplo de estrutura completa:**

```sql
SELECT
    coluna,
    SUM(valor) AS total
FROM tabela
INNER JOIN outra_tabela
    ON tabela.id = outra_tabela.id
WHERE condicao = 'valor'
GROUP BY coluna
HAVING SUM(valor) > 100
ORDER BY total DESC
LIMIT 10;
```

---

## 8. Resumo dos Comandos

| Comando | Para que serve |
|---|---|
| SELECT | Selecionar dados |
| FROM | Definir tabela |
| DISTINCT | Remover duplicidades |
| AS | Criar alias |
| WHERE | Filtrar registros |
| AND | Condição E |
| OR | Condição OU |
| NOT | Negar condição |
| BETWEEN | Filtrar intervalo |
| IN | Buscar valores específicos |
| NOT IN | Excluir valores específicos |
| LIKE | Buscar padrões de texto |
| IS NULL | Identificar valores nulos |
| IS NOT NULL | Identificar valores preenchidos |
| ORDER BY | Ordenar resultados |
| SUM() | Somar valores |
| MAX() | Maior valor |
| MIN() | Menor valor |
| AVG() | Média |
| COUNT() | Contagem |
| GROUP BY | Agrupar registros |
| HAVING | Filtrar agrupamentos |
| INNER JOIN | Correspondências entre tabelas |
| LEFT JOIN | Todos da esquerda e correspondências da direita |
| RIGHT JOIN | Todos da direita e correspondências da esquerda |
| FULL OUTER JOIN | Todos os registros das duas tabelas |

---

## 9. Observações importantes

- Utilize `SELECT *` apenas quando precisar de todas as colunas.
- Prefira selecionar somente as colunas necessárias.
- Utilize aliases para deixar os resultados mais compreensíveis.
- Lembre-se de que `WHERE` vem antes de `GROUP BY`.
- Utilize `HAVING` para filtrar resultados de agregações.
- `COUNT(DISTINCT)` é útil para contar clientes, produtos e categorias únicas.
- Observe os tipos de dados e os formatos de datas.
- Verifique a documentação do SGBD utilizado, pois algumas funções e sintaxes variam entre MySQL, PostgreSQL e SQL Server.



**Objetivo:** manter uma referência prática para consultas SQL durante estudos e projetos de análise de dados.
