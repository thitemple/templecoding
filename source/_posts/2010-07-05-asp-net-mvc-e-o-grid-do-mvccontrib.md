---
layout: post
title: ASP.NET MVC e o Grid do MvcContrib
categories:
- Desenvolvimento
- asp.net mvc
- grid
- mvccontrib
comments: true
status: publish
type: post
published: true
alias: 
---
Uma das coisas que eu sempre gostei do ASP.NET Web Forms era a produtividade que a gente ganhava com os controles, entre eles, um dos que eu mais usava sempre foi o GridView. E devo admitir que esse foi também um fator de resistência para eu começar a usar o ASP.NET MVC.

Resistência vencida, toda a vez que eu precisava de um Grid, ou seja, de dados tabulares, eu usava o bom e velho e um loop através dos meus objetos da collection.

Isso é claro, até eu me aventurar com o Grid do <a href="http://mvccontrib.codeplex.com/" target="_blank">MvcContrib</a>. O MvcContrib é um projeto open source que, entre outras coisas, possui esse Grid que é muito prático e simples.

Pra começar é só baixar o assembly do site do projeto e adicionar uma referência para o assembly MvcContrib.dll

Eu fiz uma action bem simples que retorna uns dados estáticos para a página:

{% codeblock lang:csharp %}
public ViewResult Index()
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
}
{% endcodeblock %}

E na view é muito simples também:

{% codeblock lang:html %}
<%@ Import Namespace="MvcContrib.UI.Grid" %>
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

</asp:Content>
{% endcodeblock %}

Isso gera um grid simples na página:

{% img aligncenter /images/2010/07/ScreenHunter_01_07_11.gif %}

Agora vamos permitir uma ordenação, primeiro eu altero a view para exibir os links de ordenação:

{% codeblock lang:html %}
<%= Html.Grid(Model)
	.Sort(ViewData["sort"] as GridSortOptions)
	.Columns(coluna =>
	{
		coluna.For(x => x.Nome);
		coluna.For(x => x.Valor);
		coluna.For(x => x.Categoria.Nome).Named("Categoria");
	})
%>
{% endcodeblock %}

E na action do controller eu faço seguinte:

{% codeblock lang:csharp %}
using MvcContrib.Sorting;
// ...
public ViewResult Index(GridSortOptions sort)
{
	var produtos = GetProdutos();
	if (sort.Column == null)
		sort.Column = "Nome";

	produtos = produtos.OrderBy(sort.Column, sort.Direction);
	ViewData["sort"] = sort;

	return View(produtos);
}
{% endcodeblock %}

Para ficar mais claro eu movi a inicialização dos produtos para um outro método. Mas como você pode ver no código acima, é muito simples adicionar ordenação.

Por fim, para paginar alteramos a action:

{% codeblock lang:csharp %}
using MvcContrib.Pagination;
// ...
public ViewResult Index(GridSortOptions sort, int? pagina)
{
    var produtos = GetProdutos();
    if (sort.Column == null)
        sort.Column = "Nome";

    produtos = produtos.OrderBy(sort.Column, sort.Direction);
    ViewData["sort"] = sort;

    return View(produtos.AsPagination(pagina ?? 1, 10));
}
{% endcodeblock %}

E na view usamos um helper para exibir as páginas:

{% codeblock lang:html %}
<%= Html.Grid(Model)
    .Sort(ViewData["sort"] as GridSortOptions)
    .Columns(coluna =>
    {
        coluna.For(x => x.Nome);
        coluna.For(x => x.Valor);
        coluna.For(x => x.Categoria.Nome).Named("Categoria");
    })
%>

<%= Html.Pager((IPagination)Model).Format("Exibindo {0} - {1} de {2}") %>
{% endcodeblock %}

<strong>Conclusão</strong>

Fazer paginação e ordenação manualmente em grids pode ser bem complicado às vezes. Pela simplicidade da solução hoje o Grid do MvcContrib é a minha primeira opção.
