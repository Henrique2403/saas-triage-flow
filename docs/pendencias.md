# Pendências

Documento vivo. Remover itens apenas quando resolvidos, registrando a decisão em ADR quando for o caso.

## Decisões técnicas em aberto

| Item | Depende de | Registro |
|---|---|---|
| Plataforma e linguagem do totem (.NET MAUI ou React Native; Flutter como alternativa) | Spike S1 | ADR-0007 |
| Conteúdo e formato final das regras de triagem | Licença do protocolo + validação clínica | `01-requisitos/regras-de-triagem.md` |

## Spikes (provas de conceito descartáveis)

| ID | Pergunta a responder | Critério de sucesso |
|---|---|---|
| S1 | Conseguimos ler um oxímetro BLE (perfil padrão) de forma confiável no hardware do totem? Comparar .NET MAUI e React Native. | Leituras estáveis, reconexão após perda de sinal, pareamento em < 60 s |
| S2 | O totem grava offline e reenvia ao nó sem duplicar? | 100% das triagens chegam uma única vez após quedas simuladas de rede |
| S3 | O nó roda num mini PC barato com atualização remota? | Atualização de imagem Docker via VPN sem intervenção local |

## Dependências externas

- **Licença do protocolo de classificação:** contato com o GBCR sobre uso do Protocolo de Manchester em software.
- **ANVISA:** avaliar enquadramento como software dispositivo médico (RDC 657/2022).
- **COFEN / CFM:** validar o desenho "sugestão do sistema, decisão do enfermeiro".
- **Hospital piloto:** aprovação da TI para equipamento próprio na rede e VPN de saída; observação do fluxo real (ficha antes ou depois da classificação).
- **Sensores:** escolha de oxímetro e termômetro com registro na ANVISA e Bluetooth LE com perfis padronizados.
- **Advisor de saúde:** validação das regras e dos casos de teste clínicos.

## Validação de produto

- As entrevistas com hospitais (20+) seguem sendo pré-requisito para confirmar o escopo. As decisões técnicas deste repositório assumem o escopo atual e devem ser revisadas se as entrevistas o mudarem.

## Frentes de concepção ainda não iniciadas

- **Frente 4:** arquitetura interna dos módulos e design patterns.
- **Frente 5:** padrões de desenvolvimento (código, Git, testes, CI/CD, observabilidade).
