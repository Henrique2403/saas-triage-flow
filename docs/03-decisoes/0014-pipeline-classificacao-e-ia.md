# ADR-0014: Pipeline de classificação e estratégia de IA

- **Status:** Aceita
- **Data:** 2026-09-28

## Contexto
O MVP não tem dados para treinar IA. A IA conversacional (LLM) conflita com o requisito de offline, levanta questões de LGPD e de segurança clínica.

## Decisão
- A classificação é um pipeline de etapas na API: (1) regras de alarme, (2) regras de protocolo, (3) modelo de direcionamento (fase 2).
- Regras de alarme sempre prevalecem; nenhum modelo pode rebaixar um alarme.
- A decisão final é sempre do enfermeiro.
- Fase 2: modelo treinado em Python, exportado em ONNX e executado na API com ONNX Runtime, localmente e offline. Versionado como as regras. Ativado só após período em **modo sombra** com concordância medida.
- **IA conversacional fica fora do MVP.** Se entrar, apenas para transformar relato livre em respostas estruturadas, com o questionário guiado como alternativa.
- Comunicação comercial: "apoio à decisão baseado em protocolo", não "triagem por IA", no MVP.

## Consequências
- (+) IA entra sem reescrever o fluxo.
- (+) Segurança clínica garantida por desenho.
- (−) O MVP não terá o apelo comercial de "IA".
