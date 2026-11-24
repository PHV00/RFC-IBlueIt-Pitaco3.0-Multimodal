# SQI — testes anteriores à implementação integrada

[Voltar ao portal IoT](../README.md) · [Índice técnico](../docs/README.md) · [Arquitetura integrada](../docs/ARQUITETURA.md)

> **Arquivo histórico de investigação e prototipagem, preservado para continuidade científica e técnica.** Os arquivos desta pasta não devem ser confundidos com o firmware canônico atualmente usado no PITACO integrado. A reorganização foi somente estrutural: **não altera algoritmos, limiares, pinagem, testes ou resultados** dos materiais arquivados.

## Objetivo deste arquivo

Concentrar estudos anteriores de firmware e validação conceitual da aquisição óptica PPG, do oxímetro MAX3010x/MAX30102 e dos mecanismos de Signal Quality Index (SQI). Esses estudos permitiram experimentar comunicação, aquisição, representação de amostras, controle de integridade e arquitetura modular antes da integração mais ampla com o PITACO, a Unity e a nuvem.

Seu propósito é servir como **referência histórica**, permitir reproduzir experimentos quando houver ambiente compatível e preservar material para evolução futura. A correspondência entre versões experimentais, firmware físico e integração atual deve ser comprovada pelos artefatos; não é presumida pelo nome de um diretório.

## Organização

~~~text
sqi-test-before-implementatiosn/
├── README.md
├── SQI_VERSION_SOURCES.md
├── spo2-wrist-sensor/
│   ├── ReadME.md
│   ├── docs/
│   ├── tests/
│   └── ... (código, dados e artefatos históricos)
└── spo2-wrist-sensor-arduino/
    ├── README.md
    └── ... (sketch, ensaios e decisões técnicas)
~~~

| Material | Conteúdo | Estado e função |
| --- | --- | --- |
| [spo2-wrist-sensor/](spo2-wrist-sensor/) | Protótipo ESP32/MAX3010x, G1, arquitetura futura G1–G4, estudos, testes, fundamentos e arquivos de investigação | **Pesquisa e testes**, não firmware canônico da integração final |
| [spo2-wrist-sensor-arduino/](spo2-wrist-sensor-arduino/) | Porta experimental Arduino Uno/MAX30102; testes de aquisição, compatibilidade e armazenamento de amostras | **Prova de conceito de portabilidade**, não prova de produção ESP32-C3 |
| [SQI_VERSION_SOURCES.md](SQI_VERSION_SOURCES.md) | Branches/commits preservados e versões de referência SQI (ESP32/Arduino, G1/G2, fault injection) | **Mapa de proveniência** para continuação dos estudos |

## O que já foi pesquisado e verificado

Na linha experimental `spo2-wrist-sensor`, a [documentação de estado](spo2-wrist-sensor/docs/IMPLEMENTATION_STATUS.md) registra implementação do G1 (integridade), 18 testes unitários host e cenários de injeção de falhas, além da arquitetura hierárquica de SQI. Esses resultados têm validade restrita ao ambiente/commit de cada experimento; **não comprovam** implantação do SQI hierárquico completo no firmware canônico.

[Os gates G2–G4](spo2-wrist-sensor/docs/sqi/PENDING_GATES_G2_G4.md) registram planos e fundamentos para pulsatilidade, morfologia e coerência RED/IR, mas não devem ser anunciados como funcionalidades finais implementadas.

O [estudo Arduino](spo2-wrist-sensor-arduino/README.md) aborda o MAX30102 e limitações da placa Uno; não o confundir com a linha ESP32-C3 + MAX30105 do PITACO 2.4.

## Relação com a arquitetura atual

~~~text
Protótipos e validações históricas (ESTA PASTA)
  ├─ aquisição PPG e condições de contato
  ├─ G1 e desenho de G2/G3/G4
  ├─ experimentos ESP32 / Arduino Uno
  └─ decisões e referências científicas
         |
         | conhecimento técnico e evolução de implementação
         v
PITACO integrado (OUTRO REPOSITÓRIO: PITACO_ESP32)
  ├─ ESP32-C3 / MPX5010DP / MAX30105 RED/IR
  ├─ firmware 2.4, G1 e comunicação
  └─ transmissão para Unity / I Blue It
         |
         v
Unity → Azure Functions → MongoDB → Health InfoCharts
~~~

- [Firmware PITACO 2.4 e bridge](https://github.com/PHV00/PITACO_ESP32) — fonte de verdade da implementação embarcada integrada (o acesso pode estar restrito).
- [Unity e integração PPG](https://github.com/PHV00/iblueit-psychophysiological-flow-pitaco2.4-port).
- [Backend](https://github.com/PHV00/iblueit-server-side) e [frontend](https://github.com/PHV00/iblueit-health-Infocharts).
- [Visão central e limitações](../docs/LIMITACOES.md).

**Sobre nomenclatura:** PITACO 2.4 é o nome do firmware canônico descrito no repositório do dispositivo; `PITACO 3.0` identifica uma evolução/contrato de integração nos repositórios API e frontend. Não declarar uma migração de cada arquivo deste arquivo histórico diretamente para uma versão `v3` sem rastreabilidade de commits e hardware.

## Regras de preservação e evolução

1. **Não usar protótipos desta pasta como código canônico** em demonstrações sem declarar a variante.
2. Não alterar conteúdos de experimentos passados para encaixá-los retroativamente no firmware atual.
3. Para evolução de algoritmos SQI, criar branch específica e relacionar origem, commit, configuração, referência e testes.
4. Para executar scripts, consultar caminhos atuais: a pasta foi movida; comandos antigos com caminhos absolutos devem ser ajustados.
5. Não interpretar G1 PASS como garantia de qualidade fisiológica ou calibração de SpO₂.
6. Uma limpeza posterior de artefatos gerados (compilações, logs, PDFs) deve ser **separada, revisada e não destrutiva**; esta mudança apenas organiza o arquivo inteiro.

---

[Voltar ao portal](../README.md) · [Evidências](../docs/EVIDENCIAS.md) · [Contribuições](../docs/CONTRIBUICOES.md)
