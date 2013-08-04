---
layout: post
title: Funções de callback no JavaScript
categories:
- Desenvolvimento
tags:
- javascript
status: publish
type: post
published: true
meta:
  _yoast_wpseo_metakeywords: ''
  _yoast_wpseo_meta-robots-noindex: '0'
  _yoast_wpseo_title: Funções de callback no JavaScript | Temple Coding
  _yoast_wpseo_metadesc: ''
  _edit_last: '2'
  _qts_slug_pt: funcoes-de-callback-no-javascript
  _qts_slug_en: callback-functions-in-javascript
  _yoast_wpseo_focuskw: Funções de callback no JavaScript
  qtrans_meta:title: ''
  qtrans_meta:keywords: ''
  qtrans_meta:description: ''
  _yoast_wpseo_linkdex: '59'
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
  dsq_thread_id: '1133256907'
---
<!--:pt-->
<h2>O que são funções de callback?</h2>
Uma função no JavaScript é um objeto, isso quer dizer que a gente pode passar uma função como parâmetro para outra função. Quando passamos uma função como parâmetro para outra, a função passada como parâmetro é uma função de callback.
<div style="margin: 5px 0px; border: #f48432 1px dashed; padding: 5px;">

<strong>Sofrendo menos com JavaScript</strong>
Esse post faz parte de uma série, confira os outros já feitos:
<ol>
	<li><a href="http://templecoding.com/declarao-de-variveis-com-javascript/">Declaração de variáveis com JavaScript</a></li>
	<li><a href="http://templecoding.com/elevao-em-javascript/">Elevação em JavaScript</a></li>
	<li><a href="http://templecoding.com/converter-numeros-em-javascript-e-uma-droga/">Converter números em JavaScript é uma droga</a></li>
	<li><a href="http://templecoding.com/javascript-ponto-virgula/">JavaScript e o problema com o ponto e vírgula</a></li>
	<li><a href="http://templecoding.com/escopo-no-javascript/">Escopo no JavaScript: esse this não é daqui</a></li>
	<li>Funções de callback</li>
	<li><a href="http://templecoding.com/tratamento-excecoes-javascript/">Tratamento de exceções em JavaScript</a></li>
	<li><a href="http://templecoding.com/construtores-de-arrays-do-javascript/">Construtores de Arrays</a></li>
	<li><a href="http://templecoding.com/loops-for-in-no-javascript/">Loops for in</a></li>
</ol>
</div>
Por exemplo:
<pre class="brush: js;">var pessoa = function(nome, dizerOla) {
    dizerOla(nome);
};
var olaNoConsole = function(nome) {
    console.log(nome); // 
}
pessoa('Thiago', olaNoConsole);</pre>
No código acima, eu defini uma função chamada pessoa e outra olaNoConsole. A função pessoa recebe uma string com um nome e uma função de callback que ela vai usar para fazer uma saudação. Repare que, quando passo a função olaNoConsole como parâmetro, eu não utilizo parêntesis. Os parêntesis executam a função, então nesse momento que eu não quero executar a função, eu não os utilizo.
A função de callback pode ser também uma função anônima como no exemplo abaixo:
<pre class="brush: js;">var pessoa = function(nome, dizerOla) {
    dizerOla(nome);
};
pessoa('Thiago', function(nome) {
    alert('Ola ' + nome);
});</pre>
A mesma função pessoa agora recebendo uma função anônima que faz um alert como saudação.
<h2>Por quê?</h2>
Por que fazer uso de funções de callback? Acho que o exemplo acima deixa claro que a principal vantagem é o reuso sem causar acoplamento. No caso a função pessoa nunca sabe como é feita a saudação, tudo o que ela sabe é que tem que chamar a função de callback passando um parâmetro. Foi possível trocar a saudação de console para alert sem alterar a função pessoa.

Um exemplo um pouco mais útil é quando fazemos loops de elementos, por exemplo, vamos supor que eu queira consultar todos os meus campos texto na página, fazer alguma coisa com cada um deles e no fim retornar a lista de campos.
<pre class="brush: js;">var listaCampos = function() {
    var campos = document.getElementsByTagName("input");
    var totalItens = campos.length, i;
    for (i = 0; i &lt; totalItens; i++) { 
        // algo bem complexo com cada campo
    }
    return campos;
};</pre>
Depois disso eu decido que eu quero que cada campo tenha uma borda vermelha, porque eu quero destacar esses campos. Eu poderia pegar o retorno dessa função, fazer um for em cada item de novo e adicionar a borda.
<pre class="brush: js;">var campos = listaCampos();
var totalItens = campos.length, i;
for (i = 0; i &lt; totalItens; i++) { 
    campos[i].setAttribute("style", "border: 1px solid red");
}</pre>
O problema com essa solução é que eu fiz dois loops, um quando peguei os campos e um quando quis deixá-los com a borda vermelha. É possível ser mais simples (e mais rápido) do que isso, eu poderia alterar dentro do for original da função listaCampos e adicionar a linha para deixar os campos com a borda vermelha.
<pre class="brush: js;">var listaCampos = function() {
    var campos = document.getElementsByTagName("input");
    var totalItens = campos.length, i;
    for (i = 0; i &lt; totalItens; i++) { 
        // algo bem complexo com cada campo
        campos[i].setAttribute("style", "border: 1px solid red");
    }
    return campos;
};</pre>
O problema dessa segunda opção é que se eu quiser usar essa função listaCampos em outra parte no meu código, eu sempre vou ter os campos com a borda vermelha. E nem sempre eu quero vermelho, pode ser que em outro ponto eu queria azul! Aí entra o callback.
<pre class="brush: js;">var campoVermelho = function(campo) {
    campo.setAttribute("style", "border: 1px solid red");
};

