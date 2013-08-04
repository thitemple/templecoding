---
layout: post
title: Builds automáticos com MSBuild, NUnit e NCover
categories:
- Desenvolvimento
tags:
- build
- build automatico
- msbuild
- ncover
- nunit
status: publish
type: post
published: true
meta:
  dsq_thread_id: '162286280'
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
O MSBuild é a ferramenta da Microsoft usada em conjunto com o Visual Studio para compilar os projetos. Toda vez que compilamos um projeto no Visual Studio (a partir da versão 2005 pelo menos), o que acontece é que o VS chama o MSBuild para executar a tarefa de compilar os projetos da solução.

O bom disso é que podemos customizar a compilação/deploy para executar diversas tarefas nesse momento, entre essas atividades existem algumas como zipar o projeto compilado, enviar por email, publicar no IIS, etc. Mas eu queria falar sobre duas que tenho usado que são: executar os testes unitários e executar uma ferramenta de cobertura de código para ver qual a porcentagem de código coberto pelos testes unitários.

<strong>O projeto demo</strong>

Para começar, criei um solução usando o ASP.NET MVC, com um componente para acesso a dados separado do projeto de Web. Apenas como forma de mostrar o MSBuild compilando projetos diferentes.

<a href="http://templecoding.com/wp-content/uploads/2010/06/Diagrama%20Componentes_2.png"><img style="border-width: 0px; display: block; float: none; margin-left: auto; margin-right: auto;" title="Diagrama Componentes" src="http://templecoding.com/wp-content/uploads/2010/06/Diagrama%20Componentes_thumb.png" alt="Diagrama Componentes" width="408" height="179" border="0" /></a>

O projeto é bem simples, a idéia mesmo é conectar no intergalaxialmente famoso banco de dados Northwind e exibir os dados da tabela de categorias. Uau!!! Uma tarefa que consumirá horas, mas, como eu disse o foco é mostrar um pouco do funcionamento do MSBuild. <a href="https://github.com/vintem/MSBuildDemo" target="_blank">O código da aplicação está disponível aqui.</a>

<strong>Pré-requisitos</strong>

O MSBuild é instalado junto com o Visual Studio, mas existem algumas coisas que você vai precisar para rodar esse demo. Espero que você já tenha o <a href="http://www.nunit.org/index.php?p=download" target="_blank">NUnit</a> instalado, eu estou usando a versão 2.5.3. Além do NUnit você vai precisar:
<ul>
	<li><a href="http://www.ncover.com/download/file?filename=NCover-1.5.8.zip" target="_blank">NCover</a> - eles pedem um cadastro e existem versões pagas, mas a versão 1.5.8 é gratuita e serve para os nossos propósitos.</li>
	<li><a href="http://www.kiwidude.com/dotnet/NCoverExplorer-1.4.0.7.zip" target="_blank">NCoverExplorer</a> - para gerar um relatório mais amigável do NCover.</li>
	<li><a href="http://www.kiwidude.com/dotnet/NCoverExplorer.Extras-1.4.0.5.zip" target="_blank">NCoverExplorer Extras</a> - possui tarefas customizadas para o MSBuild executar o NCoverExplorer.</li>
	<li><a href="http://msbuildtasks.tigris.org/" target="_blank">MSBuild Community Tasks</a> - tarefas customizadas feitas para o MSBuild.</li>
</ul>
O NUnit, NCover e o MSBuild Community Tasks possuem instaladores próprios.O NCoverExplorer e o NCoverExplorer Extras são um pouco diferentes. O NCoverExplorer não precisa de instalação, por isso eu já inclui dentro do zip com o código, na pasta tools.

Quando você baixar o NCoverExplorer Extras terá uma pasta zip e dentro dela uma dll chamada NCoverExplorer.MSBuildTasks.dll, eu, apenas por uma questão de organização criei uma pasta c:Program FilesNCoverBuild Task Plugins e copiei essa dll lá dentro.

Pronto, isso é tudo o que é necessário para rodar o MSBuild.

<strong>Entenda o arquivo do MSBuild</strong>

Depois de baixar, olhar o código e os testes unitários abra o arquivo MSBuildDemo.proj.

