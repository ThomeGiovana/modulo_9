# Função de recompensa do AWS Deep Racer

O objetivo do carrinho é chegar ao final da corrida no menor tempo possível. Considerando que:

- O menor tempo pode ser alcançado com a maior velocidade possível em cada estado e;
- A penalidade por sair da pista é de 3s para voltar à corrida
  Podemos fazer uma função de recompensa simples, mas que cumpre bem o objetivo, utilizando a valocidade como principal ferramenta.

A largura da pista (variável `track_width`), combinada com a distância do centro da pista, diz o quanto o carrinho está andando para fora da pista ou se está seguindo no centro. Como temos que utilizar o centro como ponto de referência para onde o carrinho está, as porcentagens de distância não podem ultrapassar 0.5. Dito isso, criei os marcos de distância do centro sendo 0.1, 0.3 e 0.5.

Cada um deles irá fazer com que a recompensa seja uma porcentagem da valocidade utilizada naquele estado:

- Se o carrinho estiver dentro de um range de 10% de distância do centro da pista, a valocidade será retornada (`speed * 1`);
- Se o carrinho estiver dentro de um range de 10 a 30% de distância do centro da pista, será retornada 50% da velocidade (`speed * 0.5`)
- Se o carrinho estiver dentro de um range de 30 a 50% de distância do centro da pista, será retornada 30% da velocidade (`speed * 0.3`)

Essa função de recompensa (presente em [reward_function.py](./reward_function.py)) faz com que o carrinho seja induzido a acelerar mais e se manter dentro da pista, levando ao objetivo de terminar a corrida o mais rápido possível sem sair da pista.

Algumas vezes, a simplicidade se sobressai em meio à complexidade.
