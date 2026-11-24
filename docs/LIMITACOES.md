# Limitações, riscos e fronteiras de validade

[Portal](../README.md) · [Evidências](EVIDENCIAS.md) · [Arquitetura](ARQUITETURA.md)

> **Escopo experimental/engenharia:** não há demonstração de dispositivo médico validado, estimativa clínica calibrada de SpO₂ nem política clínica de interrupção da terapia pronta para uso.

## 1. Limitações críticas

| Área | Limitação observada ou registrada | Consequência para documentação e demonstração |
| --- | --- | --- |
| SpO₂ | Estimador **DEMO**, sem calibração; depende da relação RED/IR | Não apresentar valores como saturação clínica, não acionar alarmes/pausas clínicas |
| BPM | Autocorrelação e heurísticas podem publicar candidatos em artefatos | Não interpretar taxa de publicação como acurácia fisiológica |
| SQI | G1 verifica integridade mínima; G2, G3, G4 pendentes na linha hierárquica | G1 PASS não é certificado de PPG utilizável para fisiologia |
| Limiares | RED_FLAT 350, IR 500 e outros parâmetros são experimentais | Exigem registros RAW comparáveis, critério de calibração e verificação no firmware físico |
| Montagem | Acoplamento óptico, cabos, pressão/dedo e luz podem alterar o sinal | Não generalizar um ensaio de bancada para outro paciente ou configuração |
| Binário | Log de Unity não identifica com segurança a versão gravada no ESP32 | Documentar placa, firmware, hash e versão de cada teste |
| Variantes | MAX30102/Uno, MAX30105/ESP32-C3 e PITACO 3.0 têm documentos diferentes | Não assumir hardware idêntico apenas por nome do produto |
| Unity | Stall de cena/fila e diferenças entre medição atual e HUD retido | Analisar atraso e validade de tempo; não reutilizar HOLD como medida nova |
| DeepDDA | Integração indireta e opt-in experimental para DEMO; modelo ONNX legado | Não alegar novo treino da IA nem segurança baseada na oximetria |
| API/segurança | Escopo por GameToken não equivale a permissão granular por paciente | Não afirmar isolamento completo entre usuários/pacientes |
| Leitura da nuvem | Séries completas podem custar tempo; frontend decima para desenho | Não anunciar desempenho escalável sem medições controladas |
| Publicidade do código | Firmware canônico aparecia privado em 23/11/2026 | Avaliação externa pode não conseguir auditar o componente |
| Observabilidade | Existem logs locais/cloud, porém correlação, retenção e consulta centralizada ainda precisam de evidências | Demonstrar rastreio por sessão antes de declarar requisito plenamente atendido |

## 2. Segurança e pesquisa

- A proposta de usar SpO₂ para interromper exercícios aparece na **RFC histórica**; não há validação clínica que autorize usar a SpO₂ DEMO para tal finalidade.
- Valores de referência genéricos exibidos no dashboard são **educativos**, não limites personalizados ou prescrição terapêutica.
- O comportamento implementado deve manter rejeição de medições inválidas, rotulagem clara e não propagação indevida para decisões.
- Evidências públicas devem ser anonimizadas; credenciais, tokens, dados pessoais e arquivos brutos vinculáveis a pacientes não devem ser incluídos.
- Os registros científicos e as versões históricas ajudam a formular hipóteses; **não** garantem que parâmetros experimentais tenham sido validados neste hardware.

## 3. Pendências técnicas por ordem de risco

**P0 — Rastreabilidade e segurança da interpretação**
1. Identificar hardware, variante, firmware gravado e branches usados no ensaio E2E.
2. Garantir que DEMO permaneça inelegível para decisões clínicas fora de ambientes controlados.
3. Revisar autorização fina por paciente e exposição de dados.
4. Resolver acesso público apropriado ao código necessário para auditoria IoT.

**P1 — Engenharia e qualidade**
1. Finalizar/anexar evidência integrada reproduzível PITACO → Unity → Azure → dashboard.
2. Implantar análise estática/segurança e associar aos pipelines.
3. Consolidar observabilidade e correlação dos logs.
4. Reconciliar a RFC com o código e atualizar matriz de requisitos.

**P2 — Pesquisa**
1. Evoluir G2–G4 e verificar condições de pulsatilidade, morfologia e coerência.
2. Calibrar G1 e investigar tolerância sem pegador anatômico antes de comparações com pegador.
3. Realizar avaliação de BPM com referência sincronizada e projeto de calibração adequado para SpO₂, sob orientação ética/técnica pertinente.

## 4. Vocabulário de evidência

| Termo | O que significa aqui |
| --- | --- |
| Implementado | Código correspondente presente em uma versão identificável |
| Testado em host | Ensaio automatizado sem comprovação de todo hardware real |
| Testado em bancada | Comportamento observado em montagem e sessão específicas |
| Persistido na nuvem | API/Atlas confirmou o registro da sessão identificada |
| Publicado | Aplicação hospedada em URL; não implica autorização anônima a dados |
| Validado clinicamente | Requer protocolo e referência independentes — **não demonstrado** |

[Ver matriz de conformidade →](CONFORMIDADE_IOT.md)
