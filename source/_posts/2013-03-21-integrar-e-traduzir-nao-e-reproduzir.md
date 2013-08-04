---
layout: post
title: Integrar é traduzir, não é reproduzir
categories:
- Desenvolvimento
tags:
- integracaoes
status: publish
type: post
published: true
meta:
  _yoast_wpseo_linkdex: '0'
  _edit_last: '2'
  _yoast_wpseo_focuskw: ''
  _yoast_wpseo_title: ''
  _yoast_wpseo_metadesc: ''
  _yoast_wpseo_metakeywords: ''
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
  dsq_thread_id: '1154436610'
  _wpas_skip_2089817: '1'
  _wpas_skip_2089821: '1'
  _wpas_skip_2089827: '1'
---
Eu trabalho fazendo integrações de aplicações web com o SAP há alguns anos já. E existem "alguns padrões" (vou chamar de padrões), que existem no SAP que são bem particulares à esse ERP.

Só para ambientar aqueles que não conhecem nada de SAP, o SAP é um grande ERP e ele possui todo um ecossistema próprio para customização e criação de soluções de negócio. Por exemplo, ele tem uma linguagem própria o <a href="https://en.wikipedia.org/wiki/Abap">ABAP</a>, tem um protocolo de comunicações proprietário, o <a href="https://en.wikipedia.org/wiki/Remote_function_call">RFC</a>, entre outras coisas. Fora isso, existe também toda uma cultura particular de desenvolvimento, que honestamente, eu não conheço muito, já que nunca fui um programador ABAP.

<a href="http://templecoding.com/integrar-e-traduzir-nao-e-reproduzir/system_integration/" rel="attachment wp-att-1231"><img class="alignleft size-full wp-image-1231" title="system_integration" src="http://templecoding.com/wp-content/uploads/2013/03/system_integration.jpeg" alt="" width="240" height="150" /></a>A minha questão aqui é, independente do porque, e se esta certo ou não, geralmente quando faço integrações com o SAP encontro coisas que não estou acostumado a ver quando desenvolvo minhas soluções usando outras tecnologias, como por exemplo, .NET e C#.

Por exemplo, em muitas integrações que fiz com o SAP quando chamo uma função remota e essa gera um erro, ao invés de receber um exceção, a chamada é executada com sucesso e uma estrutura de dados (como na imagem abaixo) é retornada preenchida contendo um tipo de erro, um código e possivelmente algumas mensagens. Esse é um padrão usado e que já vi várias vezes.

<a href="http://templecoding.com/integrar-e-traduzir-nao-e-reproduzir/sap_return/" rel="attachment wp-att-1224"><img class="aligncenter  wp-image-1224" title="sap_return" src="http://templecoding.com/wp-content/uploads/2013/03/sap_return.png" alt="Retorno de erros no SAP" width="446" height="319" /></a>

Obs.: <a href="http://cc2e.com/Page.aspx?hid=147" target="_blank">Steven McConnell deve ficar tão orgulhoso disso</a>.

OK, mas qual é o problema disso e o que tem a ver com o que eu faço?

Bom, o problema na minha opinião, é quando esse tipo de padrão começa a vazar para outras aplicações. Tenho que admitir que eu já fiz a minha parte de permitir que isso vazasse. Shame on me!

Mas voltando, se estamos fazendo uma integração com uma aplicação terceira, não importa qual, e ela tem uma particularidade, não precisamos expor essa particularidade, podemos encapsulá-la e mudar o padrão para algo mais genérico, ou que faça sentido para a nossa plataforma de desenvolvimento. Por exemplo, no caso de um Web Service, lançar um SoapException.

Outro padrão comum ao SAP é a nomenclatura de estruturas. Não sei porque, mas encontramos muitas estruturas com esse tipo de nomenclatura.

<a href="http://templecoding.com/integrar-e-traduzir-nao-e-reproduzir/nomes_sap/" rel="attachment wp-att-1223"><img class="aligncenter size-full wp-image-1223" title="Nomenclatura_SAP" src="http://templecoding.com/wp-content/uploads/2013/03/nomes_sap.png" alt="Nomenclatura no SAP" width="247" height="434" /></a>

Talvez isso faça sentido na cabeça de algum alemão louco, mas, voltando ao C#, um nome de variável pode ter até 1023 bytes, isso quer dizer que podemos ser bem mais descritivos que isso. E no caso de um Web Service também, não ha uma limitação especifica para o nome de uma tag XML.

Mais uma vez o ponto é, no momento em que estamos fazendo uma integração, seja qual for a tecnologia utilizada (no meu caso é o BizTalk), esse é o momento de abstrair particularidades, de aplicar boas práticas para quem vai consumir essa integração.<a href="http://templecoding.com/integrar-e-traduzir-nao-e-reproduzir/images_boo/" rel="attachment wp-att-1232"><img class="alignright size-full wp-image-1232" title="images_boo" src="http://templecoding.com/wp-content/uploads/2013/03/images_boo.jpeg" alt="" width="255" height="197" /></a>

Eu vejo uma integração como uma tradutora. Ela ouve em alemão e traduz para português, ela não traduz algumas partes para português e mantém outras no formato original. Para mim, que não falo nada de alemão, isso só me atrapalharia.

E nem sempre é uma questão de certo ou errado, mas sim de pontos que são mais idiomáticos em uma tecnologia do que em outra. Nesse caso o problema são as traduções literais que muitas vezes não fazem sentido, temos que contextualizar para a tecnologia que estamos usando.

<a href="http://templecoding.com/integrar-e-traduzir-nao-e-reproduzir/slip/" rel="attachment wp-att-1230"><img class="aligncenter size-full wp-image-1230" title="slip" src="http://templecoding.com/wp-content/uploads/2013/03/slip.png" alt="" width="249" height="263" /></a>Mais uma vez usando o SAP como <del>bode expiatório</del> exemplo, existem alguns casos em que as strings não podem ter mais do 132 caracteres, por isso vemos estruturas como a anterior que tem campos message1, message2, message3, etc., e, embora isso funcione, é uma limitação daquela plataforma/tecnologia. Esse não é o caso de um XML ou do próprio C#, portanto, passar uma estrutura dessas adiante é simplesmente fazer uma tradução literal e não olhar para o contexto.

Concluindo, quando for integrar duas ou mais aplicações, tenha em mente os limites tecnológicos, funcionais e semânticos de cada ponta. Leve em consideração o que é um padrão proprietário e deve ser transformado do que realmente é um padrão de integração e deve ser mantido. <a href="http://www.amazon.com/gp/product/0321200683/ref=as_li_ss_tl?ie=UTF8&amp;camp=1789&amp;creative=390957&amp;creativeASIN=0321200683&amp;linkCode=as2&amp;tag=tempcodi0f-20" target="_blank">Existem inclusive livros que podem nos ajudar com isso</a>.
