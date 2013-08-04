---
layout: post
title: Templates no Asp.Net MVC2
categories:
- Desenvolvimento
- asp.net mvc
- templates
comments: true
status: publish
type: post
published: true
alias: /templates-no-asp-net-mvc2/index.html
---
Uma das grandes novidades do Asp.Net MVC2 são os templates. Quem já fez algum projeto usando o Dynamic Data do WebForms vai ver que essa funcionalidade é bem parecida com a do Dynamic Data.

O framework agora é capaz de exibir um dado em um formato específico de acordo com o tipo de dados (integer, decimal, boolean, ou mesmo uma classe).

Como isso funciona? A forma mais simples é utililando o método <strong>Html.DisplayForModel. </strong>Vejamos, dado a classe de modelo:

{% codeblock lang:csharp %}
public class Cliente
{
    public string Nome { get; set; }
    public string Email { get; set; }
    public bool Ativo { get; set; }
    public DateTime DataNascimento { get; set; }
    public decimal Credito { get; set; }

    public static Cliente Criar()
    {
        return new Cliente {
            Nome = "Thiago Temple",
            Email = "eu@vintem.com.br",
            Ativo = true,
            Credito = 1000,
            DataNascimento = new DateTime(1981, 10, 15)
        };
    }
}
{% endcodeblock  %}

Uma view dessa forma:

{% img aligncenter /images/2010/03/Html-Index.png Html Index %}

E um controller:

{% codeblock lang:csharp %}
public class ClienteController : Controller
{
    public ViewResult Index()
    {
        return View(Cliente.Criar());
    }

    public ViewResult Edit()
    {
        return View(Cliente.Criar());
    }

    [HttpPost]
    public ViewResult Edit(Cliente cliente)
    {
        if (ModelState.IsValid)
            return View("Index", cliente);
        return View(cliente);
    }
}
{% endcodeblock %}

Então ao chamarmos a Action Index do nosso controller veremos:

{% img aligncenter /images/2010/03/Template-Index.png Template Index %}
<img class="aligncenter size-full wp-image-228" title="Template Index" src="http://templecoding.com/wp-content/uploads/2010/03/Template-Index.png" alt="" width="355" height="374" />

Veja que os nomes das propriedades aparecem nos labels e os campos já estão formatados como o booleano, data e decimal.

Para a action Edit, temos um método similar <strong>Html.EditForModel</strong>

{% img aligncenter /images/2010/03/Html-Edit.png Html Edit %}

Que vai exibir um visualização assim:

{% img aligncenter /images/2010/03/Template-Edit.png Template Edit %}

Simples não? Mas e se eu quiser exibir "Data de Nascimento" ao invés de DataNascimento no label? Basta decorar a propriedade com o atributo DisplayName do namespace System.ComponentModel assim:

{% codeblock lang:csharp %}
public class Cliente
{
    public string Nome { get; set; }
    public string Email { get; set; }
    public bool Ativo { get; set; }
    [DisplayName("Data de Nascimento")]
    public DateTime DataNascimento { get; set; }
    public decimal Credito { get; set; }
}
{% endcodeblock %}

Existem alguns atributos disponíveis para customizar o modelo, são eles:
<ul>
	<li>[HiddenInput] (System.Web.Mvc) - usando esse atributo será gerado um campo hidden no modo Edit</li>
	<li>[DataType] (System.ComponentModel.DataAnnotations) - Define o tipo de dados da propriedade</li>
	<li>[ReadOnly] (System.ComponentModel) - Deixará a propriedade como ReadOnly</li>
	<li>[DisplayFormat] (System.ComponentModel.DataAnnotations) - Definir a propriedade NullDisplayText exibe um texto para quando o valor for nulo.  Informar a propriedade DataFormatString define em qual formato o texto deverá ser exibido. Informar a propriedade ApplyFormatInEditMode como true irá usar o formato também no modo Edit. Informar a propriedade ConvertEmptyStringToNull irá converter uma string vazia para  nulo.</li>
	<li>[DisplayName] (System.ComponentModel) - Define o nome da propriedade</li>
</ul>
<strong>Exibindo campos específicos</strong>

Se for o caso de exibir ou editar apenas alguns campos do modelo, pode-se utililzar os métodos <strong>Html.DisplayFor(model =&gt; model.Propriedade) </strong>e <strong>Html.EditFor(model =&gt; model.Propriedade)</strong>.

<strong>Conclusão</strong>

É muito simples gerar uma visualização a partir dos assistentes do visual studio, mas isso irá gerar um código estático. Se você quiser uma forma mais prática com base nas propriedades do modelo basta usar os novos métodos. Existe também uma forma de customizar a exibição das propriedades. Vou mostrar isso num próximo post.
