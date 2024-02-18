# Questão Ponderada: Backpropagation

Giovana Lisbôa Thomé

Fevereiro de 2024

# 1. Forward pass

Assumindo o seguinte diagrama como a rede neural tratada no problema:

![Untitled](/sem_1_backpropagation/img/neural_network_diagram.png)

Sendo $i$ a camada de entrada ou _input_, $h$ a camada oculta e $o$ (letra “o”) a camada de saída ou _output_ e os pesos $w$ com os seguintes valores:

$w_1 = 0.15$

$w_1 = 0.20$

$w_3 = 0.25$

$w_4 = 0.30$

$w_5 = 0.40$

$w_6 = 0.45$

Calculamos os somatórios $z$ e as funções de ativação $a$ de cada neurônio $h_n$ da camada oculta e o neurônio $o$ da camada de saída. Começando com $h_1$ que recebe os pesos $w_1$ e $w_3$ e considerando os inputs $i_1 = 0.5$ e $i_2 = 0.6$:

$z_{h1} = w_1*i_1 + w_3 * i_2$

$z_{h1} = 0.15*0.5 + 0.25 * 0.6$

$z_{h1} = 0.225$

$a_{h1} = \sigma(z_{h1}) = \sigma(0.225) = 0.5560$

Para o neurônio $h_2$:

$z_{h2} = w_2*i_1+ w_4 * i_2$

$z_{h2} = 0.2*0.5 + 0.3* 0.6$

$z_{h2} = 0.28$

$a_{h2} = \sigma(z_{h2}) = \sigma(0.28) = 0.4786$

Para o neurônio $o$ da camada de saída:

$z_{o} = w_5a_{h1} + w_6a_{h2}$

$z_{o} = 0.40*0.5560 +0.45 * 0.5695$

$z_{o} = 0.4786$

$a_{o} = \sigma(z_{o}) = \sigma(0.4786) = 0.6174$

# 2. Função de erro MSE

O cálculo da função de erro se da por:

$$
C = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}i)^2
$$

Onde $y$ é o valor real ou _target_, $\hat{y}$ é o valor predito (denotado por $a_o$ no passo anterior) e $n=1$ por conta da rede conter apenas um neurônio de saída. Sendo assim, podemos simplificar o cálculo para a presente situação:

$$
C = (y - a_o)^2
$$

Seguindo assim, temos:

$C = (y - a_o)^2 = (0.5-0.6174)^2 = 0.0137$

# 3. Gradiente do erro na camada de saída

Relembrando as informações

$z_{h1} = 0.225$

$a_{h1} = 0.5560$

$z_{h2} = 0.28$

$a_{h2} = 0.4786$

$z_{o} = 0.4786$

$a_{o} = 0.6174$

$C = 0.0137$

$w_1 = 0.15$

$w_1 = 0.20$

$w_3 = 0.25$

$w_4 = 0.30$

$w_5 = 0.40$

$w_6 = 0.45$

Para encontrar o gradiente do erro $w_L$ na camada de saída em relação à um peso, temos que calcular a derivada parcial do erro $C$ em relação à esse peso.

### 3.1. Peso $w_5$

$$
\frac{dC}{dw_5} = \frac{dC}{da_o} \frac{da_o}{dz_{o}} \frac{dz_{o}}{dw_5}
$$

Calculando separadamente cada uma das derivadas:

$\frac{dC}{da_o} = \frac{d}{da_o} (y-a_o)^2 = \frac{d}{du} (u)^2 \frac{d}{da_o} (y-a_o) = 2u*(-1) = 2(y-a_o)(-1) = -2(y-a_o) = -2(0.5-0.6174) = 2.1174$

$\frac{da_o}{dz_{o}} = \frac{d}{dz_{o}} \sigma(z_o)= \sigma(z_o)*(1-\sigma(z_o)) = a_{o}*(1-a_{o})= 0.6174(1-0.6174) = 0.2362$

$\frac{dz_{o}}{dw_5} = \frac{d}{dw_5} w_5 a_{h1} + \frac{d}{dw_5}w_6 a_{h2} = a_{h1} = 0.5560$

Finalmente

