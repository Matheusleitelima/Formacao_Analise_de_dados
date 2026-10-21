# EducaDados — DataViz & Storytelling

Anotações de estudo sobre visualização de dados, formatação condicional, segmentações, interações, botões, indicadores, tooltips, relacionamentos no Power BI e criação de layouts no Figma.

## 1. Eixos X, Y e Z

- **Eixo X:** geralmente representa uma dimensão categórica ou temporal — responde a **“o quê?”** ou **“quando?”**.
- **Eixo Y:** geralmente representa uma medida — responde a **“quanto?”**.
- **Eixo Z:** pode representar uma terceira dimensão em visualizações específicas, dependendo do tipo de gráfico. Em dashboards comuns, nem todo gráfico utiliza esse eixo.

## 2. Escolha do gráfico

Escolha o tipo de visualização de acordo com a pergunta que os dados precisam responder.

| Objetivo | Visualização indicada | Exemplo |
|---|---|---|
| Comparar categorias ou produtos | Gráfico de barras/colunas | Vendas por produto |
| Acompanhar uma evolução ao longo do tempo | Gráfico de linhas | Faturamento por mês |
| Mostrar partes de um total | Gráfico de pizza/rosca | Participação percentual por categoria |

**Regra prática:**
- **Comparação:** barras ou colunas.
- **Evolução temporal:** linhas.
- **Proporção de um total:** pizza ou rosca, preferencialmente com poucas categorias.

## 3. Formatação condicional

A formatação condicional permite alterar cores ou estilos de acordo com os valores apresentados.

### Exemplo
Destacar valores altos com uma cor mais forte e valores baixos com uma cor mais clara.

### Passos gerais no Power BI
1. Selecione o visual ou campo compatível.
2. Abra as opções de formatação do campo/visual.
3. Localize **Formatação condicional** ou o botão **`fx`**.
4. Escolha a regra, escala de cores ou medida que determinará a formatação.
5. Configure as cores e confirme.

> A localização exata das opções pode variar conforme o visual e a versão do Power BI.

## 4. Segmentações de dados (Slicers) e interações

As **segmentações de dados** permitem filtrar o relatório, por exemplo, por período, cliente, região ou produto.

### Editar as interações entre visuais
Quando um filtro ou gráfico afeta outros visuais de uma forma indesejada:

1. Selecione o visual que inicia a interação.
2. Acesse **Formato > Editar interações**.
3. Nos demais visuais, escolha o comportamento disponível, como filtrar, realçar ou não interagir.
4. Saia do modo de edição de interações.

### Valores repetidos ou duplicados no dashboard
Se nomes, totais ou valores aparecem repetidos de forma inesperada, verifique o **modelo de dados e os relacionamentos**.

Confira:
- Se as chaves usadas nos relacionamentos correspondem.
- Se a tabela de dimensão possui valores únicos na coluna usada no lado “um”.
- Se a cardinalidade e a direção de filtragem estão corretas.
- Se a medida está calculando o resultado esperado.

Valores repetidos nem sempre significam que o relacionamento está errado: eles também podem ser legítimos dependendo do nível de detalhe da tabela e do visual.

## 5. Botões, formas e ações

Botões e formas podem tornar o relatório mais interativo, permitindo navegar entre páginas ou executar outras ações disponíveis no Power BI.

### Criar um botão com ação
1. Acesse **Inserir > Botões** ou insira uma forma.
2. Selecione o botão.
3. No painel de formatação, habilite **Ação**.
4. Escolha o tipo de ação.
5. Configure o destino ou as opções solicitadas.

### Navegação entre páginas
1. Insira ou selecione um botão.
2. Habilite **Ação** e escolha **Navegação de página**.
3. Selecione a página de destino.
4. Para testar a navegação no modo de edição, use **Ctrl + clique** no botão.

## 6. Indicadores (Bookmarks)

Os **indicadores** salvam um estado específico do relatório, podendo registrar, por exemplo, a exibição de visuais e filtros. Eles podem ser associados a botões para alternar entre visualizações.

### Exemplo: alternar entre dois conjuntos de filtros
1. Configure o relatório na primeira visualização desejada.
2. Abra **Exibir > Indicadores** e adicione um indicador.
3. Configure a segunda visualização.
4. Adicione outro indicador.
5. Insira botões e associe cada um ao indicador correspondente por meio da opção **Ação**.
6. Teste os botões e confirme se os indicadores estão salvando o estado correto.

