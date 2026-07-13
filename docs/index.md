# ⚡ Lightning Network Routing Optimizer

> Projeto Final da disciplina de Bitcoin da UnB

---

## Sobre o projeto

O **Lightning Network Routing Optimizer** é uma aplicação desenvolvida para otimizar o processo de roteamento de pagamentos na Lightning Network utilizando algoritmos clássicos de grafos.

A aplicação modela a rede como um **grafo direcionado**, onde:

- cada nó representa um participante da Lightning Network;
- cada aresta representa um canal de pagamento;
- cada canal possui capacidade, taxa fixa e taxa proporcional.

A partir desse modelo, é possível calcular rotas de menor custo entre dois nós utilizando o algoritmo de **Dijkstra**, considerando simultaneamente:

- capacidade disponível;
- custo de roteamento;
- liquidez dos canais.

Além do roteamento individual, o projeto também permite executar milhares de pagamentos consecutivos para simular o consumo de liquidez da rede e analisar seu comportamento sob diferentes cargas de tráfego.

---

## Objetivos

O projeto possui quatro objetivos principais:

- Modelar a Lightning Network como um grafo direcionado;
- Implementar um algoritmo de roteamento baseado em Dijkstra;
- Simular o esgotamento de liquidez após sucessivos pagamentos;
- Analisar o comportamento da rede através de experimentos.

---

## Funcionalidades

Atualmente o simulador possui:

- Busca da rota de menor custo;
- Verificação da capacidade dos canais;
- Cálculo das taxas Lightning;
- Simulação dinâmica de liquidez;
- Execução de testes de estresse;
- Identificação dos nós mais lucrativos;
- Interface gráfica desenvolvida em Streamlit;
- Visualização das rotas utilizando PyVis.

---

## Arquitetura do Projeto

```
             Snapshot da Rede
                     │
                     ▼
            Construção do Grafo
                     │
                     ▼
              Grafo (NetworkX)
                     │
          ┌───────────┴───────────┐
          ▼                     ▼
   Roteamento               Simulação
   (Dijkstra)            de Liquidez
          │                     │
          └───────────┬───────────┘
                     ▼
            Interface Streamlit
                     ▼
            Visualização da Rede
```

---

## Estrutura do Projeto

```text
.
├── app.py
├── assets/
├── src/
│   ├── script_de_construcao_do_grafo.py
│   ├── 2_route_finder.py
│   ├── gera_JSON.py
│   └── run_experiments.py
├── data/
│   └── ln_sample.json
└── docs/
```

---

## Tecnologias Utilizadas

| Tecnologia | Finalidade |
|------------|------------|
| Python | Desenvolvimento |
| NetworkX | Modelagem do grafo |
| Streamlit | Interface gráfica |
| PyVis | Visualização da rede |
| JSON | Snapshot da Lightning |
| MkDocs Material | Documentação |

---

## Fluxo de Execução

O funcionamento da aplicação pode ser resumido nas seguintes etapas:

1. Carregamento do snapshot da Lightning Network;
2. Construção do grafo direcionado;
3. Seleção da origem, destino e valor do pagamento;
4. Execução do algoritmo de Dijkstra;
5. Verificação da capacidade dos canais;
6. Cálculo das taxas de roteamento;
7. Exibição da rota encontrada;
8. Atualização da liquidez (durante simulações);
9. Geração de estatísticas.

---

## Resultados Esperados

Ao final dos experimentos, o simulador permite observar:

- taxa de sucesso dos pagamentos;
- canais congestionados;
- nós mais utilizados;
- taxas arrecadadas;
- impacto da liquidez no roteamento;
- comportamento da rede sob diferentes níveis de carga.

---

## Navegação

A documentação está organizada nas seguintes seções:

- Introdução
- Lightning Network
- Metodologia
- Implementação
- Experimentos
- Resultados
- Discussão
- Conclusão
- Referências