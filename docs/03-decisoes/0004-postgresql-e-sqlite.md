# ADR-0004: PostgreSQL no nó local, SQLite no totem

- **Status:** Aceita
- **Data:** 2026-09-28

## Contexto
O nó precisa de um banco confiável, gratuito e bem suportado pelo EF Core. O totem precisa apenas de um buffer local.

## Decisão
- Nó: PostgreSQL, com um schema por módulo. O usuário da aplicação tem apenas `INSERT` e `SELECT` no schema `auditoria`, garantindo imutabilidade no banco.
- Dados de paciente isolados no schema `pacientes`, com criptografia de coluna para CPF e CNS.
- Totem: SQLite como buffer temporário (outbox), apagado após confirmação do nó.

## Alternativas consideradas
- SQL Server — licença e consumo de recursos piores para um mini PC.
- SQLite também no nó — limita concorrência e ferramentas de backup.

## Consequências
- (+) Fronteiras entre módulos visíveis também no banco.
- (+) Auditoria protegida mesmo contra bugs ou invasão da aplicação.
- (−) Um banco a administrar no nó (backup, atualização).