**Atenção:** dependendo do objetivo, revise as opções do indicador para definir se ele deve armazenar dados/filtros, exibição e outros estados. Atualize o indicador depois de alterar a configuração.

## 7. Dicas de ferramenta (Tooltips)

Tooltips exibem informações adicionais quando a pessoa passa o cursor sobre um ponto de um gráfico, como uma barra ou um ponto de uma linha.

### Criar uma página de tooltip
1. Crie uma nova página no relatório.
2. Selecione a página e abra **Formatar página**.
3. Em **Informações da página**, habilite **Permitir uso como dica de ferramenta**.
4. Configure o tamanho da página para o tipo **Dica de ferramenta**, quando essa opção estiver disponível.
5. Adicione uma tabela, cartões ou outros visuais com as informações que deseja exibir.

### Associar a página de tooltip a um gráfico
1. Selecione o gráfico que exibirá a dica.
2. Abra o painel de formatação do visual.
3. Em **Geral > Dicas de ferramenta**, habilite a opção.
4. Selecione o tipo **Página de relatório**.
5. Escolha a página de tooltip criada, por exemplo, `Tooltip - Clientes`.

Ao passar o cursor sobre uma barra ou outro elemento compatível, o Power BI poderá exibir os dados da página de tooltip considerando o contexto daquele elemento.

## 8. Relacionamentos e função `RELATED`

Considere duas tabelas:

- `FatoVendas`: contém registros de vendas e o `id_cliente`.
- `DClientes`: contém o cadastro dos clientes, incluindo `id_cliente` e `nome`.

Se for necessário trazer o nome do cliente para a tabela de vendas, é possível criar uma **coluna calculada** com DAX:

```dax
Nome Cliente = RELATED(DClientes[nome])
```

### Como funciona
A função `RELATED` recupera um valor de uma tabela relacionada, normalmente do lado “um” de um relacionamento muitos-para-um, usando o contexto da linha atual.

### Antes de usar
- Confirme que existe um relacionamento ativo entre as tabelas.
- Verifique se as colunas-chave possuem tipos de dados compatíveis.
- Garanta que a relação entre `FatoVendas` e `DClientes` está modelada corretamente.

> Use uma coluna calculada quando precisar armazenar o atributo em cada linha da tabela. Para análises agregadas, uma medida pode ser mais apropriada.

## 9. Criação de layout no Figma

O Figma pode ser usado para planejar o layout de um dashboard antes de montá-lo no Power BI.

### Plugin Split Shape
O plugin **Split Shape** pode dividir uma forma em partes proporcionais, ajudando a criar uma grade para organizar os elementos.

### Passos anotados
1. Abra o arquivo no Figma.
2. Abra a área de plugins — por exemplo, pelo menu de contexto ou pela opção de plugins.
3. Procure por **Split Shape** e execute o plugin.
4. Selecione o retângulo que deseja dividir.
5. Configure a divisão desejada; exemplo anotado: **3 colunas e 1 linha**.
6. Use a grade resultante para organizar cartões, gráficos e outros componentes do dashboard.

A interface e a disponibilidade dos plugins podem mudar. Confira as opções atuais no próprio Figma.

## 10. Checklist para construir um dashboard

- [ ] Definir a pergunta que o dashboard deve responder.
- [ ] Escolher o gráfico adequado para cada objetivo.
- [ ] Conferir os relacionamentos e a granularidade das tabelas.
- [ ] Configurar cores e formatação condicional com significado consistente.
- [ ] Adicionar segmentações úteis e revisar as interações.
- [ ] Configurar botões e navegação entre páginas.
- [ ] Usar indicadores quando for necessário alternar estados de exibição.
- [ ] Criar tooltips para mostrar detalhes sem sobrecarregar os gráficos.
- [ ] Planejar o layout com uma grade e manter alinhamentos consistentes.
- [ ] Testar filtros, botões, indicadores e dicas de ferramenta antes de publicar.

---

**Curso:** EducaDados — DataViz & Storytelling  
**Tema:** Visualização de dados, interatividade e design de dashboards  
**Ferramentas:** Power BI e Figma
