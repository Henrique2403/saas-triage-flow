# ADR-0002: Monólito modular com fronteiras garantidas por testes

- **Status:** Aceita
- **Data:** 2026-09-28

## Contexto
Volume baixo (~33 triagens/dia no piloto), um desenvolvedor, implantação num mini PC sem equipe de TI no local.

## Decisão
A API é uma única aplicação ASP.NET Core dividida em módulos (Atendimento, Pacientes, Fila, Regras, Identidade, Auditoria, Integração, Relatórios). Cada módulo tem schema próprio no banco. Nenhum módulo acessa tabelas ou classes internas de outro. A regra é garantida por testes de arquitetura (NetArchTest ou ArchUnitNET) que quebram o build.

## Alternativas consideradas
- Microsserviços — multiplicariam deploys, bancos e pontos de falha sem problema real a resolver.
- Monólito sem fronteiras — rápido no início, mas dificulta a extração de módulos na fase 2.

## Consequências
- (+) Um deploy, um processo, depuração simples.
- (+) Módulos extraíveis no futuro por já terem fronteiras.
- (−) Exige disciplina; mitigado pelos testes de arquitetura.