$$
\frac{dC}{dw_5} = \frac{dC}{da_o} \frac{da_o}{dz_{o}} \frac{dz_{o}}{dw_5} = 2.1174*0.2362*0.5560 = 0.2780
$$

### 3.2. Peso $w_6$

$$
\frac{dC}{dw_6} = \frac{dC}{da_o} \frac{da_o}{dz_{o}} \frac{dz_{o}}{dw_6}
$$

$\frac{dC}{da_o} = -2(y-a_o) = 2.1174$

$\frac{da_o}{dz_{o}} = a_{o}*(1-a_{o}) = 0.2362$

$\frac{dz_{o}}{dw_6} = \frac{d}{dw_5} w_5 a_{h1} + \frac{d}{dw_5}w_6 a_{h2} = a_{h2} = 0.4786$

$$
\frac{dC}{dw_6} = \frac{dC}{da_o} \frac{da_o}{dz_{o}} \frac{dz_{o}}{dw_6} = 2.1174*0.2362*0.4786 = 0.2393
$$

# 4. _Backtrack_

Calcular os gradientes em relação a $w_1, w_2,w_3$ e $w_4$

É importante ressaltar que os pesos não são considerados na “cadeia” da derivada para calcular o gradiente pois seu valor não é afetado pelos valores das funções de ativação e somatórios de cada neurônio.

### 4.1. Peso $w_1$

$$
\frac{dC}{dw_1} = \frac{dC}{da_o} \frac{da_o}{dz_o} \frac{dz_o}{da_{h1}} \frac{da_{h1}}{dz_{h1}} \frac{dz_{h1}}{dw_1}
$$

$\frac{dC}{da_o} = -2(y-a_o) = 2.1174$

$\frac{da_o}{dz_{o}} = a_{o}*(1-a_{o}) = 0.2362$

$\frac{dz_o}{da_{h1}} = \frac{d}{da_{h1}} w_5 a_{h1} + \frac{d}{da_{h1}} w_6 a_{h2} = w_5 = 0.40$

$\frac{da_{h1}}{dz_{h1}} = \frac{d}{dz_{h1}} \sigma(z_{h1})(1-\sigma(z_{h1})) = a_{h1}(1-a_{h1}) = 0.5560(1-0.5560) = 0.2468$

$\frac{dz_{h1}}{dw_1} = \frac{d}{dw_1}w_1 i_1 + \frac{d}{dw_1} w_3 i_2 = i_1 = 0.5$

$$
\frac{dC}{dw_1} = 2.1174*0.2362*0.40*0.2468*0.5 = 0.0246
$$

### 4.2. Peso $w_2$

$$
\frac{dC}{dw_2} = \frac{dC}{da_o} \frac{da_o}{dz_o} \frac{dz_o}{da_{h2}} \frac{da_{h2}}{dz_{h2}} \frac{dz_{h2}}{dw_2}
$$

$\frac{dC}{da_o} = -2(y-a_o) = 2.1174$

$\frac{da_o}{dz_{o}} = a_{o}*(1-a_{o}) = 0.2362$

$\frac{dz_o}{da_{h2}} = \frac{d}{da_{h2}} w_5 a_{h1} + \frac{d}{da_{h2}} w_6 a_{h2} = w_6 = 0.45$

$\frac{da_{h2}}{dz_{h2}} = \frac{d}{dz_{h2}} \sigma(z_{h2})(1-\sigma(z_{h2})) = a_{h2}(1-a_{h2}) = 0.4786(1-0.4786) = 0.5214$

$\frac{dz_{h2}}{dw_2} = \frac{d}{dw_2}w_2 i_1 + \frac{d}{dw_2} w_4 i_2 = i_1 = 0.5$

$$
\frac{dC}{dw_2} = 2.1174*0.2362*0.45*0.5214*0.5 = 0.0586
$$

### 4.3. Peso $w_3$

$$
\frac{dC}{dw_1} = \frac{dC}{da_o} \frac{da_o}{dz_o} \frac{dz_o}{da_{h1}} \frac{da_{h1}}{dz_{h1}} \frac{dz_{h1}}{dw_3}
$$

$\frac{dC}{da_o} = -2(y-a_o) = 2.1174$

$\frac{da_o}{dz_{o}} = a_{o}*(1-a_{o}) = 0.2362$

