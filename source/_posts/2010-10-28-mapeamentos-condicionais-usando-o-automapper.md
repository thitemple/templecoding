---
layout: post
title: Mapeamentos condicionais usando o AutoMapper
categories:
- Desenvolvimento
tags:
- automapper
status: publish
type: post
published: true
meta:
  _wp_old_slug: ''
  dsq_thread_id: '164021377'
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
Eu me deparei com um problema diferente usando o AutoMapper. Eu precisava mapear um atributo do tipo DateTime de um web service para uma classe de negócio.

Se você já fez isso, provavelmente sabe que quando tem um atributo do tipo DateTime, você terá na sua classe de proxy dois atributos. Um do tipo DateTime e um booleano que informa se o seu DateTime está preenchido ou não.

Assim, supondo que no seu web service exista um atributo do tipo DateTime chamado CriadoEm, então você terá em sua classe de proxy dois atributos assim:
<pre class="brush: csharp;">
public DateTime CriadoEm { get; set; }
public bool CriadoEmSpecified { get; set; }
</pre>
Bom, nesse caso, eu queria que o AutoMapper mapeasse esse atributo apenas se CriadoEmSpecified fosse = true.

Eu resolvi isso dessa forma:
<pre class="brush: csharp;">Mapper.CreateMap<classeOrigemWs, MinhaClasseNeg>()
  .ForMember(c => c.CriadoEm, opt => {
    opt.Condition(x => x.CriadoEmSpecified);
    opt.MapFrom(x => x.CriadoEm);
  });
</pre>
É claro que isso vale para qualquer condição que você queira, então, agora é só aproveitar. <img class="wlEmoticon wlEmoticon-smile" style="border-style: none;" src="http://templecoding.com/wp-content/uploads/2010/10/wlEmoticon-smile.png" alt="Alegre" />
