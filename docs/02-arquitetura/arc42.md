# Arquitetura — Agiliza Triagem (arc42)

> Baseado no template arc42. Diagramas no estilo C4 (níveis 1 e 2).

## 1. Introdução e objetivos

Sistema de apoio à decisão para triagem inicial em unidades do SUS. Detalhes em [documento de visão](../00-visao/documento-de-visao.md).

**Objetivos de qualidade (em ordem):** segurança e privacidade; corretude e auditabilidade clínica; disponibilidade local; usabilidade; modificabilidade. Cenários mensuráveis em [requisitos não funcionais](../01-requisitos/requisitos-nao-funcionais.md).

**Observação de dimensionamento:** o piloto prevê 500 a 1.000 triagens/mês (~33/dia); o cenário otimista do ano 1, cerca de 400/dia somando todas as unidades. A dificuldade do sistema é corretude, segurança e disponibilidade, não escala. Isso exclui microsserviços, orquestradores e infraestrutura distribuída no MVP.

## 2. Restrições

| Tipo | Restrição |
|---|---|
| Equipe | Um desenvolvedor; MVP estimado em 5 a 6 meses |
| Tecnologia | Backend em .NET 10 (LTS); clientes em tecnologias independentes |
| Orçamento | Infraestrutura inicial em torno de R$ 1.000/mês |
| Regulação | LGPD (dados sensíveis de saúde); classificação validada por enfermeiro (COFEN); possível enquadramento na ANVISA (RDC 657/2022) |
| Ambiente | Internet instável; equipamentos antigos; TI hospitalar com restrições |
| Protocolo | Protocolo de classificação sujeito a licença; motor precisa ser agnóstico |

## 3. Contexto (C4 nível 1)

```
                 ┌──────────────┐
   Paciente ───► │              │ ◄─── Enfermeiro(a)
                 │   Agiliza    │ ◄─── Recepção
   Sensores ───► │   Triagem    │ ◄─── Médico especialista
   (BLE)         │              │ ◄─── Administradores
                 └──────┬───────┘
                        │ (fase 2)
          ┌─────────────┼──────────────┬───────────────┐
          ▼             ▼              ▼               ▼
     Nuvem Agiliza    CadSUS          RNDS         e-SUS APS
```

Sistemas externos da fase 2 não existem no MVP, mas o desenho não pode impedir essas integrações (FHIR R4 para RNDS; layout LEDI para e-SUS APS).

## 4. Estratégia de solução

| Decisão | Motivação | ADR |
|---|---|---|
| MVP apenas no nó local | Offline por desenho; elimina sincronização no MVP | 0001 |
| Monólito modular | Volume baixo, dev solo, operação simples | 0002 |
| API-first em .NET | Clientes independentes; contrato como fonte da verdade | 0008 |
| Regras como dados | Mudança sem deploy; avaliadores em várias linguagens | 0009 |
| Identificação desacoplada | Não bloquear o cuidado; minimização de dados | 0013 |
| Classificação em etapas | Alarmes sempre prevalecem; IA entra sem reescrita | 0014 |

## 5. Visão de blocos

### 5.1 Containers (C4 nível 2)

```
┌──────────────────────── UNIDADE DE SAÚDE (rede local) ────────────────────────┐
│                                                                                 │
│  ┌─────────────────────┐   HTTPS    ┌─────────────────────────────────────────┐ │
│  │ TOTEM               │ ─────────► │ PROXY REVERSO (Caddy)                   │ │
│  │ App (a definir)     │            │ TLS · arquivos da SPA · /api → API      │ │
│  │ Sensores BLE        │            └───────────────┬─────────────────────────┘ │
│  │ SQLite (buffer)     │                            │                           │
│  └─────────────────────┘                            ▼                           │
│                                     ┌─────────────────────────────────────────┐ │
│  ┌─────────────────────┐   HTTPS    │ API (ASP.NET Core, .NET 10)             │ │
│  │ DASHBOARDS          │ ─────────► │ Módulos · motor de regras · SSE · jobs  │ │
│  │ SPA React + TS      │  (via      └───────────────┬─────────────────────────┘ │
│  │ (navegador)         │   proxy)                   ▼                           │
│  └─────────────────────┘            ┌─────────────────────────────────────────┐ │
│                                     │ PostgreSQL                              │ │
│                                     └─────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────────┘
```

