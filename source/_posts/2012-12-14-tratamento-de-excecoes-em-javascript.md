---
layout: post
title: Tratamento de exceções em JavaScript
categories:
- Desenvolvimento
tags:
- javascript
status: publish
type: post
published: true
meta:
  qtrans_meta:keywords: <!--:pt-->javasript, tratamento excecoes<!--:-->
  _edit_last: '2'
  _qts_slug_en: tratamento-de-excecoes-em-javascript
  _yoast_wpseo_meta-robots-noindex: '0'
  _yoast_wpseo_metakeywords: ''
  _yoast_wpseo_canonical: ''
  _yoast_wpseo_redirect: ''
  _yoast_wpseo_meta-robots-adv: none
  _yoast_wpseo_meta-robots-nofollow: '0'
  _yoast_wpseo_title: Tratamento de exceções em JavaScript | Temple Coding
  _yoast_wpseo_metadesc: É possivel realizar tratamento de exceções em JavaScript
    usando os blocos try/catch e o comando throw.
  _yoast_wpseo_focuskw: Tratamento de exceções em JavaScript
  _qts_slug_pt: tratamento-excecoes-javascript
  _wpas_done_all: '1'
  qtrans_meta:title: <!--:pt-->Tratamento de exceções em JavaScript<!--:-->
  _yoast_wpseo_linkdex: '72'
  qtrans_meta:description: <!--:pt-->É possivel realizar tratamento de exceções em
    JavaScript usando os blocos try/catch e o comando throw.<!--:-->
  _yoast_wpseo_sitemap-prio: ! '-'
  _yoast_wpseo_sitemap-include: ! '-'
  _yoast_wpseo_google-plus-description: ''
  _yoast_wpseo_opengraph-description: ''
  _clicky_goal: a:2:{s:2:"id";s:0:"";s:5:"value";s:0:"";}
  _thumbnail_id: '1133'
  _wpas_skip_2089817: '1'
  _wpas_skip_2089821: '1'
  _wpas_skip_2089827: '1'
  dsq_thread_id: '1142678065'
---
<!--:pt-->Sim, existe tratamento de exceções em JavaScript. Não é uma funcionalidade muito conhecida e, por isso talvez, não muito utilizada. Mas existem os blocos try/catch dentro do JavaScript e o comando throw. E funciona de uma forma bem similiar ao C#.
<pre class="brush: js; ruler: true; smart-tabs: false;">var comErro = function() {
    throw {
        name: 'CustomError',
        message: 'Um erro muito grave para ser tratado'
    };
};

try {
    comErro();
}
catch(e) {
    console.log(e.name + ' - ' + e.message); //CustomError - Um erro muito grave para ser tratado
}</pre>
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
	<li>Tratamento de exceções em JavaScript</li>
	<li><a href="http://templecoding.com/construtores-de-arrays-do-javascript/">Construtores de Arrays</a></li>
	<li><a href="http://templecoding.com/loops-for-in-no-javascript/">Loops for in</a></li>
</ol>
</div>
Apesar do JavaScript não ser tipado, existe uma convenção que diz que todo objeto de erro deve conter ao menos duas propriedades: name e message. Acho que os nomes das propriedades são auto-explicatórios.

Obviamente, nada nos impede de adicionar outras propriedades com mais detalhes sobre o erro.
<h2>Tipos de erro do JavaScript</h2>
Por padrão o JavaScript tem internamente seis tipos diferentes de erro que ele pode gerar.
<table width="400" border="0" cellspacing="0" cellpadding="2">
<tbody>
<tr>
<td valign="top" width="100">EvalError</td>
<td valign="top" width="300">Usado quando a função eval() é usada de uma maneira incorreta</td>
</tr>
<tr>
<td valign="top" width="100">RangeError</td>
<td valign="top" width="300">Usado quando uma variável numérica excede uma faixa de valores permitida</td>
</tr>
<tr>
<td valign="top" width="100">ReferenceError</td>
<td valign="top" width="300">Usado quando é feita uma referência invalida à um objeto</td>
</tr>
<tr>
<td valign="top" width="100">SyntaxError</td>
<td valign="top" width="300">Usado quando existe um erro de sintaxe</td>
</tr>
<tr>
<td valign="top" width="100">TypeError</td>
<td valign="top" width="300">Usado quando o tipo da variável não é o mesmo que o esperado</td>
</tr>
<tr>
<td valign="top" width="100">URIError</td>
<td valign="top" width="300">Usado quando as funções encodeURI() e decodeURI() são usadas de uma maneira incorreta.</td>
</tr>
</tbody>
</table>
Isso serve apenas como referência, como mostrei no exemplo anterior, podemos criar os nossos próprios tipos de erro sem problemas.
<h2>Construtores de objetos de erro</h2>
Por fim, o JavaScript também tem alguns construtores para os objetos que podemos usar para retornar um erro, por exemplo, eu poderia reescrever a exemplo acima da seguinte forma:
<pre class="brush: js; ruler: true; smart-tabs: false;">var comErro = function() {
    var error = new Error('Um erro muito grave para ser tratado');
    error.name = 'CustomError';
    throw error;
};

try {
    comErro();
}
catch(e) {
    console.log(e.name + ' - ' + e.message); //CustomError - Um erro muito grave para ser tratado
}</pre>
Esse código gera exatamente o mesmo resultado que antes, o único detalhe que quero ressaltar é que se eu não tivesse feito:
<pre class="brush: js; ruler: true; smart-tabs: false;">error.name = 'CustomError';</pre>
o nome do erro seria Error.

Como o objeto Error, temos também um construtor para cada tipo de erro do JavaScript: EvalError, TypeError, ReferenceError, etc.<!--:-->
