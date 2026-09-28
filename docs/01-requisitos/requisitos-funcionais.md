# Requisitos Funcionais — MVP

IDs são permanentes. Cada requisito deve ser rastreável até código e testes.

## Totem (paciente)

| ID | Requisito |
|---|---|
| RF01 | O paciente inicia a triagem sem login, com linguagem simples, fonte grande, ícones e opção de áudio. |
| RF02 | O questionário guiado coleta queixa principal, tempo de início, intensidade (escala visual 0–10), condições pré-existentes, alergias e medicamentos em uso. |
| RF03 | O totem captura sinais vitais dos sensores (MVP: SpO₂, frequência cardíaca, temperatura), registrando valor, unidade, dispositivo, horário e origem (sensor ou manual). |
| RF04 | Leituras fisiologicamente implausíveis não são aceitas silenciosamente: o totem pede nova medição ou marca a medida para a enfermagem. |
| RF05 | **Sinais de alarme geram alerta imediato à enfermagem**, sem esperar o fim do questionário. Sem contato com o nó local, o totem orienta o paciente na tela a procurar a equipe imediatamente. |
| RF06 | Ao final, o totem exibe o número de atendimento e orienta onde aguardar. |
| RF07 | O totem funciona sem contato com o nó local, guardando as triagens e reenviando depois sem duplicar. |

## Motor de classificação

| ID | Requisito |
|---|---|
| RF08 | O motor calcula prioridade e especialidade sugeridas a partir de um conjunto de regras versionado, independente de protocolo específico. |
| RF09 | Cada triagem registra a versão das regras usada e quais critérios dispararam a sugestão. |
| RF10 | Conjuntos de regras têm ciclo rascunho → aprovada → ativa, gerenciado pelo administrador clínico; apenas uma versão ativa por vez. |

## Validação (enfermagem)

| ID | Requisito |
|---|---|
| RF11 | O enfermeiro vê as triagens aguardando validação, ordenadas pela prioridade sugerida, com sinais de alarme em destaque. |
| RF12 | O enfermeiro confirma ou altera prioridade e especialidade; toda alteração exige justificativa. |
| RF13 | O enfermeiro pode inserir ou corrigir sinais vitais manualmente; a medida original é preservada. |
| RF14 | Somente triagens validadas por enfermeiro entram na fila de atendimento. |

## Fila e recepção

| ID | Requisito |
|---|---|
| RF15 | A fila por especialidade é atualizada em tempo real, ordenada por prioridade e depois por tempo de espera. |
| RF16 | Cada nível de prioridade tem tempo-alvo de espera; ao ser excedido, a enfermagem é alertada para reavaliar. |
| RF17 | A recepção chama o próximo paciente e registra ausência ou evasão. |

## Especialista

| ID | Requisito |
|---|---|
| RF18 | O especialista é notificado e vê os dados estruturados da triagem: queixa, sinais vitais, prioridade e justificativas. |
| RF19 | O especialista registra início e fim do atendimento e dá feedback do direcionamento (concorda ou não, e a especialidade correta). |

## Gestão

| ID | Requisito |
|---|---|
| RF20 | O administrador da unidade gerencia usuários, perfis e especialidades disponíveis. |
| RF21 | Relatórios de volume, tempos por etapa (chegada, validação, chamada, atendimento), distribuição por prioridade, concordância do direcionamento e total de triagens concluídas. |
| RF22 | A trilha de auditoria pode ser consultada por perfil autorizado. |

## Identificação e entradas alternativas

| ID | Requisito |
|---|---|
| RF23 | A recepção vincula o atendimento a um paciente (novo ou existente) por CPF ou CNS após conferência de documento, ou o registra como não identificado. |
| RF24 | A enfermagem pode abrir uma triagem assistida diretamente no dashboard, sem passar pelo totem. |
| RF25 | A falta de identificação nunca bloqueia classificação, fila ou atendimento. |

## Fora do MVP

Nuvem e sincronização, IA, integração com e-SUS APS/CadSUS/RNDS, prontuário, múltiplas unidades, faturamento automático e aplicativo mobile.