<a href="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_01%20Jun.%2011%2018.09_2.gif"><img style="border-width: 0px; display: block; float: none; margin-left: auto; margin-right: auto;" title="ScreenHunter_01 Jun. 11 18.09" src="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_01%20Jun.%2011%2018.09_thumb.gif" alt="ScreenHunter_01 Jun. 11 18.09" width="217" height="145" border="0" /></a>

O arquivo MSBuildDemo.proj nada mais é do que um arquivo xml. Então vamos começar a entendê-lo.
<pre class="brush: xml;">&lt;Project DefaultTargets="Build"
ToolsVersion="3.5"
InitialTargets="BuscaProjetos"
xmlns="http://schemas.microsoft.com/developer/msbuild/2003"&gt;

&lt;!--
Baixar MSBuildCommunity tasks de http://msbuildtasks.tigris.org/
--&gt;
&lt;Import Project="$(MSBuildExtensionsPath)\MSBuildCommunityTasks\MSBuild.Community.Tasks.Targets"/&gt;

&lt;PropertyGroup&gt;
&lt;SolutionName&gt;MSBuildDemo.sln&lt;/SolutionName&gt;
&lt;CodeFolder&gt;$(MSBuildProjectDirectory)&lt;/CodeFolder&gt;&lt;!-- Variavel do MSBuild com diretorio do projeto --&gt;
&lt;TestFolder&gt;$(MSBuildProjectDirectory)\tests\UnitTests&lt;/TestFolder&gt;
&lt;ResultsFolder&gt;$(MSBuildProjectDirectory)\Results&lt;/ResultsFolder&gt;
&lt;Configuration Condition=" '$(Configuration)' == '' "&gt;Debug&lt;/Configuration&gt;
&lt;Platform Condition=" '$(Platform)' == '' "&gt;AnyCPU&lt;/Platform&gt;
&lt;/PropertyGroup&gt;

&lt;PropertyGroup&gt;
&lt;CaminhoNUnit&gt;C:\Program Files\NUnit 2.5.3\bin\net-2.0\&lt;/CaminhoNUnit&gt;
&lt;ComandoNUnit&gt;$(CaminhoNUnit)nunit-console.exe&lt;/ComandoNUnit&gt;
&lt;ArquivoNUnit&gt;ResultadosTestes.xml&lt;/ArquivoNUnit&gt;
&lt;CaminhoNCover&gt;C:\Program Files\NCover&lt;/CaminhoNCover&gt;
&lt;ArquivoNCover&gt;ResultadoNCover.xml&lt;/ArquivoNCover&gt;
&lt;ArquivoLogNCover&gt;Coverage.log&lt;/ArquivoLogNCover&gt;
&lt;CaminhoNCoverExplorer&gt;$(MSBuildProjectDirectory)\tools\NCoverExplorer&lt;/CaminhoNCoverExplorer&gt;
&lt;/PropertyGroup&gt;</pre>
Primeiro, veja que na linha 9 apontamos para a nossa dll instalada pelo MSBuild Community Tasks.

O resto do código acima é bem fácil de entender, estamos definindo variáveis que vamos usar depois nas tarefas de deploy, algumas coisas que valem mencionar.
<ul>
	<li>Definição do arquivo .sln na linha 12</li>
	<li>Definição dos caminhos das ferramentas no bloco que vai da linha 20 a 28</li>
</ul>
Agora precisamos identificar os projetos da solução que serão compilados e os projetos de testes, faremos assim:
<pre class="brush: xml;">&lt;Target Name="BuscaProjetos"&gt;

&lt;!-- Busca todos os projetos da solucao --&gt;
&lt;GetSolutionProjects Solution="$(CodeFolder)\$(SolutionName)"&gt;
&lt;Output TaskParameter="Output" ItemName="ProjetosSolucao" /&gt;
&lt;/GetSolutionProjects&gt;

&lt;!-- Busca os projetos de Testes --&gt;
&lt;RegexMatch Input="@(ProjetosSolucao)" Expression=".*?[\.]?(Test[s]{0,1}|IntegrationTest)[\.]csproj$"&gt;
&lt;Output TaskParameter="Output" ItemName="ProjetosTeste"/&gt;
&lt;/RegexMatch&gt;

