---
layout: post
title: Só é uma bagunça se você deixar. Responsabilidade única com JavaScript
categories:
- Desenvolvimento
tags:
- javascript
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _yoast_wpseo_linkdex: '72'
  _yoast_wpseo_focuskw: Responsabilidade única com JavaScript
  _yoast_wpseo_title: ''
  _yoast_wpseo_metadesc: Uma demonstração da aplicação do princípio da responsabilidade
    única com JavaScript.
  _yoast_wpseo_metakeywords: ''
  _yoast_wpseo_meta-robots-noindex: '0'
  _yoast_wpseo_meta-robots-nofollow: '0'
  _yoast_wpseo_meta-robots-adv: none
  _yoast_wpseo_sitemap-include: ! '-'
  _yoast_wpseo_sitemap-prio: ! '-'
  _yoast_wpseo_sitemap-html-include: ! '-'
  _yoast_wpseo_canonical: ''
  _yoast_wpseo_redirect: ''
  _yoast_wpseo_opengraph-description: ''
  _yoast_wpseo_google-plus-description: ''
  _wpas_skip_2174375: '1'
  _wpas_done_all: '1'
  _clicky_goal: a:2:{s:2:"id";s:0:"";s:5:"value";s:0:"";}
  dsq_thread_id: '1547599382'
---
Muita gente que reclama de JavaScript mas não se dá ao mínimo trabalho de entender a linguagem e, boa parte das vezes, não trata o código desenvolvido como um código de verdade, afinal de contas "é só JavaScript mesmo".

Mas esse é só uma das razões de boa parte dos problemas enfrentados quando a gente tem que dar manutenção em um código já existente. O bom e velho spaguetti de código.

<a href="http://templecoding.com/wp-content/uploads/2013/07/spagett.jpg"><img class="aligncenter size-full wp-image-1399" alt="Código spaghetti" src="http://templecoding.com/wp-content/uploads/2013/07/spagett.jpg" width="425" height="282" /></a>

Tenho que admitir que eu já contribui para bastante código spaguetti (como do exemplo que vou mostrar aqui). Exatamente porque não tratei esse código como um programa sério e não apliquei princípios básicos como o da <a href="http://robsoncastilho.com.br/2013/02/06/principios-solid-principio-da-responsabilidade-unica-srp/" target="_blank">responsabilidade única</a>.

Vamos imaginar uma cesta de compras como a da imagem abaixo, que tem um link para atualizar a quantidade de itens do carrinho. O código que vou mostrar, captura o click no link usando o jQuery e faz uma validação de que o valor informado no campo é numérico. A regra de negócio diz ainda que tem que ser uma quantidade entre 0 e 10. Não posso vender mais do que 10 unidades de um mesmo item.

<a href="http://templecoding.com/wp-content/uploads/2013/07/shopping_basket.png"><img class="aligncenter size-full wp-image-1392" alt="shopping_basket" src="http://templecoding.com/wp-content/uploads/2013/07/shopping_basket.png" width="413" height="69" /></a>

Um código que faria isso é esse:
<pre class="brush: js; ruler: true; smart-tabs: false;">  $(document).ready(function() {
    $(".update_cart").click(function(e) {
      e.preventDefault();
      var itemsCount = $(this).closest("td").prev()
        .children(".quantity").val();
      if(isNaN(itemsCount) || parseInt(itemsCount, 10) &lt; 0 ||          parseInt(itemsCount, 10) &gt; 10) {
          showError("Nao é numero valido");
          return;
      }
      $.ajax({
        url: '/updatecart',
        data: {
          // lógica para formatar os dados a serem enviados para o servidor
        },
        success: function(data) {
          // update table with cart data
        },
        error: function(xhr, status, error) {
          // handle error
        }
      });
    });
  });</pre>
Embora esse seja um código simples - e que está pouco mais curto do que realmente seria já que eu omiti alguns detalhes da implementação - ele viola um dos princípios de desenvolvimento mais importantes na minha opinião, que é o princípio da responsabilidade única. Mas, é um código que nós vemos com bastante facilidade em muitos aplicações na web.

