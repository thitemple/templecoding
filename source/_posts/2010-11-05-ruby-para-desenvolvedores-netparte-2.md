---
layout: post
title: Ruby para desenvolvedores .NET–parte 2
categories:
- Desenvolvimento
tags:
- ironruby
- ruby
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  dsq_thread_id: '169003555'
  _wp_old_slug: ''
  _revision-control: a:1:{i:0;s:8:"defaults";}
  _headspace_page_title: ''
  _headspace_description: ''
  _yoast_wpseo_linkdex: '0'
  _yoast_wpseo_focuskw: ''
  _yoast_wpseo_title: ''
  _yoast_wpseo_metadesc: ''
  _yoast_wpseo_meta-robots-noindex: '0'
  _yoast_wpseo_meta-robots-nofollow: '0'
  _yoast_wpseo_meta-robots-adv: none
  _yoast_wpseo_sitemap-include: ! '-'
  _yoast_wpseo_sitemap-prio: ! '-'
  _yoast_wpseo_canonical: ''
  _yoast_wpseo_redirect: ''
  _yoast_wpseo_opengraph-description: ''
  _yoast_wpseo_google-plus-description: ''
---
<div style="padding-left: 10px; border: 1px dashed;">

Esta é uma série de artigos sobre Ruby para desenvolvedores .NET, confira os outros em:

<a href="http://templecoding.com/2010/11/03/ruby-para-desenvolvedores-netparte-1/">Ruby para desenvolvedores .NET – parte 1</a>

</div>
Ruby, é uma linguagem orientada a objetos. É  possível reconhecer, apenas com pequenas diferenças na sintaxe, algumas similaridades e diferenças de funcionamento, se comparado com o C#.

Podemos ver no código abaixo, a definição de uma classe(Usuario), a definição de um construtor (initialize), a definição de um método (autorizar_acesso) e a criação de 2 atributos, nome e email.
<pre class="brush: ruby;">class Usuario
  attr_accessor :nome, :email

  def initialize(nome)
    @nome = nome
  end

  def autorizar_acesso
    false
  end
end</pre>
O que podemos ver de diferente aqui, é que quando chamamos o método attr_accessor para criar as propriedades nome e email, internamente o Ruby cria duas variáveis de instância chamadas @nome e @email e essas variáveis podem, obviamente, ser acessadas por qualquer método daquele objeto.’

A propósito, instanciar a class Usuario é tão simples quanto:
<pre class="brush: ruby;">usuario = Usuario.new</pre>
<strong>Retorno de valores</strong>

Uma outra coisa importante de lembrar no Ruby, é que a última instrução de um método sempre é retornada. Portanto dentro do método autorizar_acesso, estamos retornando o valor false. Colocar false como última instrução (e nesse caso, a única) é o mesmo que dizer return false.

Por exemplo, os dois métodos abaixo fazem a mesma coisa:
<pre class="brush: ruby;">def soma(x, y)
  # faz um monte de coisa desnecessaria aqui
  x + y
end

def soma(x, y)
  # faz um monte de coisa desnecessaria aqui
  resultado_soma = x + y
  return resultado_soma
end</pre>
Nos dois casos, x é somado a y e o resultado da soma é retornado.

<strong>Herança</strong>

Assim como em C#, Ruby também tem <a href="http://pt.wikipedia.org/wiki/Heran%C3%A7a_(inform%C3%A1tica)">herança</a> como podemos ver no exemplo abaixo:
<pre class="brush: ruby;">class Usuario
  attr_accessor :nome, :email

  def initialize(nome)
    @nome = nome
  end

  def autorizar_acesso
    false
  end
end

class UsuarioEspecial &lt; Usuario
  def autorizar_acesso
    true
  end
end</pre>
No código acima a classe UsuarioEspecial herda da classe Usuario e sobrescreve o método autorizar_acesso. Assim já podemos ver que é possível usar também o polimorfismo.

<strong>Métodos Estáticos</strong>

