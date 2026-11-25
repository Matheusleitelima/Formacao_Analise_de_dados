# 📘 Guia de DAX (Power BI)

Este documento reúne os principais conceitos, funções e exemplos de DAX utilizados no Power BI, organizados do básico ao intermediário.

---

# DAX Básico

## O que é DAX?

**DAX (Data Analysis Expressions)** é a linguagem de fórmulas do Power BI utilizada para criar:

- Medidas
- Colunas Calculadas
- Tabelas Calculadas
- KPIs
- Indicadores

---

# Coluna Calculada x Medida

## Coluna Calculada

Características:

- Calculada linha por linha
- Fica armazenada no modelo
- Usa **Contexto de Linha**
- É recalculada durante a atualização dos dados

Exemplo:

```DAX
Valor Imposto =
fVendas[Valor_Total] * 0.10
```

---

## Medida

Características:

- Calculada somente quando utilizada em um visual
- Não fica armazenada
- Usa **Contexto de Filtro**
- Muito mais leve que colunas calculadas

Exemplo:

```DAX
Valor Líquido =
SUM(fVendas[Valor_Liquido])
```

---

# Criando uma Tabela de Medidas

Organize todas as medidas em uma única tabela.

**Página Inicial → Inserir Dados**

Crie uma tabela chamada:

```
Medidas
```

Depois salve todas as medidas nela.

---

# Funções de Agregação

## SUM

Soma uma coluna.

```DAX
Valor Líquido =
SUM(fVendas[Valor_Liquido])
```

---

## MIN

Menor valor.

```DAX
Menor Venda =
MIN(fVendas[Valor_Liquido])
```

---

## MAX

Maior valor.

```DAX
Maior Venda =
MAX(fVendas[Valor_Liquido])
```

---

## AVERAGE

Calcula a média.

```DAX
Média Quantidade =
AVERAGE(fVendas[Quantidade])
```

---

# Contagens

## COUNT

Conta apenas números.

```DAX
Total Vendas =
COUNT(fVendas[ID_Venda])
```

---

## COUNTA

Conta valores não vazios.

```DAX
Clientes =
COUNTA(dClientes[Nome])
```

---

## DISTINCTCOUNT

Conta valores distintos.

```DAX
Clientes Distintos =
DISTINCTCOUNT(dClientes[Nome])
```

---

## COUNTROWS

Conta linhas da tabela.

```DAX
Total Vendas =
COUNTROWS(fVendas)
```

---

# DIVIDE

Evita erro de divisão por zero.

Forma comum:

```DAX
Ticket Médio =
[Vendas Valor Líquido] /
[Vendas Quantidade]
```

Forma recomendada:

```DAX
Ticket Médio =
DIVIDE(
    [Vendas Valor Líquido],
    [Vendas Quantidade],
    0
)
```

---

# KPIs

KPI significa:

**Key Performance Indicator**

São indicadores utilizados para tomada de decisão.

Exemplos:

- Receita
- Ticket Médio
- Quantidade Vendida
- Lucro
- Margem

---

# BLANK()

Quando não existe valor para retornar.

Exemplo:

```DAX
Vendas 2026 =
CALCULATE(
    [Valor Líquido],
    dCalendario[Ano] = 2026
) + 0
```

O `+0` transforma o **Blank** em **0**.

---

# IF

Estrutura:

```DAX
IF(condição, verdadeiro, falso)
```

Exemplo:

```DAX
Classificação =
IF(
    [Valor Líquido] > 50000,
    "Alta",
    "Baixa"
)
```

---

# DAX Intermediário

# Contexto de Linha

Exemplo:

Imposto de 10%.

```DAX
Valor Imposto =
fVendas[Valor_Total] * 0.10
```

Cada linha calcula seu próprio valor.

---

# CALCULATE()

É a função mais importante do DAX.

Mais de 90% das medidas utilizam CALCULATE.

Permite alterar o contexto de filtro.

Sintaxe:

```DAX
CALCULATE(
    expressão,
    filtros
)
```

---

# FILTER()

Permite criar filtros personalizados.

Exemplo:

```DAX
Vendas Altas =
CALCULATE(
    [Valor Líquido],
    FILTER(
        fVendas,
        fVendas[Valor_Liquido] > 5000
    )
)
```

---

Também pode ser escrito assim:

```DAX
Vendas Online =
CALCULATE(
    [Valor Líquido],
    fVendas[Canal_Venda] = "Online"
)
```

---

