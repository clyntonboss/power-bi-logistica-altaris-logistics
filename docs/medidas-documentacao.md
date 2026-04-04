# Documentação das Medidas: Projeto Logistica — Altaris Logistics

Este documento lista todas as medidas criadas no modelo Power BI, suas regras de negócio, dependências e retornos esperados.

---

## Medidas de Devolução
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
