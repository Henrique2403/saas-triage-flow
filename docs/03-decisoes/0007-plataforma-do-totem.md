# ADR-0007: Plataforma e linguagem do app do totem

- **Status:** Proposta (depende do spike S1)
- **Data:** 2026-09-28

## Contexto
O maior risco técnico do totem é a leitura confiável de sensores via Bluetooth LE em modo quiosque. Com os dashboards em TypeScript (ADR-0015), o ideal é manter o projeto em duas linguagens.

## Opções em avaliação
| Opção | Linguagens do projeto | Observações |
|---|---|---|
| .NET MAUI (Android ou Windows) | C# + TypeScript | Reaproveita C#; ecossistema menor e histórico de instabilidade |
| React Native (Android) | C# + TypeScript | Reaproveita TypeScript; BLE via bibliotecas de terceiros |
| Flutter (alternativa) | C# + TypeScript + Dart | BLE maduro; adiciona uma terceira linguagem |

## Critério de decisão
O spike S1 implementa a leitura de um oxímetro BLE (perfil padrão) com MAUI e React Native. Vence a opção com leituras estáveis, reconexão confiável, pareamento em menos de 60 s e modo quiosque viável. Flutter só entra se as duas falharem.

## Consequências
A decisão define também a plataforma do hardware (tablet Android ou mini PC Windows).
