# LGPD — Insumos para o Relatório de Impacto à Proteção de Dados (RIPD)

> **Status:** insumos reunidos. O RIPD formal deve ser elaborado com apoio jurídico/DPO antes do piloto.

## Papéis

- **Controlador:** a unidade de saúde (decide as finalidades do tratamento).
- **Operador:** Agiliza Triagem (trata os dados em nome da unidade). Deve constar em contrato.

## Dados tratados

| Categoria | Exemplos | Natureza |
|---|---|---|
| Identificação | Nome, CPF, CNS | Pessoal |
| Saúde | Queixas, sinais vitais, condições, alergias, medicamentos, classificação | **Sensível** (art. 11) |
| Profissionais | Nome, papel, ações realizadas | Pessoal |
| Técnicos | Logs, identificadores de dispositivo | Não pessoal (logs sem dados pessoais) |

## Finalidade e base legal (a confirmar com jurídico)

- Finalidade: classificação de risco e organização do atendimento.
- Base legal provável: tutela da saúde, em procedimento realizado por profissionais de saúde (art. 11, II, "f").

## Medidas já previstas na arquitetura

- Minimização: triagem anônima no totem; identificação só na recepção (ADR-0013); dados apagados do totem após confirmação (RNF06).
- Dados permanecem na unidade no MVP (ADR-0001).
- Criptografia em trânsito (TLS na rede local) e em repouso (disco e colunas de CPF/CNS).
- Controle de acesso por papel (RNF02) e sessão com expiração (RNF01).
- Auditoria imutável de leitura e escrita (RNF04, ADR-0004).
- Logs técnicos sem dados pessoais (RNF07).
- Backup cifrado.

## A definir

- Política de retenção (dado de triagem pode compor o prontuário, com guarda longa).
- Procedimento de atendimento a direitos do titular.
- Plano de resposta a incidentes e comunicação à ANPD.
- Transferência para nuvem na fase 2 (região Brasil, contrato com provedor).
