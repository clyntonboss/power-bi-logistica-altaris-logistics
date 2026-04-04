# Documentação das Medidas: Projeto Logistica — Altaris Logistics

Este documento lista todas as medidas criadas no modelo Power BI, suas regras de negócio, dependências e retornos esperados.

Lista de Medidas:
- [Devoluções](#medidas-de-devolucoes)
- [Entregas](#medidas-de-entregas)
- [Frete](#medidas-de-frete)
- [Lucro](#medidas-de-lucro)
- [Pedidos](#medidas-de-pedidos)

---

## Medidas de Devoluções - [Topo](#documentacao-das-medidas-projeto-logistica--altaris-logistics)
<br>

```DAX
devolucoes = 

-- Medida:
--      devolucoes
--
-- Descrição:
--      Calcula a quantidade total de pedidos que foram devolvidos, considerando o contexto de filtros aplicado no relatório.
--
-- Tabela origem:
--      fato_pedidos
--
-- Regra de negócio:
--      - Filtra os pedidos cuja coluna status_devolucao seja igual a "Sim".
--      - Soma a quantidade de pedidos filtrados.
--
-- Dependência:
--      Medida [quantidade_pedidos]
--      Coluna fato_pedidos[status_devolucao]
--
-- Retorno:
--      Número inteiro representando a quantidade total de pedidos devolvidos.
--
-- Observação:
--      COALESCE é utilizado para garantir que o retorno seja 0 quando não houver pedidos devolvidos.

VAR _Resultado =
    CALCULATE(
        [quantidade_pedidos],
        dimensao_devolucao[devolucao] = "Sim"
    )

RETURN
    COALESCE(
        _Resultado,
        0
    )
```
<br>

```DAX
devolucao_desistencia = 

-- Medida:
--      devolucao_desistencia
--
-- Descrição:
--      Retorna a quantidade de pedidos com motivo de devolução "Desistência", concatenando com o texto "Desistência: " para exibição direta no relatório.
--
-- Tabela origem:
--      fato_pedidos
--
-- Regra de negócio:
--      - Filtra os pedidos cuja coluna motivo_devolucao seja igual a "Desistência".
--      - Soma a quantidade de pedidos filtrados.
--      - Concatena o resultado com o texto "Desistência: " para apresentação.
--
-- Dependência:
--      Medida [quantidade_pedidos]
--      Coluna fato_pedidos[motivo_devolucao]
--
-- Retorno:
--      Texto (string) no formato "Desistência: X", onde X é a quantidade de pedidos.
--
-- Observação:
--      COALESCE é utilizado para garantir que o retorno seja 0 quando não houver pedidos.
--      CONCATENATE é implícito pelo uso do operador &.

VAR _Resultado =
    CALCULATE(
        [quantidade_pedidos],
        dimensao_motivo_devolucao[motivo_devolucao] = "Desistência"
    )

RETURN
    "Desistência: " &
    COALESCE(
        _Resultado,
        0
    )
```
<br>

```DAX
devolucao_mercadoria_errada = 

-- Medida:
--      devolucao_mercadoria_errada
--
-- Descrição:
--      Retorna a quantidade de pedidos com motivo de devolução "Mercadoria Errada", concatenando com o texto "Mercadoria Errada: " para exibição direta no relatório.
--
-- Tabela origem:
--      fato_pedidos
--
-- Regra de negócio:
--      - Filtra os pedidos cuja coluna motivo_devolucao seja igual a "Mercadoria Errada".
--      - Soma a quantidade de pedidos filtrados.
--      - Concatena o resultado com o texto "Mercadoria Errada: " para apresentação.
--
-- Dependência:
--      Medida [quantidade_pedidos]
--      Coluna fato_pedidos[motivo_devolucao]
--
-- Retorno:
--      Texto (string) no formato "Mercadoria Errada: X", onde X é a quantidade de pedidos.
--
-- Observação:
--      COALESCE é utilizado para garantir que o retorno seja 0 quando não houver pedidos.
--      CONCATENATE é implícito pelo uso do operador &.

VAR _Resultado =
    CALCULATE(
        [quantidade_pedidos],
        dimensao_motivo_devolucao[motivo_devolucao] = "Mercadoria Errada"
    )

RETURN
    "Mercadoria Errada: " &
    COALESCE(
        _Resultado,
        0
    )
```
<br>

```DAX
devolucao_nota_fiscal_erro = 

-- Medida:
--      devolucao_nota_fiscal_erro
--
-- Descrição:
--      Retorna a quantidade de pedidos com motivo de devolução "Nota Fiscal com Erro", concatenando com o texto "Nota Fiscal com Erro: " para exibição direta no relatório.
--
-- Tabela origem:
--      fato_pedidos
--
-- Regra de negócio:
--      - Filtra os pedidos cuja coluna motivo_devolucao seja igual a "Nota Fiscal com Erro".
--      - Soma a quantidade de pedidos filtrados.
--      - Concatena o resultado com o texto "Nota Fiscal com Erro: " para apresentação.
--
-- Dependência:
--      Medida [quantidade_pedidos]
--      Coluna fato_pedidos[motivo_devolucao]
--
-- Retorno:
--      Texto (string) no formato "Nota Fiscal com Erro: X", onde X é a quantidade de pedidos.
--
-- Observação:
--      COALESCE é utilizado para garantir que o retorno seja 0 quando não houver pedidos.
--      CONCATENATE é implícito pelo uso do operador &.

VAR _Resultado =
    CALCULATE(
        [quantidade_pedidos],
        dimensao_motivo_devolucao[motivo_devolucao] = "Nota Fiscal com Erro"
    )

RETURN
    "Nota Fiscal com Erro: " &
    COALESCE(
        _Resultado,
        0
    )
```

## Medidas de Entregas
<br>

```DAX
custo_entrega = 

-- Medida:
--      custo_entrega
--
-- Descrição:
--      Calcula o custo total de entrega de todos os pedidos considerando o contexto de filtros aplicado no relatório.
--
-- Tabela origem:
--      fato_pedidos
--
-- Regra de negócio:
--      Soma os valores da coluna custo_entrega da tabela de pedidos, representando o total de custos de entrega no contexto filtrado.
--
-- Dependência:
--      Coluna fato_pedidos[custo_entrega]
--
-- Retorno:
--      Número decimal representando o custo total de entrega.
--
-- Observação:
--      COALESCE é utilizado para evitar retorno BLANK() e garantir que o resultado seja 0 quando não houver dados.

VAR _Resultado =
    SUM(
        fato_pedidos[valor_entrega]
    )

RETURN
    COALESCE(
        _Resultado,
        0
    )
```
<br>

```DAX
entregas_atraso = 

-- Medida:
--      entregas_atraso
--
-- Descrição:
--      Calcula a quantidade total de pedidos com entrega atrasada, considerando o contexto de filtros aplicado no relatório.
--
-- Tabela origem:
--      fato_pedidos
--
-- Regra de negócio:
--      - Filtra os pedidos cuja coluna status_entrega seja igual a "Atrasada".
--      - Conta o número de linhas (pedidos) que atendem a essa condição.
--
-- Dependência:
--      Coluna fato_pedidos[status_entrega]
--
-- Retorno:
--      Número inteiro representando a quantidade de pedidos com entrega atrasada.
--
-- Observação:
--      COALESCE é utilizado para garantir que o retorno seja 0 quando não houver pedidos atrasados.
--      COUNTROWS é utilizado para contar os registros filtrados.

VAR _Resultado =
    CALCULATE(
        COUNTROWS(fato_pedidos),
        dimensao_status[status] = "Atrasada"
    )

RETURN
    COALESCE(
        _Resultado,
        0
    )
```
<br>

```DAX
entregas_prazo = 

-- Medida:
--      entregas_prazo
--
-- Descrição:
--      Calcula a quantidade total de pedidos entregues dentro do prazo, considerando o contexto de filtros aplicado no relatório.
--
-- Tabela origem:
--      fato_pedidos
--
-- Regra de negócio:
--      - Filtra os pedidos cuja coluna status_entrega seja igual a "No Prazo".
--      - Conta o número de linhas (pedidos) que atendem a essa condição.
--
-- Dependência:
--      Coluna fato_pedidos[status_entrega]
--
-- Retorno:
--      Número inteiro representando a quantidade de pedidos entregues dentro do prazo.
--
-- Observação:
--      COALESCE é utilizado para garantir que o retorno seja 0 quando não houver pedidos no prazo.
--      COUNTROWS é utilizado para contar os registros filtrados.

VAR _Resultado =
    CALCULATE(
        COUNTROWS(fato_pedidos),
        dimensao_status[status] = "No Prazo"
    )

RETURN
    COALESCE(
        _Resultado,
        0
    )
```
<br>

```DAX
meta_entregas_atraso = 
0.2

-- Medida:
--      meta_entregas_atraso
--
-- Descrição:
--      Define a meta de percentual máximo de entregas atrasadas no relatório.
--
-- Tabela origem:
--      Não depende de nenhuma tabela diretamente; valor fixo definido pelo negócio.
--
-- Regra de negócio:
--      - Define a meta como 20% (0,2) de entregas atrasadas.
--      - Valor utilizado para indicadores ou comparações em visuais de performance.
--
-- Dependência:
--      Nenhuma
--
-- Retorno:
--      Número decimal representando a meta de atraso (0,2 = 20%).
--
-- Observação:
--      Valor fixo; pode ser alterado conforme definição do KPI da empresa.
```
<br>

```DAX
meta_entregas_prazo = 
0.8

-- Medida:
--      meta_entregas_prazo
--
-- Descrição:
--      Define a meta de percentual mínimo de entregas realizadas dentro do prazo no relatório.
--
-- Tabela origem:
--      Não depende de nenhuma tabela diretamente; valor fixo definido pelo negócio.
--
-- Regra de negócio:
--      - Define a meta como 80% (0,8) de entregas dentro do prazo.
--      - Valor utilizado para indicadores ou comparações em visuais de performance.
--
-- Dependência:
--      Nenhuma
--
-- Retorno:
--      Número decimal representando a meta de entregas no prazo (0,8 = 80%).
--
-- Observação:
--      Valor fixo; pode ser alterado conforme definição do KPI da empresa.
```
<br>

```DAX
percentual_entregas_atraso = 

-- Medida:
--      percentual_entregas_atraso
--
-- Descrição:
--      Calcula o percentual de pedidos com entrega atrasada em relação à quantidade total de pedidos no contexto do relatório.
--
-- Tabela origem:
--      Medidas [entregas_atraso] e [quantidade_pedidos]
--
-- Regra de negócio:
--      - Divide o total de entregas atrasadas pela quantidade total de pedidos.
--      - Permite avaliar o desempenho logístico e comparar com metas.
--
-- Dependência:
--      Medidas [entregas_atraso] e [quantidade_pedidos]
--
-- Retorno:
--      Número decimal representando o percentual de entregas atrasadas (ex: 0,25 = 25%).
--
-- Observação:
--      DIVIDE é utilizado para evitar erros de divisão por zero.
--      COALESCE garante que o resultado seja 0 quando não houver pedidos.

VAR _Resultado =
    DIVIDE(
        [entregas_atraso],
        [quantidade_pedidos]
    )

RETURN
    COALESCE(
        _Resultado,
        0
    )
```
<br>

```DAX
percentual_entregas_prazo = 

-- Medida:
--      percentual_entregas_prazo
--
-- Descrição:
--      Calcula o percentual de pedidos entregues dentro do prazo em relação à quantidade total de pedidos no contexto do relatório.
--
-- Tabela origem:
--      Medidas [entregas_prazo] e [quantidade_pedidos]
--
-- Regra de negócio:
--      - Divide o total de entregas no prazo pela quantidade total de pedidos.
--      - Permite avaliar o desempenho logístico e comparar com metas.
--
-- Dependência:
--      Medidas [entregas_prazo] e [quantidade_pedidos]
--
-- Retorno:
--      Número decimal representando o percentual de entregas no prazo (ex: 0,80 = 80%).
--
-- Observação:
--      DIVIDE é utilizado para evitar erros de divisão por zero.
--      COALESCE garante que o resultado seja 0 quando não houver pedidos.

VAR _Resultado =
    DIVIDE(
        [entregas_prazo],
        [quantidade_pedidos]
    )

RETURN
    COALESCE(
        _Resultado,
        0
    )
```

## Medidas de Frete
<br>

```DAX
valor_frete = 

-- Medida:
--      valor_frete
--
-- Descrição: Calcula o valor total de frete de todos os pedidos, considerando o contexto de filtros aplicado no relatório.
--
-- Tabela origem:
--      fato_pedidos
--
-- Regra de negócio:
--      - Soma os valores da coluna valor_frete da tabela de pedidos, representando o total de frete cobrado no contexto filtrado.
--
-- Dependência:
--      Coluna fato_pedidos[valor_frete]
--
-- Retorno:
--      Número decimal representando o valor total de frete.
--
-- Observação:
--      COALESCE é utilizado para garantir que o retorno seja 0 quando não houver valores de frete.

VAR _Resultado =
    SUM(
        fato_pedidos[valor_frete]
    )

RETURN
    COALESCE(
        _Resultado,
        0
    )
```

## Medidas de Lucro
<br>

```DAX
percentual_lucro = 

-- Medida:
--      percentual_lucro
--
-- Descrição:
--      Calcula o percentual de lucro obtido sobre o valor do frete, considerando o custo de entrega no contexto do relatório.
--
-- Tabela origem:
--      Medidas [valor_frete] e [custo_entrega]
--
-- Regra de negócio:
--      - Subtrai o custo de entrega do valor do frete para obter o lucro.
--      - Divide o lucro pelo valor do frete para calcular o percentual.
--      - Permite avaliar a rentabilidade das operações de frete.
--
-- Dependência:
--      Medidas [valor_frete] e [custo_entrega]
--
-- Retorno:
--      Número decimal representando o percentual de lucro (ex: 0,25 = 25%).
--
-- Observação:
--      DIVIDE é utilizado para evitar erros de divisão por zero.
--      COALESCE garante que o resultado seja 0 quando não houver valores de frete.

VAR _Resultado =
    DIVIDE(
        [valor_frete] - [custo_entrega],
        [valor_frete]
    )

RETURN
    COALESCE(
        _Resultado,
        0
    )
```

## Medidas de Pedidos
<br>

```DAX
pedidos_mapa = 

-- Medida:
--      pedidos_mapa
-- Descrição:
--      Calcula a quantidade total de pedidos registrados na tabela fato_pedidos, considerando o contexto de filtros aplicado no relatório.
--
-- Tabela origem:
--      fato_pedidos
--
-- Regra de negócio:
--      - Conta todas as linhas da tabela fPedidos, representando o total de pedidos.
--
-- Dependência:
--      Coluna fato_pedidos (toda a tabela)
--
-- Retorno:
--      Número inteiro representando a quantidade total de pedidos.

VAR _Resultado =
    COUNTROWS(
        fato_pedidos
    )

RETURN
    _Resultado
```
<br>

```DAX
quantidade_pedidos = 

-- Medida:
--      quantidade_pedidos
--
-- Descrição:
--      Calcula a quantidade total de pedidos registrados na tabela fato_pedidos, considerando o contexto de filtros aplicado no relatório.
--
-- Tabela origem:
--      fato_pedidos
--
-- Regra de negócio:
--      - Conta todas as linhas da tabela fato_pedidos, representando o total de pedidos.
--
-- Dependência:
--      Coluna fPedidos (toda a tabela)
--
-- Retorno:
--      Número inteiro representando a quantidade total de pedidos.
--
-- Observação:
--      COALESCE é utilizado para garantir que o resultado seja 0 quando não houver linhas.
--      COUNTROWS é utilizado para contar os registros filtrados.

VAR _Resultado =
    COUNTROWS(
        fato_pedidos
    )

RETURN
    COALESCE(
        _Resultado,
        0
    )
```
<br>

```DAX
maior_menor_pedido = 

-- Medida:
--      maior_menor_pedido
--
-- Descrição:
--      Retorna a quantidade de pedidos apenas para os meses com maior ou menor volume, considerando o contexto de filtros aplicado no relatório.
--
-- Tabela origem:
--      [quantidade_pedidos] (medida previamente definida)
--      dimensao_calendario
--
-- Regra de negócio:
--      - Calcula o menor e maior valor de pedidos no período selecionado.
--      - Retorna a quantidade de pedidos apenas se for igual ao maior ou menor valor.
--      - Para os demais meses, retorna BLANK().
--
-- Dependência:
--      Medida [quantidade_pedidos]
--      Colunas dimensao_calendario[mes_abreviado], dimensao_calendario[mes_numero]
--
-- Retorno:
--      Número inteiro representando a quantidade de pedidos nos meses de maior e menor volume, ou BLANK() para os demais meses.
--
-- Observação:
--      ALLSELECTED é utilizado para respeitar filtros aplicados no relatório.
--      IF com operador || permite verificar múltiplas condições (maior ou menor).

VAR _MenorPedido =
    MINX(
        ALLSELECTED(
            dimensao_calendario[mes_abreviado],
            dimensao_calendario[mes_numero]
        ),
        [quantidade_pedidos]
    )

VAR _MaiorPedido =
    MAXX(
        ALLSELECTED(
            dimensao_calendario[mes_abreviado],
            dimensao_calendario[mes_numero]
        ),
        [quantidade_pedidos]
    )

RETURN
    IF(
        [quantidade_pedidos] = _MaiorPedido ||
        [quantidade_pedidos] = _MenorPedido,
        [quantidade_pedidos]
    )
```
<br>

```DAX
cores_maior_menor_pedido_dark = 

-- Medida:
--      cores_maior_menor_pedido_dark
--
-- Descrição:
--      Define cores para destacar o mês com maior e menor quantidade de pedidos considerando o contexto de filtros aplicado no relatório.
--
-- Tabela origem:
--      [quantidade_pedidos] (medida previamente definida)
--      dimensao_calendario
--
-- Regra de negócio:
--      - Calcula o menor e maior valor de pedidos no período selecionado.
--      - Aplica cores específicas para os meses com maior e menor quantidade:
--        Verde claro para o maior valor
--        Vermelho claro para o menor valor
--
-- Dependência:
--      Medida [quantidade_pedidos]
--      Colunas dimensao_calendario[mes_abreviado], dimensao_calendario[mes_numero]
--
-- Retorno:
--      Código hexadecimal de cor (string) para uso em formatação condicional.
--
-- Observação:
--      ALLSELECTED é usado para respeitar filtros aplicados no relatório
--      SWITCH(TRUE()) permite múltiplas condições para definição de cor.

VAR _MenorPedido =
    MINX(
        ALLSELECTED(
            dimensao_calendario[mes_abreviado],
            dimensao_calendario[mes_numero]
        ),
        [quantidade_pedidos]
    )

VAR _MaiorPedido =
    MAXX(
        ALLSELECTED(
            dimensao_calendario[mes_abreviado],
            dimensao_calendario[mes_numero]
        ),
        [quantidade_pedidos]
    )

RETURN
    SWITCH(
        TRUE(),
        [quantidade_pedidos] = _MaiorPedido, "#94EA54",  -- Verde claro
        [quantidade_pedidos] = _MenorPedido, "#F81414"   -- Vermelho claro
    )
```
<br>

```DAX
cores_maior_menor_pedido_light = 

-- Medida:
--      cores_maior_menor_pedido_light
--
-- Descrição:
--      Define cores para destacar o mês com maior e menor quantidade de pedidos considerando o contexto de filtros aplicado no relatório.
--
-- Tabela origem:
--      [quantidade_pedidos] (medida previamente definida)
--      dimensao_calendario
--
-- Regra de negócio:
--      - Calcula o menor e maior valor de pedidos no período selecionado.
--      - Aplica cores específicas para os meses com maior e menor quantidade:
--        Verde escuro para o maior valor
--        Vermelho escuro para o menor valor
--
-- Dependência:
--      Medida [quantidade_pedidos]
--      Colunas dimensao_calendario[mes_abreviado], dimensao_calendario[mes_numero]
--
-- Retorno:
--      Código hexadecimal de cor (string) para uso em formatação condicional.
--
-- Observação:
--      ALLSELECTED é usado para respeitar filtros aplicados no relatório
--      SWITCH(TRUE()) permite múltiplas condições para definição de cor.

VAR _MenorPedido =
    MINX(
        ALLSELECTED(
            dimensao_calendario[mes_abreviado],
            dimensao_calendario[mes_numero]
        ),
        [quantidade_pedidos]
    )

VAR _MaiorPedido =
    MAXX(
        ALLSELECTED(
            dimensao_calendario[mes_abreviado],
            dimensao_calendario[mes_numero]
        ),
        [quantidade_pedidos]
    )

RETURN
    SWITCH(
        TRUE(),
        [quantidade_pedidos] = _MaiorPedido, "#385622",  -- Verde escuro
        [quantidade_pedidos] = _MenorPedido, "#6A0A0A"   -- Vermelho escuro
    )
```
