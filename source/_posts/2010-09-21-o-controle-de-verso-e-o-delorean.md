---
layout: post
title: O Controle de Versão e o DeLorean
categories:
- Desenvolvimento
tags:
- controle de versoes
status: publish
type: post
published: true
meta:
  dsq_thread_id: '159669369'
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
Um cenário que é comumente visto em equipes de desenvolvimento que usam ferramentas de controle de versão como Subversion, Source Safe, CVS, Git, etc, é aquele onde o desenvolvedor faz um checkout no código, altera o que tem que alterar e na sequência faz o commit (check-in se vc ainda é um infeliz usuário do VSS).

Qual o problema desse cenário? Você consegue dizer? O código está num repositório central disponível para todos os desenvolvedores da equipe, não está?

O problema, nesse caso, é que na grande maioria das vezes o que se faz é simplesmente esse procedimento de check-out e commit, não é efetivamente um <strong>Controle de Versão</strong>, existe apenas um <strong>repositório central de código</strong>.

Existem diversas razões as quais eu poderia dizer que você não quer fazer apenas um repositório central, desde a mais simples, que é o fato de você controlar o que é efetivamente disponibilizado a cada versão, até o<a href="http://templecoding.com/wp-content/uploads/2010/09/DeLorean-DeVoltaparaoFuturo-1_2.jpg"><img style="display: inline; margin-left: 0px; margin-right: 0px; border: 0px;" title="DeLorean-DeVoltaparaoFuturo-1" src="http://vintem.com.br/wp-content/uploads/2010/09/DeLorean-DeVoltaparaoFuturo-1_thumb.jpg" border="0" alt="DeLorean-DeVoltaparaoFuturo-1" width="244" height="184" align="right" /></a> que acho que é o maior benefício e que aqui vou chamar de a possibilidade de "viajar no tempo".

Digamos que você disponibilizou a versão 1.0 do seu mais novo cliente do Twitter. Software no ar, milhares de downloads em poucas horas. Hora de preparar a inovadora versão 2.0 que vai ler as mensagens em voz alta e que você vai poder ditar as mensagens também. Ótimo para não perder nada do Twitter quando você está dirigindo.

Check-out feito, algumas alterações, novas funcionalidades e uns commits, afinal de contas, você é um praticante de <a href="http://templecoding.com/2010/06/20/integracao-continua-com-cruise-control-net/" target="_blank">Integração Contínua</a>. De repente, seu telefone toca e aquela versão 1.0 tem um bug que trava o aplicativo e você precisa corrigir. O que você faz? Fala que não pode porque o código da versão 2.0 está no meio e ainda não foi testado? Desfaz o que foi feito?

Voltemos para o dia em que a versão 1.0 foi lançada, o código está no repositório e você decide iniciar a versão 2.0, que é um ponto no futuro onde você gostaria de chegar. É para isso que existe uma coisa chamada branch, ele é uma cópia separada da sua versão 1.0 onde você pode fazer alterações e ainda por cima manter a versão 1.0 disponível do jeito que você deixou. Quando esse bug chega, você altera o código do branch principal, o da versão em produção (no caso 1.0), e pode mandar essa versão, digamos, 1.0.1 com a correção sem se preocupar com o código da versão 2.

Parece simples, e realmente é, mas é um hábito muito pouco comum em equipes de desenvolvimento, esse de criar branches de versões.

Ferramentas de controle de versão são para isso, para dar esse poder a desenvolvedores, o poder de viajar entre as versões, as modificações, etc. Mas para isso é preciso, no mínimo, deixar um rastro do caminho a ser percorrido.

<a href="http://vintem.com.br/wp-content/uploads/2010/09/de-volta-para-o-futuro_2.jpg"><img style="display: inline; border: 0px;" title="de-volta-para-o-futuro" src="http://vintem.com.br/wp-content/uploads/2010/09/de-volta-para-o-futuro_thumb.jpg" border="0" alt="de-volta-para-o-futuro" width="244" height="210" /></a>
