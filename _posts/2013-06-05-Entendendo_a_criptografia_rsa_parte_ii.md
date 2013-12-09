---
layout: post_page
title: Entendendo a criptografia RSA - Parte II
---
No [post anterior][1] eu falei sobre como era o funcionamento do RSA e se você não o leu recomendo que [dê uma espiada nele][1] antes de continuar lendo este aqui. Neste post eu vou começar a mostrar a matemática usada no RSA, por isto recomendo que você peguem um papel, um lápis e uma calculadora…

###1. Definir valores para os números

Vamos criar uma tabela atribuindo a cada letra um número. Isto é necessário pois o RSA codifica somente números. Poderíamos usar a tabela ASCII, mas para facilitar as contas vamos criar a nossa própria:

|  A  |  B  |  C  |  D  |  E  |  F  |  G  |  H  |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:| 
|  1  |  2  |  3  |  4  |  5  |  6  |  7  |  8  |


|  I  |  J  |  K  |  L  |  M  |  N  |  O  |  P  |  Q  |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|  9  | 10  | 11  | 12  | 13  | 14  | 15  | 16  | 17  |


|  R  |  S  |  T  |  V  |  U  |  W  |  X  |  Y  |  Z  |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 18  | 19  | 20  | 21  | 22  | 23  | 24  | 25  | 26  |

###2. Escolhendo os números primos

A estratégia para a escolha dos primos é simples: quanto maior melhor. Para se ter uma ideia, a [RSA Data Security][3], que faz a padronização do RSA, recomenda que se utilize chaves de 2048 bits (o que dá um número com 617 dígitos!) se você quiser garantir que sua chave não seja quebrada até 2030. Para nosso exemplo vamos usar primos bem pequenos, que nossas calculadoras conseguem processar. Os primos serão

    p = 17, q = 41

Multiplicando-os nós obtemos um número n:

    n = p * q

Sendo assim, no nosso caso:

    n = 17 * 41 = 697

Este n é o tamanho do nosso conjunto. É necessário termos um conjunto finito de valores para que possamos fazer o caminho inverso ao realizado para cifrar nossa mensagem. Podemos, chamar nosso conjunto de Z697.

###3. A função totiente

Agora temos que calcular a [função totiente][4], ou φ(x) \[pronuncia-se "fi"\] de Euler – o [matemático][5], não o [filho do vento][6]. O que faz esta função? Eu diria que ela é o cerne do RSA, ela me diz a quantidade de co-primos de um numero que são menores que ele mesmo.

 
####• Co-primos ou Números primos entre si

Dois números são [primos entre si][7] quando o [Máximo Divisor Comum (MDC)][8] entre eles é 1.
Calcular a quantidade de co-primos de um número que são menores que ele é um trabalho tão ou mais difícil quanto calcular seus fatores primos… Exceto quando eu já conheço os fatores primos deste número. Para números formados por dois fatores primos ela fica com a seguinte forma:

    φ(x) = (p - 1) * (q - 1)

No nosso caso queremos calcular a função totiente do n.

    φ(n) = (p - 1) * (q - 1)
    φ(697) = (17 - 1) * (41 - 1)
    φ(697) = 640

###4. Calculando a chave pública

Devemos escolher um número e em que 1 < e < φ(n), de forma que e seja co-primo de φ(n). Em outras palavras, queremos um e onde o MDC(φ(n), e) = 1, sendo e > 1.

Para encontrar este número podemos ir tentando sequencialmente, iniciando os testes com o número 2:

    MDC(640, 2) = 2
    MDC(640, 3) = 1

O número 3 atende aos requisitos, mas podemos continuar calculando e usar qualquer co-primo que atenda os requisitos. Para este exemplo vamos usar o número 13.

Chave pública = (n, e)
Chave pública = (697, 13)
Beleza! Descobrimos nossa chave pública! No nosso exemplo ela será os números 697 e 13.

###5. Cifrando  a mensagem

Com a chave pública em mãos, é bem simples cifrar uma mensagem. Mas antes temos que fazer uma pausa e entender o que é e como funciona a Aritmética modular e vocês vão perceber que não estou louco quando digo que 13 vezes 9 pode ser 7.

