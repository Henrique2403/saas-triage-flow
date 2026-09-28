# ADR-0006: Nó local em Ubuntu com Docker Compose e VPN de saída

- **Status:** Proposta
- **Data:** 2026-09-28

## Contexto
O nó precisa ser reproduzível, atualizável remotamente e barato. A rede hospitalar costuma bloquear conexões de entrada.

## Decisão
Mini PC com Ubuntu Server rodando Docker Compose (Caddy + API + PostgreSQL), com nobreak. Acesso remoto por VPN apenas de saída (WireGuard ou Tailscale). Atualização por nova imagem Docker.

## Alternativas consideradas
- Windows com serviços nativos — menos reproduzível e atualização mais manual.
- Abrir portas no firewall do hospital — dificilmente aprovado e mais inseguro.

## Consequências
- (+) Mesmo ambiente no desenvolvimento (Docker Desktop) e em produção.
- (−) Depende de aprovação da TI do hospital. **Confirmar com a unidade piloto antes de aceitar.**