&lt;!-- Separa somente os projetos da solução sem os testes --&gt;
&lt;CreateItem Include="@(ProjetosSolucao)"
Exclude="@(ProjetosTeste)"&gt;
&lt;Output TaskParameter="Include" ItemName="CodeProjects"/&gt;
&lt;/CreateItem&gt;

&lt;/Target&gt;</pre>
Vale mencionar aqui que a propriedade ItemName de cada tag Output é uma váriavel criada que pode ser utilizada depois. Basicamente estamos buscando todos os projetos da solução, usando uma Expressão Regular para encontrar os Projetos que tenham a palavra Test ou IntegrationTest para encontrar os projetos de testes e separando os dois tipos de projetos. Agora vamos compilar os projetos que não são testes.
<pre class="brush: xml;">&lt;Target Name="Build"&gt;

&lt;Message Text="#--------- Compilando Projetos ---------#" /&gt;

&lt;!-- Compila os assemblies --&gt;
&lt;MSBuild Projects="@(CodeProjects)"
Targets="$(BuildTargets)"
Properties="Configuration=$(Configuration);Platform=$(Platform)"&gt;
&lt;Output TaskParameter="TargetOutputs"
ItemName="CodeAssemblies"/&gt;
&lt;/MSBuild&gt;
&lt;/Target&gt;</pre>
E agora compilamos os projetos de Testes

&lt;Target Name="BuildTests" DependsOnTargets="Build"&gt;

&lt;Message Text="#--------- Compilando Testes ---------#" /&gt;

&lt;MSBuild Projects="@(ProjetosTeste)"
Targets="$(BuildTargets)"
Properties="Configuration=$(Configuration);Platform=$(Platform)"&gt;
&lt;Output TaskParameter="TargetOutputs"
ItemName="AssembliesTeste"/&gt;
&lt;/MSBuild&gt;
&lt;/Target&gt;</pre>

E a última etapa é rodar o NCover e o NCoverExplorer

<pre class="brush: xml;">&lt;Target Name="Tests" DependsOnTargets="BuildTests"&gt;
&lt;Message Text="#--------- Executando NUnit ---------#" /&gt;

&lt;NUnit Assemblies="@(AssembliesTeste)"
ToolPath="$(CaminhoNUnit)"
WorkingDirectory="%(AssembliesTeste.RootDir)%(AssembliesTeste.Directory)"
OutputXmlFile="@(AssembliesTeste-&gt;'%(FullPath).$(ArquivoNUnit)')"
ContinueOnError="true"&gt;
&lt;Output TaskParameter="ExitCode" ItemName="NUnitExitCodes"/&gt;
&lt;/NUnit&gt;
&lt;/Target&gt;

&lt;UsingTask TaskName="NCoverExplorer.MSBuildTasks.NCover" AssemblyFile="$(CaminhoNCover)\Build Task Plugins\NCoverExplorer.MSBuildTasks.dll" /&gt;
&lt;UsingTask TaskName="NCoverExplorer.MSBuildTasks.NCoverExplorer" AssemblyFile="$(CaminhoNCover)\Build Task Plugins\NCoverExplorer.MSBuildTasks.dll" /&gt;

&lt;Target Name="CoberturaTestes" DependsOnTargets="Tests"&gt;
&lt;Message Text="#--------- Executando NCover ---------#" /&gt;

&lt;MakeDir Directories="$(ResultsFolder)" Condition="!Exists('$(ResultsFolder)')" /&gt;

&lt;NCover ToolPath="$(CaminhoNCover)"
CommandLineExe="$(ComandoNUnit)"
CommandLineArgs="$(TestFolder)\bin\$(Configuration)\UnitTests.dll"
WorkingDirectory="$(TestFolder)\bin\$(Configuration)"
CoverageFile="$(ResultsFolder)\$(ArquivoNCover)"
LogFile="$(ArquivoLogNCover)"
ExcludeAttributes="MSBuildDemo.Common.CoverageExcludeAttribute"
AssemblyList="@(CodeProjects-&gt;'%(FileName)')" /&gt;

