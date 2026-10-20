# SQL Essencial — EducaDados

> **Objetivo:** reunir os principais comandos de consulta SQL para revisão rápida e consulta durante estudos e projetos de análise de dados.

## Sumário

1. [Fundamentos de consulta SQL](#1-fundamentos-de-consulta-sql)
2. [Selecionar tabelas e colunas](#2-selecionar-tabelas-e-colunas)
3. [DISTINCT: resultados sem duplicidade](#3-distinct-resultados-sem-duplicidade)
4. [Alias: nomes temporários](#4-alias-nomes-temporários)
5. [ORDER BY: ordenar resultados](#5-order-by-ordenar-resultados)
6. [Filtros e operadores](#6-filtros-e-operadores)
7. [Funções de agregação](#7-funções-de-agregação)
8. [Exemplos combinados](#8-exemplos-combinados)
9. [Resumo para revisão](#9-resumo-para-revisão)
10. [Erros comuns e boas práticas](#10-erros-comuns-e-boas-práticas)

---

## 1. Fundamentos de consulta SQL

SQL (*Structured Query Language*) é uma linguagem usada para consultar e manipular dados em bancos relacionais.

Uma consulta pode ser pensada em três partes:

1. **O que quero consultar?** — colunas ou expressões depois de `SELECT`.
2. **De qual tabela vêm os dados?** — tabela indicada em `FROM`.
3. **Quais condições e regras aplicar?** — por exemplo, `WHERE`, `ORDER BY` e outros comandos.

Exemplo básico:

```sql
SELECT *
FROM aulas;
```

- `SELECT`: define o que será exibido.
- `*`: seleciona todas as colunas.
- `FROM`: indica a tabela.
- `aulas`: nome da tabela.
- `;`: encerra a instrução SQL em muitos ambientes.

> **Boa prática:** use `SELECT *` para explorar uma tabela durante o aprendizado. Em consultas recorrentes ou profissionais, prefira selecionar apenas as colunas necessárias.

## 2. Selecionar tabelas e colunas

### Consultar todas as colunas

```sql
SELECT *
FROM aulas;
```

### Consultar uma coluna específica

```sql
SELECT instrutor
FROM cursos;
```

A consulta retorna somente a coluna `instrutor`.

### Consultar várias colunas

```sql
SELECT curso_id, nome_curso, instrutor
FROM cursos;
```

**Por que selecionar apenas o necessário?**
- Deixa o resultado mais fácil de ler.
- Evita trazer dados desnecessários.
- Pode reduzir o volume de dados transferidos e melhorar o desempenho, dependendo do banco e da consulta.

### Contar registros

Para contar quantas matrículas existem na tabela:

```sql
SELECT COUNT(*) AS total_matriculas
FROM matriculas;
```

`COUNT(*)` conta as linhas que atendem aos filtros da consulta. Ele não conta apenas valores preenchidos de uma coluna específica.

## 3. `DISTINCT`: resultados sem duplicidade

Use `DISTINCT` para retornar combinações únicas dos valores selecionados.

### Listar instrutores sem repetição

```sql
SELECT DISTINCT instrutor
FROM cursos;
```

### Listar cidades distintas

```sql
SELECT DISTINCT cidade
FROM alunos;
```

**Atenção:** se você selecionar mais de uma coluna, o `DISTINCT` elimina linhas repetidas considerando a combinação de todas as colunas selecionadas.

```sql
SELECT DISTINCT cidade, uf
FROM alunos;
```

Isso retorna combinações únicas de `cidade` e `uf`, e não necessariamente cidades únicas isoladamente.

## 4. Alias: nomes temporários

Um *alias* é um nome temporário dado a uma coluna ou tabela durante a consulta. É útil para tornar o resultado mais claro.

### Alias para colunas

```sql
SELECT
    categoria_id AS id_categoria,
    nome_categoria AS categoria
FROM categorias_curso;
```

`AS` define o nome que será exibido no resultado. Em muitos bancos, ele é opcional para aliases simples.

### Alias para tabelas

```sql
SELECT cat.categoria_id, cat.nome_categoria
FROM categorias_curso AS cat;
```

`cat` é um apelido para a tabela `categorias_curso`. Aliases de tabela são especialmente úteis em consultas com `JOIN`, quando duas ou mais tabelas participam da consulta.

> **Compatibilidade:** para evitar diferenças entre bancos, prefira aliases sem espaços, como `total_pedidos`. Se precisar de um título com espaços, a forma de delimitar o nome depende do banco de dados.

## 5. `ORDER BY`: ordenar resultados

`ORDER BY` organiza as linhas retornadas.

- `ASC`: ordem crescente (padrão).
- `DESC`: ordem decrescente.

### Ordenar por data mais recente

```sql
SELECT *
FROM alunos
WHERE uf = 'RJ'
ORDER BY data_cadastro DESC;
```

### Ordenar por carga horária, da maior para a menor

```sql
SELECT *
FROM cursos
ORDER BY carga_horaria_horas DESC;
```

Você também pode ordenar por mais de uma coluna:

```sql
SELECT nome_curso, instrutor, preco
FROM cursos
ORDER BY instrutor ASC, preco DESC;
```

Primeiro ordena pelo instrutor; dentro de cada instrutor, ordena pelo preço do maior para o menor.

## 6. Filtros e operadores

### 6.1 `WHERE`: filtrar linhas

`WHERE` retorna somente as linhas que satisfazem uma condição.

```sql
SELECT *
FROM alunos
WHERE uf = 'RJ';
```

Filtrar registros ativos, supondo que `ativo` seja uma coluna numérica ou booleana compatível com o banco:

```sql
SELECT *
FROM alunos
WHERE ativo = 1;
```

> **Atenção à tipagem:** textos geralmente usam aspas simples (`'RJ'`); números normalmente não usam aspas (`10`). Booleanos e datas podem variar conforme o banco de dados e o tipo da coluna.

### 6.2 Operadores de comparação

| Operador | Significado |
|---|---|
| `=` | Igual a |
| `<>` | Diferente de |
| `>` | Maior que |
| `<` | Menor que |
| `>=` | Maior ou igual a |
| `<=` | Menor ou igual a |

Exemplo:

```sql
SELECT *
FROM cursos
WHERE preco > 900;
```

### 6.3 `AND`, `OR` e `NOT`

- `AND` (**E**): todas as condições devem ser verdadeiras.
- `OR` (**OU**): pelo menos uma condição deve ser verdadeira.
- `NOT` (**NÃO**): nega uma condição.

**AND — todas as condições:**

```sql
SELECT *
FROM matriculas
WHERE status = 'Ativa'
  AND canal = 'TikTok';
```

Retorna matrículas que estejam ativas **e** tenham vindo do TikTok.

**OR — qualquer uma das condições:**

```sql
SELECT *
FROM matriculas
WHERE status = 'Ativa'
   OR canal = 'TikTok'
   OR curso_id = 10;
```

Retorna linhas que atendam a pelo menos uma das condições.

**NOT — negar uma condição:**

```sql
SELECT *
FROM matriculas
WHERE NOT (status = 'Ativa')
  AND NOT (canal = 'TikTok');
```

Retorna linhas cujo status não seja `Ativa` e cujo canal não seja `TikTok`. Valores `NULL` exigem cuidado: comparações comuns com `NULL` não resultam em verdadeiro ou falso, mas em desconhecido.

> **Pegadinha:** use parênteses quando combinar `AND` e `OR` e quiser deixar a lógica explícita.

### 6.4 `BETWEEN`: intervalo de valores

`BETWEEN` verifica se um valor está dentro de um intervalo, incluindo os limites.

```sql
SELECT *
FROM acessos
WHERE dispositivo = 'Celular'
  AND data_acesso BETWEEN '2025-12-24' AND '2025-12-31';
```

**Importante para datas com horário:** se `data_acesso` for um campo de data e hora, o limite final pode representar a meia-noite do dia 31. Para incluir o dia inteiro de 31/12, uma alternativa comum é usar um intervalo semiaberto, adaptando a sintaxe ao banco:

```sql
SELECT *
FROM acessos
WHERE dispositivo = 'Celular'
  AND data_acesso >= '2025-12-24'
  AND data_acesso <  '2026-01-01';
```

### 6.5 `IN` e `NOT IN`: lista de valores

- `IN`: valor pertence à lista.
- `NOT IN`: valor não pertence à lista.

```sql
SELECT *
FROM acessos
WHERE dispositivo IN ('Celular', 'Tablet');
```

```sql
SELECT *
FROM acessos
WHERE dispositivo NOT IN ('Celular', 'Tablet');
```

> **Atenção:** `NULL` dentro da lista de um `NOT IN` pode alterar o resultado de forma inesperada. Se a lista vier de uma consulta, confira se ela pode retornar `NULL`.

### 6.6 `LIKE`: buscar padrões de texto

`LIKE` permite localizar textos que correspondam a um padrão. O caractere `%` representa zero ou mais caracteres.

| Padrão | O que procura |
|---|---|
| `'Power BI%'` | Texto que começa com `Power BI` |
| `'%Power BI'` | Texto que termina com `Power BI` |
| `'%Power BI%'` | Texto que contém `Power BI` em qualquer posição |
| `'_ower%'` | Um caractere qualquer antes de `ower`, seguido de qualquer sequência |

Exemplo:

```sql
SELECT *
FROM cursos
WHERE nome_curso LIKE '%Power BI%';
```

**Observação:** a diferenciação entre maiúsculas e minúsculas depende do banco de dados e da collation/configuração usada.

### 6.7 `NULL`: valores ausentes ou desconhecidos

`NULL` não é a mesma coisa que zero, texto vazio ou a palavra `'NULL'`. Representa um valor ausente ou desconhecido.

**Encontrar instrutores sem valor informado:**

```sql
SELECT *
FROM cursos
WHERE instrutor IS NULL;
```

**Encontrar instrutores com valor informado:**

```sql
SELECT *
FROM cursos
WHERE instrutor IS NOT NULL;
```

> **Pegadinha de prova:** não use `= NULL` nem `<> NULL`. Use `IS NULL` ou `IS NOT NULL`.

### 6.8 Combinar filtros e ordenação

```sql
SELECT *
FROM cursos
WHERE instrutor IS NOT NULL
  AND preco > 900
ORDER BY carga_horaria_horas DESC;
```

A consulta:
1. seleciona os registros de `cursos`;
2. mantém cursos com instrutor informado;
3. mantém cursos com preço acima de 900;
4. ordena pela carga horária, da maior para a menor.

## 7. Funções de agregação

Funções de agregação resumem vários registros em um resultado, como soma, média, maior valor, menor valor e contagem.

| Função | Para que serve |
|---|---|
| `SUM()` | Soma valores |
| `MAX()` | Retorna o maior valor |
| `MIN()` | Retorna o menor valor |
| `AVG()` | Calcula a média |
| `COUNT(*)` | Conta linhas |
| `COUNT(coluna)` | Conta valores não `NULL` da coluna |
| `COUNT(DISTINCT coluna)` | Conta valores distintos não `NULL` da coluna |

### 7.1 `SUM`: soma

Somar o valor dos pedidos de 2026:

```sql
SELECT SUM(valor_total) AS total_pedidos
FROM pedidos
WHERE data_pedido >= '2026-01-01'
  AND data_pedido <  '2027-01-01';
```

### 7.2 `MAX`: maior valor

```sql
SELECT MAX(valor_total) AS maior_pedido
FROM pedidos;
```

### 7.3 `MIN`: menor valor

```sql
SELECT MIN(valor_total) AS menor_pedido
FROM pedidos;
```

### 7.4 `AVG`: média

```sql
SELECT AVG(valor_total) AS media_pedidos
FROM pedidos;
```

`AVG()` normalmente ignora valores `NULL`. A média é calculada sobre os valores não nulos.

### 7.5 `COUNT`: contar registros ou valores preenchidos

Contar todas as linhas:

```sql
SELECT COUNT(*) AS total_linhas
FROM pedidos;
```

Contar apenas pedidos com `valor_total` preenchido:

```sql
SELECT COUNT(valor_total) AS total_valores_preenchidos
FROM pedidos;
```

Esses resultados podem ser diferentes se `valor_total` contiver `NULL`.

### 7.6 `COUNT(DISTINCT ...)`: contar valores únicos

```sql
SELECT COUNT(DISTINCT valor_total) AS total_valores_distintos
FROM pedidos;
```

Conta quantos valores diferentes de `valor_total` existem, ignorando `NULL`.

> **Atenção ao nome:** `DISTINCT` não é uma função de agregação por si só; é uma palavra-chave que pode ser usada dentro de `COUNT()` para contar valores únicos.

### 7.7 Usar várias agregações na mesma consulta

```sql
SELECT
    MAX(data_pedido) AS pedido_mais_recente,
    MIN(data_pedido) AS pedido_mais_antigo,
    SUM(valor_total) AS total_vendido
FROM pedidos;
```

A consulta retorna uma linha com:
- a data mais recente;
- a data mais antiga;
- a soma dos valores dos pedidos.

As funções `MAX()` e `MIN()` também podem trabalhar com datas, não apenas números.

## 8. Exemplos combinados

### Exemplo 1 — Cursos caros com instrutor informado

```sql
SELECT
    nome_curso,
    instrutor,
    preco,
    carga_horaria_horas
FROM cursos
WHERE instrutor IS NOT NULL
  AND preco > 900
ORDER BY preco DESC;
```

### Exemplo 2 — Quantidade de matrículas ativas

```sql
SELECT COUNT(*) AS matriculas_ativas
FROM matriculas
WHERE status = 'Ativa';
```

### Exemplo 3 — Total de pedidos em um período

```sql
SELECT
    COUNT(*) AS quantidade_pedidos,
    SUM(valor_total) AS valor_total,
    AVG(valor_total) AS valor_medio,
    MAX(valor_total) AS maior_pedido,
    MIN(valor_total) AS menor_pedido
FROM pedidos
WHERE data_pedido >= '2026-01-01'
  AND data_pedido <  '2027-01-01';
```

## 9. Resumo para revisão

| Quero... | Uso |
|---|---|
| Todas as colunas | `SELECT * FROM tabela` |
| Colunas específicas | `SELECT coluna1, coluna2 FROM tabela` |
| Remover linhas duplicadas do resultado | `SELECT DISTINCT coluna FROM tabela` |
| Dar um nome temporário | `AS alias` |
| Filtrar linhas | `WHERE condição` |
| Exigir todas as condições | `AND` |
| Aceitar qualquer condição | `OR` |
| Negar uma condição | `NOT` |
| Filtrar um intervalo inclusivo | `BETWEEN valor1 AND valor2` |
| Filtrar por uma lista | `IN (...)` / `NOT IN (...)` |
| Buscar parte de um texto | `LIKE '%texto%'` |
| Encontrar valor ausente | `IS NULL` |
| Encontrar valor informado | `IS NOT NULL` |
| Ordenar crescente/decrescente | `ORDER BY coluna ASC/DESC` |
| Somar | `SUM(coluna)` |
| Encontrar maior/menor | `MAX(coluna)` / `MIN(coluna)` |
| Calcular média | `AVG(coluna)` |
| Contar linhas | `COUNT(*)` |
| Contar valores não nulos | `COUNT(coluna)` |
| Contar valores únicos | `COUNT(DISTINCT coluna)` |

## 10. Erros comuns e boas práticas

- Escrever `DISTINCT` corretamente — não `DISTICT`.
- Colocar texto entre aspas simples: `status = 'Ativa'`.
- Não colocar aspas em números quando a coluna é numérica: `curso_id = 10`.
- Usar `IS NULL` em vez de `= NULL`.
- Lembrar que `BETWEEN` inclui os dois limites.
- Conferir o efeito de datas com horário ao usar um limite final.
- Usar parênteses em condições complexas com `AND` e `OR`.
- Selecionar somente as colunas necessárias em consultas profissionais.
- Dar aliases claros, como `total_pedidos` ou `media_valor`.
- Conferir a compatibilidade da sintaxe com o SGBD utilizado (PostgreSQL, MySQL, SQL Server, SQLite etc.).

---

**Dica de estudo:** pratique cada comando em uma tabela pequena. Depois, combine `SELECT`, `WHERE`, `ORDER BY` e agregações para responder perguntas reais, como “quantas matrículas estão ativas?” ou “qual foi o valor total dos pedidos em 2026?”.
