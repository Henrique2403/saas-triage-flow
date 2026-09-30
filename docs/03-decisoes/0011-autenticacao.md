# ADR-0011: Autenticação por tipo de cliente

- **Status:** Aceita
- **Data:** 2026-09-28

## Decisão
- **SPA (navegador):** cookie de sessão `HttpOnly` e `SameSite` via ASP.NET Core Identity, com a SPA na mesma origem da API (ADR-0012). Expiração por inatividade de 15 min (RNF01).
- **Totem:** token de dispositivo emitido no registro do equipamento, revogável.
- **Fase 2:** servidor OAuth 2.0/OpenID Connect (OpenIddict ou Keycloak) quando houver nuvem e clientes externos.

## Alternativas consideradas
- JWT guardado no navegador — exposto a XSS.
- OIDC já no MVP — complexidade antecipada.

## Consequências
- (+) Modelo simples e seguro para o MVP.
- (−) Migração para OIDC na fase 2.
