---
layout: post
title: Globalização de validação do jQuery com o ASP.NET MVC 3
categories:
- Desenvolvimento
tags:
- asp.net mvc
- globalizacao
- javascript
- jquery
- programacao
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _revision-control: a:1:{i:0;s:8:"defaults";}
  dsq_thread_id: '417833571'
  _yoast_wpseo_linkdex: '63'
  _yoast_wpseo_focuskw: Globalização de validação do jQuery
  _yoast_wpseo_title: Globalização da validação do jQuery - Temple Coding
  _yoast_wpseo_metadesc: Veja como fazer globalização da validação do jQuery quando
    validar datas.
  _yoast_wpseo_meta-robots-noindex: '0'
  _yoast_wpseo_meta-robots-nofollow: '0'
  _yoast_wpseo_meta-robots-adv: none
  _yoast_wpseo_sitemap-include: ! '-'
  _yoast_wpseo_sitemap-prio: ! '-'
  _yoast_wpseo_canonical: ''
  _yoast_wpseo_redirect: ''
  _yoast_wpseo_opengraph-description: ''
  _yoast_wpseo_google-plus-description: ''
  _clicky_goal: a:2:{s:2:"id";s:0:"";s:5:"value";s:0:"";}
  qtrans_meta:title: <!--:pt-->Globalização de validação do jQuery com o ASP.NET MVC
    3<!--:--><!--:en-->Globalizing jQuery validation with ASP.NET MVC 3<!--:-->
  qtrans_meta:keywords: <!--:pt-->validação jquery, Globalização, jquery<!--:--><!--:en-->jQuery
    validation, globalizing, jquery<!--:-->
  qtrans_meta:description: <!--:pt-->Veja como fazer globalização da validação do
    jQuery quando validar datas.<!--:--><!--:en-->Here's how the globalization of
    the jQuery validation when validating dates.<!--:-->
  _wpas_skip_2089817: '1'
  _wpas_skip_2089821: '1'
  _wpas_skip_2089827: '1'
  _qts_slug_pt: globalizacao-de-validacao-do-jquery-com-o-asp-net-mvc-3
  _qts_slug_en: globalizing-jquery-validation-with-asp-net-mvc-3
  _yoast_wpseo_metakeywords: ''
---
<!--:pt-->O Asp.Net MVC 3 tem uma ótima funcionalidade que é a validação no cliente usando o plugin do <a href="http://docs.jquery.com/Plugins/Validation" target="_blank">jQuery validations</a> e usando um script da própria Microsoft a validação é feita sem que seja necessário nenhum codificação especial. Basta usar os atributos do namespace <em>System.ComponentModel.DataAnnotations.</em>

Pra quem não sabe, para fazer essa validação basta adicionar os seguintes scripts (que já estão no template padrão de um novo site Asp.Net Mvc 3):
<pre class="brush: html;">&lt;script src=”/Scripts/jquery-1.6.2.min.js” type=”text/javascript”&gt;&lt;/script&gt;
&lt;script src=”/Scripts/jquery.validate.min.js” type=”text/javascript”&gt;&lt;/script&gt;
&lt;script src=”/Scripts/jquery.validate.unobtrusive.min.js” type=”text/javascript”&gt;&lt;/script&gt;</pre>
Muito simples de verdade! Mas existe um problema, esse script não é globalizado, então digamos que você tenha que usar uma data na sua classe e você gostaria de validá-la, por exemplo eu tinha uma classe que entre outras coisas tinha que salvar uma data, para simplificar, algo como isso:
<pre class="brush: csharp;">public class Pessoa
{
    public string Nome { get; set; }
    public string Sexo { get; set; }
    public string Rg { get; set; }

    [DataType(DataType.EmailAddress)]
    public string Email { get; set; }

    [DataType(DataType.Date)]
    public DateTime? DataNascimento { get; set; }
}</pre>
Quando eu informava a data 16/09/1980, por exemplo, automaticamente era detectado uma data inválida, já que o esperado é que a data estivesse no padrão mm/dd/aaaa, mas é claro que no Brasil o padrão é outro. Então como corrigir? Primeiro fazer uso do plugin <a href="https://github.com/jquery/globalize">Globalize do jQuery</a>, adicione referências a dois scripts (que é claro, podem ser baixados no site do projeto):<script type="text/javascript" src="@Url.Content("></script><script type="text/javascript" src="@Url.Content("></script>

Depois disso, tudo o que precisa ser feito é determinar qual a cultura que quer usar e indicar o parser do globalize para validar a data:
<pre class="brush: js;">Globalize.culture("pt-BR");
$.validator.methods.date = function(value, element) {
    return this.optional(element) || Globalize.parseDate(value);
};</pre>
Não podia ser mais simples!<!--:--><!--:en--><!--:-->
