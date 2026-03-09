# Documentação de Medidas - Projeto TransFlow Logistics

Este documento lista todas as medidas criadas no modelo Power BI, suas regras de negócio, dependências e retornos esperados.

---

## Medidas de Contagem

### quantidade_pedidos

```DAX
VAR Resultado = COUNTROWS(fPedidos)
RETURN COALESCE(Resultado, 0)
```

Descrição: Total de pedidos registrados na tabela fPedidos.

### quantidade_produtos

```DAX
VAR Resultado = SUM(fPedidos[quantidade_produto])
RETURN COALESCE(Resultado, 0)
```

Descrição: Total de produtos registrados na tabela fPedidos.

---

## Medidas de Devolução

### devolucao_desistencia

```DAX
VAR QtdeDesistencia = CALCULATE([quantidade_pedidos], fPedidos[motivo_devolucao] = "Desistência")
RETURN "Desistência: " & COALESCE(QtdeDesistencia, 0)
```

Descrição: Quantidade de pedidos devolvidos por motivo Desistência, exibido como texto.

### devolucao_mercadoria_errada

```DAX
VAR QtdeMercErrada = CALCULATE([quantidade_pedidos], fPedidos[motivo_devolucao] = "Merc Errada")
RETURN "Mercadoria Errada: " & COALESCE(QtdeMercErrada, 0)
```

Descrição: Quantidade de pedidos devolvidos por motivo Mercadoria Errada, exibido como texto.

### devolucao_nota_fiscal_erro

```DAX
VAR QtdeNFErro = CALCULATE([quantidade_pedidos], fPedidos[motivo_devolucao] = "NF com Erro")
RETURN "Nota Fiscal com Erro: " & COALESCE(QtdeNFErro, 0)
```

Descrição: Quantidade de pedidos devolvidos por motivo Nota Fiscal com Erro, exibido como texto.

### devolucoes

```DAX
VAR QtdeDevolucoes = CALCULATE([quantidade_pedidos], fPedidos[status_devolucao] = "Sim")
RETURN COALESCE(QtdeDevolucoes, 0)
```

Descrição: Total de pedidos devolvidos.

---

## Medidas de Entregas

### entregas_atraso

```DAX
VAR QtdeAtraso = CALCULATE(COUNTROWS(fPedidos), fPedidos[status_entrega] = "Atrasada")
RETURN COALESCE(QtdeAtraso, 0)
```

Descrição: Quantidade de pedidos com entrega atrasada.

### entregas_prazo

```DAX
VAR QtdePrazo = CALCULATE(COUNTROWS(fPedidos), fPedidos[status_entrega] = "No Prazo")
RETURN COALESCE(QtdePrazo, 0)
```

Descrição: Quantidade de pedidos entregues dentro do prazo.

---

## Medidas de Maior/Menor

### maior_menor_pedido

```DAX
VAR _MenorPedido = MINX(ALLSELECTED(dCalendario[mes_abreviado], dCalendario[mes_numero]), [quantidade_pedidos])
VAR _MaiorPedido = MAXX(ALLSELECTED(dCalendario[mes_abreviado], dCalendario[mes_numero]), [quantidade_pedidos])
RETURN IF([quantidade_pedidos] = _MaiorPedido || [quantidade_pedidos] = _MenorPedido, [quantidade_pedidos])
```

Descrição: Retorna a quantidade de pedidos apenas para os meses com maior ou menor volume; BLANK() para os demais meses.

### cores_maior_menor_pedido

```DAX
VAR _MenorPedido = MINX(ALLSELECTED(dCalendario[mes_abreviado], dCalendario[mes_numero]), [quantidade_pedidos])
VAR _MaiorPedido = MAXX(ALLSELECTED(dCalendario[mes_abreviado], dCalendario[mes_numero]), [quantidade_pedidos])
RETURN
SWITCH(
    TRUE(),
    [quantidade_pedidos] = _MaiorPedido, "#309654",
    [quantidade_pedidos] = _MenorPedido, "#C45050",
    "#808080"
)
```

Descrição: Define cores para os meses com maior (verde), menor (vermelho) e demais (cinza) quantidades de pedidos.

---

## Medidas de Metas

### meta_entregas_atraso

```DAX
RETURN 0.2
```

Descrição: Meta de percentual máximo de entregas atrasadas (20%).

### meta_entregas_prazo

```DAX
RETURN 0.8
```

Descrição: Meta de percentual mínimo de entregas dentro do prazo (80%).

---

## Medidas de Percentual

### percentual_entregas_atraso

```DAX
VAR Resultado = DIVIDE([entregas_atraso], [quantidade_pedidos])
RETURN COALESCE(Resultado, 0)
```

Descrição: Percentual de pedidos com entrega atrasada.

### percentual_entregas_prazo

```DAX
VAR Resultado = DIVIDE([entregas_prazo], [quantidade_pedidos])
RETURN COALESCE(Resultado, 0)
```

Descrição: Percentual de pedidos entregues dentro do prazo.

### percentual_lucro

```DAX
VAR Resultado = DIVIDE([valor_frete] - [custo_entrega], [valor_frete])
RETURN COALESCE(Resultado, 0)
```

Descrição: Percentual de lucro obtido sobre o valor do frete.

---

## Medidas de Valores

### valor_frete

```DAX
VAR Resultado = SUM(fPedidos[valor_frete])
RETURN COALESCE(Resultado, 0)
```

Descrição: Valor total de frete de todos os pedidos.

### custo_entrega

```DAX
VAR Resultado = SUM(fPedidos[custo_entrega])
RETURN COALESCE(Resultado, 0)
```

Descrição: Custo total de entrega de todos os pedidos.

```
```
