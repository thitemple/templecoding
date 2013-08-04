---
layout: post
title: ASP.NET MVC e o Grid do MvcContrib
categories:
- Desenvolvimento
tags:
- asp.net mvc
- grid
- mvccontrib
status: publish
type: post
published: true
meta:
  dsq_thread_id: '162422262'
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
Uma das coisas que eu sempre gostei do ASP.NET Web Forms era a produtividade que a gente ganhava com os controles, entre eles, um dos que eu mais usava sempre foi o GridView. E devo admitir que esse foi também um fator de resistência para eu começar a usar o ASP.NET MVC.

Resistência vencida, toda a vez que eu precisava de um Grid, ou seja, de dados tabulares, eu usava o bom e velho e um loop através dos meus objetos da collection.

Isso é claro, até eu me aventurar com o Grid do <a href="http://mvccontrib.codeplex.com/" target="_blank">MvcContrib</a>. O MvcContrib é um projeto open source que, entre outras coisas, possui esse Grid que é muito prático e simples.

Pra começar é só baixar o assembly do site do projeto e adicionar uma referência para o assembly MvcContrib.dll

Eu fiz uma action bem simples que retorna uns dados estáticos para a página:
<pre class="brush: csharp;">public ViewResult Index()
{
	var produtos = new List
	{
		new Produto
		{
			Id = 1,
			Nome = "Chaveiro",
			Valor = 11.50m,
			Categoria = new Categoria {Id = 1, Nome = "Utilitários"}
		},
		new Produto
		{
			Id = 2,
			Nome = "Mouse",
			Valor = 100m,
			Categoria = new Categoria {Id = 2, Nome = "Informática"}
		},
		new Produto
		{
			Id = 3,
			Nome = "Pen-Drive 8GB",
			Valor = 150.99m,
			Categoria = new Categoria {Id = 2, Nome = "Informática"}
		}
	};

	return View(produtos);
}</pre>
E na view é muito simples também:
<pre class="brush: html;"><%@ Import Namespace="MvcContrib.UI.Grid" %>
<asp:Content ID="Content1" ContentPlaceHolderID="TitleContent" runat="server">
	Lista de Produtos
</asp:Content>

<asp:Content ID="Content2" ContentPlaceHolderID="MainContent" runat="server">

	<h2>Lista de Produtosh2>

	<%= Html.Grid(Model).Columns(coluna =>
	{
		coluna.For(x => x.Nome);
		coluna.For(x => x.Valor);
		coluna.For(x => x.Categoria.Nome);
	})
	%>

</asp:Content></pre>
Isso gera um grid simples na página:

<a href="http://templecoding.com/wp-content/uploads/2010/07/ScreenHunter_01_07_11.gif"><img style="display: inline; border-width: 0px;" title="ScreenHunter_01 Jul. 05 11.55" src="http://templecoding.com/wp-content/uploads/2010/07/ScreenHunter_01_07_11_thumb.gif" alt="ScreenHunter_01 Jul. 05 11.55" width="297" height="178" border="0" /></a>

Agora vamos permitir uma ordenação, primeiro eu altero a view para exibir os links de ordenação:
<pre class="brush: html;"><%= Html.Grid(Model)
	.Sort(ViewData["sort"] as GridSortOptions)
	.Columns(coluna =>
	{
		coluna.For(x => x.Nome);
		coluna.For(x => x.Valor);
		coluna.For(x => x.Categoria.Nome).Named("Categoria");
	})
%></pre>
E na action do controller eu faço seguinte:
<pre class="brush: csharp;">using MvcContrib.Sorting;
// ...
public ViewResult Index(GridSortOptions sort)
{
	var produtos = GetProdutos();
	if (sort.Column == null)
		sort.Column = "Nome";

	produtos = produtos.OrderBy(sort.Column, sort.Direction);
	ViewData["sort"] = sort;

	return View(produtos);
}</pre>
Para ficar mais claro eu movi a inicialização dos produtos para um outro método. Mas como você pode ver no código acima, é muito simples adicionar ordenação.

Por fim, para paginar alteramos a action:
<pre class="brush: csharp;">using MvcContrib.Pagination;
// ...
public ViewResult Index(GridSortOptions sort, int? pagina)
{
    var produtos = GetProdutos();
    if (sort.Column == null)
        sort.Column = "Nome";

    produtos = produtos.OrderBy(sort.Column, sort.Direction);
    ViewData["sort"] = sort;

    return View(produtos.AsPagination(pagina ?? 1, 10));
}</pre>
E na view usamos um helper para exibir as páginas:
<pre class="brush: html;"><%= Html.Grid(Model)
    .Sort(ViewData["sort"] as GridSortOptions)
    .Columns(coluna =>
    {
        coluna.For(x => x.Nome);
        coluna.For(x => x.Valor);
        coluna.For(x => x.Categoria.Nome).Named("Categoria");
    })
%>

<%= Html.Pager((IPagination)Model).Format("Exibindo {0} - {1} de {2}") %></pre>
<strong>Conclusão</strong>

Fazer paginação e ordenação manualmente em grids pode ser bem complicado às vezes. Pela simplicidade da solução hoje o Grid do MvcContrib é a minha primeira opção.
