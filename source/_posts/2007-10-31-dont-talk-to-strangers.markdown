--- 
layout: post
title: Don't talk to strangers
wordpress_id: "6"
wordpress_url: http://tardivo.wordpress.com/2007/10/31/dont-talk-to-strangers/
---
A <a href="http://en.wikipedia.org/wiki/Law_of_Demeter">Lei de Deméter ou o Princípio do Mínimo Conhecimento</a> nos orienta a reduzir as interações entre objetos, limitando-as a apenas alguns "amigos" mais próximos.

No projeto de um sistema, você deve tomar cuidado com o número de classes com que qualquer objeto interage e também com a forma como essa interação ocorre.

Este princípio nos orienta a não criar projetos com um grande número de classes interconectadas, o que faz com que qualquer alteração numa parte do sistema exerça um efeito em cascata sobre outras partes. Um sistema com muitas dependências entre múltiplas classes é um sistema frágil, de difícil manutanção e complexo demais para ser compreendido por outros.

O princípio nos fornece algumas dicas: pegamos um objeto qualquer e, a partir de qualquer método nesse objeto, só podemos invocar métodos quer pertençam:
<ul>
	<li>Ao próprio objeto</li>
	<li>A objetos que tenham sido passados como parâmetro para o método</li>
	<li>A qualquer objeto que seja criado ou instanciado pelo método</li>
	<li>A quaisquer componentes do objeto - Pense em "componente" como qualquer objeto que seja referenciado por uma variável de instância. Em outras palavras, esta seria uma relação TEM-UM.</li>
</ul>

Um exemplo: 

<strong>Sem</strong> o princípio:

{% codeblock lang:java %}
public float getTemp() {
	Thermometer thermometer = station.getThermometer();
	return thermometer.getTemperature();
}
{% endcodeblock %}

<strong>Com</strong> o princípio:
{% codeblock lang:java %}
public float getTemp() {
	return station.getTemperature();
}
{% endcodeblock %}

Em um outro exemplo:
{% codeblock lang:java %}
String nomeFilial = funconario.getDepartamento().getFilial().getNome();
{% endcodeblock %}

Caso você tenha que navegar muito nos seus objetos você está, provavelmente, com um problema na sua modelagem. Neste caso, talvez o que você teria que fazer seria <a href="http://en.wikipedia.org/wiki/Delegation_%28programming%29">delegar</a> isso para uma classe de "meio de campo".

O uso de <a href="http://en.wikipedia.org/wiki/Design_Patterns">Padrões de Projeto</a> e outros Princípios OO ajudam na aplicação da Lei de Deméter.

Já que comentei sobre Padrões de Projeto e outros Princípios OO, vou listar abaixo alguns desses princípios:
<ul>
	<li>Encapsule o que varia</li>
	<li>Dê prioridade à <a href="http://en.wikipedia.org/wiki/Composite_pattern">composição</a> em relação à herança</li>
	<li>Programe para interface, não para implementações - <a href="http://en.wikipedia.org/wiki/Strategy_pattern">Estratégia</a></li>
	<li>Tente manter conexões flexíveis entre objetos que interagem.</li>
	<li>As classes devem estar abertas para a extensão, mas fechadas para modificação.</li>
</ul>
E alguns padrões:
<ul>
	<li><a href="http://en.wikipedia.org/wiki/Decorator_pattern">Decorador</a></li>
	<li><a href="http://en.wikipedia.org/wiki/Facade_pattern">Fachada</a></li>
</ul>
Claro que existem outros padrões que podem auxiliá-lo e, por isso, recomendo a leitura de livros sobre o assunto.

A Lei de Deméter é uma <a href="http://en.wikipedia.org/wiki/Rule_of_thumb">Rule of thumb</a> e seu uso deve ser priorizado, mas, por diversos motivos, nem sempre podemos utilizá-la.

Embora o Princípio do Mínimo Conhecimento reduza as dependências entre objetos, e estudos tenham comprovado que isso simplifica a manutenção do software, sua aplicação também exige que mais classes "envelopadoras" sejam escritas para lidar com as chamadas de métodos em outros componentes. Isso pode aumentar a complexidade e o tempo de desenvolvimento do software, além de reduzir seu desempenho durante a execução.

Uma boa parte desse texto foi retirado do livro <a href="http://www.headfirstlabs.com/books/hfdp/">Head First Design Patterns</a> que, com certeza, é uma ótima leitura.
