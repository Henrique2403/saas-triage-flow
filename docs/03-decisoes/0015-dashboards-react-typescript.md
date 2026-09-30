# ADR-0015: Dashboards em React com TypeScript

- **Status:** Aceita
- **Data:** 2026-09-28

## Contexto
Com a API-first (ADR-0008), os dashboards são uma SPA independente. São telas densas em dados (filas, relatórios, validação).

## Decisão
SPA em React com TypeScript, consumindo a API por cliente gerado do OpenAPI e eventos via SSE.

## Alternativas consideradas
- Angular ou Vue — viáveis; React tem o maior ecossistema e material.
- Flutter Web — mais fraco para interfaces densas em dados.

## Consequências
- (+) Ecossistema amplo de componentes (tabelas, gráficos, formulários).
- (+) Permite totem em React Native mantendo duas linguagens (ADR-0007).
- (−) Uma segunda linguagem para aprender e manter.
