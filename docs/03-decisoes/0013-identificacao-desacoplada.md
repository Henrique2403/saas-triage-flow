# ADR-0013: Triagem anônima no totem, identificação pela recepção

- **Status:** Aceita
- **Data:** 2026-09-28

## Contexto
No acolhimento com classificação de risco, ninguém deve deixar de ser avaliado por falta de documento. Sem conexão ao CadSUS no MVP, identificar no totem traz pouco ganho e cria riscos (exposição do CPF em área pública, CPF de outra pessoa).

## Decisão
- O totem cria um `Atendimento` apenas com número de atendimento; `PacienteId` é nulo.
- A recepção vincula o paciente por CPF ou CNS após conferência de documento, ou o registra como não identificado. O vínculo é auditado.
- `Cpf` e `Cns` são Value Objects com validação dos dígitos verificadores.
- Número de atendimento: prefixo do totem + sequência diária local (ex.: `T1-042`), funcionando offline.

## Consequências
- (+) Não bloqueia o cuidado; menor risco de dados no totem.
- (+) Preparado para CadSUS/RNDS na fase 2.
- (−) Uma etapa adicional na recepção. Observar o fluxo real da unidade piloto.
