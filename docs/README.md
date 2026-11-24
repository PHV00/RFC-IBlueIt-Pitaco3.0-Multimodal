# Documentação técnica central — I Blue It IoT

[Voltar ao portal principal](../README.md)

**Propósito:** oferecer uma entrada única para compreender, demonstrar e auditar a melhoria de monitoramento PPG/SQI integrada ao ecossistema I Blue It. Documentação de integração não substitui a documentação interna de firmware, Unity, API ou frontend.

**Corte de evidências:** 23/11/2026. Estados posteriores devem ser atualizados com PR, commit, teste ou registro verificável.

## Trilhas de leitura

| Perfil / objetivo | Ordem sugerida |
| --- | --- |
| Avaliador IoT | [Portal](../README.md) → [Arquitetura](ARQUITETURA.md) → [Conformidade](CONFORMIDADE_IOT.md) → [Evidências](EVIDENCIAS.md) |
| Desenvolvedor de integração | [Arquitetura](ARQUITETURA.md) → [Contratos](INTEGRACOES.md) → [Execução](EXECUCAO.md) → repositório especializado |
| Avaliador de contribuição/autoria | [Contribuições](CONTRIBUICOES.md) → [Evidências](EVIDENCIAS.md) → PRs/commits originais |
| Pesquisador SQI | [Projeto SQI](../sqi-test-before-implementatiosn/spo2-wrist-sensor/ReadME.md) → [Status G1](../sqi-test-before-implementatiosn/spo2-wrist-sensor/docs/IMPLEMENTATION_STATUS.md) → [Gates futuros](../sqi-test-before-implementatiosn/spo2-wrist-sensor/docs/sqi/PENDING_GATES_G2_G4.md) |
| Usuário da demonstração | [Execução](EXECUCAO.md) → [Limitações](LIMITACOES.md) |

## Documentos centrais

- [ARQUITETURA.md](ARQUITETURA.md) — C4/contexto, camadas IoT, variantes e limites de responsabilidade.
- [INTEGRACOES.md](INTEGRACOES.md) — interfaces, fluxo da telemetria, armazenamento e tratamento de falhas.
- [CONTRIBUICOES.md](CONTRIBUICOES.md) — herança do LARVA, melhorias, autoria e maturidade.
- [EXECUCAO.md](EXECUCAO.md) — como obter a demonstração e reproduzir verificações existentes.
- [EVIDENCIAS.md](EVIDENCIAS.md) — rastreabilidade para Actions, marcos e testes.
- [LIMITACOES.md](LIMITACOES.md) — riscos técnicos, incertezas e limites clínicos.
- [CONFORMIDADE_IOT.md](CONFORMIDADE_IOT.md) — requisito → solução → evidência → pendência.
- [RFC preservada](rfc/RFC_v1.5_original.md) — proposta antiga completa, intencionalmente não reescrita.

## Onde continuar nos outros repositórios

| Área | Fonte de verdade técnica |
| --- | --- |
| Firmware PITACO (código, pinagem, thresholds, USB/BLE, G1) | [PITACO_ESP32 / docs](https://github.com/PHV00/PITACO_ESP32/tree/main/docs) — acesso condicionado à visibilidade do repo |
| [Arquivo experimental de SQI e suas versões](../sqi-test-before-implementatiosn/README.md) | [spo2-wrist-sensor / docs](../sqi-test-before-implementatiosn/spo2-wrist-sensor/docs) |
| Arduino Uno MAX30102 (estudo arquivado) | [spo2-wrist-sensor-arduino](../sqi-test-before-implementatiosn/spo2-wrist-sensor-arduino) |
| Unity, HUD, Mixer/DeepDDA e testes | [Unity / docs](https://github.com/PHV00/iblueit-psychophysiological-flow-pitaco2.4-port/tree/main/docs) |
| Azure Functions/API | [Backend / docs](https://github.com/PHV00/iblueit-server-side/tree/main/docs) |
| React e gráficos | [Frontend / docs](https://github.com/PHV00/iblueit-health-Infocharts/tree/main/docs) |
| Pesquisas e código originários | [UDESC-LARVA](https://github.com/UDESC-LARVA) |

## Regras de manutenção — uma fonte de verdade por assunto

1. **Mudança local** de limiar, algoritmo ou contrato: atualizar primeiro o repositório proprietário do código, com PR e evidência.
2. **Mudança de comportamento entre sistemas**: atualizar aqui ARQUITETURA/INTEGRACOES, apontando para o PR/commit da origem.
3. **Evidência**: não copiar um log bruto ou print sem contexto; indicar data, branch/commit, ambiente, resultado, restrições e fonte.
4. **Requisito proposto ≠ funcionalidade entregue**: diferenciar explicitamente; a RFC antiga não é um retrato automático do produto.
5. **Dados sensíveis**: não publicar nomes de pacientes, GameTokens, credenciais, chaves de conexão ou amostras vinculadas a identidade.
6. **Documentação antiga**: preservar para auditoria; sinalizar obsolescência, nunca reescrever o passado como se já estivesse implementado.
7. **Versões**: não tratar PITACO 2.4, PITACO 3.0, MAX30102 e MAX30105 como iguais sem identificar a montagem e a branch do teste.

Esta pasta é o **equivalente navegável em Markdown** proposto para o requisito de documentação. A aceitação formal como Wiki ou equivalente depende da avaliação; se for exigida uma GitHub Wiki, ela poderá apontar para os mesmos arquivos sem duplicação editorial.
