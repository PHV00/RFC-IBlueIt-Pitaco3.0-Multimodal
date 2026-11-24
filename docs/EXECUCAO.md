# Executar, demonstrar e verificar o sistema

[Portal](../README.md) · [Integrações](INTEGRACOES.md) · [Limitações](LIMITACOES.md)

**Nota:** há um **frontend em Azure** documentado e aprovado em CI/CD. O jogo Unity e o PITACO dependem da estação e do hardware local; não são representados como jogo executado dentro do navegador. A visualização de dados de pacientes pode exigir uma conta/autorização.

## 1. Demonstração web publicada

- [Resultados](https://iblueit-health-infocharts-cqcfckepc9cma7fq.switzerlandnorth-01.azurewebsites.net/plataforma/resultados)
- [Comparativos](https://iblueit-health-infocharts-cqcfckepc9cma7fq.switzerlandnorth-01.azurewebsites.net/plataforma/comparativo)

**Roteiro de verificação com acesso autorizado:**
1. Acessar o site hospedado na Azure e realizar login usando conta de demonstração apropriada, sem inserir credenciais no repositório.
2. Abrir Resultados e selecionar paciente/execução com dados PITACO 3.0 disponíveis.
3. Conferir os gráficos de fluxo, BPM, SpO₂ **DEMO** e PPG RED/IR, observando lacunas e filtros.
4. Abrir Comparativos e conferir as séries/resumos por execução.
5. Registrar data, URL, conta de teste não identificável e resultado. Não capturar dados pessoais em prints públicos.

O responsável relatou verificação funcional das duas páginas em 23/11/2026; este texto **não substitui** teste novo de disponibilidade ou autenticação.

## 2. Firmware PITACO canônico

Fonte: [PITACO_ESP32 / README](https://github.com/PHV00/PITACO_ESP32/blob/main/README.md) (repo privado no inventário).

Pré-requisitos: ambiente de compilação ESP32-C3, bibliotecas/vendor do projeto e montagem conforme a documentação técnica.

~~~bash
arduino-cli compile --fqbn esp32:esp32:esp32c3 frimeware2.4
~~~

O comando verifica compilação, **não grava a placa**. É indispensável registrar porta utilizada, versão/hash do firmware efetivamente gravado, modelo de sensor e montagem para atribuir resultados físicos ao código.

Testes host do firmware:

~~~bash
python -m unittest discover -s test -p "test_*.py" -v
g++ -std=c++11 -Wall -Wextra -Werror test/test_sqi_gate1.cpp -o /tmp/pitaco_g1
/tmp/pitaco_g1
~~~

Os comandos devem ser executados na raiz do repositório PITACO; o exemplo C++ pressupõe ambiente Linux com g++.

## 3. Unity

Fonte: [Unity / README](https://github.com/PHV00/iblueit-psychophysiological-flow-pitaco2.4-port/blob/main/README.md) e [consolidação PITACO 2.4](https://github.com/PHV00/iblueit-psychophysiological-flow-pitaco2.4-port/blob/main/docs/marcos/CONSOLIDACAO_PITACO24_UNITY_PPG_DEEPDDA_HUD_2026-11-22.md).

**Procedimento mínimo a preparar com o operador:**
1. Instalar a versão de Unity compatível com o projeto (consultar repositório Unity).
2. Confirmar qual caminho recebe o PITACO: leitura direta USB/serial ou bridge Python da variante.
3. Identificar COM, versão efetiva do firmware e dispositivos conectados; não disputar a mesma porta entre processos.
4. Executar a cena de teste e observar contato/G1, janela PPG, BPM, SpO₂ DEMO e HUD.
5. Encerrar aquisição e confirmar limpeza/expiração dos valores.
6. Quando exercitar as regras experimentais de Mixer, usar somente ambiente Editor/Development e registrar que **não é validação de segurança clínica**.

A rota de gravação de sessões PITACO 3.0 em Unity foi desenvolvida em [PR #14](https://github.com/PHV00/iblueit-psychophysiological-flow-pitaco2.4-port/pull/14); verificar estado do merge e versão da cena antes de alegar funcionamento na main.

## 4. Backend e frontend — verificações de desenvolvimento

No repositório [backend](https://github.com/PHV00/iblueit-server-side), o script de testes definido em package.json executa contratos, endpoints e consultas:

~~~bash
npm ci
npm test
~~~

No repositório [frontend](https://github.com/PHV00/iblueit-health-Infocharts), existem os scripts de React test/build; em ambiente CI adequado:

~~~bash
npm ci
CI=true npm test -- --watch=false
npm run build
~~~

Executar conforme as versões de Node e as instruções atuais das respectivas **GitHub Actions**; estes comandos são uma referência para reprodução local, **não** afirmam que o teste foi repetido durante a escrita deste portal.

## 5. Evidência de ponta a ponta

A evidência publicada no [Marco 04 backend](https://github.com/PHV00/iblueit-server-side/blob/main/docs/MARCO_04_ENCERRAMENTO.md) identifica uma sessão de engenharia gravada por Unity/Azure/Atlas com HTTP 201 e séries completas. Uma nova reprodução robusta deve anexar:
- identificação de sensor, placa e firmware realmente gravado;
- hash/branch Unity, backend e frontend;
- período e condições da sessão;
- correlação de seq/timestamps e identificador de execução sem dados pessoais;
- status HTTP, identificador interno anonimizado e confirmação de leitura de API;
- captura autorizada do resultado gráfico;
- falhas, perdas e limitação do experimento.

Não publicar raw com identificadores pessoais, segredos ou credenciais.

## 6. Evidência de CI/CD

- [Backend — pipeline com deploy aprovado](https://github.com/PHV00/iblueit-server-side/actions/runs/45204560332)
- [Frontend — pipeline com deploy aprovado](https://github.com/PHV00/iblueit-health-Infocharts/actions/runs/45227836897)
- [Firmware — workflows PITACO](https://github.com/PHV00/PITACO_ESP32/tree/main/.github/workflows) (sujeito a acesso)

Resultados históricos de pipeline são evidência da execução apontada, não garantia de disponibilidade contínua.

[Ver matriz de evidências →](EVIDENCIAS.md)
