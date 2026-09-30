# ADR-0003: Dashboards em Blazor Server no processo da API

- **Status:** Substituída por ADR-0008
- **Data:** 2026-09-28

## Contexto
Buscava-se minimizar o número de tecnologias para um dev solo com experiência em C#.

## Decisão
Dashboards em Blazor Server, hospedados no mesmo processo da API, chamando a camada de aplicação diretamente.

## Consequências
- (+) Sem API intermediária para as telas; tempo real nativo.
- (−) Acopla a interface ao backend e ao ecossistema .NET.

## Motivo da substituição
Decidiu-se que o backend será exclusivamente uma API e que os clientes usarão tecnologias independentes (ADR-0008).
