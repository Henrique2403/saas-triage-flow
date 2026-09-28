# Especificação das Regras de Triagem

> **Status: bloqueado.** O conteúdo clínico depende da licença do protocolo (GBCR) e da validação por um profissional de saúde. Este documento registra apenas os princípios e o formato já decididos.

## Princípios

- Regras são **dados versionados em JSON**, não código (ADR-0009).
- O motor é **independente de protocolo**: o protocolo licenciado é carregado como conteúdo.
- A classificação ocorre em etapas (ADR-0014):
  1. **Regras de alarme** — determinísticas; se disparam, definem prioridade máxima e geram alerta imediato.
  2. **Regras de protocolo** — fonte principal da sugestão no MVP.
  3. **Modelo de direcionamento** (fase 2) — sugestão adicional, nunca rebaixa um alarme.
- Toda sugestão registra a versão das regras e os critérios que dispararam.
- A decisão final é sempre do enfermeiro.
- As regras de alarme são o único subconjunto executado também no totem.

## Ciclo de vida de um conjunto de regras

`rascunho` → `aprovada` (por administrador clínico) → `ativa` (uma por vez) → `arquivada`

## Casos de teste compartilhados

Cada regra deve vir acompanhada de casos de teste em `regras/casos-de-teste/`, no formato:

```json
{
  "descricao": "SpO2 abaixo do limiar dispara alarme",
  "entrada": { "sinaisVitais": { "spo2": 88 }, "queixas": ["falta_de_ar"] },
  "esperado": { "alarme": true, "prioridade": "maxima" }
}
```

Os mesmos casos rodam contra o avaliador de referência (C#) e o avaliador do totem na integração contínua. Eles também servem como especificação clínica revisável pelo advisor de saúde.

> Os valores do exemplo são ilustrativos e não constituem critério clínico.

## A definir

- Formato final do JSON (próprio ou baseado em JSON Logic).
- Níveis de prioridade e tempos-alvo.
- Lista de sinais de alarme e limiares.
- Mapeamento queixa → especialidade.
