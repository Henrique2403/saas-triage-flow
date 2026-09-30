# Riscos Clínicos e Controles

> **Status:** parcial. Referência futura: ISO 14971 (gestão de risco de dispositivos médicos) e IEC 62304 (ciclo de vida de software), caso haja enquadramento pela ANVISA.

| ID | Risco ao paciente | Controle | Rastreio |
|---|---|---|---|
| RC01 | Paciente grave aguarda o fim do questionário | Alerta imediato por sinais de alarme | RF05 |
| RC02 | Paciente grave sem contato do totem com o nó | Avaliação de alarme local e orientação na tela | RF05, ADR-0009 |
| RC03 | Sugestão errada aceita sem revisão | Validação obrigatória por enfermeiro | RF12, RF14 |
| RC04 | Leitura de sensor incorreta | Verificação de plausibilidade; entrada manual; registro da origem | RF04, RF13 |
| RC05 | IA rebaixa prioridade de paciente grave | Alarmes sempre prevalecem; modo sombra antes de ativar | ADR-0014 |
| RC06 | Paciente aguarda além do seguro | Tempos-alvo com alerta de reavaliação | RF16 |
| RC07 | Paciente sem documento deixa de ser atendido | Identificação nunca bloqueia o cuidado | RF25, ADR-0013 |
| RC08 | Paciente incapaz de usar o totem | Triagem assistida pela enfermagem | RF24 |
| RC09 | Vínculo com a pessoa errada | Conferência de documento pela recepção; vínculo auditado | RF23 |
| RC10 | Regra alterada sem controle | Ciclo de aprovação; versão registrada em cada triagem | RF09, RF10 |
| RC11 | Relógio incorreto distorce tempos de espera | NTP a partir do nó; horários em UTC | arc42 §7 |

## A definir

- Análise de severidade e probabilidade de cada risco.
- Riscos específicos das regras clínicas (após definição do protocolo).
