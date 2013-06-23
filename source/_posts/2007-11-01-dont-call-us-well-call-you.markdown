--- 
layout: post
title: Don't call us, we'll call you.
wordpress_id: "7"
wordpress_url: http://tardivo.wordpress.com/2007/11/01/dont-call-us-well-call-you/
---
O padrão <a href="http://en.wikipedia.org/wiki/Template_method_pattern">Template Method</a> define o esqueleto de um algoritmo dentro de um método transferindo alguns de seus passos para as subclasses. O Template Method permite que as subclasses redefinam certos passos de um algoritmo sem alterar a estrutura do próprio algoritmo.

Basicamente, o padrão consiste na criação de um template para o algoritmo. Mas o que é um template? É apenas um método - ou, mais especificamente, é um método que define um algoritmo como uma seqüência de passos. Um ou mais desses passos podem ser definidos como abstratos e implementados por uma subclasse. Isto assegura que a estrutura do algoritmo permaneça inalterada mesmo quando as subclasses fornecem parte da implementação.

Verifiquemos o diagrama de classes: 
{% img center http://s3.amazonaws.com/matheustardivo_blog/templatemethod2.png %}

<ul>
	<li>A ClasseAbstrata contém o template method e versões abstratas das operações usadas no template method.</li>
	<li>O template method utiliza operações primitivas para implementar um algoritmo, mas desconectada da implementação efetiva dessas operações.</li>
	<li>A ClasseConcreta implementa as operações abstratas, que são chamadas quando o templateMethod() precisa delas.</li>
	<li>Podem existir muitas classes concretas, cada uma implementando o conjunto de operações exigidas pelo template method.</li>
</ul>

E o código:
<strong>ClasseAbstrata</strong>

{% codeblock lang:java %}
public abstract class ClasseAbstrata {

	public final void templateMethod() {
		operacaoPrimitiva1();
		operacaoPrimitiva2();
		operacaoConcreta1();
		operacaoConcreta2();
	}

	public abstract void operacaoPrimitiva1();

	public abstract void operacaoPrimitiva2();

	public void operacaoConcreta1() {
		// implementação
	}

	public void operacaoConcreta2() {
		// implementação
	}
}
{% endcodeblock %}

<strong>ClasseConcreta</strong>
{% codeblock lang:java %}
public class ClasseConcreta extends ClasseAbstrata {

	public void operacaoPrimitiva1() {
		// implementação da operação primitiva 1
	}

	public void operacaoPrimitiva2() {
		// implementação da operação primitiva 1
	}
}
{% endcodeblock %}

Usando:
{% codeblock lang:java %}
ClasseAbstrata classe = new ClasseConcreta();
classe.templateMethod();
{% endcodeblock %}

Uma forma de controle das extensões de subclasses são os métodos gancho (hook methods, em inglês). É possível então criar um template method que chama os métodos gancho, permitindo um controle do comportamento somente nestes pontos.

Agora um código de exemplo usando o gancho:
{% codeblock lang:java %}
public abstract class ClasseAbstrata {

	public final void templateMethod() {
		// Agora a operação está condicionada ao gancho.
		if (gancho()) {
			operacaoPrimitiva1();
		}
		operacaoPrimitiva2();
		operacaoConcreta1();
		operacaoConcreta2();
	}

	public abstract void operacaoPrimitiva1();

	public abstract void operacaoPrimitiva2();

	public void operacaoConcreta1() {
		// implementação
	}

	public void operacaoConcreta2() {
		// implementação
	}

	// Por padrão, o gancho apenas retorna true.
	public boolean gancho() {
		return true;
	}
}
{% endcodeblock %}

As subclasses podem ou não sobrescrever o gancho. Devem fazer isso apenas se desejam mudar o comportamento padrão, como por exemplo, adicionar alguma condição para a execução da operacaoPrimitiva1.
Exemplo:
{% codeblock lang:java %}
public class ClasseConcreta extends ClasseAbstrata {

	public void operacaoPrimitiva1() {
		// implementação da operação primitiva 1
	}

	public void operacaoPrimitiva2() {
		// implementação da operação primitiva 1
	}

	@Override
	public boolean gancho() {
		// Faz alguma verificação
		if (...) {
			return true;
		}

		return false;
	}
}
{% endcodeblock %}

Mas você deve estar se perguntando: "O que o título deste post tem haver com esse padrão?". A resposta é simples: muito!
Este título é o <a href="http://en.wikipedia.org/wiki/Hollywood_Principle">Princípio de Hollywood</a>, que traduzido significa: <em>Não nos telefone, nós telefonaremos para você.</em>

O Princípio de Hollywood nos proporciona uma maneira de evitar o "colapso das dependências". Colapso das dependências é o que acontece quando você tem componentes de alto nível dependendo de componentes de baixo nível que dependem de componentes de alto nível que dependem de componentes laterais que dependem de componentes de baixo nível, e assim por diante (ufa). Quando essa estrutura entra em colapso, ninguém mais consegue entender como o sistema foi originalmente projetado.

Com o Princípio de Hollywood, nós permitimos que componentes de baixo nível se conectem ao sistema através de ganchos, mas são os componentes de alto nível que determinam quando e como eles serão solicitados. Em outras palavras, os componentes de alto nível dizem aos componentes de baixo nível: <em>"Não nos telefone, nós telefonaremos para você."</em>

É provável que você já tenha detectado a conexão entre o Princípio de Hollywood e o Template method: quando usamos um template method, na verdade estamos dizendo às subclasses: <em>"não nos telefone, nós telefonaremos para vocês"</em>.
