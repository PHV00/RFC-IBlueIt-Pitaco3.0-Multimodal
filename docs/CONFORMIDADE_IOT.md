# Conformidade com o Playbook — categoria IoT

[Portal](../README.md) · [Evidências](EVIDENCIAS.md) · [Limitações](LIMITACOES.md)

**Base normativa:** [The Portfolio Playbook — linha IoT](https://github.com/CatolicaSC-Portfolio/The-Portfolio-Playbook/blob/main/directions/portfolio-directions-iot.md) e [Direcionamentos gerais](https://github.com/CatolicaSC-Portfolio/The-Portfolio-Playbook/blob/main/directions/portfolio-directions-GERAL.md). **Avaliação provisória técnica**, não parecer oficial da banca. Estado de evidências: 23/11/2026.

Legenda: **Evidenciado** = há código/teste/registro associado; **Parcial** = existente, mas requisito completo ou comprovação não fechados; **Pendente** = sem evidência suficiente nesta auditoria.

## 1. Obrigatórios

| Exigência | Evidência e encaminhamento | Estado |
| --- | --- | --- |
| Requisitos + arquitetura IoT fundamentados | [Arquitetura em camadas](ARQUITETURA.md) e [RFC histórica](rfc/RFC_v1.5_original.md); falta conciliar requisitos propostos com o estado implementado | **Parcial** |
| Implementação aderente ao problema | PITACO/Unity/API/frontend, com [contribuições](CONTRIBUICOES.md); verificar variantes físicas e requisito a requisito | **Parcial** |
| Código-fonte + artefatos especificados em repositório público | Repos públicos de pesquisa/Unity/backend/frontend; [PITACO_ESP32](https://github.com/PHV00/PITACO_ESP32) aparecia **privado** | **Parcial / risco** |
| Internet na solução IoT | [Unity → Azure Functions → Atlas → React](INTEGRACOES.md), via HTTPS | **Evidenciado no fluxo registrado** |
| Interação web/mobile | [Health InfoCharts / Resultados](EXECUCAO.md) e Comparativos | **Evidenciado** |
| Aplicação web pública estável | URL Azure e [Action de deploy](EVIDENCIAS.md); autor relata aplicação acessível, sem teste independente de uptime | **Parcial na comprovação de estabilidade** |
| Wiki ou equivalente | Portal README + [índice navegável](README.md) com documentação versionada; validar aceitação do formato com avaliador | **Implementado como equivalente; aceite a confirmar** |
| CI/CD da aplicação | [Backend](https://github.com/PHV00/iblueit-server-side/actions/runs/45204560332) e [Frontend](https://github.com/PHV00/iblueit-health-Infocharts/actions/runs/45227836897) com deploy aprovado | **Evidenciado** |
| Análise estática e segurança | Necessário pipeline com analisador C/C++, Unity C#, JS/backend/frontend e resultados; não apenas compilar | **Pendente** |
| Monitoramento ou observabilidade + telemetria do dispositivo | Logs PITACO/G1, fila Unity, HTTP Azure existem; falta roteiro de correlação e evidência de consulta/retensão | **Parcial** |

**Atenção:** o Playbook não exige, para esta linha, um percentual fixo de cobertura de teste. Não dispensar testes das rotinas críticas.

## 2. Desejáveis

| Exigência | Estado e referência |
| --- | --- |
| Hardware, esquemáticos, pinos e conectividade | **Parcial:** [documentação PITACO](https://github.com/PHV00/PITACO_ESP32/tree/main/docs) e [estudo Arduino Uno](../spo2-wrist-sensor-arduino/README.md); consolidar o esquema físico da montagem apresentada |
| Escolhas de protocolo justificadas | **Parcial:** USB/JSON, BLE conforme variante, HTTPS para nuvem; [interfaces](INTEGRACOES.md) descrevem usos; aprofundar trade-offs |
| Testes de aplicação e processamento | **Evidenciado em partes:** testes backend, frontend e G1; [matriz de evidências](EVIDENCIAS.md) |
| Testes de rotinas críticas do firmware | **Parcial:** G1 e host compilação/testes, mas falta ciclo físico reproduzível e ensaio de falha completo da configuração final |

## 3. Diferenciais documentáveis

| Diferencial | Evidência / cautela |
| --- | --- |
| Aplicação web | Health InfoCharts com resultados e comparativos |
| Dados IoT usados em dashboard e IA | Dashboard com PPG/medidas e integração experimental Mixer/DeepDDA; **não** alegar novo treino de modelo |
| Prática de cibersegurança | Idempotência e controles existentes não bastam, por si, para provar uma prática de segurança integral; selecionar e comprovar |
| Integração com sistema externo | Vários serviços Azure/Atlas e ecossistema I Blue It originalmente existente |

## 4. Antipadrões e enquadramento

- Não é implementação apenas embarcada: há internet, API, banco e web.
- Não se apresenta como solução simulada: existem relatos de bancada e persistência reais, com limites claros.
- É uma extensão de sistema de pesquisa preexistente; deixar evidente a parte autoral e a licença/atribuição das bases.
- A declaração de compatibilidade IoT não equivale à aprovação final do portfólio; permanecem requisitos acadêmicos gerais, orientações, autoria, validação e apresentação.

## 5. Próximas ações na ordem acordada

1. **Documentação (esta etapa):** centralizar, criar diagramas e fonte de verdade; revisar vínculos/versões e habilitar navegação equivalente à Wiki.
2. **Análise estática e segurança:** criar/verificar pipelines e registrar qualidade por repo.
3. **RFC:** revisar promessa vs implementação, atualizar requisitos e modelos sem apagar a versão histórica.
4. **Observabilidade:** correlacionar logs PITACO, Unity e Azure por sessão, documentar exemplos e cobertura real.

**Pendência transversal que não deve esperar a RFC:** disponibilização adequada de código do firmware para auditabilidade pública, caso necessária e autorizada, e registro da variante física demonstrada.

[Voltar ao README principal](../README.md)
