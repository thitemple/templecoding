---
layout: post
title: Selecionando o código que será contabilizado pelo NCover
categories:
- Desenvolvimento
tags:
- desenvolviment
- ncover
- nunit
- programacao
status: publish
type: post
published: true
meta:
  dsq_thread_id: '159221697'
  _edit_last: '2'
  _yoast_wpseo_linkdex: '72'
  _yoast_wpseo_focuskw: NCover
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
<a href="http://vintem.com.br/2010/06/14/builds-automticos-com-msbuild-nunit-e-ncover/">No meu post anterior sobre build automático com MSBuild e NCover</a> e usei uma opção do NCover chamada ExcludeAttributes como pode ser visto na linha 7 abaixo:
<pre class="brush: xml;">&lt;ncover ToolPath="$(CaminhoNCover)"
    CommandLineExe="$(ComandoNUnit)"
    CommandLineArgs="$(TestFolder)bin$(Configuration)UnitTests.dll"
    WorkingDirectory="$(TestFolder)bin$(Configuration)"
    CoverageFile="$(ResultsFolder)$(ArquivoNCover)"
    LogFile="$(ArquivoLogNCover)"
    ExcludeAttributes="MSBuildDemo.Common.CoverageExcludeAttribute"
    AssemblyList="@(CodeProjects-&gt;'%(FileName)')" /&gt;</pre>
<strong>O que isso quer dizer e por que isso importa?</strong>

Isso importa porque, nem todo o código que temos em nossos projetos vamos querer testar e porque esse código pode atrapalhar a métrica de percentual de código coberto pelos testes.

Usando como exemplo o projeto que fiz no modelo anterior que <a href="https://github.com/vintem/MSBuildDemo" target="_blank">pode ser visto aqui</a>, podemos ver que em todo projeto ASP.NET MVC algumas classes são criadas automaticamente e essas classes não precisam ser testadas (assumindo que elas funcionam e não vamos modificá-las). Outro exemplo que temos nesse projeto, são as classes geradas automaticamente pelo LINQ to SQL para mapear a base de dados.

<strong>Como resolver?</strong>

Para resolver esse problema eu criei a classe MSBuildDemo.Common.CoverageExcludeAttribute e configurei na tarefa do NCover como visto acima. Veja como ficou a classe:
<pre class="brush: csharp;">[AttributeUsage(AttributeTargets.All)]
public sealed class CoverageExcludeAttribute : Attribute
{
}</pre>
Bem simples não? Agora é só colocar esse atributo nas classes ou métodos que não queremos contar para a cobertura do código. Como nesse exemplo, veja a linha 2:
<pre class="brush: csharp;">[HandleError]
[CoverageExclude]
public class AccountController : Controller
{
    // ...
}</pre>