var listaCampos = function(callback) {
    var campos = document.getElementsByTagName("input");
    var totalItens = campos.length, i;

    if (typeof callback !== 'function') {
        callback = false;
    }

    for (i = 0; i &lt; totalItens; i++) { 
        // algo bem complexo com cada campo
        if(callback) {
            callback(campos[i]);
        }
    }
    return campos;
};

listaCampos(campoVermelho);</pre>
No código acima, a primeira coisa que eu fiz foi criar a função de callback que espera um campo como parâmetro e faz com que a borda desse campo fique vermelha. Depois, na função listaCampos eu alterei a função para que ela receba um parâmetro, que vai ser uma função de callback.

Em seguida eu testo para ver se uma função foi realmente passada (já que no JavaScript os parâmetros não são obrigatórios). Dentro do loop, se a função de callback foi passada como parâmetro, eu a executo.

Nesse caso agora se eu quisesse que os campos tivessem a borda azul era só criar uma outra função que fizesse isso e passar como parâmetro para a função listaCampos. A função listaCampos continua sendo genérica e pode ser utilizada por outras. Melhor que isso, é uma função que faz uma coisa só e não está com a funcionalidade acoplada a ela.
<h2>Funções de callback e escopo</h2>
Dito isso, agora vem a parte um pouco mais complicada que é a parte do escopo com as funções de callback. Imagine que ao invés de passar uma função, eu tenho um objeto e eu passo um método desse objeto como callback para a função listaCampos.
<pre class="brush: js;">var listaCampos = function(callback) {
    var campos = documente.getElementsByTagName("input");
    var totalItens = campos.length, i;

    if (typeof callback !== 'function') {
        callback = false;
    }

    for (i = 0; i &lt; totalItens; i++) {
        // algo bem complexo com cada campo
        if(callback) {
            callback(campos[i]);
        }
    }

    return campos;
};

var customCampo = {
    'cor': 'red',
    'defineCor': function(campo){
        campo.setAttribute('style', 'border: 1px solid ' + this.cor);
    }
};

listaCampos(customCampo.defineCor);</pre>
No exemplo acima, o objeto customCampo tem um método defineCor que eu passo como callback para a função listaCampos. O problema é que o método defineCor usa this para fazer acesso ao valor da propriedade cor. Mas, a função listaCampos faz parte do objeto global e portanto nesse momento this vai fazer acesso ao objeto global também.

A solução para esse caso é sempre que passar uma função de callback, passar também o objeto que será o contexto da função de callback.
<pre class="brush: js;">listaCampos(customCampo.defineCor, customCampo);</pre>
E aí eu preciso modificar também a função listaCampos
<pre class="brush: js; highlight: [1, 11, 12];">var listaCampos = function(callback, ctx) {
    var campos = documente.getElementsByTagName("input");
    var totalItens = campos.length, i;

    if (typeof callback !== 'function') {
        callback = false;
    }

    for (i = 0; i &lt; totalItens; i++) {
        // algo bem complexo com cada campo
        if(typeof callback === 'function') {
            callback.call(ctx, campos[i]);
        }
    }

    return campos;
};</pre>
Isso resolve o problema, mas quando chamamos a função listaCampos temos que informar duas vezes o objeto customCampo:
<pre class="brush: js;">listaCampos(customCampo.defineCor, customCampo);</pre>
Existe uma forma um pouco mais elegante para isso, a gente pode chamar a função listaCampos dessa forma:
<pre class="brush: js;">listaCampos('defineCor', customCampo);</pre>
E aí alteramos a função listaCampos para:
<pre class="brush: js; highlight: [5, 6, 7];">var listaCampos = function(callback, ctx) {
    var campos = documente.getElementsByTagName("input");
    var totalItens = campos.length, i;

    if (typeof callback === 'string') {
        callback = ctx[callback];
    }

    if (typeof callback !== 'function') {
        callback = false;
    }

    for (i = 0; i &lt; totalItens; i++) {
        // algo bem complexo com cada campo

        if(typeof callback === 'function') {
            callback.call(ctx, campos[i]);
        }
    }

    return campos;
};

var customCampo = {
    'cor': 'red',
    'defineCor': function(campo){
        campo.setAttribute('style', 'border: 1px solid ' + this.cor);
    }
};</pre>
Um pouco mais elegante.

Por hoje é só, espero que tenha sido útil!<!--:-->
