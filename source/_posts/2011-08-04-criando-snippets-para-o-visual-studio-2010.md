---
layout: post
title: Criando Snippets para o Visual Studio 2010
categories:
- Desenvolvimento
tags:
- snippet
- visualstudio
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  dsq_thread_id: '377405288'
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
<strong>Motivação</strong>

Apesar de ser e gostar muito de ser um desenvolvedor, sou totalmente a favor de escrever a menor quantidade de código possível. Porque menos código, geralmente quer dizer menos erros. Com isso em mente eu resolvi criar Snippets de código no Visual Studio 2010. Você pode até não saber o que é um snippet mas com certeza já usou um. Um snippet é uma pequena porção de código que é reutilizável. Por exemplo, entre no seu Visual Studio, e dentro de um método digite <em>foreach</em> e aperte <strong>Tab</strong>. Pronto, isso é um snippet.

Seguindo essa mentalidade e fazendo testes unitários eu me peguei escrevendo diversas vezes esse código:
<pre class="brush: csharp;">[Test]
public void Meuteste()
{

}</pre>
Meu objetivo era que digitasse apenas um comando de atalho (nesse caso escolhi <em>ntest</em>) e que esse código fosse gerado pra mim, permitindo que eu apenas alterasse o nome do método.

<strong>Snippet Designer</strong>

O Visual Studio 2010 permite que você crie snippets através do seu editor de XML, que é o padrão, afinal de contas um snippet nada mais é do que um arquivo xml. Mas existe uma extensão para o VS2010 chamada Snippet Desiner que facilita um pouco o trabalho. Então vá em Tools –&gt; Extension Manager e adicione a extensão.

<a href="http://templecoding.com/wp-content/uploads/2011/08/Snippet-Designer.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: block; margin-left: auto; margin-right: auto; padding-top: 0px; border-style: initial; border-color: initial; border-width: 0px;" title="Snippet Designer" src="http://templecoding.com/wp-content/uploads/2011/08/Snippet-Designer_thumb.png" alt="Snippet Designer" width="569" height="397" border="0" /></a>

<strong>Criando o Snippet</strong>

Crie um novo projeto no VS2010 e adicione um arquivo do tipo XML. Dê o nome que quiser, mas use a extensão .snippet. No meu caso o nome do arquivo é NUnitTest.snippet.

Ao criar o arquivo ele estará vazio e, portanto, o Snippet Designer não irá abri-lo. Clique com o botão direito no arquivo e me Open With, selecione o XML (Text) Editor e clique em OK.

<a href="http://templecoding.com/wp-content/uploads/2011/08/EditorXml.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: block; margin-left: auto; margin-right: auto; padding-top: 0px; border-style: initial; border-color: initial; border-width: 0px;" title="EditorXml" src="http://templecoding.com/wp-content/uploads/2011/08/EditorXml_thumb.png" alt="EditorXml" width="346" height="229" border="0" /></a>

Dentro do documento XML clique com o botão direito e selecione a opção Insert Snipet

<a href="http://templecoding.com/wp-content/uploads/2011/08/Inser-.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: block; float: none; margin-left: auto; margin-right: auto; padding-top: 0px; border: 0px;" title="Inser" src="http://templecoding.com/wp-content/uploads/2011/08/Inser-_thumb.png" alt="Inser " width="382" height="170" border="0" /></a>

Em seguida selecione a opção Snippet.

<a href="http://templecoding.com/wp-content/uploads/2011/08/Inserir-Snippet.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: block; float: none; margin-left: auto; margin-right: auto; padding-top: 0px; border: 0px;" title="Inserir Snippet" src="http://templecoding.com/wp-content/uploads/2011/08/Inserir-Snippet_thumb.png" alt="Inserir Snippet" width="463" height="173" border="0" /></a>

O xml necessário para a criação do snippet será gerado, como você pode perceber ele mesmo é um snippet. #inception #feelings :)

É possível editar ali mesmo o xml, ou então podemos usar o Snippet Designer que acabos de instalar. Salve o arquivo e feche-o. No Solution Explorer, mais uma vez clique com o botão direito no arquivo e em Open With e dessa vez selecione a opção Snippet Desiger, uma tela como a abaixo deverá aparecer.