| Container | Tecnologia | Responsabilidade |
|---|---|---|
| App do totem | A definir (ADR-0007) | Questionário, sensores BLE, alarmes locais, buffer offline |
| Banco do totem | SQLite | Buffer temporário (outbox) |
| Dashboards | SPA React + TypeScript (ADR-0015) | Telas de enfermagem, recepção, médico e gestão |
| Proxy reverso | Caddy (ADR-0012) | TLS, entrega da SPA, roteamento de `/api` |
| API | ASP.NET Core, .NET 10 (ADR-0008) | Módulos de negócio, motor de regras, eventos SSE, jobs |
| Banco do nó | PostgreSQL (ADR-0004) | Dados operacionais, regras versionadas, auditoria |

### 5.2 Módulos da API

| Módulo | Responsabilidade | Schema |
|---|---|---|
| Atendimento | Atendimentos, triagens, sinais vitais, validação, reclassificação | `atendimento` |
| Pacientes | Identidade civil (CPF, CNS) e vínculos | `pacientes` |
| Fila | Filas por especialidade, chamadas, tempos-alvo e alertas | `fila` |
| Regras | Conjuntos de regras versionados, especialidades, avaliador de referência | `regras` |
| Identidade | Usuários, papéis, dispositivos registrados | `identidade` |
| Auditoria | Eventos de auditoria imutáveis | `auditoria` |
| Integração | Outbox e recepção idempotente de envios do totem | `integracao` |
| Relatórios | Consultas de leitura (podem atravessar schemas, só leitura) | — |

**Regra de fronteira:** um módulo nunca acessa tabelas ou classes internas de outro; conversa apenas pela interface pública do módulo ou por eventos. Garantido por testes de arquitetura (NetArchTest ou ArchUnitNET).

### 5.3 Estrutura do repositório (`saas-triage-flow`)

```
saas-triage-flow/
├── docs/                          documentação (este diretório)
├── api/
│   ├── AgilizaTriagem.sln
│   ├── src/
│   │   ├── AgilizaTriagem.SharedKernel      tipos base, Value Objects (Cpf, Cns...)
│   │   ├── AgilizaTriagem.Regras            avaliador de referência das regras
│   │   ├── AgilizaTriagem.Api               host ASP.NET Core
│   │   └── AgilizaTriagem.Modulos.*         Atendimento, Pacientes, Fila, Identidade, Auditoria, Integracao
│   └── tests/
├── clientes/
│   ├── dashboard/                 SPA React + TypeScript
│   └── totem/                     app do totem (tecnologia a definir)
├── regras/
│   ├── schema/                    esquema JSON das regras
│   └── casos-de-teste/            casos compartilhados entre avaliadores
└── infra/
    ├── docker-compose.yml
    └── Caddyfile
```

## 6. Visão de execução

### 6.1 Fluxo principal

1. O paciente faz a triagem no totem. Leituras BLE passam pelo adaptador do sensor; o avaliador local verifica sinais de alarme.
2. O totem grava a triagem no SQLite (outbox) e envia `POST /api/v1/triagens` com ID gerado no cliente. Em falha, reenvia depois; a API ignora duplicatas pelo ID.
3. A API recalcula a sugestão com a versão ativa das regras, persiste, grava auditoria e publica evento interno.
4. Os dashboards recebem o evento via SSE (`GET /api/v1/eventos`). Alarmes aparecem em destaque.
5. O enfermeiro valida (`POST /api/v1/triagens/{id}/validacao`); o paciente entra na fila da especialidade.
6. Um serviço em segundo plano monitora os tempos-alvo e gera alertas de reavaliação.

### 6.2 Principais endpoints (orientados a tarefas)

