# Evidências e rastreabilidade de engenharia

[Portal](../README.md) · [Execução](EXECUCAO.md) · [Conformidade IoT](CONFORMIDADE_IOT.md)

**Corte do inventário: 23/11/2026.** Não confundir documento de fechamento, teste local relatado, pipeline automatizado e execução física completa. Os links referenciam fontes publicadas por seus repositórios; PITACO_ESP32 pode exigir permissão.

## 1. Matriz por componente

| Componente | Evidência disponível | O que permite concluir | O que NÃO permite concluir |
| --- | --- | --- | --- |
| SQI — arquivos de testes anteriores à integração | [Status de implementação](../sqi-test-before-implementatiosn/spo2-wrist-sensor/docs/IMPLEMENTATION_STATUS.md); [testes G1](../sqi-test-before-implementatiosn/spo2-wrist-sensor/docs/sqi/G1_VALIDATION_REPORT.md) | 18 testes host relatados, G1 e fault injection em linha de estudo | G2–G4 entregues, qualidade fisiológica ou calibração clínica |
| PITACO 2.4 | [Marco 03 do firmware](https://github.com/PHV00/PITACO_ESP32/blob/main/docs/marcos/MARCO_03_FECHAMENTO_PPG_SQI_2026-11-22.md); [workflow](https://github.com/PHV00/PITACO_ESP32/tree/main/.github/workflows) | Experimento RED_FLAT 350/IR 500 no código e indicadores G1 em bancada | Identificar binário realmente gravado; eficácia clínica dos thresholds |
| Unity | [Marco 03 Unity](https://github.com/PHV00/iblueit-psychophysiological-flow-pitaco2.4-port/blob/main/docs/marcos/CONSOLIDACAO_PITACO24_UNITY_PPG_DEEPDDA_HUD_2026-11-22.md) | Logs de série, BPM, estado G1, HUD, relógio/fila e caminho experimental Mixer | Treinamento novo do ONNX, acurácia fisiológica ou Test Runner aprovado em todas as cenas |
| Backend | [Marco 04 backend](https://github.com/PHV00/iblueit-server-side/blob/main/docs/MARCO_04_ENCERRAMENTO.md); [Actions](https://github.com/PHV00/iblueit-server-side/actions/runs/45204560332) | Testes e deploy aprovados; operação HTTP 201/Atlas documentada | Segurança/autorizações finas e desempenho de todas as consultas em produção |
| Frontend | [Marco 04 frontend](https://github.com/PHV00/iblueit-health-Infocharts/blob/main/docs/MARCO_04_ENCERRAMENTO.md); [Actions](https://github.com/PHV00/iblueit-health-Infocharts/actions/runs/45227836897) | Build/testes/deploy aprovados; funcionamento relatado de Resultados/Comparativos | E2E automatizado autenticado no navegador ou acesso anônimo aos dados |
| Integração persistente | [Marco 04 backend](https://github.com/PHV00/iblueit-server-side/blob/main/docs/MARCO_04_ENCERRAMENTO.md); [Unity PR #14](https://github.com/PHV00/iblueit-psychophysiological-flow-pitaco2.4-port/pull/14) | Sessão gravada na nuvem com quatro séries; evolução de integração em Unity | Que a PR de Unity foi incorporada e ensaiada fisicamente em todas as variantes |

## 2. Evidência numérica documentada

### PITACO 2.4 + Unity — bancada, 22/11/2026

O registro de bancada descrito pelos documentos Marco 03 reporta:
- **34 janelas G1:** 30 PASS, quatro FAIL;
- **315 / 338** tentativas de BPM publicadas no Unity;
- **zero** inválidos JSON/amostra e zero drops contados nas categorias relatadas;
- **um stall Unity de 4,70 s**, com backlog e recuperação da fila;
- resultados experimentais sujeitos à condição de cabos, contato e versão desconhecida do firmware físico.

A taxa de publicações **não é precisão fisiológica**. A estatística de amplitude shadow da Unity não prova o threshold aplicado pelo firmware.

### Backend — sessão de engenharia, 23/11/2026

O [Marco 04 do backend](https://github.com/PHV00/iblueit-server-side/blob/main/docs/MARCO_04_ENCERRAMENTO.md) registra HTTP 201 e:
- 3.487 amostras de fluxo;
- 366 valores/entradas BPM (incluindo nulos);
- 366 entradas de SpO₂ DEMO;
- 4.374 pares RED/IR.

É evidência de persistência daquela sessão, **não** de calibração nem benchmark de disponibilidade geral.

## 3. Rastreamento de PRs

- Unity: [consolidação Marco 03 — PR #13](https://github.com/PHV00/iblueit-psychophysiological-flow-pitaco2.4-port/pull/13); [persistência ponta a ponta — PR #14](https://github.com/PHV00/iblueit-psychophysiological-flow-pitaco2.4-port/pull/14).
- Backend: [PR #2](https://github.com/PHV00/iblueit-server-side/pull/2), [PR #3](https://github.com/PHV00/iblueit-server-side/pull/3).
- Frontend: [PR #4](https://github.com/PHV00/iblueit-health-Infocharts/pull/4), [PR #5](https://github.com/PHV00/iblueit-health-Infocharts/pull/5), [PR #6](https://github.com/PHV00/iblueit-health-Infocharts/pull/6), [PR #7](https://github.com/PHV00/iblueit-health-Infocharts/pull/7).

As referências são um **índice**, não um atestado de estado atual do merge. Usar GitHub para verificar a revisão exata ao apresentar.

## 4. Checklist de auditoria para nova evidência

- [ ] Data/hora, ambiente e objetivo do ensaio.
- [ ] Repo, branch, commit e, se hardware, versão/binário gravado.
- [ ] Montagem, sensor e transporte efetivamente usados.
- [ ] Entrada, saída e resultado esperado/obtido.
- [ ] Log ou artefato anonimizado, com origem e identificadores correlacionáveis.
- [ ] Falhas/limitações e como repetir.
- [ ] Link de issue, PR, Action ou documento versionado.
- [ ] Distinção entre teste sintético, bancada, E2E de engenharia e validação clínica.

## 5. Evidências a acrescentar nas próximas etapas

- [ ] Saída de análise estática e segurança por componente com versão da ferramenta e pipeline.
- [ ] Evidência de observabilidade: logs PITACO, Unity e Azure correlacionados por sessão/evento.
- [ ] Teste físico reproduzível identificando firmware gravado, Unity e cloud.
- [ ] Capturas anonimizadas da demonstração web hospedada.
- [ ] Verificação formal de acesso público ao código do firmware essencial.
- [ ] Matriz requisito da RFC → commit → teste → status, após revisão da RFC.

[Ver riscos correspondentes →](LIMITACOES.md)
