# Introdução

O Bitcoin revolucionou o conceito de moeda digital ao introduzir um sistema descentralizado baseado em blockchain, permitindo a realização de transações sem a necessidade de uma autoridade central. Entretanto, embora apresente elevado nível de segurança e resistência à censura, sua arquitetura possui limitações relacionadas à escalabilidade. O tempo necessário para confirmação de blocos, aliado à capacidade limitada de processamento de transações por segundo, dificulta sua utilização em aplicações que demandam pagamentos rápidos e de baixo valor.

Nesse contexto, a **Lightning Network (LN)** surge como uma solução de segunda camada (*Layer 2*), permitindo que pagamentos sejam realizados fora da blockchain principal através de canais de pagamento bidirecionais. Em vez de registrar cada transação diretamente na rede Bitcoin, apenas a abertura e o encerramento dos canais são registrados na blockchain, reduzindo custos e aumentando significativamente a capacidade de processamento da rede.

O funcionamento da Lightning Network pode ser modelado como um problema clássico de teoria dos grafos. Cada participante da rede corresponde a um vértice, enquanto os canais de pagamento representam arestas que conectam esses vértices. Cada canal possui características próprias, como capacidade disponível e políticas de cobrança de taxas, tornando o processo de roteamento de pagamentos um problema de otimização em grafos ponderados.

Encontrar uma rota eficiente entre dois participantes não depende apenas da existência de um caminho na rede. É necessário considerar simultaneamente fatores como liquidez disponível, capacidade dos canais e custo de roteamento, uma vez que canais insuficientemente financiados impossibilitam a conclusão da transação, mesmo quando existe conectividade entre origem e destino.

Com base nesse cenário, este projeto propõe o desenvolvimento de um simulador e otimizador de roteamento para a Lightning Network utilizando algoritmos clássicos de grafos. A aplicação representa a topologia da rede como um grafo direcionado construído a partir de um *snapshot* público da Lightning Network, permitindo calcular rotas de menor custo por meio do algoritmo de Dijkstra. Além do cálculo de rotas individuais, o otimizador implementa um mecanismo de atualização dinâmica da liquidez dos canais, possibilitando a execução de experimentos que simulam o comportamento da rede sob diferentes níveis de carga.

A aplicação desenvolvida inclui ainda uma interface gráfica construída com Streamlit para visualização das rotas encontradas, identificação de gargalos e execução de simulações em lote. Essas funcionalidades permitem observar o impacto do consumo progressivo da liquidez sobre a taxa de sucesso dos pagamentos, bem como analisar o comportamento dos principais nós roteadores durante cenários de tráfego leve, moderado e intenso.

## Objetivos

O objetivo geral deste trabalho é desenvolver uma ferramenta capaz de modelar a Lightning Network como um grafo direcionado e analisar o comportamento do roteamento de pagamentos utilizando algoritmos clássicos de caminhos mínimos.

Como objetivos específicos, destacam-se:

- modelar a topologia da Lightning Network a partir de um *snapshot* público da rede;
- construir um grafo direcionado contendo informações de capacidade e políticas de taxas dos canais;
- implementar um algoritmo de roteamento baseado em Dijkstra considerando restrições de capacidade e custo de roteamento;
- simular o consumo de liquidez após sucessivos pagamentos;
- executar experimentos sob diferentes níveis de carga;
- analisar o impacto da liquidez sobre a eficiência do roteamento e a concentração das taxas de roteamento em nós intermediários.

## Organização da documentação

Esta documentação está organizada da seguinte forma:

- **Lightning Network:** apresenta os conceitos fundamentais necessários para compreender o funcionamento da rede e do problema de roteamento.
- **Metodologia:** descreve a construção do conjunto de dados, a modelagem do grafo e as decisões de implementação adotadas.
- **Implementação:** detalha a arquitetura da aplicação, os principais módulos do sistema e a interface desenvolvida.
- **Experimentos:** apresenta os cenários utilizados durante as simulações.
- **Resultados:** reúne os dados obtidos durante a execução dos experimentos.
- **Discussão:** analisa criticamente os resultados, comparando-os com o comportamento esperado da Lightning Network.
- **Conclusão:** resume os principais resultados alcançados e apresenta possibilidades para trabalhos futuros.