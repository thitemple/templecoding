---
layout: post
title: Mapeando suas classes com o AutoMapper
categories:
- Desenvolvimento
tags:
- automapper
status: publish
type: post
published: true
meta:
  dsq_thread_id: '156313818'
  _edit_last: '2'
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
Um framework que eu tenho usado com frequência ultimamente é o <a href="http://automapper.codeplex.com/" target="_blank">AutoMapper</a>, e pra quem não conhece ele, bom, ele faz.. mapeamento de objetos. Iupi!

Existem vários cenários onde isso é feito, no meu caso, principalmente, eu tenho usado para mapear estruturas de web services que eu consumo para objetos dentro da minha aplicação. Mas outro cenário comum é, quando usamos o padrão ViewModel, mapear objetos de negócio para a ViewModel.

A motivação para o uso do AutoMapper é manter as responsabilidades separadas, assim, temos um framework só para fazer isso e não precisamos colocar esse mapeamento no meio das nossas classes.

O uso do AutoMapper é bem simples, apenas um assembly que deve ser referenciado no projeto, depois disso devemos criar/definir os mapeamentos.

Vamos imaginar um cenário onde eu tenho um Web Service e quero mapear uma classe PedidoWs para a minha classe Pedido e para manter simples, digamos que as propriedades tem os mesmos nomes. Veja o diagrama abaixo:

<a href="http://templecoding.com/wp-content/uploads/2010/09/Classes.png"><img style="display: block; float: none; margin-left: auto; margin-right: auto; border-width: 0px;" title="Classes" src="http://vintem.com.br/wp-content/uploads/2010/09/Classes_thumb.png" border="0" alt="Classes" width="197" height="345" /></a>

Para fazer um mapeamento num cenário desses é bem simples, criamos o mapeamento assim:
<pre class="brush: csharp;">Mapper.CreateMap<pedidoWs, Pedido>();</pre>

E depois para usarmos o mapeamento basta executar:
<pre class="brush: csharp;">Mapper.Map<pedidoWs, Pedido>(meuObjetoDoTipoPedidoWs);</pre>
Esse é o padrão que vamos usar para mapear todos os objetos independentemente de como o mapeamento for criado.

Agora imagine que existam propriedades iguais, mas com nomes diferentes. Por exemplo, imagine que no PedidoWs existe uma propriedade chamada Cliente_ID ao invés de ClienteId, o mapeamento ficaria assim:
<pre class="brush: csharp;">Mapper.CreateMap<pedidoWs, Pedido>()
     .ForMember(c => c.ClienteId, opt => opt.MapFrom(x => x.Cliente_ID));</pre>
Veja, todos os outros atributos serão mapeados normalmente, apenas o ClienteId que foi declarado de forma explícita de onde vem.

Um outro caso importante é quando temos que fazer alguma conversão. Por exemplo, digamos que no nosso caso a data vem do web service em formato de string e temos que converter para DateTime. Faríamos assim:
<pre class="brush: csharp;">Mapper.CreateMap<pedidoWs, Pedido>()
     .ForMember(c => c.DataPedido, opt => opt.ResolveUsing<dateTimeResolver>()
         .FromMember(x => x.DataPedido);</pre>
Para usar o ResolveUsing precisamos definir a classe DateTimeResover e eu fiz dessa forma:
<pre class="brush: csharp;">public class DateTimeResolver : ValueResolver<string, DateTime>
{
    protected override DateTime ResolveCore(string source)
    {
        return DateTime.Parse(source);
    }
}</pre>
Para finalizar, vale dizer que essa configuração dos mapeamentos pode ser demorada, então não é uma boa idéia executar as configurações toda a vez. O que eu faço, já que uso isso para a web a maior parte do tempo, é no meu Application_Start dentro do global.asax, executar essas configurações, assim elas são feitas apenas uma vez.

Existem várias outras opções de configuração para o AutoMapper e no site deles tem vários exemplos, vale a pena conferir. Aqui, foi só pra dar um gostinho.