Outro exemplo:

```DAX
Vendas Ceará =
CALCULATE(
    [Valor Líquido],
    dClientes[Estado] = "CE"
)
```

---

# SUMX

Executa cálculo linha a linha e soma os resultados.

```DAX
Receita =
SUMX(
    fVendas,
    fVendas[Quantidade] *
    fVendas[Valor_Unitario]
)
```

Ideal quando não existe uma coluna de receita.

---

# AVERAGEX

Calcula média de uma expressão.

```DAX
Ticket Médio =
AVERAGEX(
    fVendas,
    fVendas[Quantidade] *
    fVendas[Valor_Unitario]
)
```

---

# COUNTX

Conta resultados de uma expressão.

```DAX
Vendas com Valor =
COUNTX(
    fVendas,
    IF(
        fVendas[Canal_Venda] = "Online",
        1
    )
)
```

---

# MAXX

Maior resultado de um cálculo.

```DAX
Maior Receita =
MAXX(
    fVendas,
    fVendas[Quantidade] *
    fVendas[Valor_Unitario]
)
```

---

# MINX

Menor resultado de um cálculo.

```DAX
Menor Receita =
MINX(
    fVendas,
    fVendas[Quantidade] *
    fVendas[Valor_Unitario]
)
```

---

# ALL()

Remove todos os filtros.

```DAX
Receita Geral =
CALCULATE(
    [Receita],
    ALL(fVendas)
)
```

Muito utilizado para calcular percentuais do total.

---

# ALLEXCEPT()

Remove todos os filtros, exceto os informados.

```DAX
Receita Estado =
CALCULATE(
    [Receita],
    ALLEXCEPT(
        dClientes,
        dClientes[Estado]
    )
)
```

---

# ALLSELECTED()

Respeita:

- Segmentações
- Filtros da página
- Filtros do relatório

Ignora apenas o contexto do visual.

```DAX
% Total =
DIVIDE(
    SUM(Vendas[Valor]),
    CALCULATE(
        SUM(Vendas[Valor]),
        ALLSELECTED(
            Vendas[Produto]
        )
    )
)
```

---

# SAMEPERIODLASTYEAR()

Compara com o mesmo período do ano anterior.

```DAX
Vendas Ano Anterior =
CALCULATE(
    [Valor Líquido],
    SAMEPERIODLASTYEAR(
        dCalendario[Date]
    )
)
```

Necessita de uma tabela calendário.

---

# DATEADD()

Desloca datas.

Exemplo: mês anterior.

```DAX
Vendas Mês Anterior =
CALCULATE(
    [Quantidade],
    DATEADD(
        dCalendario[Date],
        -1,
        MONTH
    )
)
```

50 dias anteriores:

```DAX
DATEADD(
    dCalendario[Date],
    -50,
    DAY
)
```

---

# TOTALYTD()

Acumulado do ano.

```DAX
YTD Valor =
TOTALYTD(
    [Valor Líquido],
    dCalendario[Date]
)
```

---

# TOTALMTD()

Acumulado do mês.

```DAX
MTD Valor =
TOTALMTD(
    [Valor Líquido],
    dCalendario[Date]
)
```

---

# Acumulado Total

Não reinicia ao trocar período.

```DAX
Acumulado =
CALCULATE(
    [Valor Líquido],
    FILTER(
        ALL(dCalendario),
        dCalendario[Date] <= MAX(dCalendario[Date])
    )
)
```

---

# YoY (Year over Year)

Comparação anual.

```DAX
Crescimento YoY =
DIVIDE(
    [Valor Líquido] -
    [Vendas Ano Anterior],
    [Vendas Ano Anterior]
)
```

---

# MoM (Month over Month)

Comparação mensal.

```DAX
Crescimento MoM =
DIVIDE(
    [Valor Líquido] -
    [Vendas Mês Anterior],
    [Vendas Mês Anterior]
)
```

---

# SELECTEDVALUE()

Captura o valor selecionado pelo usuário.

```DAX
Produto Selecionado =
SELECTEDVALUE(
    dProdutos[Nome_Produto],
    "Outros Cursos"
)
```

---

# SWITCH()

Substitui vários IF.

```DAX
Categoria =
SWITCH(
    SELECTEDVALUE(dProdutos[Categoria]),
    "Cursos","Presencial",
    "Mentoria","Individual",
    "Formação","Presencial",
    "Workshop","Online",
    "Outros"
)
```

---

# SWITCH(TRUE())