Ruby não tem métodos estáticos realmente, mas ele tem, o que é conhecido como <span style="text-decoration: underline;">métodos de classe</span> e funcionam de forma similar aos métodos estáticos do C#. Para  usar métodos de classe basta que na declaração do método seja colocada a palavra chave <span style="text-decoration: underline;">self</span>, como podemos ver:
<pre class="brush: ruby;">class UsuarioEspecial &lt; Usuario

  def self.salvar
    enviar_email("texto do email")
    #salvar os dados do usuario
  end

  def autorizar_acesso
    true
  end

private

  def enviar_email(txt)
    # envia email
  end
end</pre>
O método salvar pode ser chamado diretamente na classe UsuarioEspecial, não é necessário criar uma instância.
<pre class="brush: ruby;">UsuarioEspecial.salvar</pre>
Ainda no código acima podemos ver que para declarar um método privado pasta adicionar a palavra chave <span style="text-decoration: underline;">private</span> e qualquer método declarado abaixo dela será considerado privado.

<strong>Interfaces</strong>

Interfaces são um artifício  das linguagens estáticas para dizer ao compilador que uma classe tem um ou mais métodos.

No Ruby não existe um compilador e portanto não existe essa checagem prévia. Também existe a questão de que podemos alterar uma classe Ruby mesmo depois que ela já foi declarada, mas vamos falar disso depois. Portanto não existe a necessidade de interfaces.

Digamos que exista a seguinte situação feita em C#:
<pre class="brush: csharp;">interface IControlePagamento
{
    void Pagar();
}

class Pagamento : IControlePagamento
{
    public void Pagar()
    {
        // logica de pagamento
    }
}</pre>
O que esse código bem simples faz, é dizer ao compilador que a classe Pagamento implementa os métodos da Interface IControlePagamento. Em Ruby a única coisa que temos que fazer é efetivamente implementar o método Pagar()
<pre class="brush: ruby;">class ControlePagamento
  def pagar
  end
end</pre>
Isso tem vantagens e desvantagens.

A vantagem é que o código em Ruby é muito mais sucinto e objetivo, não é preciso escrever código para satisfazer o compilador. Existe apenas código para resolver o problema do negócio.

A desvantagem é que você não tem o aviso imediato do compilador caso o método não seja implementado.

Ainda assim, se você é adepto de técnicas como <a href="http://pt.wikipedia.org/wiki/Test_Driven_Development">TDD</a>, esse é um problema fácil de resolver, já que seus testes ficariam responsáveis por isso.

Outro recurso disponível no Ruby é que, toda classe automaticamente herda da classe Class, na classe Class existe um método chamado <span style="text-decoration: underline;">respond_to?</span>. Esse método verifica se um objeto responde/contém a um determinado método.

<a href="http://templecoding.com/wp-content/uploads/2010/11/respond_to_false.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border-width: 0px;" title="respond_to_false" src="http://templecoding.com/wp-content/uploads/2010/11/respond_to_false_thumb.png" alt="respond_to_false" width="419" height="147" border="0" /></a>

No exemplo acima a classe ControlePagamento foi definida sem o método pagar, veja que quando foi chamado o método respond_to? o retorno foi false.

Já abaixo o método foi definido e a resposta foi true.

<a href="http://templecoding.com/wp-content/uploads/2010/11/respond_to_true.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border-width: 0px;" title="respond_to_true" src="http://templecoding.com/wp-content/uploads/2010/11/respond_to_true_thumb.png" alt="respond_to_true" width="379" height="181" border="0" /></a>

Enfim, existem opções para controlar isso e há gente que faça, mas vale dizer que pelo que tenho visto, o mais comum é simplesmente seguir uma convenção.

<strong>Conclusão</strong>

Nesse artigo já vimos mais algumas diferenças mais marcantes do Ruby, no próximo vou falar um pouco do Object Model do Ruby.

Duvidas, criticas ou sugestões? Deixe um comentário.<!--:-->
