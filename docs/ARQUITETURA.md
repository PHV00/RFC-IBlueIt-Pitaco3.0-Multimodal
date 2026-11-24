# Arquitetura IoT ponta a ponta

[Portal](../README.md) · [Índice](README.md) · [Integrações](INTEGRACOES.md)

**Natureza:** descrição da arquitetura integrada observada nos repositórios, com identificação explícita do que é experimental e do que ainda requer validação física. Atualizada com evidências de 23/11/2026.

## 1. Motivação e limites do sistema

O I Blue It já contempla um exergame respiratório, o PITACO, serviços de backend e um painel analítico. A extensão estudada adiciona aquisição PPG e processamento/visualização de medidas fisiológicas experimentais, preservando os dados respiratórios e o funcionamento legado. Por ser IoT, a integração não termina no firmware: dados seguem por um gateway/software local, internet, serviço remoto e aplicação web.

A conexão física do dispositivo com a Unity pode ser **USB**; a **internet é utilizada pela aplicação para comunicar-se com serviços Azure**. Isso configura um arranjo IoT com gateway de aplicação, não exige que o ESP32 acesse diretamente a internet. A adequação ao entendimento formal da banca deve ser demonstrada no diagrama.

## 2. C4 — contexto funcional (visão de alto nível)

~~~mermaid
flowchart LR
  P["Pessoa em sessão"] --> U["I Blue It · Unity / PC"]
  D["PITACO físico + sensores"] --> U
  U -->|"HTTPS"| A["API / Azure Functions"]
  A <--> DB[("MongoDB Atlas")]
  T["Profissional / usuário autorizado"] --> W["Health InfoCharts / navegador"]
  W -->|"HTTPS"| A
~~~

A Unity é o ponto de integração local do hardware e da sessão do jogo. A API oferece escrita e leitura; o dashboard consome as rotas autenticadas/escopadas. O controle de acesso é um requisito a verificar de maneira específica, não uma propriedade implícita da plataforma Azure.

## 3. Camadas IoT — nível de contêineres

~~~mermaid
flowchart TB
  subgraph S["Sensores e dispositivo"]
    R["MPX5010DP<br/>pressão/fluxo respiratório"]
    O["MAX30105<br/>PPG RED/IR"]
    E["ESP32-C3 · PITACO 2.4<br/>aquisição, contato, G1, telemetria"]
    R --> E
    O --> E
  end
  subgraph L["Gateway e processamento local"]
    B["Transporte USB/JSON<br/>e bridge Python opcional por fluxo"]
    U["Unity I Blue It 5.0<br/>PPG, BPM, SpO₂ DEMO, HUD,<br/>Mixer/DeepDDA"]
    B --> U
  end
  subgraph N["Rede e serviços"]
    H["Internet / HTTPS"]
    A["Azure Functions / Node.js<br/>API e contratos PITACO 3.0"]
    M[("MongoDB Atlas")]
    H --> A
    A <--> M
  end
  subgraph V["Aplicação web"]
    W["React / Health InfoCharts<br/>Resultados e Comparativos"]
  end
  E -->|"USB; BLE em variante suportada"| B
  U -->|"POST de sessão"| H
  W -->|"GET por HTTPS"| A
~~~

**Importante:** o middleware Python é canônico no repositório do dispositivo, mas os registros Unity também descrevem consumo de serial USB. Por isso, não o declaramos obrigatório para **todas** as variantes de operação; o caminho exato depende da integração/execução.

## 4. Componentes e responsabilidades

