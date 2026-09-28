# ADR-0010: Eventos em tempo real via Server-Sent Events

- **Status:** Proposta
- **Data:** 2026-09-28

## Contexto
Dashboards precisam de atualização em menos de 2 s (RNF12). O fluxo é de mão única: servidor notifica, comandos vão pela API REST.

## Decisão
SSE em `GET /api/v1/eventos`, usando o suporte nativo das Minimal APIs no .NET 10.

## Alternativas consideradas
- SignalR — protocolo próprio; clientes fora de JS/.NET dependem da comunidade.
- WebSockets puros — bidirecional desnecessário e mais código.
- Polling — latência e carga maiores.

## Consequências
- (+) HTTP padrão; reconexão nativa no navegador com retomada do último evento.
- (−) Validar na rede real da unidade: proxies corporativos podem derrubar conexões longas.