&lt;!-- Resumo da Cobertura --&gt;
&lt;NCoverExplorer ToolPath="$(CaminhoNCoverExplorer)"
ProjectName="$(ProjectName)"
OutputDir="$(ResultsFolder)"
CoverageFiles="$(ResultsFolder)\$(ArquivoNCover)"
SatisfactoryCoverage="80"
ReportType="3"
XmlReportName="ResumoCobertura.xml"
HtmlReportName="ResumoCobertura.html" /&gt;
&lt;NCoverExplorer ToolPath="$(CaminhoNCoverExplorer)"
ProjectName="$(ProjectName)"
OutputDir="$(ResultsFolder)"
CoverageFiles="$(ResultsFolder)\$(ArquivoNCover)"
SatisfactoryCoverage="80"
ReportType="4"
XmlReportName="RelatorioCoberturaPorClasse.xml"
HtmlReportName="RelatorioCoberturaPorClasse.html" /&gt;
&lt;/Target&gt;</pre>

No código acima mandamos gerar dois relatórios e, entre outras coisas, definimos que queremos que nosso código tenha 80% de cobertura. Pronto, temos aqui um script para compilar, executar os testes e a cobertura de testes. Agora, como rodamos isso? Abra o Visual Studio Command Prompt, pra quem não sabe, veja onde ele está: <a href="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_02%20Jun.%2011%2018.53_2.gif"><img style="border-width: 0px; display: block; float: none; margin-left: auto; margin-right: auto;" title="ScreenHunter_02 Jun. 11 18.53" src="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_02%20Jun.%2011%2018.53_thumb.gif" alt="ScreenHunter_02 Jun. 11 18.53" width="770" height="66" border="0" /></a> Vá até a pasta onde está o arquivo MSBuildDemo.proj e digite MSBuild MSBuildDemo.proj Se você executar isso, os projetos serão apenas compilados porque no início do script definimos como atividade padrão a target Build.
<pre style="background-color: #dadada; margin: 0em; width: 100%; font-family: consolas,'Courier New',courier,monospace; font-size: 12px;">  1: DefaultTargets="Build"</pre>
Targets, são tarefas ou conjuntos de tarefas que podem ser definidas. Queremos executar a tarefa CoberturaTestes, porque se você prestar atenção no script ela depende da tarefa Tests que por sua vez depende de BuildTests que depende de Build, ou seja executando CoberturaTestes todo o script será executado, então rode o comando:

MSBuild MSBuildDemo.proj /target:CoberturaTestes

Se tudo der certo e sob as condições ideais de temperatura você deve ver algo como:

<a href="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_03%20Jun.%2011%2019.01_2.gif"><img style="border-width: 0px; display: block; float: none; margin-left: auto; margin-right: auto;" title="ScreenHunter_03 Jun. 11 19.01" src="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_03%20Jun.%2011%2019.01_thumb.gif" alt="ScreenHunter_03 Jun. 11 19.01" width="669" height="328" border="0" /></a>

Além disso, de acordo com nosso script foi criada uma pasta dentro do diretório da solução chamada Results, ali você pode ver o resultado da cobertura de testes. Abrindo o arquivo RelatorioCobertura.html veremos:

<a href="http://templecoding.com/wp-content/uploads/2010/06/image_2.png"><img style="border-width: 0px; display: block; float: none; margin-left: auto; margin-right: auto;" title="image" src="http://templecoding.com/wp-content/uploads/2010/06/image_thumb.png" alt="image" width="609" height="516" border="0" /></a>

Além disso, se você olhar um pouco mais acima na tela do prompt verá o resultado da execução dos testes unitários:

<a href="http://templecoding.com/wp-content/uploads/2010/06/image_4.png"><img style="border-width: 0px; display: block; float: none; margin-left: auto; margin-right: auto;" title="image" src="http://templecoding.com/wp-content/uploads/2010/06/image_thumb_1.png" alt="image" width="630" height="96" border="0" /></a>

Pronto, seus testes unitários são sempre executados e você já sabe quanto falta para testar seu código.
