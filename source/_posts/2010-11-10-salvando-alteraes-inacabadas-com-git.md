---
layout: post
title: Salvando alterações inacabadas com Git
categories:
- Desenvolvimento
tags:
- controle de versoes
- git
status: publish
type: post
published: true
meta:
  dsq_thread_id: ''
  _edit_last: '2'
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
Quanto mais eu uso o Git, mais eu gosto dele. Fato.

Sabe quando você está no meio de uma atividade, ela não está acabada, e chega um bug que você tem que corrigir? Ai o que você faz com a sua alteração que está no meio do caminho?

O Git tem um comando chamado stash. O stash é uma área separada dos seus commits feita exatamente para isso, salvar modificações inacabadas. E o uso é bem simples.

git stash save

Esse comando salva as modificações do diretório de trabalho e limpa o log para o último commit.

Por exemplo, digamos que você tenha um projeto que você concluiu a versão 1 e está no meio do desenvolvimento da versão 2.0 e você criou um branch para isso.

<a href="http://templecoding.com/wp-content/uploads/2010/11/v2.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border: 0px;" title="v2" src="http://templecoding.com/wp-content/uploads/2010/11/v2_thumb.png" alt="v2" width="346" height="139" border="0" /></a>

E então, enquanto você no meio do desenvolvimento de uma outra atividade, aparece um bug para corrigir, seu status do Git estaria mais ou menos assim:

<a href="http://templecoding.com/wp-content/uploads/2010/11/git_status.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border: 0px;" title="git_status" src="http://templecoding.com/wp-content/uploads/2010/11/git_status_thumb.png" alt="git_status" width="633" height="217" border="0" /></a>

Você não pode fazer um commit, porque se você fizer isso o build irá quebrar, e você é muito responsável para fazer uma coisa dessas, né?. O que fazer?`

<a href="http://templecoding.com/wp-content/uploads/2010/11/moron.jpg"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border: 0px;" title="moron" src="http://templecoding.com/wp-content/uploads/2010/11/moron_thumb.jpg" alt="moron" width="244" height="184" border="0" /></a>

É ai que o git stash resolve. Com o comando git stash save você obtém como resultado:

<a href="http://templecoding.com/wp-content/uploads/2010/11/stash.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border: 0px;" title="stash" src="http://templecoding.com/wp-content/uploads/2010/11/stash_thumb.png" alt="stash" width="660" height="265" border="0" /></a>

O buffer fica limpo, você já pode fazer um checkout da versão 1.0, corrigir o bug e depois voltar para o branch da versão 2.0 e fazer um rebase da versão 1.0.

Quando estiver nesse ponto basta executar o comando git stash pop para resgatar suas atualizações.

<a href="http://templecoding.com/wp-content/uploads/2010/11/stash_pop.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border: 0px;" title="stash_pop" src="http://templecoding.com/wp-content/uploads/2010/11/stash_pop_thumb.png" alt="stash_pop" width="553" height="343" border="0" /></a>

Pronto, você está no ponto onde parou. E viva o controle de versões. <img class="wlEmoticon wlEmoticon-smile" style="border-style: none;" src="http://templecoding.com/wp-content/uploads/2010/11/wlEmoticon-smile1.png" alt="Alegre" />

Existem mais opções no comando stash. Eu recomendo dar uma olhada na <a href="http://www.kernel.org/pub/software/scm/git/docs/git-stash.html">documentação do git</a>.
