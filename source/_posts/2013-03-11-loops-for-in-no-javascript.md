---
layout: post
title: Loops for in no JavaScript
categories:
- Desenvolvimento
tags:
- javascript
status: publish
type: post
published: true
meta:
  _yoast_wpseo_linkdex: '0'
  _edit_last: '2'
  _yoast_wpseo_focuskw: ''
  _yoast_wpseo_title: ''
  _yoast_wpseo_metadesc: O JavaScript tem suporte nativo a loops do tipo for in e,
    apesar de ter uma funcionalidade similar à outras linguagens, precisamos tomar
    alguns cuidados.
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
  _wpas_skip_2089817: '1'
  _clicky_goal: a:2:{s:2:"id";s:0:"";s:5:"value";s:0:"";}
  _wpas_skip_2089821: '1'
  _wpas_skip_2089827: '1'
  dsq_thread_id: '1132970939'
---
O JavaScript tem o suporte nativo a loops do tipo <em>for in</em> e, ele funciona de uma forma muito semelhante ao <em>foreach</em> do C# (por exemplo), mas temos que tomar alguns cuidados, por que como tudo no JavaScript, sempre temos uma pegadinha.
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
	<li><a href="http://templecoding.com/construtores-de-arrays-do-javascript/">Construtores de Arrays</a></li>
	<li>Loops for in</li>
</ol>
</div>
Primeiro um exemplo. Se eu quiser iterar por todas as propriedades é métodos de um objeto o código é exatamente o que imaginamos.
<pre class="brush: js; ruler: true; smart-tabs: false;">var propriedade;
var pessoa = {
    nome: 'Thiago',
    sobrenome: 'Temple',
    dizOla: function() {
        alert('Ola');
    }
};
for(propriedade in pessoa){
    document.writeln(propriedade);
}
//exibe: nome sobrenome dizOla</pre>
Esse código produz um retorno com: nome sobrenome dizOla

Se eu quiser exibir somente os atributos e não os métodos, eu preciso adicionar um filtro no loop, dessa forma:
<pre class="brush: js; ruler: true; smart-tabs: false;">for(propriedade in pessoa){
    if (typeof pessoa[propriedade] !== 'function') {
        document.writeln(propriedade);
    };
}
//exibe: nome sobrenome</pre>
Até aqui bem simples.

O primeiro ponto a considerar é que nunca podemos garantir a ordem do retorno de um loop for in. Geralmente isso não importaria muito, como no exemplo anterior, mas no caso de um array, que é também um objeto, isso pode ser importante. Nesse momento é melhor usar mesmo um loop for do que um loop for in.

A segundo ponto a considerar é que o JavaScript tem herança através da propriedade prototype, e que todo objeto herda de Object. Isso é importante porque o JavaScript é uma linguagem dinâmica e, portanto, se alterarmos Object isso vai refletir nos nossos objetos, não importa o momento que alteramos Object. Por exemplo:
<pre class="brush: js; ruler: true; smart-tabs: false;">var propriedade;
var pessoa = {
    nome: 'Thiago',
    sobrenome: 'Temple',
    dizOla: function() {
        alert('Ola');
    }
};

Object.prototype.naoRelacionado = 'eu nao devia estar aqui';

for(propriedade in pessoa){
    document.writeln(propriedade);
}

//exibe: nome sobrenome dizOla naoRelacionado</pre>
No código acima eu defini um objeto chamado pessoa e depois disso eu adicionei um atributo chamado naoRelacionado à Object. Mesmo assim esse atributo foi exibido dentro loop for in. Isso pode vir a ser um problema quando alteramos o prototype de Object em outra parte do código que não esta relacionado ao código que estamos fazendo.

No caso do loop for in, podemos resolver isso com o método <strong>hasOwnProperty</strong>.
<pre class="brush: js; ruler: true; smart-tabs: false;">for(propriedade in pessoa){
    if(pessoa.hasOwnProperty(propriedade)) {
        document.writeln(propriedade);
    }
}

//exibe: nome sobrenome dizOla</pre>
O método hasOwnProperty verifica se a propriedade foi definida no objeto em questão.

É isso ai.
