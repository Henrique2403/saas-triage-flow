# Documento de Visão — Agiliza Triagem

## 1. Problema

A triagem inicial em unidades do SUS costuma ser manual, desorganizada e dependente de papel. Isso gera filas mal priorizadas, pacientes encaminhados à especialidade errada, informação dispersa entre recepção e equipe clínica e desperdício de tempo profissional.

## 2. Proposta

Um sistema de **apoio à decisão** para a triagem inicial: um totem de autoatendimento coleta queixas e sinais vitais, um motor de classificação sugere prioridade e especialidade, e a equipe de enfermagem valida antes de o paciente entrar na fila. O sistema não substitui a decisão clínica: organiza, agiliza e registra.

## 3. Atores

| Ator | Papel no sistema |
|---|---|
| Paciente | Faz a triagem no totem, sem login |
| Enfermeiro(a) | Valida ou altera a classificação; faz triagens assistidas |
| Recepção | Vincula a identificação civil; chama pacientes |
| Médico especialista | Recebe pacientes; registra feedback do direcionamento |
| Administrador da unidade | Gerencia usuários, perfis e especialidades |
| Administrador clínico | Gerencia e aprova versões das regras |
| Suporte técnico | Atualiza e mantém o nó local remotamente |

## 4. Escopo do MVP

- Uma unidade piloto em São Paulo.
- Sistema rodando inteiramente na rede local da unidade (ver ADR-0001).
- Totem com questionário guiado e sensores de SpO₂/frequência cardíaca e temperatura, com entrada manual como alternativa.
- Funcionamento sem internet.
- Classificação por regras versionadas, com validação obrigatória por enfermeiro.
- Fila em tempo real, notificação ao especialista, feedback de direcionamento e relatórios básicos.

## 5. Fora do escopo do MVP

- Nuvem, sincronização e múltiplas unidades.
- Inteligência artificial (modelo de direcionamento e IA conversacional).
- Integração com e-SUS APS, CadSUS, RNDS e prontuário eletrônico.
- Faturamento automático.
- Aplicativo mobile.

## 6. Restrições principais

- Um único desenvolvedor; MVP estimado em 5 a 6 meses.
- Backend em .NET; clientes em tecnologias independentes (ADR-0008).
- Dados de saúde sob a LGPD; classificação de risco validada por enfermeiro (COFEN); possível enquadramento na ANVISA.
- Infraestrutura hospitalar com internet instável e equipamentos antigos.
- Protocolo de classificação sujeito a licença.

## 7. Critérios de sucesso do piloto

- 80% ou mais das triagens da unidade feitas pelo sistema após 30 dias.
- Concordância do direcionamento sugerido com a decisão profissional medida e acompanhada.
- Redução do tempo entre chegada e classificação.
- Nenhum incidente de segurança de dados.
