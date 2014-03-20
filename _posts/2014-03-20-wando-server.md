---
layout: post_page
title: Wando server
comments: true
---

Últimamente tenho programado bastante em Python. Uma coisa que me incomodava muito ao lidar com aplicações web em Python era o servidor HTTP de desenvolvimento. Lidando com o [Flask][1] eu descobri o excelente servidor do projeto [Werkzeug][2]... só faltava uma coisa pra ele ficar 100%: logs mais legíveis.
Apesar de ele logar todas as requisições e erros, os logs eram muito chatos de ler. Eu mudei um pouco o layout deles, coloquei umas cores... e nasceu o [wando-server][3]. Acho que ficou melhor:

![Logs HTTP do wando-server][logs]

Para colorir eu uso os códigos de cores ANSI; graças a biblioteca [colorama][4] os códigos funcionam inclusive no Windows (que insiste em não seguir certos padrões).

A implementação foi bem simples pois o server do Werkzeug é uma extensão do servidor HTTP base do Python. A parte feia foi que tive que fazer um monkey patch para logar umas strings mágicas que estavam no meio do código do werkzeug.

O próximo passo será fazer com que os logs de erro também sejam mais legíveis. Se você quiser ajudar dê uma olhada no [github do projeto][5]; para instalar dê uma passada na [página do wando-server no Pypi][3], lá estarão todas as intruções necessárias.

ps. O nome do projeto surgiu de uma conversa com [@giovannibassi][6] sobre a morte do [Wando][7]. Eu disse que o próximo projetinho que eu fizesse pra comunidade eu colocaria "wando" no nome - e agora eu cumpri o combinado.

[1]: http://flask.pocoo.org/
[2]: http://werkzeug.pocoo.org/
[3]: https://pypi.python.org/pypi/wando-server
[4]: https://pypi.python.org/pypi/colorama
[5]: https://github.com/fholiveira/wando-server
[6]: https://twitter.com/giovannibassi
[7]: http://g1.globo.com/minas-gerais/noticia/2012/02/cantor-wando-morre-no-hospital-em-nova-lima.html
[logs]: /img/logs_wando.png