| Contêiner | Entradas | Processamento / saída | Fonte técnica |
| --- | --- | --- | --- |
| MAX30105 e MPX5010DP | Contato óptico, sinal respiratório | RED/IR e valores respiratórios | [PITACO / arquitetura](https://github.com/PHV00/PITACO_ESP32/blob/main/docs/ARCHITECTURE.md) |
| ESP32-C3 / firmware 2.4 | Sensores | G1, contato, timestamps/seq e telemetria; controle USB/BLE | [PITACO / firmware](https://github.com/PHV00/PITACO_ESP32/tree/main/frimeware2.4) |
| Unity | Amostras e estado do jogo | Janela ~8 s, BPM IR, SpO₂ DEMO RED/IR, HUD, resultados e integração experimental Mixer | [Unity / consolidação](https://github.com/PHV00/iblueit-psychophysiological-flow-pitaco2.4-port/blob/main/docs/marcos/CONSOLIDACAO_PITACO24_UNITY_PPG_DEEPDDA_HUD_2026-11-22.md) |
| Azure Functions | Escrita/consulta HTTP | Contratos, idempotência, consultas, validações | [Backend / Marco 04](https://github.com/PHV00/iblueit-server-side/blob/main/docs/MARCO_04_ENCERRAMENTO.md) |
| MongoDB Atlas | Sessões e séries | Persistência de fluxo, BPM, SpO₂ DEMO e RED/IR | [Backend / API](https://github.com/PHV00/iblueit-server-side/blob/main/docs/PITACO3_READ_API.md) |
| Health InfoCharts | API | Gráficos/resultados/comparativos e filtros | [Frontend / Marco 04](https://github.com/PHV00/iblueit-health-Infocharts/blob/main/docs/MARCO_04_ENCERRAMENTO.md) |

## 5. Detalhamento do processamento Unity

~~~mermaid
flowchart TD
  C["PitacoConnectionManager / SerialControllerPitaco24"] --> P["PpgProcessingCoordinator<br/>contato, G1, freshness, janela"]
  P --> BPM["BpmEstimator<br/>autocorrelação IR"]
  P --> SPO["Spo2Estimator<br/>RED/IR · DEMO"]
  BPM --> BR["OximetryDataBridge<br/>medição elegível + origem"]
  SPO --> BR
  P --> HUD["OximetryHudPresenter<br/>captura atual e média visual 60 s"]
  BR --> MM["MixerManager / MixerAdaptationGrid<br/>regras e recompensa experimental"]
  MM --> AG["DeepDDAAgent / ONNX legado<br/>16 observações preexistentes"]
~~~

- O **Gate 01** é decidido pelo firmware. Uma análise shadow do RAW na Unity não substitui a decisão do dispositivo.
- Dados inválidos, velhos ou sem contato não podem tornar-se medições utilizáveis por simples retenção visual.
- O histórico de 60 segundos do HUD é somente visual e não é a janela de 8 segundos dos estimadores.
- A SpO₂ DEMO tem bloqueio de elegibilidade em release comum e opt-in restrito ao desenvolvimento.
- A integração com DeepDDA é experimental **por regras/recompensas**. Não foi demonstrada a incorporação de PPG como novas entradas do modelo ONNX.

Fonte: [consolidação técnica Unity, 22/11/2026](https://github.com/PHV00/iblueit-psychophysiological-flow-pitaco2.4-port/blob/main/docs/marcos/CONSOLIDACAO_PITACO24_UNITY_PPG_DEEPDDA_HUD_2026-11-22.md).

## 6. Hierarquia de requisitos IoT

A representação abaixo organiza os requisitos da extensão em níveis e **não é um diagrama SysML normativo**; o diagrama de arquitetura em camadas acima cumpre a opção de arquitetura permitida no Playbook.

~~~mermaid
flowchart TD
  SYS["Sistema IoT: monitoramento complementar no I Blue It"]
  SYS --> S["R1 · Capturar sinais físicos"]
  SYS --> L["R2 · Processar/qualificar dados"]
  SYS --> N["R3 · Comunicar pela internet"]
  SYS --> W["R4 · Disponibilizar interface web"]
  SYS --> Q["R5 · Engenharia e rastreabilidade"]
  S --> S1["Fluxo respiratório + PPG RED/IR"]
  L --> L1["Integridade G1 + estimativas demonstrativas"]
  N --> N1["Unity → API Azure → MongoDB"]
  W --> W1["Resultados e comparativos"]
  Q --> Q1["CI/CD, análise estática, observabilidade e documentação"]
~~~

Os requisitos de engenharia são tratados individualmente em [CONFORMIDADE_IOT.md](CONFORMIDADE_IOT.md). Este diagrama organiza o escopo; não prova que todas as exigências já estejam atendidas.

## 7. Variantes e identificadores

| Nome encontrado | O que identifica, conforme a fonte | Não presumir |
| --- | --- | --- |
| PITACO 2.4 | Firmware ESP32-C3 canônico com MPX5010DP e MAX30105 | Que equivale ao sketch Uno ou ao binário físico usado em todos os logs |
| MAX30102 / Arduino Uno | Estudo de portabilidade no repositório de RFC | Que a integração ESP32-C3/Unity em bancada utiliza necessariamente esse sensor |
| PITACO 3.0 | Denominação nos contratos e telas de persistência/consulta | Que implica um firmware 3.0 publicamente versionado |
| I Blue It 5.0 | Base de jogo/Unity e ecossistema legado | Que toda ideia da RFC inicial foi implementada |
| I Blue It 6.0 | Nome da **proposta** registrado na RFC histórica | Que existe uma release clínica 6.0 validada |

A matriz montagem física → firmware gravado → branch Unity → payload → dashboard é uma **pendência de rastreabilidade**. Registrar hash/versão do firmware na inicialização e ambiente de cada teste é recomendado.

## 8. Decisões e riscos arquiteturais

1. Manter cada componente em seu repositório: reduz acoplamento de releases e conserva pipelines existentes.
2. Centralizar somente contratos, limites e visão sistêmica: evita múltiplas fontes divergentes.
3. Usar HTTPS/API para a nuvem, sem exigir conexão Wi-Fi direta do firmware.
4. Preservar as séries RAW para rastreabilidade; decimação no frontend afeta o desenho, não a base de dados documentada.
5. Não extrapolar a validade do SQI G1 para uma afirmação de qualidade fisiológica.
6. Não utilizar estimativas DEMO como gatilhos clínicos de alerta/pausa sem validação e autorização apropriadas.
7. Tratar autenticação, autorização por paciente, privacidade e observabilidade como requisitos auditáveis, não pressupostos.

[Seguir para a comunicação e os contratos →](INTEGRACOES.md)
