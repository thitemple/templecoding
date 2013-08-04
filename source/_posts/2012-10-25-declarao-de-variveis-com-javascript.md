---
layout: post
title: Declaração de variáveis com JavaScript
categories:
- Desenvolvimento
tags:
- javascript
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _revision-control: a:1:{i:0;s:8:"defaults";}
  _syntaxhighlighter_encoded: '1'
  dsq_thread_id: '901383594'
  _yoast_wpseo_linkdex: '72'
  _yoast_wpseo_focuskw: variáveis com javascript
  _yoast_wpseo_title: Sofrendo menos com JavaScript - Declaração de variáveis com
    JavaScript
  _yoast_wpseo_metadesc: Entender como declarar variáveis com JavaScript é muito simples
    e pode salvar uma grande quantidade de tempo e trabalho quando fazemos os nossos
    scripts.
  _yoast_wpseo_meta-robots-noindex: '0'
  _yoast_wpseo_meta-robots-nofollow: '0'
  _yoast_wpseo_meta-robots-adv: none
  _yoast_wpseo_sitemap-include: ! '-'
  _yoast_wpseo_sitemap-prio: ! '-'
  _yoast_wpseo_canonical: ''
  _yoast_wpseo_redirect: ''
  _yoast_wpseo_opengraph-description: ''
  _yoast_wpseo_google-plus-description: ''
  _wp_old_slug: sofrendo-menos-com-javascript-declarao-de-variveis
  _clicky_goal: a:2:{s:2:"id";s:0:"";s:5:"value";s:0:"";}
  _wpas_skip_2089817: '1'
  _wpas_skip_2089821: '1'
  _wpas_skip_2089827: '1'
  _yoast_wpseo_metakeywords: ''
  qtrans_meta:title: <!--:pt-->Sofrendo menos com JavaScript - Declaração de variáveis
    com JavaScript<!--:-->
  qtrans_meta:keywords: <!--:pt-->declaração variáveis javascript<!--:-->
  qtrans_meta:description: <!--:pt-->Entender como declarar variáveis com JavaScript
    é muito simples e pode salvar uma grande quantidade de tempo e trabalho quando
    fazemos os nossos scripts.<!--:-->
  _qts_slug_pt: declaracao-de-variaveis-com-javascript
  _qts_slug_en: declaracao-de-variaveis-com-javascript
---
<!--:pt-->Então você tem sofrido com JavaScript, odeia a linguagem e não vê a hora de achar aquele script pronto na internet e terminar logo com isso? Bom, a maioria dos devs que eu conheço pensam assim.

Se você pensa assim, ou mesmo se você quer conhecer um pouco melhor de JavaScript, a ideia aqui é passar 11 pequenas dicas de como a linguagem funciona, seja para entender melhor um script encontrado na internet ou para escrever o seu próprio de uma maneira melhor.
<h3><strong>Por que?</strong></h3>
Simples, porque javascript hoje roda em (praticamente) todo lugar: browsers, celulares, tablets, Windows Apps, jogos, no servidor, etc. E, embora seja possível encontrar scripts, frameworks e plugins que façam quase tudo, sempre tem um momento em que temos que escrever um pouco de JavaScript. Por fim, entender como declarar variáveis com JavaScript é muito simples e pode salvar uma grande quantidade de tempo e trabalho quando fazemos os nossos scripts.
<h3><strong>A Lista</strong></h3>
Então, esses são os 11 itens sobre os quais eu quero falar:
<ol>
	<li>Declaração de variáveis</li>
	<li><a href="http://templecoding.com/elevao-em-javascript/">Elevação</a></li>
	<li><a href="http://templecoding.com/converter-numeros-em-javascript-e-uma-droga/">Converter números em JavaScript é uma droga</a></li>
	<li><a href="http://templecoding.com/javascript-ponto-virgula/">JavaScript e o problema com o ponto e vírgula</a></li>
	<li><a href="http://templecoding.com/tratamento-excecoes-javascript/">Tratamento de exceções em JavaScript</a></li>
	<li><a href="http://templecoding.com/escopo-no-javascript/">Escopo no JavaScript: esse this não é daqui</a></li>
	<li><a href="http://templecoding.com/funcoes-de-callback-no-javascript/">Loops for in</a></li>
	<li><a href="http://templecoding.com/funcoes-de-callback-no-javascript/">Funções de callback</a></li>
	<li><a href="http://templecoding.com/construtores-de-arrays-do-javascript/">Construtores de Arrays</a></li>
	<li>Construindo objetos</li>
	<li>O JSLint existe pra ajudar</li>
