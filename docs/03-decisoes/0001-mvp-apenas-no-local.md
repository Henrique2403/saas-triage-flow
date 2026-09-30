# ADR-0001: MVP implantado apenas como nó local na unidade

- **Status:** Aceita
- **Data:** 2026-09-28

## Contexto
O MVP precisa funcionar sem internet, com sensores integrados, e será desenvolvido por uma única pessoa. A sincronização entre nuvem e unidade é a parte de maior risco e complexidade.

## Decisão
O MVP roda inteiramente na rede local da unidade piloto (nó local + totem + dashboards). A nuvem fica para a fase 2. O código será pronto para nuvem: `TenantId` em todas as entidades, IDs gerados no cliente (GUID v7) e outbox registrando mudanças.

## Alternativas consideradas
- Nuvem com totem offline — a fila e os dashboards parariam sem internet.
- Nuvem + nó local sincronizados já no MVP — complexidade de sincronização e conflitos incompatível com dev solo.

## Consequências
- (+) Offline garantido por desenho; sem sincronização no MVP.
- (+) Dados permanecem na unidade, o que facilita aprovação de TI e jurídico.
- (−) Exige hardware local, backup externo e atualização remota.
- (−) Relatórios consolidados entre unidades só na fase 2.
