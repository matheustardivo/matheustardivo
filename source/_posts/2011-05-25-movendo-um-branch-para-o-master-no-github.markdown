--- 
layout: post
title: Movendo um branch para o master no github
---
Quando eu começei a estudar Rails resolvi fazer um <a href="https://github.com/matheustardivo/financeiro">projetinho</a> para botar em prática o que eu estava estudando. Pensei num projeto que também me ajudasse de alguma forma, e naquele momento, um grande problema que eu tinha era meu controle financeiro - veja bem, problemas financeiros eu ainda tenho e acho que sempre vou ter, mas estou falando apenas de <strong>controle</strong> :D

Na época a grande maioria dos livros só falavam sobre a versão 2 do Rails e por isso iniciei meu projeto nessa versão - no meu <a href="http://matheustardivo.github.com/blog/2009/03/29/ruby-inside-brasil">último post</a> tem as fontes de estudo que usei.

Logo percebi que eu estava investindo muito tempo estudando uma versão antiga do Rails e resolvi passar a estudar a versão 3. Por consequência, eu tive que fazer uma mudança razoável na estrutura do meu projeto. Como a mudança foi grande pensei que fazer o merge desse código no master do repositório do github seria muito complicado, então resolvi criar um branch para a versão nova e trabalhar nele.

A questão é que esse novo branch acabou virando o principal e achei que a coisa ficou meio desorganizada. Solução: mover o branch para o master e renomear o branch para "rails2". Cheguei até a cadastrar um <a href="https://github.com/matheustardivo/financeiro/issues/9">issue</a> para isso no github e ele ficou lá por muito tempo, até que hoje resolvi acertar esse problema.

Encontrei uma <a href="http://limi.co.uk/posts/renaming-master-branch-on-github">referência</a> que me ajudou bastante a fazer essa mudança, mas no meu caso tive que fazer algumas pequenas alterações:

<ol>
<li>No seu repositório do github, clicar em Admin e mudar o branch default para o branch de desenvolvimento.</li>
<li>Renomear os branches locais (executar os comandos abaixo na sua cópia local sem clonar seu repositório depois da alteração do branch default):
``` bash
    git branch -m master rails2 
    git branch -m v1.1.0.rails3 master
```
Onde: <strong>rails2</strong> é o novo branch para manter o código antigo e <strong>v1.1.0.rails3</strong> é o branch de desenvolvimento atual</li>
<li>Ao invés de remover o master remoto, fiz o push forçado do código atual:
``` bash
    git push -f origin master
```
</li>
<li>Mudar novamente o repositório default no github para o master e depois apagar o branch antigo:
``` bash
    git branch origin :v1.1.0.rails3
```
</li>
</ol>

E pronto! Agora o repositório ficou organizado da seguinte forma:
<ul>
<li><strong>master</strong>: Código de desenvolvimento com o Rails na versão 3</li>
<li><strong>rails2</strong>: Código da versão inicial utilizando Rails 2</li>
</ul>

E o branch v1.1.0.rails3 que era o de desenvolvimento se foi :D

Espero que isso seja útil pra quem também fez <strike>cagada</strike> confusão com seu repositório ou esteja tentando renomear um branch.
