---
layout: post
title: ! 'Escopo no JavaScript: esse this não é daqui'
categories:
- Desenvolvimento
tags:
- javascript
status: publish
type: post
published: true
meta:
  _yoast_wpseo_linkdex: '75'
  _edit_last: '2'
  _yoast_wpseo_focuskw: escopo no javascript
  _yoast_wpseo_title: ! 'Escopo no JavaScript: esse this não é daquiScope in JavaScript:
    that this is not from here - Temple Coding'
  _yoast_wpseo_metadesc: Entendendo o escopo no JavaScript. O escopo de variáveis
    é a função, mas o escopo no JavaScript de this pode variar de acordo com o contexto
    desejado.
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
  _thumbnail_id: '1011'
  qtrans_meta:title: ! '<!--:pt-->Escopo no JavaScript: esse this não é daqui | Temple
    Coding<!--:--><!--:en-->Scope in JavaScript: that this is not from here | Temple
    Coding<!--:-->'
  qtrans_meta:keywords: <!--:pt-->escopo javascript<!--:-->
  qtrans_meta:description: <!--:pt-->Entendendo o escopo no JavaScript. O escopo de
    variáveis é a função, mas o escopo no JavaScript de this pode variar de acordo
    com o contexto desejado.<!--:-->
  _qts_slug_pt: escopo-no-javascript
  _qts_slug_en: scope-in-javascript
  _wpas_done_all: '1'
  _clicky_goal: a:2:{s:2:"id";s:0:"";s:5:"value";s:0:"";}
  _wpas_skip_2089817: '1'
  _wpas_skip_2089821: '1'
  _wpas_skip_2089827: '1'
  dsq_thread_id: '1142697780'
---
<!--:pt-->Existem duas questões importantes com relação ao escopo no JavaScript.  Declaração de variáveis e o valor de this.
<div style="margin: 5px 0px; border: #f48432 1px dashed; padding: 5px;"><strong>Sofrendo menos com JavaScript</strong>
Esse post faz parte de uma série, confira os outros já feitos:
<ol>
	<li><a href="http://templecoding.com/declarao-de-variveis-com-javascript/">Declaração de variáveis com JavaScript</a></li>
	<li><a href="http://templecoding.com/elevao-em-javascript/">Elevação em JavaScript</a></li>
	<li><a href="http://templecoding.com/converter-numeros-em-javascript-e-uma-droga/">Converter números em JavaScript é uma droga</a></li>
	<li><a href="http://templecoding.com/javascript-ponto-virgula/">JavaScript e o problema com o ponto e vírgula</a></li>
	<li>Escopo no JavaScript: esse this não é daqui</li>
	<li><a href="http://templecoding.com/funcoes-de-callback-no-javascript/">Funções de callback</a></li>
	<li><a href="http://templecoding.com/tratamento-excecoes-javascript/">Tratamento de exceções em JavaScript</a></li>
	<li><a href="http://templecoding.com/construtores-de-arrays-do-javascript/">Construtores de Arrays</a></li>
	<li><a href="http://templecoding.com/loops-for-in-no-javascript/">Loops for in</a></li>
</ol>
</div>
A primeira parte é fácil, eu já falei um pouco disso quando falei de <a href="http://templecoding.com/declarao-de-variveis-com-javascript/">declaração de variáveis</a>. Toda variável declarada com a palavra chave var tem o escopo da função em que foi declarada. Portanto, na função abaixo:
<pre class="brush: js; ruler: true; smart-tabs: false;">var func1 = function() { 
    var i = 100; 
    for(var i = 0; i &lt; 10; i++) { 
        // qualquer coisa aqui 
    } 
    console.log(i); // 10 
}</pre>
A variável i vai ser 10 no fim do método, porque dentro de um método, não importa onde a variável é declarada, ela sempre faz parte do escopo da função. E claro, não pode ser acessada fora da função.

Agora o this é um pouco mais complicado. O this sempre faz referência ao objeto que a função faz parte. Por exemplo:
<pre class="brush: js; ruler: true; smart-tabs: false;">var pessoa = { 
    'nome': 'Thiago', 
    'getNome': function(){ 
        console.log('Olá, ' + this.nome); 
    } 
}; 
pessoa.getNome(); // Olá, Thiago</pre>
No exemplo acima, eu declaro um objeto e atribuo à variável pessoa. E getNome é definido com uma função, portanto essa função é um método desse objeto. Dessa forma o contexto de this dentro desse método é o objeto em que ele foi definido e chamada de getNome funciona como o esperado.

Já no exemplo abaixo:
<pre class="brush: js; ruler: true; smart-tabs: false;">var pessoa = {  
    'nome': 'Thiago',  
    'getNome': function(){  
        var fx = function() { 
            console.log('Olá, ' + this.nome); 
        }; 
        fx(); 
    }  
};  
pessoa.getNome(); // Olá, undefined</pre>
Dentro do método, fx é apenas uma variável que recebe uma função, não é um método que faz parte de um objeto, por isso, this nesse caso faz referência ao escopo global, que no caso do browser, quer dizer o objeto window.
<h2>Trocando o escopo de uma função</h2>
A última coisa que eu quero dizer sobre o escopo e this é que no JavaScript nós podemos alterar a referência this. Veja o exemplo abaixo:
<pre class="brush: js; ruler: true; smart-tabs: false;">var dizOla = function() { 
    console.log('Ola, ' + this.nome); 
}; 

var pessoa = { 
    'nome': 'Thiago Temple' 
} 

dizOla.call(pessoa);</pre>
A função dizOla nao faz parte de nenhum objeto, portanto this dentro daquela função faz referencia ao objeto window. Mas, quando executo a função usando o método call, eu passo o objeto pessoa como parâmetro. Nesse momento estou dizendo para a função dizOla usar o objeto pessoa como contexto.

Alterar o escopo no JavaScript é uma característica muito importante e muito utilizada, porque é uma forma bem simples e eficaz de fazer reuso de funções.

É isso aí por hoje. Até a próxima!<!--:-->
