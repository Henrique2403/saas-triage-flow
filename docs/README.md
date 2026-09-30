# Documentação técnica — Agiliza Triagem

Documentação do projeto mantida como código (docs-as-code), versionada junto com o repositório `saas-triage-flow`.

> Última revisão: 2026-09-28 · Versão 0.1 (rascunho consolidado)

## Índice

| Pasta / arquivo | Conteúdo | Status |
|---|---|---|
| [00-visao/documento-de-visao.md](00-visao/documento-de-visao.md) | Problema, atores, escopo do MVP e fora do escopo | Rascunho |
| [01-requisitos/requisitos-funcionais.md](01-requisitos/requisitos-funcionais.md) | RF01–RF25 por módulo | Rascunho |
| [01-requisitos/requisitos-nao-funcionais.md](01-requisitos/requisitos-nao-funcionais.md) | RNF01–RNF24 com metas | Rascunho (metas a calibrar no piloto) |
| [01-requisitos/regras-de-triagem.md](01-requisitos/regras-de-triagem.md) | Formato e princípios das regras clínicas | Não iniciado (bloqueado) |
| [02-arquitetura/arc42.md](02-arquitetura/arc42.md) | Arquitetura no modelo arc42 + C4 | Maior parte coberta |
| [03-decisoes/](03-decisoes/) | Registros de decisão (ADRs) 0001–0015 | Ver índice abaixo |
| [04-conformidade/lgpd-ripd.md](04-conformidade/lgpd-ripd.md) | Insumos para o Relatório de Impacto (LGPD) | Insumos reunidos |
| [04-conformidade/riscos-clinicos.md](04-conformidade/riscos-clinicos.md) | Riscos à segurança do paciente e controles | Parcial |
| [glossario.md](glossario.md) | Termos do domínio | Primeira versão |
| [pendencias.md](pendencias.md) | Decisões em aberto, spikes e dependências externas | Vivo |

## Índice de decisões (ADRs)

| ADR | Título | Status |
|---|---|---|
| [0001](03-decisoes/0001-mvp-apenas-no-local.md) | MVP implantado apenas como nó local | Aceita |
| [0002](03-decisoes/0002-monolito-modular.md) | Monólito modular com fronteiras testadas | Aceita |
| [0003](03-decisoes/0003-dashboards-blazor-server.md) | Dashboards em Blazor Server | Substituída pela 0008 |
| [0004](03-decisoes/0004-postgresql-e-sqlite.md) | PostgreSQL no nó, SQLite no totem | Aceita |
| [0005](03-decisoes/0005-biblioteca-regras-compartilhada.md) | Biblioteca de regras C# compartilhada | Substituída pela 0009 |
| [0006](03-decisoes/0006-implantacao-ubuntu-docker-vpn.md) | Nó em Ubuntu + Docker Compose + VPN de saída | Proposta |
| [0007](03-decisoes/0007-plataforma-do-totem.md) | Plataforma e linguagem do totem | Proposta (depende de spike) |
| [0008](03-decisoes/0008-api-first.md) | Backend API-first em .NET 10 | Aceita |
| [0009](03-decisoes/0009-regras-como-dados.md) | Regras como dados JSON + casos de teste compartilhados | Aceita |
| [0010](03-decisoes/0010-tempo-real-sse.md) | Tempo real via Server-Sent Events | Proposta |
| [0011](03-decisoes/0011-autenticacao.md) | Cookie para SPA, token de dispositivo para o totem | Aceita |
| [0012](03-decisoes/0012-proxy-reverso-caddy.md) | Proxy reverso Caddy na mesma origem | Proposta |
| [0013](03-decisoes/0013-identificacao-desacoplada.md) | Triagem anônima, identificação pela recepção | Aceita |
| [0014](03-decisoes/0014-pipeline-classificacao-e-ia.md) | Pipeline de classificação e estratégia de IA | Aceita |
| [0015](03-decisoes/0015-dashboards-react-typescript.md) | Dashboards em React + TypeScript | Aceita |

## Como manter esta documentação

- Toda decisão arquitetural relevante vira um ADR novo, a partir de [03-decisoes/template.md](03-decisoes/template.md).
- ADR com status **Aceita** nunca é editado. Se a decisão mudar, cria-se um ADR novo e o antigo passa a **Substituída por ADR-XXXX**.
- ADR com status **Proposta** pode ser ajustado até ser aceito ou rejeitado.
- Requisitos têm ID fixo (RF/RNF). Um requisito removido é marcado como removido, não renumerado, para manter a rastreabilidade com código e testes.
