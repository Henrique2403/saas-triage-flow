# Glossário

Linguagem comum entre saúde e código. Os nomes das classes, tabelas e endpoints devem seguir estes termos.

| Termo | Definição | No código |
|---|---|---|
| **Atendimento** | Passagem de uma pessoa pela unidade, do início da triagem até a saída. Existe mesmo sem paciente identificado. | `Atendimento` |
| **Número de atendimento** | Identificador curto exibido no totem e chamado em voz alta (ex.: `T1-042`). Prefixo do totem + sequência diária local. | `NumeroAtendimento` |
| **Triagem** | Avaliação clínica inicial dentro de um atendimento: respostas, sinais vitais e classificação. | `Triagem` |
| **Triagem assistida** | Triagem iniciada pela enfermagem no dashboard, sem passar pelo totem. | `OrigemTriagem.Assistida` |
| **Paciente** | Identidade civil da pessoa (CPF e/ou CNS, nome). Vinculado ao atendimento pela recepção. | `Paciente` |
| **Paciente não identificado** | Atendimento sem vínculo civil, por impossibilidade ou recusa. Nunca bloqueia o cuidado. | `PacienteId == null` |
| **Vínculo de paciente** | Ato, auditado, de associar um atendimento a um paciente após conferência de documento. | `VincularPaciente()` |
| **Sinal vital** | Medida fisiológica (SpO₂, frequência cardíaca, temperatura...) com valor, unidade, origem, dispositivo e horário. | `SinalVital` |
| **Origem da medida** | Se o sinal vital veio de sensor ou de digitação manual. | `OrigemMedida` |
| **Sinal de alarme** | Critério que indica possível gravidade imediata (ex.: dor no peito, SpO₂ abaixo do limiar). Gera alerta imediato e prevalece sobre qualquer outra sugestão. | `RegraAlarme` |
| **Classificação de risco** | Prioridade clínica atribuída ao paciente. Sugerida pelo sistema, decidida pelo enfermeiro. | `ClassificacaoRisco` |
| **Prioridade sugerida** | Resultado do motor de classificação, antes da validação humana. | `PrioridadeSugerida` |
| **Validação** | Ato do enfermeiro de confirmar ou alterar a classificação e a especialidade. Alteração exige justificativa. | `Validacao` |
| **Reclassificação** | Alteração da prioridade após a validação (ex.: piora do paciente, tempo-alvo excedido). | `Reclassificacao` |
| **Especialidade** | Destino do atendimento dentro da unidade. | `Especialidade` |
| **Encaminhamento / direcionamento** | Envio do paciente validado para a fila de uma especialidade. | `EntradaFila` |
| **Fila** | Lista de pacientes validados por especialidade, ordenada por prioridade e tempo de espera. | `Fila` |
| **Tempo-alvo** | Tempo máximo de espera esperado para cada nível de prioridade. Excedido, gera alerta de reavaliação. | `TempoAlvo` |
| **Feedback de direcionamento** | Avaliação do médico sobre a especialidade sugerida (concorda ou não, e qual seria a correta). | `FeedbackDirecionamento` |
| **Conjunto de regras** | Versão das regras de classificação, em JSON, com ciclo rascunho → aprovada → ativa. | `ConjuntoRegras` |
| **Avaliador de regras** | Componente que interpreta um conjunto de regras e produz a sugestão. Existe em C# (referência) e na linguagem do totem. | `AvaliadorRegras` |
| **Casos de teste de regras** | Arquivos JSON com entradas e resultados esperados, usados para validar todos os avaliadores. | `regras/casos-de-teste/` |
| **Unidade** | Estabelecimento de saúde cliente (hospital, UPA, UBS). Corresponde a um tenant. | `Unidade` / `TenantId` |
| **Tenant** | Isolamento lógico de dados por unidade. | `TenantId` |
| **Nó local** | Servidor instalado na unidade que roda a API e o banco, funcionando sem internet. | — |
| **Totem** | Equipamento de autoatendimento com app de triagem e sensores. | — |
| **Outbox** | Registro local de mudanças pendentes de envio, usado para envio confiável e idempotente. | `outbox` |
| **Modo sombra** | Execução de um modelo de IA em paralelo, sem afetar o resultado, apenas para medir concordância. | — |
