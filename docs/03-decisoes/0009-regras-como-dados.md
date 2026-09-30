# ADR-0009: Regras como dados JSON com casos de teste compartilhados

- **Status:** Aceita
- **Data:** 2026-09-28
- **Substitui:** ADR-0005

## Contexto
Regras mudam com frequência (RNF17), precisam ser auditáveis (RF09) e o totem precisa avaliar sinais de alarme localmente em outra linguagem.

## Decisão
- Regras são conjuntos JSON versionados, com ciclo rascunho → aprovada → ativa.
- A API tem o avaliador de referência em C#. O totem tem seu próprio avaliador e baixa as regras por `GET /api/v1/regras/ativa`.
- Casos de teste em JSON (entrada + resultado esperado) em `regras/casos-de-teste/` rodam contra todos os avaliadores na integração contínua.
- O formato pode ser próprio ou baseado em JSON Logic (a definir).

## Consequências
- (+) Mudança de regra sem deploy; rastreabilidade por versão.
- (+) Casos de teste servem como especificação clínica revisável.
- (−) Avaliador duplicado por linguagem; mitigado pelos casos de teste comuns.
