# Discussão

O desenvolvimento deste projeto teve como objetivo investigar como algoritmos clássicos de grafos podem ser aplicados para otimizar o processo de roteamento de pagamentos na Lightning Network. Ao modelar a rede como um grafo direcionado e ponderado, tornou-se possível utilizar técnicas consolidadas de otimização para encontrar rotas de menor custo respeitando restrições de capacidade.

Os experimentos demonstraram que, mesmo utilizando uma modelagem simplificada, a estratégia implementada foi capaz de reproduzir diversos desafios encontrados em redes de pagamento descentralizadas, evidenciando tanto os benefícios quanto as limitações da abordagem adotada.

---

## Otimização do Roteamento por Menor Custo

A principal contribuição deste trabalho consiste na implementação de um mecanismo de roteamento baseado no algoritmo de Dijkstra, utilizando uma função de custo personalizada para representar as características da Lightning Network.

Enquanto o algoritmo tradicional busca minimizar apenas a soma dos pesos das arestas, a solução desenvolvida atribui aos canais um custo calculado a partir de suas políticas de roteamento, considerando simultaneamente:

- Taxa fixa do canal (*fee_base_msat*);
- Taxa proporcional ao valor transmitido (*fee_rate*);
- Capacidade mínima necessária para transportar o pagamento.

Essa adaptação permite que o algoritmo selecione caminhos financeiramente mais vantajosos, descartando automaticamente canais incapazes de encaminhar a transação.

Como consequência, o roteamento deixa de ser apenas um problema de conectividade e passa a considerar aspectos econômicos e operacionais da rede, aproximando-se do comportamento esperado em sistemas de pagamento reais.

---

## Otimização pela Filtragem de Canais

Outro aspecto importante da implementação é a filtragem prévia dos canais durante o cálculo das rotas.

Antes que uma aresta participe da busca, sua capacidade é comparada ao valor da transação solicitada. Caso o canal não possua capacidade suficiente, ele é removido do espaço de busca para aquele pagamento.

Essa estratégia apresenta duas vantagens principais:

- Reduz o número de caminhos analisados pelo algoritmo;
- Evita a seleção de rotas que inevitavelmente falhariam.

Essa otimização também melhora a eficiência computacional da busca, especialmente em redes maiores, onde milhares de canais podem estar indisponíveis para um determinado pagamento.

---

## Simulação da Liquidez como Processo Dinâmico

Diferentemente de exemplos estáticos normalmente utilizados para demonstrar algoritmos de caminhos mínimos, o simulador desenvolvido considera que a rede evolui continuamente durante sua utilização.

Após cada pagamento realizado com sucesso, a capacidade dos canais utilizados é reduzida, alterando o estado da rede para as próximas transações.

Essa estratégia reproduz um aspecto fundamental da Lightning Network: a disponibilidade de uma rota depende não apenas da topologia da rede, mas também da distribuição momentânea da liquidez.

Os experimentos evidenciaram esse comportamento. À medida que novos pagamentos eram realizados, a quantidade de rotas viáveis diminuía, reduzindo progressivamente a taxa de sucesso observada nas simulações.

Essa característica demonstra que problemas de roteamento em redes de pagamento possuem natureza dinâmica, exigindo algoritmos capazes de adaptar suas decisões às mudanças do ambiente.

---

## Eficiência da Modelagem por Grafos

Os resultados obtidos confirmam que a representação da Lightning Network como um grafo direcionado constitui uma abordagem adequada para o estudo do problema de roteamento.

Cada participante da rede foi modelado como um vértice, enquanto os canais de pagamento foram representados como arestas contendo atributos relevantes para o processo de decisão, como capacidade e políticas de cobrança.

Essa modelagem permitiu utilizar diretamente estruturas e algoritmos disponibilizados pela biblioteca NetworkX, reduzindo a complexidade da implementação sem comprometer a clareza do modelo computacional.

Além disso, a organização modular do projeto facilita futuras substituições do algoritmo de roteamento ou da própria estrutura de dados utilizada.

---

## Comparação com Implementações Reais da Lightning Network

Embora o simulador reproduza os principais conceitos envolvidos no roteamento, implementações utilizadas em produção, como LND e Core Lightning, empregam estratégias adicionais para aumentar a eficiência dos pagamentos.

Entre essas estratégias destacam-se:

- Estimativas probabilísticas de liquidez dos canais;
- Histórico de sucesso e falha das rotas utilizadas;
- Penalização temporária de canais com baixo desempenho;
- Seleção adaptativa de rotas baseada em tentativas anteriores;
- Divisão automática de pagamentos utilizando **Multi-Part Payments (MPP)**.

Esses mecanismos procuram maximizar a probabilidade de sucesso do pagamento, mesmo quando a menor rota em termos de custo não representa necessariamente a alternativa mais confiável.

Em comparação, a abordagem implementada neste projeto prioriza simplicidade e interpretabilidade, permitindo compreender claramente como algoritmos clássicos de grafos podem ser adaptados para resolver problemas de roteamento na Lightning Network.

---

## Possíveis Estratégias de Otimização

A arquitetura desenvolvida permite incorporar novas estratégias de otimização sem alterações significativas na estrutura do sistema.

Entre as melhorias que poderiam ser implementadas destacam-se:

### Multi-Part Payments (MPP)

Grandes pagamentos poderiam ser divididos automaticamente em diversas transações menores, distribuídas por rotas independentes.

Essa estratégia reduziria a dependência de canais individuais com alta capacidade, aumentando significativamente a probabilidade de sucesso em redes congestionadas.

---

### Algoritmos Heurísticos

Embora Dijkstra produza caminhos ótimos em relação ao custo definido, algoritmos heurísticos como **A\*** poderiam reduzir o tempo de processamento em grafos de grande porte ao limitar o espaço explorado durante a busca.

Essa otimização seria especialmente relevante caso o simulador passasse a utilizar snapshots completos da Lightning Network contendo dezenas de milhares de nós e canais.

---

### Balanceamento de Carga

Os experimentos mostraram que poucos nós concentram grande parte das taxas arrecadadas.

Uma possível evolução seria incorporar mecanismos capazes de distribuir o tráfego entre múltiplas rotas equivalentes, reduzindo a sobrecarga dos principais hubs e aumentando a utilização de canais alternativos.

Além de melhorar o aproveitamento da rede, essa estratégia poderia contribuir para reduzir a concentração de pagamentos em poucos participantes.

---

### Modelagem Mais Realista da Liquidez

Na implementação atual, a liquidez é aproximada pela capacidade restante do canal.

Uma evolução natural seria representar separadamente os saldos existentes em cada direção do canal, reproduzindo de maneira mais fiel o funcionamento da Lightning Network.

Essa melhoria permitiria investigar problemas relacionados ao rebalanceamento de canais, um dos principais desafios enfrentados pelos operadores da rede.

---

## Contribuições do Projeto

Além da implementação do simulador, este trabalho apresenta contribuições relevantes do ponto de vista didático e computacional.

Entre elas destacam-se:

- Modelagem da Lightning Network como um grafo direcionado;
- Utilização de snapshots reais para construção da topologia;
- Adaptação do algoritmo de Dijkstra para considerar taxas e capacidade dos canais;
- Simulação dinâmica do consumo de liquidez;
- Ambiente de experimentação capaz de reproduzir diferentes níveis de utilização da rede;
- Documentação técnica detalhada permitindo reprodução e extensão do projeto.

Essas contribuições demonstram como conceitos clássicos de Estruturas de Dados e Teoria dos Grafos podem ser aplicados na resolução de problemas atuais relacionados às criptomoedas e aos sistemas distribuídos.

---

## Considerações Finais

Os resultados obtidos indicam que a utilização de algoritmos clássicos de grafos constitui uma abordagem eficiente para o problema de roteamento na Lightning Network.

A estratégia desenvolvida foi capaz de encontrar rotas de menor custo respeitando restrições de capacidade, além de reproduzir o impacto do consumo progressivo da liquidez sobre a disponibilidade de caminhos.

Embora simplificada em relação às implementações utilizadas em produção, a solução demonstra como técnicas de otimização podem ser aplicadas para resolver problemas reais envolvendo redes descentralizadas de pagamento.

A arquitetura modular construída durante o projeto também estabelece uma base sólida para futuras pesquisas envolvendo algoritmos mais sofisticados, novos modelos de liquidez e mecanismos avançados de otimização de rotas, permitindo que o simulador evolua para representar cenários cada vez mais próximos da Lightning Network real.