<a href="http://templecoding.com/wp-content/uploads/2011/08/Snippet-Designer2.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: block; margin-left: auto; margin-right: auto; padding-top: 0px; border-style: initial; border-color: initial; border-width: 0px;" title="Snippet Designer2" src="http://templecoding.com/wp-content/uploads/2011/08/Snippet-Designer2_thumb.png" alt="Snippet Designer2" width="555" height="350" border="0" /></a>

Algumas coisas que precisam ser alteradas, no topo do lado esquerdo, dê um título e selecione a linguangem, no meu caso foi NUnit Test e C#. As proprieades do arquivo são bem claras, mas o que você deve ter uma atenção é pra propriedade <strong>Shortcut. </strong>O que você colocar ali é o que você vai digitar antes do Tab na hora de codificar. Eu preenchi o meu como <em>ntest.</em>

Na parte de baixo é onde você define a parte do código que é customizável quando você fica dando tabs no seu snippet, no meu caso eu queria customizar apenas o nome do método, então alterei o item que vem ali para <em>testName</em> e deixei campo Defaults to para <em>testName</em> também.

Por fim, a principal e maior parte da tela é onde você coloca o seu código do snippet, apenas preencha o código que você quer ali normalmente e substituia a parte customizável pelas variáveis que você criou, prestando atenção que o nome dessa variável deve estar entre $$, assim: <em>$testName$. </em>Veja como ficou meu snippet após as mudanças.

<a href="http://templecoding.com/wp-content/uploads/2011/08/Snippet-Editado.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: block; margin-left: auto; margin-right: auto; padding-top: 0px; border-style: initial; border-color: initial; border-width: 0px;" title="Snippet Editado" src="http://templecoding.com/wp-content/uploads/2011/08/Snippet-Editado_thumb.png" alt="Snippet Editado" width="556" height="355" border="0" /></a>

<strong>Utilizando o Snippet</strong>

O VS2010 usa diversos snippets, mas obviamente é muito fácil adicionar aqueles que foram criados pelos usuários. Basta carregar os arquivos .snippet criados por você dentro dele.

Por padrão quando se instala o VS2010 ele cria uma pasta dentro da pasta Meus Documentos chamada Visual Studio 2010, ali navegue para –&gt; Code Snippets –&gt; Visual C# –&gt; My Code Snippets. Copie o(s) arquivo(s) .snippet que você criou e cole dentro dessa pasta. Isso é claro, considerando que são snippets de C#, caso contrário coloque na pasta correta.

É provável que tudo já esteja funcionando, mas para ter certeza, dentro do VS2010 vá em Tools –&gt; Code Snippets Manager, selecione a linguagem e deve aparecer algo assim.

<a href="http://templecoding.com/wp-content/uploads/2011/08/Code-Snippet-Manager.png"><img style="background-image: none; padding-left: 0px; padding-right: 0px; display: block; margin-left: auto; margin-right: auto; padding-top: 0px; border-style: initial; border-color: initial; border-width: 0px;" title="Code Snippet Manager" src="http://templecoding.com/wp-content/uploads/2011/08/Code-Snippet-Manager_thumb.png" alt="Code Snippet Manager" width="374" height="280" border="0" /></a>

Se não aparecer ali, basta clicar em Add, navegar e selecionar a pasta onde estão os arquivos .snippet. Pronto! Isso é tudo que você precisa.

Agora é só testar. Vá até uma classe, digite <em>ntest</em> e aperte <strong>Tab. </strong>O código do snippet deve aparecer e o cursor deve estar posicionado no nome do método, pronto para que este seja alterado.

<strong>Finalizando</strong>

O snippet que eu criei é bem simples, mas de um código que eu faço toda hora, se você tem alguma coisa desse tipo e quer facilitar, esse é o caminho. :)

<a href="https://github.com/vintem/VS2010Snippets" target="_blank">O código desse projeto está diposnível para download aqui.</a>
