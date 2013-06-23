---
layout: post
title: "Pull Request Driven Development"
date: 2011-09-25 21:42
comments: true
categories: github git brach merge
---

Recentemente li um post muito interessante escrito pelo **Zach Holman** com o seguinte título: [How GitHub Works: Be Asynchronous](http://zachholman.com/posts/how-github-works-asynchronous/).

Esse é o segundo post de um total de três falando um pouco do modo de trabalho no Github. Segue os links para os outros posts:

  *  [How GitHub Works: Hours are Bullshit](http://zachholman.com/posts/how-github-works-hours/)</li>
  *  [How GitHub Works: Creativity is Important](http://zachholman.com/posts/how-github-works-creativity/)</li>

{% img center https://raw.github.com/mojombo/octobeer/master/images/2011-03-14/inlay.jpg %} 

A imagem acima não tem nada haver com o assunto desse post, mas não resisti colocar ela aqui. Essa é uma foto do [Octobeer](http://octobeer.me/), o [Kegerator](http://en.wikipedia.org/wiki/Kegerator) do escritório do Github. Ou seja, enquanto aqui no [iG](http://www.ig.com.br) convidamos os colegas de trabalho para tomar um café, lá eles tomam um choppinho - claro ou escuro :) 

Bom, voltando ao assunto... Pela primeira vez tenho a oportunidade de trabalhar em um projeto com vários desenvolvedores que usam o Github como repositório de código. Até então usavamos o [Mercurial](http://mercurial.selenic.com/). Apesar da semelhança entre o Git e o Mercurial, tivemos algum trabalho com a utilização do Mercurial que esperamos acabar ou pelo menos reduzir usando o Github. 

E é ai que entra uma parte do texto do Holman: tornar o workflow de desenvolvimento assíncrono envolve a utilização de [Pull Requests](https://github.com/features/projects/codereview#codereview_bucket). Segue abaixo uma breve descrição dessa funcionalidade retirada do site do Github: 

> **Pull Requests** are **living discussions** that streamline the process of discussing, reviewing, and managing changes to **code**. 

PRDD na prática
===============

Uau... **PRDD**... Muito boa essa sigla ein? Já pensou você falando numa entrevista de emprego assim: *Já trabalhei num projeto usando Rails, com BDD + TDD e PRDD*. Com certeza será contratado na hora :) 

Brincadeiras a parte, vamos entender na prática como funciona a utilização de Pull Requests no workflow de desenvolvimento. 

Quando você for trabalhar na inclusão de uma nova funcionalidade ou então em algo que traga algum impacto no codebase do projeto, então crie um novo branch para isso. Um exemplo: um desenvolvedor vai trabalhar numa funcionalidade de streaming para o projeto [resque-pastie](https://github.com/matheustardivo/resque-pastie) (esse projeto é uma só uma brincadeira que fiz aqui para testar o [Resque](https://github.com/blog/542-introducing-resque) com Rails). Então vamos criar o novo branch **streaming** e já mudar para ele com o seguinte comando: 

{% codeblock %}
git checkout -b streaming
{% endcodeblock %}

E faça o push desse branch para o repositório remoto: 

{% codeblock %}
git push github streaming
{% endcodeblock %}

Quando terminar a codificação da funcionalidade, crie um Pull Request para esse branch: 
Acesse o repositório no Github e mude o branch de **master** para **streaming** e depois clique no botão **Pull Request** que fica a direita da tela. Veja a figura abaixo: 

{% img center https://s3.amazonaws.com/matheustardivo_blog/1_pull_request.png %} 

Revise o seu Pull Request e coloque os comentários necessários. Lembrando que o texto desse comentário será parseado pelo [Github Flavored Markdown](http://github.github.com/github-flavored-markdown/). Então você pode formatar o texto, incluir imagens, blocos de código e tudo mais que o parser do GFM suportar. Agora basta clicar no botão **Send pull request** para criá-lo. 

{% img center https://s3.amazonaws.com/matheustardivo_blog/2_pull_request.png %} 

O workflow agora está no ponto onde os demais desenvolvedores do projeto fazem a revisão do Pull Request criado. Você ainda pode fazer um deploy parcial desse branch em um ambiente de QA ou até mesmo em algumas máquinas de produção para testar o que foi feito, verificar se tudo está ok e então fazer o **merge** para o **master**. Segue uma imagem da tela de revisão do Pull Request: 

{% img center https://s3.amazonaws.com/matheustardivo_blog/3_pull_request.png %} 

Veja que neste caso, como a alteração realizada no código foi bem simples, o Github exibe um botão para fazer o merge automático do código. 

Mesmo depois de criado o Pull Request, você e os outros desenvolvedores ainda podem fazer alterações e incluir outros commits nesse mesmo Pull Request. Basta fazer o push desses commit no mesmo branch, neste caso o branch **streaming**. Veja na figura abaixo que um outro commit foi incluído no mesmo Pull Request: 

{% img center https://s3.amazonaws.com/matheustardivo_blog/4_pull_request.png %} 

Agora vamos fazer o merge automático desse pull request. Para isso é só clicar em **Merge pull request** e depois em **Confirm merge**. Veja que o Pull Request foi fechado e as alterações feitas naquele branch agora estão no **master**. 

{% img center https://s3.amazonaws.com/matheustardivo_blog/5_pull_request.png %} 

{% img center https://s3.amazonaws.com/matheustardivo_blog/6_pull_request.png %} 

Você também pode fazer o merge desse Pull Request manualmente pelo terminal. Para isso criei um outro branch com o nome de **manual**, fiz o commit do código alterado, o push desse branch para o repositório e criei um novo Pull Request. 

Agora vamos fazer o merge: 

O primeiro passo é mudar para o branch master:
{% codeblock %}
$ git checkout master
Switched to branch 'master'
{% endcodeblock %}

Não se esqueça de fazer o pull para obter as últimas alterações:
{% codeblock %}
$ git pull github master
From github.com:matheustardivo/resque-pastie
 * branch            master     -> FETCH_HEAD
Updating d33046c..e63d721
Fast-forward
 README.md |    4 ++++
 1 files changed, 4 insertions(+), 0 deletions(-)
{% endcodeblock %}

Agora faça o **fetch do branch** que originou o Pull Request:
{% codeblock %}
$ git fetch github manual
From github.com:matheustardivo/resque-pastie
 * branch            manual     -> FETCH_HEAD
{% endcodeblock %}

Faça o **merge** do brach:
{% codeblock %}
$ git merge manual
Merge made by recursive.
 README.md |    2 ++
 1 files changed, 2 insertions(+), 0 deletions(-)
{% endcodeblock %}

E finalmente o **push** para o master:
{% codeblock %}
$ git push github master
Counting objects: 1, done.
Writing objects: 100% (1/1), 227 bytes, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:matheustardivo/resque-pastie.git
  e63d721..e2ad64a  master -> master
{% endcodeblock %}

Entre no Github e você verá que o código do branch manual foi incorporado no master e o Pull Request foi fechado. 

Bom, a ideia é essa. No decorrer da utilização do **PRDD** no nosso dia-a-dia com certeza irão surgir alguns problemas e talvez outras maneiras de lidar com o workflow de desenvolvimento, mas é importante ver o que ganhamos usando os Pull Requests: comunicação assíncrona, revisão do código por outros desenvolvedores, organização do repositório por funcionalidades desenvolvidas, possibilidade de testar tais funcionalidades isoladamente de forma simples e etc. 

Não deixe de ler a documentação do Github sobre Pull Requests:

  *  [Futuristic Code Review](https://github.com/features/projects/codereview#codereview_bucket)
  *  [Send pull requests](http://help.github.com/send-pull-requests/)
