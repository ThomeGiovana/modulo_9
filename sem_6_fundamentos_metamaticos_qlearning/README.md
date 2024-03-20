a) O algoritmo Q-learning é um método off-policy, isso quer dizer que aprende a melhor política independente das ações tomadas tomadas pelo agente. Ele leva em conta a qualidade máxima entre as ações do estado subsequente ( $ maxQ(s*{k+1}, a) $ ), ao contrário do algoritmo SARSA, que leva em conta a qualidade da ação futura a ser tomada pelo agente no estado subsequente ( $ Q(S*{t+1}, A\_{t+1}) $ , basicamente o agente se compromete a tomar uma ação futura). Podemos ver a equação para atualização das qualidades dos conjuntos de estados/ações utilizando Q-learning a seguir:

$$ Q^{novo} (S*t, A_t) \leftarrow Q^{velho}(S_t, A_t) + \alpha [R*{t+1} + \gamma _ maxQ(s\_{k+1}, a) - Q^{velho}(S_t, A_t)] $$
Sendo assim, calculamos a qualidade de estar em A e escolher se mover para o estado B:
$Q(S_a, A_b) \leftarrow 0 + 0.1 _ [0.5 + 0.9 * 0 - 0] = 0.1 \* 0.5 = 0.05 $
Chamamos $A_x$ a ação de escolher estado $x$, podendo ser B ou C.
Nossa tabela passa a assumir os valores:
| Ir para B| Ir para C |
|--|--|
|0.05 | 0 |
| - | 0 |

b) Como o agente não pode tomar ação nenhuma no estado C (pois é um estado terminal), existem apenas três possíveis conjuntos de estado/ação $ Q(s,a)$, como mostra a tabela citada no enunciado:

- $Q(A, B)$
- $Q(A, C)$
- $Q(B, C)$

Podemos observar também que existem apenas dois caminhos possíveis para chegar em C. Dito isso, rodaremos uma simulação com a mesma quantidade de iterações para cada caminho, explorando os dois igualmente. O código está disponível [aqui](./qlearning_example.ipynb)

Para olhos humanos neste exemplo, fica óbvio que o caminho que oferecerá maior recompensa total final é escolhendo ir para B e, logo depois, ir para C começando do estado A, somando uma recompensa de 2,5, enquanto ir diretamente para C oferece uma recompensa total final de 1. Em Q-learning (e também na Equação de Bellman), devemos ter uma taxa de desconto $\gamma$ alta o suficiente para o agente considerar recompensas futuras. Como no exemplo fornecido a taxa é próxima de 1, o agente irá considerar bastante as recompensas futuras e, portanto, o agente provavelmente escolherá mover-se para o estado B para maximizar sua recompensa total final esperada, pois a soma da recompensa imediata mais a recompensa futura esperada ao tomar essa rota é maior do que a recompensa imediata de mover-se diretamente para o estado C.

Como demonstrado no código, com apenas 100 iterações para cada caminho, atualizando os pesos a cada ação tomada, a qualidade de ir para B aumenta demasiadamente e adquirimos a seguinte tabela:

| Ir para B          | Ir para C          |
| ------------------ | ------------------ |
| 2.2994076808048067 | 0.9999734386011123 |
| -                  | 1.9999468772022246 |

c) Em Q-learning, o trade-off entre exploração e explotação influencia diretamente na eficácia do aprendizado do agente. Ao explorar, o agente busca novas ações para descobrir novas recompensas, o que é essencial para buscar a política ótima. Já a explotação envolve a escolha das ações entre as quais ele já conhece e já sabe que maximizam as recompensas dentro do ambiente conhecido. Resumidamente, o o equilíbrio entre exploração e explotação determinará a rapidez com que o agente aprende a sequência ótima de ações para atingir o estado C com a maior recompensa total.

Se o agente explora demais, pode demorar para convergir para a política ótima de transitar do estado A para B e, em seguida, para C. Por outro lado, uma ênfase excessiva em explotação pode fazer com que o agente se prenda prematuramente a uma política subótima, como mover-se diretamente de A para C, sem explorar suficientemente a opção de passar pelo estado B, que oferece uma recompensa total maior. Portanto, o desafio reside em ajustar dinamicamente a estratégia do agente entre explorar novas possibilidades e explorar o conhecimento adquirido para maximizar as recompensas ao longo do tempo.
