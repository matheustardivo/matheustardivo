---
layout: post
title: "Endereço Novo, de novo"
date: 2011-09-02 10:20
comments: true
categories: blog ruby jekyll octopress wordpress github
---

Pois é... Mais uma mudança de endereço. Se alguém ainda lê esse blog deve estar se perguntando: "Por que esse cara não para quieto ein?" 

Pra responder essa pergunta, vamos fazer uma retrospectiva das minhas indas e vindas em meus blogs. 

{% img center http://s.wordpress.org/style/images/wp3-logo.png %} 

No ano de 2006 eu ainda era um participante assíduo do [fórum do GUJ](http://www.guj.com.br/forums/list.java) e via que uma boa parte dos participantes do fórum também escreviam coisas interessantes em seus blogs. Bom, eu também quero um :) 

Grande partes desses blogs estavam no [wordpress.com](http://wordpress.com/), então foi assim que eu criei o [meu primeiro blog](http://tardivo.wordpress.com/). 

A princípio o wordpress me atendia perfeitamente e não via necessidade nenhuma de ter meu próprio domínio. Mas um tempo depois percebi que grande parte desses caras que tinham um blog agora tinham também um domínio. 

{% img center https://s3.amazonaws.com/matheustardivo_blog/man_crying.png %} 

Então pensei: 

> Bom, tudo bem vai. Não vamos fazer tanto drama. Eu não preciso de um domínio... 
>
> Mas agora que estou fazendo uns projetinhos com Ruby on Rails, eu podia aproveitar pra comprar um domínio e um host que suporte Rails. 
>
> E olha só... Tem aqui um serviço de hospedagem baratinho que suporta Rails e também PHP. Posso colocar meus projetinhos e também usar o Wordpress. 
>
> Brilhante! Vou comprar.

E foi assim que em 2009 comprei o domínio [**Tardivo.org**](http://tardivo.org)

Matheus Tardivo.org
===================

Agora sim estou na moda de novo. Tenho meu blog, meu domínio e posso hospedar meus projetinhos pessoais. 

Continuei postando coisas novas durante um tempo, geralmente assuntos relacionados com métodos ágeis de desenvolvimento de software porque era onde eu concentrava meus estudos naquele momento. 

Foi então que eu mudei de emprego, mudei de cidade e com isso veio uma grande carga de trabalho e viagens que antes eu não tinha. Resultado: em 2 anos foram apenas 2 novos posts. E o pior é que eu fiquei pagando a hospedagem e o domínio (pouco... mas pagando).

E finalmente, a mudança pra cá
==============================

Com o surgimento do [Heroku](http://www.heroku.com/) a necessidade de ficar pagando a hospedagem para hospedar meus projetinhos pessoais se foi (aliás, desde o dia 25 de agosto o [Heroku está oficialmente suportando Java](http://blog.heroku.com/archives/2011/8/25/java/)). 

Fazer o deploy de uma aplicação em Rails ou [Sinatra](http://www.sinatrarb.com/) no Heroku é tão simples e prático que acabei esquecendo do meu host pessoal e passei a utilizar apenas o Heroku.

Mas e o blog? Simples: poderia voltar para o Wordpress ou então usar qualquer outro serviço das dezenas disponíveis. Foi ai que li muita gente abandonando o Wordpress por N motivos diferentes e passando a usar uma engine simples de geração de conteúdo estático, o [Jekyll](http://jekyllrb.com/). 

Pra entender melhor o que é o Jekyll, vou colocar aqui a descrição feita pelo próprio autor:

> Jekyll is a simple, blog aware, static site generator. It takes a template directory (representing the raw form of a website), runs it through Textile or Markdown and Liquid converters, and spits out a complete, static website suitable for serving with Apache or your favorite web server. This is also the engine behind [GitHub Pages](http://pages.github.com/), which you can use to host your project’s page or blog right here from GitHub.

E o mais legal de tudo: **feito em Ruby :)** 

Outro detalhe: o autor do Jekyll é o [Tom Preston-Werner](http://tom.preston-werner.com/), Cofounder do GitHub. 

Foi então que resolvi fazer um teste utilizando o Jekyll e hospedar o blog no GitHub Pages. O processo todo foi muito simples e pra incluir um novo post basta escrever o texto no seu editor preferido (no meu caso o [TextMate](http://macromates.com/)), git commit e git push. Pronto! Muito mais nerd que o Wordpress... com certeza. 

{% img center https://s3.amazonaws.com/matheustardivo_blog/troll_face.png %} 

A única coisa que não curti muito no Jekyll foi o fato dele não ter um layout padrão mais legal. E não é que já tinham pensado nisso? 

Octopress
=========

{% img center http://octopress.org/images/logo.png %} 

Mais uma vez, segue a descrição do autor (do Octopress, duh):

> Octopress is a framework designed by [Brandon Mathis](http://brandonmathis.com/) for Jekyll, the blog aware static site generator powering Github Pages. To start blogging with Jekyll, you have to write your own HTML templates, CSS, Javascripts and set up your configuration. But with Octopress All of that is already taken care of. Simply [clone or fork Octopress](https://github.com/imathis/octopress), install dependencies and the theme, and you’re set. 

Ou seja, usar o Jekyll já com um tema bem legal IMHO e também com diversos outros enhancements. Para mais detalhes, veja a [documentação](http://octopress.org/docs/). 

Então só faltava importar os meus posts antigos do Wordpress pro Octopress e subir pro github. Obviamente que alguém já tinha feito isso. Encontrei esse post: [Migrated To Octopress](http://martin.elwin.com/blog/2010/03/migrated-to-octopress/) do [Martin Elwin](http://martin.elwin.com/about/) onde ele descreve como fez a migração e o script que ele usou pra importar seus posts. 

Vou aproveitar pra colocar aqui também o gist que ele disponibilizou com o script de importação. 

{% gist 374148 %} 

Agora já posso parar de pagar a toa o domínio e a hospedagem (na verdade já parei... assim que terminar o período que paguei, já era). 

Muito bem. É isso... Depois desse trabalho todo é só voltar a escrever alguns posts de vez em quando. 

[update]
Pra manter um padrão, já que quase todos os meus contatos são <strong>matheustardivo</strong> (email, twitter, facebook, linkedin) resolvi manter um domínio [matheustardivo.com](http://matheustardivo.com)
[/update]
