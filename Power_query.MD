# 📊 Curso - EducaDados

# Power Query Básico

## 📁 Base de Dados

O curso utiliza três tabelas principais:

### Clientes
Contém informações cadastrais dos clientes, essenciais para:
- Análise de perfil;
- Segmentação de clientes;
- Comportamento de compra.

### Produtos
Reúne informações sobre os produtos, como:
- Categorias;
- Preços;
- Características.

Esses dados são utilizados para análises de portfólio e desempenho dos produtos.

### Vendas
Contém o histórico de transações realizadas, permitindo análises como:
- Faturamento;
- Volume de vendas;
- Indicadores de performance (KPIs).

---

# 🧹 Limpeza Básica de Dados

## Converter valores monetários

Na tabela **Vendas**, alguns valores monetários podem estar como números inteiros.

**Passo:**

- Selecione a coluna.
- Altere o tipo de dado para **Número Decimal**.

> Isso garante que valores financeiros sejam tratados corretamente.

---

## Padronizar nomes

Transformar todos os nomes em letras maiúsculas.

**Passo:**

```
Selecionar coluna
→ Transformar
→ Formatar
→ MAIÚSCULAS
```

---

## Remover espaços extras

Eliminar espaços no início e no final dos textos.

**Passo:**

```
Selecionar coluna
→ Transformar
→ Formatar
→ Cortar (Trim)
```

---

## Verificar erros na coluna

Para identificar rapidamente valores inválidos.

**Passo:**

```
Exibição
→ Qualidade da Coluna
```

A qualidade mostrará:
- ✅ Válidos
- ⚠️ Vazios
- ❌ Erros

---

# 📅 Tratamento de Datas

Quando a coluna de datas possui formatos diferentes.

Exemplo:

```
01.01.2025
01-01-2025
01/01/2025
```

### Padronização

Substituir:

```
"." → "/"
"-" → "/"
```

Depois tratar manualmente os erros restantes utilizando **Substituir Valores**.

---

# ✂️ Dividir e Mesclar Colunas

## Dividir Colunas

Exemplo:

```
Brasília/DF
```

Resultado:

| Cidade | Estado |
|---------|---------|
| Brasília | DF |

### Passo

```
Página Inicial
→ Dividir Coluna
→ Por Delimitador
```

---

## Mesclar Colunas

Juntar duas ou mais colunas em uma única.

### Passo

```
Selecionar as colunas
→ Transformar
→ Mesclar Colunas
```

---

# 🔄 Pivot e Unpivot

## Pivot

Transforma **linhas em colunas**.

Exemplo:

| Produto | Mês | Valor |
|----------|-----|--------|
| A | Janeiro | 100 |
| A | Fevereiro | 120 |

↓

| Produto | Janeiro | Fevereiro |
|----------|----------|-----------|
| A | 100 | 120 |

---

## Unpivot

Transforma **colunas em linhas**.

Exemplo:

| Produto | Janeiro | Fevereiro |
|----------|----------|-----------|
| A | 100 | 120 |

↓

| Produto | Mês | Valor |
|----------|------|--------|
| A | Janeiro | 100 |
| A | Fevereiro | 120 |

### Passo

```
Selecionar as colunas
→ Botão direito
→ Transformar Colunas em Linhas (Unpivot)
```

---

# 🔗 Merge e Append

## Merge (Mesclar Consultas)

Utilizado para combinar tabelas através de uma chave comum.

### Exemplo

Adicionar o **Nome do Cliente** na tabela **Vendas**, utilizando o **ID Cliente**.

> Embora o relacionamento no modelo seja a prática mais recomendada, o Merge pode ser útil durante o tratamento dos dados.

### Passo

```
Selecionar coluna ID Cliente
→ Página Inicial
→ Mesclar Consultas
→ Selecionar tabela Clientes
→ Selecionar ID Cliente
```

Após expandir a tabela, selecionar apenas:

```
Nome do Cliente
```

---

## Append (Acrescentar Consultas)

Utilizado para unir tabelas **verticalmente**, adicionando linhas.

### Regras

As tabelas precisam possuir:

- Mesmo nome das colunas;
- Mesmo tipo de dados;
- Estrutura semelhante.

### Passo

```
Página Inicial
→ Acrescentar Consultas
```

---

# 🧮 Colunas Condicionais

Criar classificações automaticamente com base em regras.

## Exemplo

Classificar o **Valor Total**.

### Regras

```
Valor Total >= 7000
→ Alto
```

```
Valor Total > 3000
→ Médio
```

```
Caso contrário
→ Baixo
```

### Passo

```
Adicionar Coluna
→ Coluna Condicional
```

### Boa prática

Sempre criar as condições do **maior valor para o menor**, evitando classificações incorretas.

---

# ✅ Resumo dos Principais Recursos

| Recurso | Finalidade |
|----------|------------|
| Alterar Tipo de Dados | Corrigir tipos (texto, número, decimal, data etc.) |
| MAIÚSCULAS | Padronizar textos |
| Cortar (Trim) | Remover espaços extras |
| Qualidade da Coluna | Identificar erros e valores vazios |
| Substituir Valores | Corrigir dados rapidamente |
| Dividir Colunas | Separar informações por delimitador |
| Mesclar Colunas | Juntar informações em uma única coluna |
| Pivot | Transformar linhas em colunas |
| Unpivot | Transformar colunas em linhas |
| Merge | Relacionar dados entre tabelas |
| Append | Unir tabelas verticalmente |
| Coluna Condicional | Criar classificações automáticas |

---

# 💡 Boas Práticas no Power Query

- Padronize os tipos de dados logo no início.
- Remova erros antes de iniciar as análises.
- Utilize nomes claros para tabelas e colunas.
- Evite tratamentos manuais repetitivos.
- Prefira realizar toda a limpeza no Power Query antes de carregar os dados para o Power BI.
- Sempre valide o resultado após cada transformação.
