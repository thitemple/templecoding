---
layout: post
title: Construtores de Arrays do JavaScript
categories:
- Desenvolvimento
tags:
- javascript
status: publish
type: post
published: true
meta:
  _yoast_wpseo_linkdex: '80'
  _edit_last: '2'
  _yoast_wpseo_focuskw: construtores de arrays
  _yoast_wpseo_title: ''
  _yoast_wpseo_metadesc: Os construtores de Arrays do JavaScript podem ter comportamentos
    um pouco inesperados, por isso procuramos usar os construtores de Arrays implícitos.
  _yoast_wpseo_metakeywords: ''
  _yoast_wpseo_meta-robots-noindex: '0'
  _yoast_wpseo_meta-robots-nofollow: '0'
  _yoast_wpseo_meta-robots-adv: none
  _yoast_wpseo_sitemap-include: ! '-'
  _yoast_wpseo_sitemap-prio: ! '-'
  _yoast_wpseo_canonical: ''
  _yoast_wpseo_redirect: ''
  _yoast_wpseo_opengraph-description: ''
  _yoast_wpseo_google-plus-description: ''
  _wpas_done_all: '1'
  _clicky_goal: a:2:{s:2:"id";s:0:"";s:5:"value";s:0:"";}
  _wpas_skip_2089817: '1'
  _wpas_skip_2089821: '1'
  _wpas_skip_2089827: '1'
  dsq_thread_id: '1132841693'
---
Bom, o ano começou, lá se foi o carnaval e agora sim podemos dizer que o ano começou. Pelo menos aqui no blog. <img class="wlEmoticon wlEmoticon-smile" style="border-style: none;" src="http://templecoding.com/wp-content/uploads/2013/02/wlEmoticon-smile.png" alt="Alegre" />

Bora falar um pouco mais de JavaScript e o que existe de estranho com os <strong>construtores de Arrays do JavaScript</strong>.
<div style="margin: 5px 0px; border: #f48432 1px dashed; padding: 5px;">

<strong>Sofrendo menos com JavaScript</strong>
Esse post faz parte de uma série, confira os outros já feitos:
<ol>
	<li><a href="http://templecoding.com/declarao-de-variveis-com-javascript/">Declaração de variáveis com JavaScript</a></li>
	<li><a href="http://templecoding.com/elevao-em-javascript/">Elevação em JavaScript</a></li>
	<li><a href="http://templecoding.com/converter-numeros-em-javascript-e-uma-droga/">Converter números em JavaScript é uma droga</a></li>
	<li><a href="http://templecoding.com/javascript-ponto-virgula/">JavaScript e o problema com o ponto e vírgula</a></li>
	<li><a href="http://templecoding.com/escopo-no-javascript/">Escopo no JavaScript: esse this não é daqui</a></li>
	<li><a href="http://templecoding.com/funcoes-de-callback-no-javascript/">Funções de callback</a></li>
	<li><a href="http://templecoding.com/tratamento-de-excecoes-em-javascript/">Tratamento de exceções em JavaScript</a></li>
	<li>Construtores de Arrays</li>
	<li><a href="http://templecoding.com/loops-for-in-no-javascript/">Loops for in</a></li>
</ol>
</div>
<h2>Construtores de Arrays do JavaScript</h2>
No JavaScript podemos construir um Array da seguinte forma:
<pre class="brush: js;">var a = new Array("texto");
console.log(a.length); // 1
console.log(a[0]); // texto</pre>
Quando passamos uma string ou outro tipo de objeto para o construtor de uma Array, o JavaScript cria uma array com um item e atribui o valor passado para esse item.

Agora vamos dar uma olhada em outro exemplo:
<pre class="brush: js;">var a = new Array(3);
console.log(a.length) // 3
console.log(a[0]); // undefined</pre>
Quando passamos um número inteiro para o construtor ele cria um array com o número de elementos informados e, todos os elementos criados não tem um valor atribuído a eles, por isso o undefined.

Um terceiro exemplo:
<pre class="brush: js;">var a = new Array(3.2); // RangeError: invalid array length</pre>
Quando passamos um número decimal para o construtor, ele obviamente não cria um array decimal, mas também não cria um array com um só elemento e com o valor passado, o JavaScript retorna um erro para esse comando.
<h2>Uma prática um pouco melhor</h2>
A melhor forma de criar arrays no JavaScript é usando o construtor implícito dele, por exemplo:
<pre class="brush: js;">var a = ["texto"]; // Cria um elemento e atribui o valor a ele

var b = [1, 2, 3]; // Cria um array com 3 elementos e com os valores 1, 2 e 3</pre>
Um problema simples de resolver esse, não é?
