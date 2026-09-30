# ADR-0005: Motor de regras em biblioteca C# compartilhada

- **Status:** Substituída por ADR-0009
- **Data:** 2026-09-28

## Contexto
O totem precisa avaliar sinais de alarme localmente, sem depender do nó.

## Decisão
Motor de regras como biblioteca C# usada pelo nó e pelo totem.

## Consequências
- (+) Uma única implementação.
- (−) Obriga o totem a ser .NET.

## Motivo da substituição
Com clientes em tecnologias independentes (ADR-0008), o compartilhamento passa a ser feito via regras como dados e casos de teste comuns (ADR-0009).