Ideal para intervalos.

```DAX
Classificação =
SWITCH(
    TRUE(),
    [Valor Líquido] < 5000,"Baixa",
    [Valor Líquido] < 50000,"Média",
    [Valor Líquido] < 100000,"Alta",
    "Muito Alta"
)
```

---

# Títulos Dinâmicos

```DAX
Título =
"Vendas da categoria: "
&
SELECTEDVALUE(
    dProdutos[Categoria],
    "Todas as Categorias"
)
```

Depois utilize a medida no título do visual.

---

# FORMAT()

Formata números.

```DAX
FORMAT(
    [Valor Líquido]/1000000,
    "0.0"
)
&
" Mi"
```

---

# VAR / RETURN

Permite criar variáveis.

```DAX
Lucro =
VAR Receita =
    [Valor Líquido]

VAR Custo =
    SUM(fVendas[Custo_Total])

RETURN
Receita - Custo
```

Não existe limite prático para a quantidade de variáveis.

---

# Exercícios

## Questão 01

```DAX
Total Quantidade =
SUM(Vendas[Quantidade])
```

---

## Questão 02

```DAX
Maior Salário =
MAX(Funcionarios[Salário])
```

---

## Questão 03

```DAX
Total Funcionários =
COUNTROWS(Funcionarios)
```

---

## Questão 04

```DAX
Ticket Médio =
AVERAGE(Vendas[Valor])
```

---

## Questão 05

```DAX
Produtos Distintos =
DISTINCTCOUNT(Vendas[Produto])
```

---

## Questão 06

### Objetivo

Criar:

- Idade
- Faixa Etária
- Ordem da Faixa

A solução pode ser feita pelo **Power Query** ou utilizando DAX.

Exemplo para ordenação:

```DAX
Ordem Faixa =
SWITCH(
    TRUE(),
    Funcionarios[Idade] <= 20,1,
    Funcionarios[Idade] <= 29,2,
    Funcionarios[Idade] <= 39,3,
    Funcionarios[Idade] <= 49,4,
    Funcionarios[Idade] <= 59,5,
    6
)
```

Depois:

**Modelagem → Classificar por Coluna**

Selecionar:

```
Faixa Etária
```

Classificar por:

```
Ordem Faixa
```

---

## Questão 07

```DAX
Total Vendas =
COUNTROWS(Vendas)
```

---

## Questão 08

```DAX
Vendas Qtde >= 30 =
CALCULATE(
    COUNTROWS(Vendas),
    Vendas[Quantidade] >= 30
)
```

---

## Questão 09

```DAX
Receita Total =
SUMX(
    Vendas,
    Vendas[Valor] *
    Vendas[Quantidade]
)
```

---

## Questão 10

```DAX
% Funcionários Ativos =
DIVIDE(
    CALCULATE(
        COUNTROWS(Funcionarios),
        Funcionarios[Status] = "Ativo"
    ),
    COUNTROWS(Funcionarios),
    0
)
```

---

## Questão 11

```DAX
Média Salário Ativos =
CALCULATE(
    AVERAGE(Funcionarios[Salário]),
    Funcionarios[Status] = "Ativo"
)
```

---

## Questão 12

```DAX
Classificação Receita =
SWITCH(
    TRUE(),
    [Receita Total] > 1000000, "Alta",
    [Receita Total] > 500000, "Média",
    "Baixa"
)
```

---

## Questão 13

```DAX
Receita Total Geral =
CALCULATE(
    [Receita Total],
    ALL(Vendas)
)
```

---

# Resumo das Funções Mais Utilizadas

| Categoria | Funções |
|-----------|----------|
| Agregação | `SUM`, `AVERAGE`, `MIN`, `MAX` |
| Contagem | `COUNT`, `COUNTA`, `COUNTROWS`, `DISTINCTCOUNT` |
| Iteradoras | `SUMX`, `AVERAGEX`, `COUNTX`, `MAXX`, `MINX` |
| Contexto | `CALCULATE`, `FILTER` |
| Filtros | `ALL`, `ALLEXCEPT`, `ALLSELECTED` |
| Tempo | `DATEADD`, `SAMEPERIODLASTYEAR`, `TOTALYTD`, `TOTALMTD` |
| Lógicas | `IF`, `SWITCH`, `SWITCH(TRUE())` |
| Utilidades | `DIVIDE`, `FORMAT`, `SELECTEDVALUE`, `VAR`, `RETURN` |

---
