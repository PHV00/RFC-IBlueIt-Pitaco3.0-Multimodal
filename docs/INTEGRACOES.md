# Integrações e contratos entre repositórios

[Portal](../README.md) · [Arquitetura](ARQUITETURA.md) · [Evidências](EVIDENCIAS.md)

**Objetivo:** identificar os limites de responsabilidade e as interfaces que tornam possível a demonstração IoT ponta a ponta. Os contratos completos e valores de configuração permanecem nos repositórios proprietários.

## 1. Ciclo dos dados

~~~mermaid
sequenceDiagram
    participant S as Sensores
    participant F as PITACO / Firmware
    participant U as Unity / I Blue It
    participant A as Azure Functions
    participant M as MongoDB Atlas
    participant W as Health InfoCharts
    S->>F: pressão/fluxo e PPG RED/IR
    F->>U: USB/telemetria, contato, G1, seq, tempo
    U->>U: janela, BPM e SpO₂ DEMO; indicadores
    U->>A: POST sessão / séries por HTTPS
    A->>M: validação, idempotência e gravação
    W->>A: GET histórico, sessão e resumo
    A->>M: consulta de dados registrados
    A-->>W: séries e metadados
    W->>W: filtros, gráficos e comparativos
~~~

Este desenho representa o caminho de integração; não implica que toda execução da Unity já esteja consolidada na branch principal. Conferir [PR Unity #14](https://github.com/PHV00/iblueit-psychophysiological-flow-pitaco2.4-port/pull/14) ao reproduzir.

## 2. Interface sensor → firmware

- **Linha canônica PITACO 2.4:** ESP32-C3, MPX5010DP e MAX30105 nos canais RED/IR, com driver SparkFun local e verificações da FIFO.
- O firmware é responsável por leitura do sensor, sinalização de contato, decisão G1, continuidade e envio de telemetria.
- **G1 PASS não significa SpO₂ válida** e não substitui os gates de pulsatilidade/morfologia/coerência ainda estudados.
- Parâmetros reais, pinagem, comandos e protocolo: [PITACO / docs/firmware](https://github.com/PHV00/PITACO_ESP32/tree/main/docs/firmware) e [USB/BLE](https://github.com/PHV00/PITACO_ESP32/blob/main/docs/bridge/BLE_USB.md).

## 3. Interface firmware → Unity

**Transporte principal documentado na integração:** serial USB com amostras JSON PPG e eventos G1; comandos e fluxo respiratório legado preservados. O repositório PITACO contém também bridge USB/BLE Python; a necessidade de usá-la depende do caminho de execução.

**Campos conceituais relevantes:** amostras RED/IR, sequência, tempo do dispositivo, presença/contato, status G1, motivo de rejeição, sinais respiratórios. Este documento **não define um payload novo**; consultar a implementação do firmware e o parsing Unity para esquema exato e tipos.

A Unity:
1. identifica a porta/versão e evita conflitos de abertura de serial;
2. recebe mensagens por worker/fila, com instrumentação de backlog e perdas;
3. correlaciona tempo do firmware com o relógio monotônico;
4. aplica contato, G1, frescor e disponibilidade de janela PPG;
5. calcula BPM pelo IR e SpO₂ RED/IR **DEMO**, registrando rejeições;
6. diferencia medidas elegíveis, medição atual e valores retidos exclusivamente para o HUD.

**Referência de comportamento:** [consolidação Unity Marco 03](https://github.com/PHV00/iblueit-psychophysiological-flow-pitaco2.4-port/blob/main/docs/marcos/CONSOLIDACAO_PITACO24_UNITY_PPG_DEEPDDA_HUD_2026-11-22.md).

## 4. Interface Unity → API Azure

A persistência PITACO 3.0 registrada na API inclui quatro grupos de série: **fluxo respiratório**, **BPM**, **SpO₂ DEMO** e **PPG RAW RED/IR**.

O backend documenta:
- envio de sessão por **SendPlataformOverview**; contratos em [PITACO3_BACKEND_TESTING.md](https://github.com/PHV00/iblueit-server-side/blob/main/docs/PITACO3_BACKEND_TESTING.md);
- suporte a identificador de execução (clientRunId) e reenvio idempotente;
- respostas de criação/reenvio/conflito **HTTP 201 / 200 / 409**, conforme o contrato;
- persistência em documentos do histórico de plataforma e de dados de dispositivo.

**Não publicar** chaves, credenciais do Azure/Atlas, GameToken ou dados identificáveis de pacientes nas evidências.

## 5. Interface API → Health InfoCharts

Consultas principais documentadas:
- **GetPacientPlataformOverview**: histórico de partidas e filtros;
- **GetFlowDataDevice**: séries de uma execução;
- **GetPacientPlataformPhysiologySummary**: resumo fisiológico por execução;
- estatísticas/comparativos no escopo de conta associado ao GameToken.

O frontend preserva a SPA React e amplia **Resultados** e **Comparativos**, com filtros de fase/nível/qualidade/data, séries de fluxo/BPM/SpO₂ DEMO/RED-IR, cache e decimação de desenho para séries densas.

Fontes: [API de leitura](https://github.com/PHV00/iblueit-server-side/blob/main/docs/PITACO3_READ_API.md) e [regras do frontend](https://github.com/PHV00/iblueit-health-Infocharts/blob/main/docs/PITACO3_FRONTEND_REFERENCES.md).

## 6. Regras de confiabilidade e falhas

| Situação | Conduta documentada ou necessária |
| --- | --- |
| Sem dedo/contato inválido | Não transformar o sinal em medição fisiológica utilizável |
| G1 FAIL | Bloquear publicação elegível conforme pipeline implementado; registrar motivo |
| Janela incompleta ou temporização inválida | Aguardar dados suficientes / descartar leitura desatualizada |
| Unity travada temporariamente | Diferenciar lag da fila e amostra antiga; observar contadores de drops |
| Valor exibido por HOLD no HUD | Não reenviar como medição nova à IA ou à persistência |
| SpO₂ DEMO | Rotular explicitamente; não usar para segurança clínica |
| POST repetido | Aplicar a idempotência de clientRunId prevista no backend |
| Falha de backend / rede | Não afirmar sucesso de persistência sem resposta confirmada e verificação |
| Consulta entre pacientes | Verificar escopo de acesso; GameToken por conta não é autorização fina por pessoa |

## 7. Traço de integração de referência (23/11/2026)

O Marco 04 do backend registra **uma execução de engenharia** com HTTP 201 e documento persistido contendo 3.487 amostras de fluxo, 366 registros BPM, 366 SpO₂ DEMO e 4.374 pares RED/IR. Isso comprova aquela operação de escrita e leitura identificada, **não** calibração fisiológica, cobertura geral do sistema nem automação E2E do navegador.

Fonte: [Marco 04 backend](https://github.com/PHV00/iblueit-server-side/blob/main/docs/MARCO_04_ENCERRAMENTO.md).

## 8. Checklist para alterações futuras dos contratos

- [ ] Identificar componente originador, versão/commit e proprietário da interface.
- [ ] Atualizar parser/validação nas duas pontas sem quebrar dispositivos legados.
- [ ] Testar dados válidos, campos ausentes, valores nulos, repetição e falhas.
- [ ] Incluir teste automatizado do contrato no repositório relevante.
- [ ] Anexar evidência de sessão real quando aplicável.
- [ ] Atualizar esta visão integrada somente **após** o comportamento efetivamente mudar.

[Seguir para demonstração e reprodução →](EXECUCAO.md)
