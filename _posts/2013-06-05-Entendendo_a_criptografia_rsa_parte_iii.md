---
layout: post_page
title: Entendendo a criptografia RSA - Parte III
---
Olá pessoal. Este post vai explicar como decifrar as mensagens cifradas com RSA. Você pode ler a série do começo [clicando aqui][1]. No [post anterior][2] nós ciframos uma pequena mensagem – “TURING”, e neste vamos decifrá-la. Continuando o nosso passo-a-passo:

###6. Calculando a chave privada

Para descobrirmos a chave privada, precisamos encontrar o [inverso multiplicativo][3] do número e.

####• Inverso multiplicativo

A multiplicação de um número pelo seu inverso multiplicativo tem como resultado a identidade multiplicativa do conjunto. Nos conjuntos que estamos estudando esta identidade é sempre 1.

    a * (a ^ -1) = 1

####• Inverso multiplicativo modular

A primeira coisa que temos que saber é que como e é co-primo de φ(n) ele obrigatoriamente tem um inverso multiplicativo, pois dado que
MDC(φ(n), e) = 1 podemos aﬁrmar que existe um r e um d tal que

    1 = d * e - ( r * φ(n) )

onde d é o inverso modular de e.

Para calcular o inverso multiplicativo modular podemos usar o [Algoritmo de Euclides estendido][4]:

    [1]. Dividimos e por φ(n). No nosso exemplo ﬁcaria

        13 : 640 = 0 com resto 13

    [2]. Dividimos o divisor anterior pelo resto

        640 : 13 = 49 com resto 3

    [3]. Novamente, dividimos o divisor anterior pelo resto

        13 : 3 = 4 com resto 1
    
Ao chegarmos no resto 1 devemos parar. Até agora nós só executamos a [parte tradicional][5] do Algoritmo de Euclides, para executar a parte estendida, devemos isolar o resto na formula da deﬁnição de divisão inteira:

    x = c * y + r
    r = x - (c * y)

que também pode ser escrito como

    r = (1 * x) - (c * y)

Continuando com a parte extendida do algoritmo:

    [4]. Escrevemos o passo [1] aplicando a fórmula anterior

        13 = (1 * 13) - (0 * 640)
        
    [5]. Vamos fazer o mesmo, mas agora usando o passo [2]

        3 = (1 * 640) - (49 * 13)

    e como o passo [4] nos diz que 13 = (1 * 13), usamos este resultado na nossa equação

        3 = (1 * 640) - 49 * ((1 * 13) - (0 * 640))

    distribuímos a multiplicação, mas não operamos com os valores de x e y

        3 = (1 * 640) - (49 * 13) - (0 * 640)

    e somamos os múltiplos comuns

        3 = (1 * 640) - (49 * 13)

    [6]. Fazemos a mesma coisa feita no passo [5], mas agora usando a igualdade do passo [3]

        1 = (1 * 13) - (4 * 3)
    
    e como o passo [5] nos diz que 3 = (1 * 640) – (49 * 13), usamos este resultado na nossa equação

        1 = (1 * 13) - 4 * ( (1 * 640) - (49 * 13) )

    distribuímos a multiplicação, mas não operamos com os valores de x e y

        1 = (1 * 13) - (4 * 640) + (196 * 13)

    agora podemos somar os múltiplos comuns

        1 = (197 * 13) - (4 * 640)

Pronto! O inverso do nosso e = 13 é um d = 197, pois na equação acima 197 multiplica 13. A chave privada é composta pelo n e o d, que no nosso caso são os números 697 e 197.

###7. Decifrando a mensagem

Agora que temos a chave privada podemos decifrar a mensagem que criptografamos no último post. A mensagem cifrada era:

    15 692 391 501 421 176

Para decifrar a mensagem basta aplicar realizar uma exponenciação modular para o valor de cada letra, que podemos descrever com a seguinte fórmula:

    m = c ^ d mod n

onde d é a chave privada, n é o tamanho do conjunto e c é o valor numérico da letra cifrada. Adaptando ao nosso exemplo temos:

    m = c ^ 197 mod 697

Para facilitar podemos organizar uma tabela; recomendo que utilizem o [Wolfran][6] para conferir as contas…

| Número |      Operação     | Letra |
|:------:|:-----------------:|:-----:|
|   15   | 15 ^ 197 mod 697  |	 19  |
|   692  | 692 ^ 197 mod 697 |	 20  |
|   391  | 391 ^ 197 mod 697 |	 17  |
|   501  | 501 ^ 197 mod 697 |	 09  |
|   421  | 421 ^ 197 mod 697 |	 13  |
|   176  | 176 ^ 197 mod 697 |	 07  |

Se utilizarmos a tabela de valor/letra que definimos no [primeiro post][1] para descobrir qual letra representa cada número obtido, teremos o texto

    TURING

Deu certo! :)

[1]: {% post_url 2013-06-05-Entendendo_a_criptografia_rsa %}
[2]: {% post_url 2013-06-05-Entendendo_a_criptografia_rsa_parte_ii %}
[3]: http://pt.wikipedia.org/wiki/Inverso_multiplicativo
[4]: http://pt.wikipedia.org/wiki/Algoritmo_de_Euclides_estendido
[5]: http://pt.wikipedia.org/wiki/Algoritmo_de_Euclides
[6]: http://www.wolframalpha.com/