Mesmo que a gente não tenha classes em JavaScript, as funções são usadas para fazer separações de responsabilidades. Veja, no exemplo acima, a função responsável por responder ao evento de click de atualização faz a validação, faz a chamada ajax e também atualiza a página após o resultado, seja com sucesso ou não.

Fazendo uma breve analogia, a chamada ajax como essa acima, pode ser comparada a uma chamada de um web service feito por um código C# ou Java. Ou seja, é um serviço externo ao código que está sendo executado, ou melhor ainda, é uma dependência externa que está gerando acoplamento no código. Taí a primeira separação que a gente pode fazer.

Outra ponto, o script acima lê informações da página (DOM) e depois atualiza informações nessa mesma página de acordo com o retorno do processamento. Então, em uma outra analogia, pode-se dizer que o DOM nesse caso, é uma forma de banco de dados do programa, portanto, é outra dependência externa que a gente pode separar no código.

Então vamos ver o que seria um código melhor estruturado para esse caso.
<pre class="brush: js; ruler: true; smart-tabs: false;">$(document).ready(function() {

  function BasketUpdater (basketView, basketService) {
    this.init = function() {
      basketView.handleUpdate(this.update);
    }

    this.isValid = function(item) {
      var quantity = item.quantity;
      return !isNaN(itemsCount) &amp;&amp; 
        parseInt(itemsCount, 10) &gt;= 0 &amp;&amp; parseInt(itemsCount, 10) &lt;= 10
    }

    this.update = function(item) {
      if(this.isValid(item)) {
        basketService.update(item, 
          basketView.refreshViewSuccess, basketView.refreshViewError)
      }
    }
  }

  function BasketView() {
    this.handleUpdate = function(callback) {
      $(".update_cart").click(function(e) {
        e.preventDefault();
        // logica para formatar um objeto com os dados do item no formulario
        callback(item);
      }
    }

    this.refreshViewSuccess = function() {
      // 
    }

    this.refreshViewError = function() {
      // 
    }
  }

  function BasketService(url) {
    this.update = function(item, callbackSuccess, callbackError) {
      $.ajax({
        url: url,
        data: item,
        success: function(data) {
          algum codigo para formatar os dados de retorno
          callbackSuccess(data);
        },
        error: function(xhr, status, error) {
		  // algum codigo para tratar a mensagem de erro
          callbackError(error);
        }
      });
    }
  }

  var basketService = new BasketService('http://localhost/updateCart');
  var basketView = new BasketView();
  var basketUpdater = new BasketUpdater(basketView, basketService);

  basketUpdater.init();

});</pre>
Mesmo que esse código seja um tanto quanto mais extenso que o anterior, fica bem clara a divisão de responsabilidades. Temos uma função para atualizar a view <em>BasketView</em>, uma para fazer a chamada ajax <em>BasketService</em> e uma com a lógica de negócio necessária para a página <em>BasketUpdater</em>.

Os benefícios de um código como esse (comparado com o anterior) são muitos, e bem semelhantes aos que a gente vê em códigos "servidor" bem estruturados.
<ol>
	<li>As dependências necessárias para que um objeto funcione (sim, uma função em JavaScript é um objeto) estão claras e definidas já no construtor.</li>
	<li>Uma função não depende explicitamente de outra, ela apenas espera um objeto que implemente determinados métodos</li>
	<li>É possível testar as funções isoladamente com um framework de testes</li>
	<li>É possível utilizar injeção de dependência e no caso dos testes, usar mocks</li>
	<li>É possível alterar uma função sem interferir na outra (desde que os nomes dos métodos se mantenham os mesmos)</li>
</ol>
Para concluir, temos um código muito mais organizado, de fácil manutenção e com responsabilidades claras. Para chegar nesse resultado, fiz uso apenas de técnicas já conhecidas de outras linguagens e sem inventar muita coisa. Então da próxima vez que você for dizer que é muito difícil dar manutenção em código JavaScript, pense se você está contribuindo para um código sustentável ou só preparando o almoço do próximo domingo.

<a href="http://templecoding.com/wp-content/uploads/2013/07/Spaghetti.gif"><img class="aligncenter size-full wp-image-1401" alt="Spaghetti" src="http://templecoding.com/wp-content/uploads/2013/07/Spaghetti.gif" width="245" height="184" /></a>