</ol>
<h2>Declaração de variáveis com JavaScript</h2>
<h3><strong>Sempre use var</strong></h3>
Vai declarar uma variável? Use <strong>var</strong>! Simples, essa é a regra.
Toda a vez que uma variável é declara/usada sem o <strong>var</strong> ela passa a ser uma variável global, que, no caso do browser é sempre o objeto window.
Veja o código abaixo:
<pre class="brush: js; ruler: true; smart-tabs: false;">nome = "Thiago";

var func1 = function (param) {
  var nome = param;
  document.write(nome); // Michael
};

var func2 = function (param) {
  var nome = param;
  document.write(nome); // Jordan
};
document.write(nome); // Thiago
func1('Michael');
document.write(nome); // Thiago
func2('Jordan');
document.write(nome); // Thiago
document.write(window.nome); // Thiago</pre>
O que acontece aqui é que no momento em que declaro <strong>nome = "Thiago"</strong>, como não usei <strong>var</strong>, nome passa a fazer parte do objeto <strong>window</strong>. Dentro das funções func1 e func2 também não declarei nome usando <strong>var</strong> e, portanto, nesse caso <strong>nome</strong> também faz parte do objeto <strong>window</strong>.
Isso fica claro quando eu exibo o valor de <strong>nome</strong> usando as duas formas abaixo e obtenho o mesmo resultado:
<pre class="brush: js; ruler: true; smart-tabs: false;">document.write(nome);
document.write(window.nome);</pre>
Quando executamos código fora de uma função esse código também esta no escopo global, ou seja, no <strong>window</strong>. Portanto acessar <strong>nome</strong> e <strong>window.nome</strong> nesse caso é a mesma coisa.
Agora veja a diferença desse código:
<pre class="brush: js; ruler: true; smart-tabs: false;">nome = "Thiago";

var func1 = function (param) {
  var nome = param;
  document.write(nome); // Michael
};

var func2 = function (param) {
  var nome = param;
  document.write(nome); // Jordan
};
document.write(nome); // Thiago
func1('Michael');
document.write(nome); // Thiago
func2('Jordan');
document.write(nome); // Thiago
document.write(window.nome); // Thiago</pre>
A primeira declaração <strong>nome = "Thiago"</strong> continua global, mas dentro das funções func1 e func2 eu redeclarei a variável <strong>nome</strong> usando <strong>var</strong>, nesse momento elas passam a fazer parte do escopo da função em que são declaradas, assim, não interferindo com a variável global.
<h3>Evite atribuição de valores em cadeia</h3>
Veja o exemplo abaixo:
<pre class="brush: js; ruler: true; smart-tabs: false;">var f1 = function() {
  var a = b = "Isso nao deve estar certo";
};
document.write(b); // Isso nao deve estar certo
document.write(a); // ReferenceError: a is not defined</pre>
Pode até parecer que estamos declarando <strong>a</strong> e <strong>b</strong> dentro da função, mas nesse caso, somente <strong>a</strong> esta sendo declarado no escopo da função, <strong>b</strong> foi declarado como variável global, por isso fora da função a gente consegue acessar <strong>b</strong>, mas no caso de <strong>a</strong>, recebemos um erro, <strong>a</strong> só esta disponível dentro da função, conforme esperado.

A declaração correta para termos <strong>a</strong> e <strong>b</strong> dentro do escopo da função seria:
<pre class="brush: js; ruler: true; smart-tabs: false;">var f1 = function() {
  var a, b = "Isso nao deve estar certo";
};</pre>
A diferença é sutil, mas o suficiente para causar uma boa dor de cabeça.
<h3>Por que isso é imporante?</h3>
A razão é simples, para não termos interferências inesperadas. Por exemplo: se, sem querer algum programador <del>desleixado</del> descuidado, declarar variáveis com o mesmo nome em funções diferentes elas podem interferir uma com a outra, e esse é um caso bem complicado de debugar.

Outro cenário muito comum nos dias de hoje: usamos muitos scripts, frameworks e plugins prontos que encontramos na internet, e a gente nunca sabe como foi feito o código desses plugins. Com certeza, muitos deles usam nomes comuns como result, response, request, etc. E esses nomes podem conflitar com nosso próprio código.
<h3>Solução</h3>
Sempre usar var, e evite fazer atribuição de valores em cadeia. Também não escreva scripts que não façam parte de um escopo, que no caso de JavaScript, é basicamente uma função. Mas vou falar de escopo um pouco mais pra frente.<!--:-->
