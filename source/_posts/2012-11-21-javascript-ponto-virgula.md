---
layout: post
title: JavaScript e o problema com o ponto e vírgula
categories:
- Desenvolvimento
tags:
- javascript
status: publish
type: post
published: true
meta:
  _yoast_wpseo_linkdex: '61'
  _yoast_wpseo_metadesc: O JavaScript por padrão adiciona um ponto e vírgula ao final
    de cada linha. Entenda porque isso pode não ser bom e porque é melhor ser explícito.
  _thumbnail_id: '929'
  _yoast_wpseo_focuskw: javascript ponto virgula
  _yoast_wpseo_title: JavaScript e o problema com o ponto e vírgula - Temple Coding
  _edit_last: '2'
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
  _yoast_wpseo_metakeywords: ''
  qtrans_meta:title: <!--:pt-->JavaScript e o problema com o ponto e vírgula | Temple
    Coding<!--:-->
  qtrans_meta:keywords: <!--:pt-->javascript, ponto vírgula<!--:-->
  qtrans_meta:description: <!--:pt-->O JavaScript por padrão adiciona um ponto e vírgula
    ao final de cada linha. Entenda porque isso pode não ser bom e porque é melhor
    ser explícito.<!--:-->
  _qts_slug_pt: javascript-e-o-problema-com-o-ponto-e-virgula
  _qts_slug_en: javascript-e-o-problema-com-o-ponto-e-virgula
  dsq_thread_id: '1122794672'
---
<!--:pt-->Dessa vez eu quero falar sobre uma questão bem simples, o problema do ponto e vírgula e das chaves.
<div style="margin: 5px 0px; border: #f48432 1px dashed; padding: 5px;"><strong>Sofrendo menos com JavaScript</strong>
Esse post faz parte de uma série, confira os outros já feitos:
<ol>
	<li><a href="http://templecoding.com/declarao-de-variveis-com-javascript/">Declaração de variáveis com JavaScript</a></li>
	<li><a href="http://templecoding.com/elevao-em-javascript/">Elevação em JavaScript</a></li>
	<li><a href="http://templecoding.com/converter-numeros-em-javascript-e-uma-droga/">Converter números em JavaScript é uma droga</a></li>
	<li>JavaScript e o problema com o ponto e vírgula</li>
	<li><a href="http://templecoding.com/escopo-no-javascript/">Escopo no JavaScript: esse this não é daqui</a></li>
	<li><a href="http://templecoding.com/funcoes-de-callback-no-javascript/">Funções de callback</a></li>
	<li><a href="http://templecoding.com/tratamento-excecoes-javascript/">Tratamento de exceções em JavaScript</a></li>
	<li><a href="http://templecoding.com/construtores-de-arrays-do-javascript/">Construtores de Arrays</a></li>
	<li><a href="http://templecoding.com/loops-for-in-no-javascript/">Loops for in</a></li>
</ol>
</div>
O JavaScript tenta nos ajudar cada vez que esquecemos ou simplesmente deixamos de colocar um ponto e vírgula no final de uma linha.

Assim, o código abaixo funciona perfeitamente:
<pre class="brush: js;">var nome  = "Thiago"
nome = "Thiago Temple"
console.log(nome)</pre>
Dito isso, um outro ponto relevante para esse caso é que o JavaScript também suporta duas formas diferentes de usar as chaves {}. Os dois exemplos abaixo funcionam da mesma maneira.
<pre class="brush: js;">if(true) {
}

if(true)
{
}</pre>
Agora, vamos dizer que eu tenha uma função que retorna um objeto, como as duas funções abaixo:
<pre class="brush: js;">function getNome1() {
  return {
    nome: "Thiago"
  };
}
console.log(getNome1().nome); // Thiago

function getNome2() {
    return
    {
        nome: "Thiago"
    };
}
console.log(getNome2().nome); // syntax error</pre>
No exemplo acima a função getNome1 funciona normalmente, mas no segundo caso, a função getNome2 retorna um erro. Isso acontece porque o JavaScript entende que, depois da palavra chave return, está faltando um ponto e vírgula e automaticamente insere um. Obviamente, tudo que está depois desse ponto e vírgula passa a não ter nenhum sentido e vira um erro de sintaxe.
<h2>Solução</h2>
É bem simples nesse caso, use sempre a abertura de chave na mesma linha da instrução, como na função getNome1 e o problema estará resolvido.
<pre class="brush: js;">function getNome1() {
  return {
    nome: "Thiago"
  };
}
console.log(getNome1().nome); // Thiago</pre>
E também sempre coloque o ponto e vírgula ao final das sentenças, nesse caso é melhor ser explícito do que confiar no JavaScript.<!--:-->