####• Divisão inteira

Pode parecer estranho, mas é importante termos uma visão bem clara do que é uma divisão inteira para podermos realizar operações modulares. Vejam:

Se y divide x, podemos dizer que

    x : y = c

portanto existe um c tal que

    x = c * y + r

onde r é o resto da divisão. Por exemplo:

    23 : 7 = c
    23 = c * 7 + r
    23 = 3 * 7 + 2

Esta definição nos ajuda a perceber o que acontece quando o divisor é maior que o dividendo, por exemplo:

    5 : 18 = c
    5 = c * 18 + r
    5 = 0 * 18 + 5

portanto

    5 : 18 = 0
    5 mod 18 = 5

####• [Aritmética modular][9]

No dia a dia nós fazemos cálculos em conjuntos numéricos de tamanho infinito. Mas, como poderíamos fazer os mesmos cálculos em um conjunto de tamanho finito? Deixe eu explicar melhor:

    13 * 9 = 117

Ok, até agora nada de mais. Mas, e se eu tivesse um conjunto que fosse de 0 a 22? Vamos chamar o nosso conjunto de Z22.

    Z22 = {0, 1, 2, ... , 22}

Temos um problema, 117 não existe no conjunto Z22. Agora, dentro do conjunto Z22, como fazemos aquela conta? Simples, é só pensarmos que o nosso conjunto funciona como um relógio. No relógio, depois das 12 – que é seu valor máximo – ele volta para o 1. Seguindo a lógica, no Z22 quando passarmos de 22 voltamos para o 0. Para realizarmos uma multiplicação modular seguimos os seguintes passos:

    1. Realizamos a multiplicação comum

        13 * 9 = 117
    
    2. Calculamos o resto da divisão inteira do resultado pelo tamanho do conjunto

        117 mod 22 = 7

No RSA as operações modulares são extremamente importantes, principalmente a exponenciação. Ela funciona de maneira bem parecida com a multiplicação, vamos ver um exemplo (ainda em Z22):

    4 ^ 3 mod 22 = 4 * 4 * 4 mod 22

portanto

    64 mod 22 = 20

Com estas definições em mente, basta aplicar a seguinte fórmula a cada letra:

    c = m ^ e mod n

onde e é a chave pública e m é o valor numérico da letra. Adaptando para o nosso exemplo, ela fica:

    c = m ^ 13 mod 697

Agora que já sabemos como, vamos cifrar uma mensagem? O ano passado foi considerado o ano de Alan Turing, pai da computação e grande criptólogo. Vamos cifrar seu nome: 

| Letra |     Operação    | Resultado |
|:-----:|:---------------:|:---------:|
|   T   | 19 ^ 13 mod 697 |    15     |
|   U   | 20 ^ 13 mod 697 |    692    |
|   R   | 17 ^ 13 mod 697 |    391    |
|   I   | 09 ^ 13 mod 697 |    501    |
|   N   | 13 ^ 13 mod 697 |    421    |
|   G   | 07 ^ 13 mod 697 |    176    |

Ok, ciframos o texto e obtivemos o seguinte código:

    15 692 391 501 421 176

No próximo post vamos fazer o caminho oposto. Vamos calcular a chave privada e ver como tornar um texto criptografado com nossa chave pública legível novamente.

[1]: {% post_url 2013-06-05-Entendendo_a_criptografia_rsa %}
[3]: http://www.emc.com/domains/rsa/index.htm
[4]: http://pt.wikipedia.org/wiki/Fun%C3%A7%C3%A3o_totiente_de_Euler
[5]: http://pt.wikipedia.org/wiki/Leonhard_Euler
[6]: http://terceirotempo.bol.uol.com.br/quefimlevou/qfl/sobre/euller-o-filho-do-vento-4542.html
[7]: http://pt.wikipedia.org/wiki/N%C3%BAmeros_primos_entre_si
[8]: http://pt.wikipedia.org/wiki/M%C3%A1ximo_divisor_comum
[9]: http://pt.wikipedia.org/wiki/Aritm%C3%A9tica_modular
