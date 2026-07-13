# Metodologia

Esta seção descreve a metodologia empregada para a construção do otimizador de roteamento da Lightning Network, desde a obtenção dos dados até a execução dos experimentos.

O desenvolvimento foi dividido em cinco etapas principais:

1. Obtenção do snapshot da Lightning Network;
2. Preparação dos dados para análise;
3. Construção do grafo direcionado;
4. Implementação do algoritmo de roteamento;
5. Execução de experimentos de simulação.

---

## Obtenção dos Dados

Para representar uma topologia real da Lightning Network foi utilizado um snapshot público da rede no formato Gossip.

O protocolo Lightning mantém informações públicas sobre seus nós e canais através do mecanismo conhecido como **Gossip Protocol**, responsável por divulgar continuamente alterações na topologia da rede.

O snapshot utilizado pelo projeto é armazenado no arquivo:

```text
data/gossip.bz2
```

Esse arquivo contém informações sobre:

- Nós participantes;
- Canais ativos;
- Capacidade dos canais;
- Políticas de roteamento;
- Taxas cobradas por cada direção do canal.

Como esse formato não pode ser utilizado diretamente pela aplicação, foi desenvolvido um processo de conversão.

---

## Construção do Grafo

O arquivo JSON é processado pelo módulo

```text
script_de_construcao_do_grafo.py
```

Durante essa etapa são executadas as seguintes operações:

- Leitura do arquivo JSON;
- Criação de um grafo direcionado (`networkx.DiGraph`);
- Inserção dos nós da Lightning Network;
- Criação das arestas correspondentes aos canais;
- Armazenamento dos atributos necessários para o roteamento.

Cada nó recebe como atributo principal seu identificador público (*public key*) e um alias descritivo.

Cada canal é convertido em duas arestas direcionadas, permitindo representar políticas distintas para cada direção do pagamento.

Para cada aresta são armazenadas informações como:

- Capacidade do canal;
- Identificador do canal;
- Taxa fixa (*fee_base_msat*);
- Taxa proporcional (*fee_rate_milli_msat*).

Canais que não possuem políticas válidas em ambos os sentidos são descartados durante essa etapa, evitando que canais inativos sejam considerados durante o roteamento.

---

## Algoritmo de Roteamento

Após a construção do grafo, o cálculo das rotas é realizado pelo módulo

```text
2_route_finder.py
```

O simulador utiliza o algoritmo de Dijkstra implementado pela biblioteca NetworkX.

Entretanto, em vez de utilizar pesos fixos nas arestas, foi implementada uma função de custo personalizada.

Essa função considera simultaneamente:

- Capacidade disponível do canal;
- Taxa fixa;
- Taxa proporcional.

Caso a capacidade de um canal seja inferior ao valor da transação solicitada, esse canal é automaticamente descartado da busca.

Os demais canais recebem como peso o custo financeiro correspondente ao encaminhamento do pagamento.

Dessa forma, o algoritmo procura uma rota que minimize o custo total da transação respeitando as restrições de capacidade existentes na rede.

---

## Simulação de Liquidez

Uma característica importante da Lightning Network é que a liquidez disponível em um canal se altera após cada pagamento.

Para reproduzir esse comportamento foi implementada uma camada de simulação sobre o algoritmo de roteamento.

Sempre que um pagamento é concluído com sucesso:

- A capacidade disponível do canal é reduzida;
- As taxas cobradas são contabilizadas;
- Os lucros dos nós intermediários são registrados.

Essa estratégia permite representar o esgotamento progressivo da liquidez da rede durante a execução de sucessivos pagamentos.

Embora simplificada em relação à Lightning Network real, essa abordagem permite analisar o impacto da redução de liquidez sobre o desempenho do algoritmo de roteamento.

---

## Experimentos

Os experimentos foram executados através do módulo

```text
run_experiments.py
```

Foram definidos três cenários independentes:

| Cenário | Objetivo |
|---------|----------|
| Tráfego Leve | Avaliar o comportamento da rede sob baixa utilização |
| Tráfego Médio | Simular utilização moderada dos canais |
| Stress Alto | Avaliar o comportamento da rede sob esgotamento de liquidez |

Cada cenário inicia a partir do mesmo estado inicial da rede.

Ao término da execução são registradas métricas como:

- Número total de pagamentos;
- Pagamentos concluídos com sucesso;
- Falhas por falta de liquidez;
- Taxas arrecadadas;
- Nós mais lucrativos durante a simulação.

Os resultados produzidos são armazenados em formato JSON para posterior análise.

---

## Considerações

A metodologia adotada busca reproduzir os principais aspectos envolvidos no roteamento de pagamentos da Lightning Network utilizando uma abordagem simplificada, porém consistente com os conceitos fundamentais da rede.

Essa estrutura permitiu separar claramente as etapas de aquisição dos dados, modelagem do grafo, cálculo das rotas e análise experimental, facilitando tanto a implementação quanto a reprodução dos experimentos realizados.