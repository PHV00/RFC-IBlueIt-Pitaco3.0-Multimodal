# Contribuições, escopo e relação com o legado

[Portal](../README.md) · [Arquitetura](ARQUITETURA.md) · [Evidências](EVIDENCIAS.md)

## 1. Proveniência e autoria

O **I Blue It** e o ecossistema multimodal/Flow Psicofisiológico são produtos de trabalhos anteriores da equipe [UDESC-LARVA](https://github.com/UDESC-LARVA). O projeto deste portfólio é uma **extensão e integração experimental**, e não a criação original do exergame, do PITACO ou do DeepDDA.

Fontes históricas para delimitar a base: [IBlueIt/Flow](https://github.com/UDESC-LARVA/iblueit-psychophysiological-flow), [I Blue It server-side](https://github.com/UDESC-LARVA/iblueit-server-side), [Health InfoCharts](https://github.com/UDESC-LARVA/iblueit-health-Infocharts) e [RFC preservada](rfc/RFC_v1.5_original.md).

## 2. Resumo do trabalho desenvolvido

| Frente | Base preexistente | Extensão/incremento documentado | Evidência e ressalva |
| --- | --- | --- | --- |
| PITACO | Dispositivo de aquisição respiratória | Coleta PPG RED/IR, controle de contato, G1 e telemetria na linha ESP32-C3 | [PITACO / arquitetura](https://github.com/PHV00/PITACO_ESP32/blob/main/docs/ARCHITECTURE.md); limites de hardware/limiares |
| SQI (protótipos preservados) | Coleta PPG e algoritmos preliminares | Projeto modular hierárquico, implementação G1 com fail-fast, 18 testes host na linha SQI e fault injection | [Status SQI](../sqi-test-before-implementatiosn/spo2-wrist-sensor/docs/IMPLEMENTATION_STATUS.md); G2–G4 ainda pendentes |
| Arduino MAX30102 | Portabilidade não resolvida no projeto de investigação | Estudo de sketch para Uno e decisões de armazenamento/18 bits | [Estudo Arduino](../sqi-test-before-implementatiosn/spo2-wrist-sensor-arduino/README.md); não confundir com firmware canônico ESP32-C3 |
| Unity | Exergame 5.0 / DeepDDA | Leitura PPG, tempo/fila, BPM IR, SpO₂ DEMO, estados de validade, HUD e recompensa experimental | [Marco 03 Unity](https://github.com/PHV00/iblueit-psychophysiological-flow-pitaco2.4-port/blob/main/docs/marcos/CONSOLIDACAO_PITACO24_UNITY_PPG_DEEPDDA_HUD_2026-11-22.md) |
| Backend | Azure Functions e MongoDB do I Blue It | Contratos de quatro séries, idempotência, consultas e resumos fisiológicos | [Marco 04 backend](https://github.com/PHV00/iblueit-server-side/blob/main/docs/MARCO_04_ENCERRAMENTO.md) |
| Frontend | Health InfoCharts React | Visualização das quatro séries, comparativos e filtros específicos | [Marco 04 frontend](https://github.com/PHV00/iblueit-health-Infocharts/blob/main/docs/MARCO_04_ENCERRAMENTO.md) |
| Engenharia | Repositórios e pipelines próprios | Documentação central e correlação de evidências IoT; CI/CD mantido em cada componente | [Este portal](README.md), [Conformidade](CONFORMIDADE_IOT.md) |

## 3. Contribuições que **não** são alegadas

- Não foi criada uma plataforma clínica certificada nem um oxímetro validado.
- A SpO₂ demonstrativa não comprova saturação real.
- O Gate 01 não equivale ao SQI G1–G4 totalmente implementado.
- Não é correto dizer que novas variáveis PPG foram adicionadas diretamente às 16 observações ONNX preexistentes do DeepDDA.
- As propostas de pausa clínica, alertas e segurança da RFC histórica não devem ser declaradas entregues apenas por existirem no documento.
- O deploy Azure e os testes existentes não provam, por si só, que todo teste de ponta a ponta com hardware e navegador foi concluído em produção.

## 4. Como auditar autoria sem duplicar código

1. Acesse a frente na tabela acima e identifique **PR, diff e commit**.
2. Compare o repositório original UDESC-LARVA com o fork/extensão relevante, quando aplicável.
3. Observe os testes e registros de integração associados à alteração.
4. Separe contribuições do firmware canônico, protótipos de pesquisa SQI e adaptações posteriores da Unity/backend/frontend.
5. Para apresentações, demonstre um caminho concreto: aquisição → elegibilidade → sessão → API → dashboard, deixando explícitas as limitações.

Uma descrição de autoria acadêmica detalhada deve identificar colaboradores e as fontes de cada alteração; links de repositórios isoladamente não substituem essa investigação.

## 5. Estratégia documental

Este portal descreve **o comportamento conjunto**. Ajustes de implementação, parâmetros e testes devem ser mantidos junto ao código correspondente. Históricos de PR e documentos de marco preservam decisões passadas e não devem ser reescritos para parecerem conclusões posteriores.

[Ver evidências por componente →](EVIDENCIAS.md)