| Ação | Endpoint |
|---|---|
| Totem envia triagem | `POST /api/v1/triagens` |
| Enfermeiro valida | `POST /api/v1/triagens/{id}/validacao` |
| Enfermeiro reclassifica | `POST /api/v1/triagens/{id}/reclassificacao` |
| Enfermeiro abre triagem assistida | `POST /api/v1/triagens/assistidas` |
| Recepção vincula paciente | `POST /api/v1/atendimentos/{id}/vinculo-paciente` |
| Recepção chama próximo | `POST /api/v1/filas/{especialidade}/chamadas` |
| Médico registra feedback | `POST /api/v1/atendimentos/{id}/feedback-direcionamento` |
| Totem baixa regras ativas | `GET /api/v1/regras/ativa` |
| Dashboards recebem eventos | `GET /api/v1/eventos` (SSE) |

O contrato formal é o documento OpenAPI gerado pela API.

## 7. Implantação

| Elemento | Descrição |
|---|---|
| Nó local | Mini PC com Ubuntu Server, Docker Compose (Caddy + API + PostgreSQL), nobreak |
| Rede | Totem ligado ao nó por cabo sempre que possível; dashboards em PCs existentes via navegador |
| TLS | Autoridade certificadora própria; certificado do nó fixado (pinned) no totem |
| Totem | Registrado como dispositivo; token revogável |
| Acesso remoto | VPN apenas de saída (WireGuard ou Tailscale), sujeita à aprovação da TI |
| Horário | Nó serve NTP aos totens; todos os horários em UTC (`DateTimeOffset`) |
| Backup | Dump frequente do PostgreSQL em segundo disco + cópia externa cifrada diária |
| Atualização | Nova imagem Docker baixada pelo nó via VPN |

## 8. Conceitos transversais

- **Identificadores:** GUID v7 (`Guid.CreateVersion7()`) gerados no cliente, para funcionar offline e manter ordem temporal nos índices.
- **Multi-tenancy:** `TenantId` em todas as entidades; filtro global no EF Core.
- **Idempotência:** envios do totem identificados pelo ID gerado no cliente; reenvio não duplica.
- **Outbox:** mudanças registradas para envio confiável (totem → nó no MVP; nó → nuvem na fase 2).
- **Auditoria:** interceptor de persistência grava eventos; o usuário da aplicação no PostgreSQL não tem `UPDATE` nem `DELETE` no schema `auditoria`.
- **Tempo:** horários sempre em UTC; conversão apenas na exibição.
- **Erros:** Problem Details (RFC 9457) em todas as respostas de erro.
- **Versionamento:** `/v1` no caminho; dentro da versão, apenas mudanças aditivas.
- **Autenticação:** cookie `HttpOnly`/`SameSite` para a SPA; token de dispositivo para o totem (ADR-0011).
- **Privacidade em logs:** nenhum dado pessoal ou clínico em logs técnicos.
- **Dados de paciente:** isolados no schema `pacientes`, com criptografia de coluna para CPF e CNS.

## 9. Decisões

Ver [03-decisoes](../03-decisoes/) e o índice no [README](../README.md).

## 10. Requisitos de qualidade

Ver [requisitos não funcionais](../01-requisitos/requisitos-nao-funcionais.md), incluindo os cenários de qualidade.

## 11. Riscos e dívidas técnicas

| Risco | Mitigação |
|---|---|
| Leitura BLE instável no hardware do totem | Spike S1 antes de qualquer tela |
| Duplicação ou perda de triagens offline | Spike S2; IDs no cliente, outbox, idempotência |
| TI do hospital não aprovar equipamento/VPN | Levar o tema cedo na conversa com o piloto |
| Licença do protocolo indisponível | Motor agnóstico; regras como conteúdo |
| Enquadramento ANVISA exigir documentação extensa | Rastreabilidade de requisitos e ADRs desde o início |
| Dev solo sobrecarregado | Escopo do MVP explícito; duas linguagens no máximo, se possível |
| Premissa de produto não validada | Entrevistas com hospitais antes de desenvolvimento pesado |

## 12. Glossário

Ver [glossario.md](../glossario.md).
