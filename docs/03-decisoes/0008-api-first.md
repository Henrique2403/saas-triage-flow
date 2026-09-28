# ADR-0008: Backend API-first em .NET 10

- **Status:** Aceita
- **Data:** 2026-09-28
- **Substitui:** ADR-0003

## Contexto
O backend deve ser o núcleo do sistema em .NET, com totem, dashboards e futuros clientes em tecnologias independentes.

## Decisão
- API HTTP em ASP.NET Core sobre .NET 10 (LTS), como único ponto de acesso à lógica de negócio.
- Contrato OpenAPI gerado pela API; clientes tipados gerados a partir dele (Kiota ou openapi-generator).
- Endpoints orientados a tarefas do domínio, não CRUD genérico.
- Versionamento no caminho (`/api/v1`); dentro da versão maior, apenas mudanças aditivas.
- Erros no formato Problem Details (RFC 9457).
- Idempotência nos envios do totem via ID gerado no cliente.

## Alternativas consideradas
- Blazor Server integrado (ADR-0003) — acopla interface ao backend.
- GraphQL — flexibilidade desnecessária para o MVP e mais complexo de proteger.

## Consequências
- (+) Clientes independentes; regras de negócio centralizadas na API.
- (+) Novos clientes (mobile, integrações) sem mudar o backend.
- (−) Toda ação precisa de endpoint; mais trabalho de contrato e autenticação.
