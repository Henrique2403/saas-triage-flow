# Requisitos Não Funcionais — MVP

As metas numéricas são hipóteses iniciais e devem ser calibradas com as observações do piloto.

## Priorização dos atributos de qualidade

1. Segurança e privacidade
2. Corretude e auditabilidade clínica
3. Disponibilidade (autonomia local, sem depender de internet)
4. Usabilidade
5. Modificabilidade
6. Interoperabilidade com dispositivos
7. Desempenho
8. Escalabilidade (não bloquear, mas não otimizar no MVP)

## Requisitos

| ID | Categoria | Requisito | Meta |
|---|---|---|---|
| RNF01 | Segurança | Autenticação obrigatória para perfis profissionais | Sessão expira após 15 min de inatividade |
| RNF02 | Segurança | Controle de acesso por papel (RBAC) | Todo endpoint exige papel explícito |
| RNF03 | Segurança | Criptografia em trânsito e em repouso | TLS inclusive na rede local; disco do nó e do totem criptografado |
| RNF04 | Segurança | Auditoria de toda leitura e escrita de dados de paciente | Registros somente de inserção, garantido no banco |
| RNF05 | Segurança | Totem em modo quiosque | Nenhum acesso ao sistema operacional pelo paciente |
| RNF06 | Privacidade | Minimização de dados no totem | Dados apagados do totem após confirmação do nó |
| RNF07 | Privacidade | Logs sem dados pessoais | Nenhum nome, CPF, CNS ou dado clínico em log técnico |
| RNF08 | Disponibilidade | Nó local independente de internet | Operação indefinida sem internet |
| RNF09 | Disponibilidade | Autonomia do totem sem o nó | Pelo menos 8 horas de buffer |
| RNF10 | Disponibilidade | Recuperação após queda de energia | Serviços de volta automaticamente em < 5 min |
| RNF11 | Disponibilidade | Backup | Backup local frequente + cópia externa cifrada diária |
| RNF12 | Desempenho | Atualização de fila e alertas | < 2 s entre validação e exibição |
| RNF13 | Desempenho | Resposta das telas e da API | < 1 s no percentil 95 |
| RNF14 | Desempenho | Leitura de sensor | < 60 s incluindo pareamento |
| RNF15 | Usabilidade | Tempo de triagem no totem | 80% dos pacientes em até 5 min |
| RNF16 | Usabilidade | Acessibilidade | Referência WCAG 2.1 AA; alto contraste e áudio |
| RNF17 | Manutenibilidade | Mudança de regra sem deploy | Nova versão ativa sem recompilar |
| RNF18 | Manutenibilidade | Atualização remota | Nó e totem atualizados sem visita ao hospital |
| RNF19 | Manutenibilidade | Testes no domínio e nas regras | Alta cobertura nas regras; casos de teste compartilhados passando em todos os avaliadores |
| RNF20 | Interoperabilidade | Abstração de dispositivos | Novo modelo de sensor = novo adaptador, sem mudar o domínio |
| RNF21 | Portabilidade | Pronto para nuvem | TenantId, GUID v7 gerado no cliente e outbox desde o início |
| RNF22 | Observabilidade | Saúde do sistema | Health checks e logs estruturados no nó e no totem |
| RNF23 | Interoperabilidade | Compatibilidade do contrato da API | Dentro de uma versão maior (`/v1`), apenas mudanças aditivas; totens antigos continuam funcionando |
| RNF24 | Interoperabilidade | Contrato padronizado | OpenAPI publicado pela API; erros no formato Problem Details (RFC 9457); clientes gerados a partir do contrato |

## Cenários de qualidade

- **Segurança:** um usuário da Unidade A tenta acessar pela API uma triagem da Unidade B. O acesso é negado e registrado.
- **Auditabilidade:** um médico questiona uma classificação. O sistema mostra as respostas, a versão das regras, os critérios disparados e o enfermeiro que validou, com data e hora.
- **Modificabilidade:** uma mudança de regra aprovada entra em produção sem deploy; triagens antigas mantêm referência à versão que as gerou.
- **Disponibilidade:** a API reinicia durante o plantão. A fila volta em menos de 1 minuto, sem perda de triagens confirmadas.
- **Usabilidade:** um paciente de 70 anos sem familiaridade com tecnologia conclui a triagem em até 5 minutos sem ajuda.
- **Desempenho:** após a validação, o paciente aparece no dashboard do especialista em menos de 2 segundos.
