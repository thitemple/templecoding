---
layout: post
title: Localização de mensagem de erro no ASP.NET MVC
categories:
- Desenvolvimento
tags:
- asp.net
- asp.net mvc
- localizacao
status: publish
type: post
published: true
meta:
  dsq_thread_id: '160327702'
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
Sabe quando você preenche uma data inválida em uma página usando o ASP.NET MVC e o erro vem em inglês assim:

<a href="http://templecoding.com/wp-content/uploads/2010/07/ErroIngles.png"><img style="border: 0px currentColor; display: inline;" title="Erro en inglês" src="http://templecoding.com/wp-content/uploads/2010/07/ErroIngles_thumb.png" alt="Erro em inglês" width="451" height="157" border="0" /></a>

Isso acontece porque o DefaultModelBinder do MVC não conseguiu converter essa data, no caso acima, é óbvio porque não existe um mês 15. A questão é, como traduzir essa mensagem?

Um jeito que achei foi adicionando uma pasta App_GlobalResources e dentro dessa pasta adicionar um arquivo de resources, que no caso, chamei de CustomMvcResources.resx.

<a href="http://templecoding.com/wp-content/uploads/2010/07/ArquivoResources.png"><img style="border: 0px currentColor; display: inline;" title="Arquivo de Resources" src="http://templecoding.com/wp-content/uploads/2010/07/ArquivoResources_thumb.png" alt="Arquivo de Resources" width="243" height="141" border="0" /></a>

Nesse arquivo de resources adicione a seguinte linha:

<a href="http://templecoding.com/wp-content/uploads/2010/07/ItemResource.png"><img style="border: 0px currentColor; display: inline;" title="Item do Resource" src="http://templecoding.com/wp-content/uploads/2010/07/ItemResource_thumb.png" alt="Item do Resource" width="500" height="60" border="0" /></a>

No global.asax altere o método Application_Start para que fique assim:
<pre class="brush: csharp;">protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    DefaultModelBinder.ResourceClassKey = "CustomMvcResources";
    RegisterRoutes(RouteTable.Routes);
}</pre>
O importante é que o valor informado à propriedade ResourceClassKey tenha o nome do arquivo de resource criado.

Agora o erro será apresentado do jeito que foi configurado no arquivo de resources.

<a href="http://templecoding.com/wp-content/uploads/2010/07/ErroTraduzido.png"><img style="border: 0px currentColor; display: inline;" title="Erro Traduzido" src="http://templecoding.com/wp-content/uploads/2010/07/ErroTraduzido_thumb.png" alt="Erro Traduzido" width="495" height="166" border="0" /></a>
