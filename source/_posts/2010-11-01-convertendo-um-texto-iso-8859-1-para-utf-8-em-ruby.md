---
layout: post
title: Convertendo um texto ISO-8859-1 para UTF-8 em Ruby
categories:
- Desenvolvimento
tags:
- encoding
- ruby
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _wp_old_slug: ''
  dsq_thread_id: '166111675'
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
No Ruby 1.9 uma coisa que você pode ter certeza é que terá dor de cabeça é com Encoding. Faça uma busca rápida no google por "ruby 1.9 encoding" e você vai entender do que eu estou falando.

Hoje eu tive que converter um conteúdo que estava em iso-8859-1 para utf-8 e recebi o seguinte erro:

incompatible character encodings: UTF-8 and ISO-8859-1

Para resolver isso eu usei:

<pre class="brush: ruby;">"meu texto".encode("UTF-8", undef: :replace, invalid: :replace)</pre>

E o problema foi resolvido. Segundo a <a href="http://ruby-doc.org/ruby-1.9/classes/String.html#M000553" target="_blank">documentação do Ruby 1.9</a> o parâmetro :invalid com o valor :replace, faz com que o método encode substitua o carácter inválido. O padrão é gerar uma exceção.
