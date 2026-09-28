# ADR-0012: Proxy reverso Caddy servindo SPA e API na mesma origem

- **Status:** Proposta
- **Data:** 2026-09-28

## Decisão
Caddy na frente da API: termina TLS, entrega os arquivos estáticos da SPA e encaminha `/api` para a API.

## Alternativas consideradas
- nginx — equivalente, com mais configuração de TLS.
- API servindo os arquivos da SPA — mistura responsabilidades.

## Consequências
- (+) Mesma origem: sem CORS e com cookies simples.
- (+) Fácil de substituir.