$\frac{dz_o}{da_{h1}} = w_5 = 0.40$

$\frac{da_{h1}}{dz_{h1}} = a_{h1}(1-a_{h1}) = 0.2468$

$\frac{dz_{h2}}{dw_2} = \frac{d}{dw_2} w_1 i_1 + \frac{d}{dw_2} w_3 i_2 = i_2 = 0.6$

$$
\frac{dC}{dw_3} = 2.1174*0.2362*0.40*0.2468*0.60 = 0.0296
$$

### 4.4. Peso $w_4$

$$
\frac{dC}{dw_3} = \frac{dC}{da_o} \frac{da_o}{dz_o} \frac{dz_o}{da_{h2}} \frac{da_{h2}}{dz_{h2}} \frac{dz_{h2}}{dw_4}
$$

$\frac{dC}{da_o} = -2(y-a_o) = 2.1174$

$\frac{da_o}{dz_{o}} = a_{o}*(1-a_{o}) = 0.2362$

$\frac{dz_o}{da_{h2}} = \frac{d}{da_{h2}} w_5 a_{h1} + \frac{d}{da_{h2}} w_6 a_{h2} = w_6 = 0.45$

$\frac{da_{h2}}{dz_{h2}} = \frac{d}{dz_{h2}} \sigma(z_{h2})(1-\sigma(z_{h2})) = a_{h2}(1-a_{h2}) = 0.4786(1-0.4786) = 0.5214$

$\frac{dz_{h2}}{dw_2} = \frac{d}{dw_2}w_2 i_1 + \frac{d}{dw_2} w_4 i_2 = i_2 = 0.6$

$$
\frac{dC}{dw_3} = 2.1174*0.2362*0.45*0.5214*0.6 = 0.0704
$$

# 5. Atualização de pesos

Utilizaremos a taxa de aprendizado $\alpha = 0.5$

$$
\alpha - w_n * gradiente_{w_n}
$$

$new_{w1} = \alpha-w_1 * gradiente_{w_1} = 0.5 - 0.15 * 0.0246 = 0.4963$

$new_{w2} = \alpha-w_2 * gradiente_{w_2} = 0.5 - 0.20 * 0.0586 = 0.4882$

$new_{w3} = \alpha-w_3 * gradiente_{w_3} = 0.5 - 0.25 * 0.0296 = 0.4926$

$new*{w4} = \alpha-w_4 \* gradiente*{w_4} = 0.5 - 0.30 \* 0.0704 = 0.4957$0,4882

$new_{w5} = \alpha-w_5 * gradiente_{w_5} = 0.5 - 0.40 * 0.2780 = 0.3888$

$new_{w6} = \alpha-w_6 * gradiente_{w_6} = 0.5 - 0.45 * 0.2393 = 0.3923$

# 6. Recalculando a função de perda

$z_{h1} = w_1 i_1 + w_3 i_2 = 0.4963*0.5 + 0.4926*0.6 = 0.5436$

$a_{h1} = \sigma(z_{h1}) = \sigma(0.5436) = 0.6326$

$z_{h2} = w_2 i_1 + w_4 i_2 = 0.4882*0.5+0.4957*0.6 = 0.5415$

$a_{h2} = \sigma(z_{h2}) = \sigma(0.5415) = 0.6321$

$z_o = w_5 a_{h1} + w_6 a_{h2} = 0.3888*0.6326+0.3923*0.6321 = 0.4938$

$a_o = \sigma(z_o) = \sigma(0.4938) = 0.6210$

Utilizando a função de perda:

$C_{new}(y-a_o)^2 = (0.5-0.6210)^2 = 0.0146$

Comparando os resultados de $C$ e $C_{new}$, respectivamente $0.0137$ e $0.0146$, concluímos que os pesos novos pioraram o resultado final da rede. Porém, isso não é necessariamente algo ruim, uma vez que o algoritmo pode estar em uma área de mínimo local e pode estar “buscando” outra área para explorar.

Outro ponto relevante é que, se a taxa de aprendizado $\alpha$ estivesse menor, a rede poderia ter melhorado o resultado. Porém, como citado anteriormente, isso não é necessariamente bom, pois a rede pode estar em um mínimo local.
