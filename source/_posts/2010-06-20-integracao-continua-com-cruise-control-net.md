---
layout: post
title: Integração Contínua com Cruise Control .NET
categories:
- Desenvolvimento
tags:
- cruise control
- desenvolvimento
- integracao continua
- msbuild
- nunit
- programacao
status: publish
type: post
published: true
meta:
  _yoast_wpseo_metadesc: Como configurar sua aplicaçao para fazer Integração Contínua
    com Cruise Control .NET
  _yoast_wpseo_meta-robots-noindex: '0'
  _yoast_wpseo_title: Integração Contínua com Cruise Control .NET - Temple Coding
  dsq_thread_id: '159336075'
  _edit_last: '2'
  _yoast_wpseo_focuskw: Integração Contínua com Cruise Control
  _wp_old_slug: integrao-contnua-com-cruise-control-net
  _yoast_wpseo_linkdex: '83'
  _yoast_wpseo_meta-robots-nofollow: '0'
  _yoast_wpseo_meta-robots-adv: none
  _yoast_wpseo_sitemap-include: ! '-'
  _yoast_wpseo_sitemap-prio: ! '-'
  _yoast_wpseo_canonical: ''
  _yoast_wpseo_redirect: ''
  _yoast_wpseo_opengraph-description: ''
  _yoast_wpseo_google-plus-description: ''
  _oembed_a25a70a1b94bd079e3330406e40f77c4: ! '{{unknown}}'
  _clicky_goal: a:2:{s:2:"id";s:0:"";s:5:"value";s:0:"";}
  qtrans_meta:title: <!--:pt-->Integração Contínua com Cruise Control .NET | Temple
    Coding<!--:-->
  qtrans_meta:keywords: <!--:pt-->Integração Contínua Cruise Control .NET<!--:-->
  qtrans_meta:description: <!--:pt-->Como configurar sua aplicaçao para fazer Integração
    Contínua com Cruise Control .NET<!--:-->
  _wpas_skip_2089817: '1'
  _wpas_skip_2089821: '1'
  _wpas_skip_2089827: '1'
  _qts_slug_pt: integracao-continua-com-cruise-control-net
  _qts_slug_en: integracao-continua-com-cruise-control-net
  _yoast_wpseo_metakeywords: ''
  _oembed_8dd9c37422fa29d2f3e836284f2dafeb: ! '{{unknown}}'
---
<!--:pt--><a href="http://templecoding.com/2010/06/14/builds-automticos-com-msbuild-nunit-e-ncover/" target="_blank">Anteriormente eu falei sobre builds automáticos com o MSBuild</a>, bem, se você levar ao pé da letra, os builds feitos de acordo com aquele post não eram exatamente automáticos, o que acontecia de forma automática no momento em que o build era executado eram os testes unitários e a análise de cobertura de código feita pelos testes unitários. Agora, eu vou falar sobre como fazer a integração contínua com Cruise Control .NET.

<strong>Sobre Integração Contínua</strong>

Eu não vou parar e explicar aqui sobre integração contínua, acredito que uma ótima fonte é o texto do <a href="http://martinfowler.com/" target="_blank">Martin Fowler</a> traduzido pelo <a href="http://www.blogdopedro.net/" target="_blank">Pedro Mendes</a> e que pode <a href="http://www.blogdopedro.net/2009/03/04/traducao-do-artigo-sobre-integracao-continua" target="_blank">ser encontrado aqui</a>.

Apenas como um resumo: integração contínua é uma prática, ou melhor, um conjunto de práticas que visa, entre outras coisas, detectar problemas no código o mais rápido possível para que esse código se <em>integre </em>a um repositório de código compartilhado por outros desenvolvedores.

Algumas dessas práticas são:
<ul>
	<li>Possuir um repositório central para o código</li>
	<li>Realização de builds automáticos</li>
	<li>Check-in ou commit do código frequentemente</li>
	<li>Testes executados de forma automática</li>
	<li>Que todos os participantes tenham uma visualização do código</li>
</ul>
Como disse, esse é apenas um resumo, eu recomendo fortemente que você leia o texto indicado acima.

<strong>O Cruise Control .NET</strong>

O Cruise Control .NET é uma ferramenta open source para implementação de integração contínua. Obviamente não é única, mas é uma das mais conhecidas no mercado juntamente com as irmãs Cruise Control (feita em java) e Cruise Control rb (feita em ruby). Segue aqui um <a href="http://sourceforge.net/projects/ccnet/files/CruiseControl.NET%20Releases/" target="_blank">link para baixar o Cruise Control .NET.</a>

A instalação é bem simples, as únicas observações são que após a instalação deve-se definir uma senha para o administrador do dashboard, para isso vá até a pasta onde está instalado o Cruise Control NETwebdashboard e abra o arquivo dashboard.config e altere a configuração com a senha escolhida:
<pre class="brush: xml;"></pre>
Além disso é preciso dar permissão de escrita no arquivo dashboard.config e na pasta xls para o usuário IUSR_XXX, onde XXX é o nome da máquina onde você está realizando a instalação do Cruise Control. Após a instalação do Cruise Control, um serviço é adicionado ao windows que precisa ser iniciado. <strong>Configurando o Cruise Control .NET</strong> Antes de colocar um projeto CCNet vamos configurá-lo para exibir os dados que gostaríamos que são os dados do MSBuild, do NUnit e do NCover. Se você fez uma instalação padrão deve acessar o dashboard pelo endereço <a href="http://localhost/ccnet">http://localhost/ccnet</a> <a href="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_01%20Jun.%2019%2021.33_2.gif"><img style="display: block; margin-left: auto; margin-right: auto; border: 0px none;" title="Dashboard do Cruise Control" src="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_01%20Jun.%2019%2021.33_thumb.gif" alt="Dashboard do Cruise Control" width="215" height="250" border="0" /></a> Clique em Administer Dashboard, entre com a senha configurada para o administrador e você deve ver uma lista com os Packages disponíveis. Instale os 3 que vamos usar clicando em cada um deles, MSBuild Results, NCover Results e NUnit Results. <strong>Configurando um novo projeto no Cruise Control</strong> Configurar um projeto no Cruise Control é editar um arquivo XML, não é muito amigável mas é de certa forma bem simples. O arquivo que vamos editar é o ccnet.config que fica na pasta server onde foi instalado o CCNet. <a href="http://vintem.com.br/archive/2010/06/14/builds-automaacuteticos-com-msbuild-nunit-e-ncover.aspx" target="_blank">Estou usando o projeto do post anterior sobre o MSBuild</a> e configurando ele no CCNet. Primeiro, estou assumindo que você tem o seu projeto em um repósitorio, no meu caso, estou usando o Subversion, mas o CCNet suporta diversos. Veja como ficou configurado o meu arquivo ccnet.config:
<pre class="brush: xml;">    
        MSBuildDemo

        c:CCNetWorkingMSBuildDemo
        c:CCNetArtifactsMSBuildDemo
        2

            http://localhost:8181/svn/blog/MSBuildDemo

                C:WINDOWSMicrosoft.NETFrameworkv3.5MSBuild.exe
                MSBuildDemo.proj
                /p:Configuration=Debug /v:diag
                CoberturaTestestargets&gt;
                C:Program FilesCruiseControl.NETserverThoughtWorks.CruiseControl.MsBuild.dll

                    c:CCNetArtifactsMSBuildDemo*ResultadosTestes.xml
                    c:CCNetArtifactsMSBuildDemo*ResumoCobertura.xml</pre>
Vamos por partes.

Na linha 3 definimos o nome do projeto.

Na linha 4 definimos o que o CCNet usará para determinar se deve ou não executar as atividades do projeto, no caso do nosso exemplo, o CCNet irá, em um intervalo de tempo, verificar se existe alguma modificação no repositório, e se houver, executar as atividades.

Na linha 7 definimos uma pasta onde o CCNet vai baixar os arquivos do repositório e compilar os projetos.

Na linha 8 definimos uma pasta onde o CCNet irá salvar os arquivos que serão gerados como produto da compilação, como os relatórios do NUnit e do NCover.

Das linhas 10 a 12 definimos os dados do repositório, nesse caso o subversion.

Das linhas 13 a 21 definimos as tarefas, nesse caso, que a compilação será executada pelo MSBuild, onde está o executável do MSBuild, o script do MSBuild que deve ser executado, qual o target do MSBuild e logger que será usado para as atividades do MSBuild.

Por último das linhas 22 a 28 definimos quais arquivos gerados pelo script do MSBuild devem ser usados para gerar os relatórios do CCNet.

Se der tudo certo, ao acessar o dashboard você deve ver algo como:

<a href="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_02%20Jun.%2019%2021.36_2.gif"><img style="display: block; margin-left: auto; margin-right: auto; border: 0px none;" title="Status do build" src="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_02%20Jun.%2019%2021.36_thumb.gif" alt="Status do build" width="622" height="86" border="0" /></a>

Clique em MSBuildDemo e você deverá ver as compilações já feitas, e pode clicar nas compilações e ver os detalhes. O importante é que a partir de agora a cada vez que for realizado um novo check-in o CCNet irá perceber que existe uma alteração e executará o script do MSBuild que definimos novamente.

<strong>Alterando o script do MSBuild</strong>

Se você está seguindo o exemplo feito no post anterior, precisamos fazer algumas alterações no script do MSBuild. Precisamos apenas alterar o script para que salve os arquivos do NUnit e do NCover na pasta de artefatos definida no XML que configuramos o projeto do CCNet. Quanto um script do MSBuild é executado pelo CCNet uma variável CCNetArtifactDirectory é definida com o caminho da pasta de artefatos então a tarefa do NCover ficou assim:
<pre class="brush: xml;"></pre>
E as tarefas do NCover Explorer ficaram assim:
<pre class="brush: xml;"></pre>
E as tarefas do NUnit foram alteradas assim:
<pre class="brush: xml;"></pre>
Um detalhe importante é que os nomes dos arquivos gerados pelo NUnit e pelo NCover devem ser iguais aos nomes dos arquivos dentro da tag files no projeto configurado dentro do ccnet.config. <strong>Utilizando o CCTray</strong> O CCTray é um aplicativo que instalamos e ele fica constantemente consultando o servidor do CCNet e nos avisa se algum build ou teste unitário foi quebrado. No dashboard do CCNet tem um link para fazer download do CCTray e novamente a instalação é muito simples. <img style="display: block; margin-left: auto; margin-right: auto; border: 0px none;" title="Download CCTray" src="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_01%20Jun.%2019%2021.33_thumb.gif" alt="Download CCTray" width="215" height="250" border="0" /> A configuração dele também é simples, após instalado, vá em File -&gt; Settings -&gt; Build Projects clique em Add para e depois em Add Server, aponte para o endereço onde está instalado o servidor depois em OK, agora é só selecionar o seu projeto. <strong>Customizando os relatórios do NCover Explorer</strong> Quando você baixa o NCover Explorer Extras, dentro do zip existe uma pasta Cruise Control .NET, copie os arquivos .xls que estão nessa pasta para a pasta webdashboard/xls onde foi instalado o seu CCNet. Depois a abra o arquivo dashboard.config e edite para que fique assim:
<pre class="brush: xml;">...

    ...
    xslNCoverExplorerSummary.xslxslFile&gt;
    ...</pre>
Entre no dashboard e clique em Force Build, depois que o build estiver completo e você entrar pelo dashboard nesse último build, você deverá ver um relatório como o abaixo:

<a href="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_02%20Jun.%2020%2015.28_2.gif"><img style="display: block; margin-left: auto; margin-right: auto; border: 0px none;" title="Relatorio de testes" src="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_02%20Jun.%2020%2015.28_thumb.gif" alt="Relatorio de testes" width="644" height="463" border="0" /></a>

<strong>Conclusão</strong>

Pronto, agora sim você tem um projeto que é compilado automaticamente a cada check-in/commit, os testes unitários são executados também de forma automática e se por acaso o projeto parar de compilar ou algum teste unitário falhar o CCTray avisará com uma mensagem e inclusive fará o trabalho de dedo duro dizendo quem foi o usuário que fez o último commit com a falha.

O ideal é que todos do projeto tenham o CCTray e que a primeira, ou uma das primeiras atividades seja configurar o script para compilação automática.

Com isso você pode ter mais certeza, a cada commit, que seu código não irá causar problemas a outros e se causar, você terá uma resposta imediata, o que torna a correção do problema muito mais fácil.<!--:--><!--:en--><p><a href="http://templecoding.com/2010/06/14/builds-automticos-com-msbuild-nunit-e-ncover/" target="_blank">Anteriormente eu falei sobre builds automáticos com o MSBuild</a>, bem, se você levar ao pé da letra, os builds feitos de acordo com aquele post não eram exatamente automáticos, o que acontecia de forma automática no momento em que o build era executado eram os testes unitários e a análise de cobertura de código feita pelos testes unitários. Agora, eu vou falar sobre como fazer a integração contínua com Cruise Control .NET.</p>
<p><strong>Sobre Integração Contínua</strong></p>
<p>Eu não vou parar e explicar aqui sobre integração contínua, acredito que uma ótima fonte é o texto do <a href="http://martinfowler.com/" target="_blank">Martin Fowler</a> traduzido pelo <a href="http://www.blogdopedro.net/" target="_blank">Pedro Mendes</a> e que pode <a href="http://www.blogdopedro.net/2009/03/04/traducao-do-artigo-sobre-integracao-continua" target="_blank">ser encontrado aqui</a>.</p>
<p>Apenas como um resumo: integração contínua é uma prática, ou melhor, um conjunto de práticas que visa, entre outras coisas, detectar problemas no código o mais rápido possível para que esse código se <em>integre </em>a um repositório de código compartilhado por outros desenvolvedores.</p>
<p>Algumas dessas práticas são:</p>
<ul>
<li>Possuir um repositório central para o código</li>
<li>Realização de builds automáticos</li>
<li>Check-in ou commit do código frequentemente</li>
<li>Testes executados de forma automática</li>
<li>Que todos os participantes tenham uma visualização do código</li>
</ul>
<p>Como disse, esse é apenas um resumo, eu recomendo fortemente que você leia o texto indicado acima.</p>
<p><strong>O Cruise Control .NET</strong></p>
<p>O Cruise Control .NET é uma ferramenta open source para implementação de integração contínua. Obviamente não é única, mas é uma das mais conhecidas no mercado juntamente com as irmãs Cruise Control (feita em java) e Cruise Control rb (feita em ruby). Segue aqui um <a href="http://sourceforge.net/projects/ccnet/files/CruiseControl.NET%20Releases/" target="_blank">link para baixar o Cruise Control .NET.</a></p>
<p>A instalação é bem simples, as únicas observações são que após a instalação deve-se definir uma senha para o administrador do dashboard, para isso vá até a pasta onde está instalado o Cruise Control NETwebdashboard e abra o arquivo dashboard.config e altere a configuração com a senha escolhida:</p>
<pre class="brush: xml;"><administrationPlugin password="" /></pre>
<p>Além disso é preciso dar permissão de escrita no arquivo dashboard.config e na pasta xls para o usuário IUSR_XXX, onde XXX é o nome da máquina onde você está realizando a instalação do Cruise Control. Após a instalação do Cruise Control, um serviço é adicionado ao windows que precisa ser iniciado. <strong>Configurando o Cruise Control .NET</strong> Antes de colocar um projeto CCNet vamos configurá-lo para exibir os dados que gostaríamos que são os dados do MSBuild, do NUnit e do NCover. Se você fez uma instalação padrão deve acessar o dashboard pelo endereço <a href="http://localhost/ccnet">http://localhost/ccnet</a> <a href="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_01%20Jun.%2019%2021.33_2.gif"><img style="display: block; margin-left: auto; margin-right: auto; border: 0px none;" title="Dashboard do Cruise Control" src="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_01%20Jun.%2019%2021.33_thumb.gif" alt="Dashboard do Cruise Control" width="215" height="250" border="0" /></a> Clique em Administer Dashboard, entre com a senha configurada para o administrador e você deve ver uma lista com os Packages disponíveis. Instale os 3 que vamos usar clicando em cada um deles, MSBuild Results, NCover Results e NUnit Results. <strong>Configurando um novo projeto no Cruise Control</strong> Configurar um projeto no Cruise Control é editar um arquivo XML, não é muito amigável mas é de certa forma bem simples. O arquivo que vamos editar é o ccnet.config que fica na pasta server onde foi instalado o CCNet. <a href="http://vintem.com.br/archive/2010/06/14/builds-automaacuteticos-com-msbuild-nunit-e-ncover.aspx" target="_blank">Estou usando o projeto do post anterior sobre o MSBuild</a> e configurando ele no CCNet. Primeiro, estou assumindo que você tem o seu projeto em um repósitorio, no meu caso, estou usando o Subversion, mas o CCNet suporta diversos. Veja como ficou configurado o meu arquivo ccnet.config:</p>
<pre class="brush: xml;"><cruisecontrol xmlns:cb="urn:ccnet.config.builder">
    <project>
        <name>MSBuildDemo</name>
        <triggers>
            <intervalTrigger />
        </triggers>
        <workingDirectory>c:CCNetWorkingMSBuildDemo</workingDirectory>
        <artifactDirectory>c:CCNetArtifactsMSBuildDemo</artifactDirectory>
        <modificationDelaySeconds>2</modificationDelaySeconds>
        <sourcecontrol type="svn">
            <trunkUrl>http://localhost:8181/svn/blog/MSBuildDemo</trunkUrl>
        </sourcecontrol>
        <tasks>
            <msbuild>
                <executable>C:WINDOWSMicrosoft.NETFrameworkv3.5MSBuild.exe</executable>
                <projectFile>MSBuildDemo.proj</projectFile>
                <buildArgs>/p:Configuration=Debug /v:diag</buildArgs>
                <targets>CoberturaTestestargets>
                <logger>C:Program FilesCruiseControl.NETserverThoughtWorks.CruiseControl.MsBuild.dll</logger>
            </msbuild>
        </tasks>
        <publishers>
            <merge>
                <files>
                    <file>c:CCNetArtifactsMSBuildDemo*ResultadosTestes.xml</file>
                    <file>c:CCNetArtifactsMSBuildDemo*ResumoCobertura.xml</file>
                </files>
            </merge>
            <xmllogger />
        </publishers>
    </project>
</cruisecontrol></pre>
<p>Vamos por partes.</p>
<p>Na linha 3 definimos o nome do projeto.</p>
<p>Na linha 4 definimos o que o CCNet usará para determinar se deve ou não executar as atividades do projeto, no caso do nosso exemplo, o CCNet irá, em um intervalo de tempo, verificar se existe alguma modificação no repositório, e se houver, executar as atividades.</p>
<p>Na linha 7 definimos uma pasta onde o CCNet vai baixar os arquivos do repositório e compilar os projetos.</p>
<p>Na linha 8 definimos uma pasta onde o CCNet irá salvar os arquivos que serão gerados como produto da compilação, como os relatórios do NUnit e do NCover.</p>
<p>Das linhas 10 a 12 definimos os dados do repositório, nesse caso o subversion.</p>
<p>Das linhas 13 a 21 definimos as tarefas, nesse caso, que a compilação será executada pelo MSBuild, onde está o executável do MSBuild, o script do MSBuild que deve ser executado, qual o target do MSBuild e logger que será usado para as atividades do MSBuild.</p>
<p>Por último das linhas 22 a 28 definimos quais arquivos gerados pelo script do MSBuild devem ser usados para gerar os relatórios do CCNet.</p>
<p>Se der tudo certo, ao acessar o dashboard você deve ver algo como:</p>
<p><a href="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_02%20Jun.%2019%2021.36_2.gif"><img style="display: block; margin-left: auto; margin-right: auto; border: 0px none;" title="Status do build" src="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_02%20Jun.%2019%2021.36_thumb.gif" alt="Status do build" width="622" height="86" border="0" /></a></p>
<p>Clique em MSBuildDemo e você deverá ver as compilações já feitas, e pode clicar nas compilações e ver os detalhes. O importante é que a partir de agora a cada vez que for realizado um novo check-in o CCNet irá perceber que existe uma alteração e executará o script do MSBuild que definimos novamente.</p>
<p><strong>Alterando o script do MSBuild</strong></p>
<p>Se você está seguindo o exemplo feito no post anterior, precisamos fazer algumas alterações no script do MSBuild. Precisamos apenas alterar o script para que salve os arquivos do NUnit e do NCover na pasta de artefatos definida no XML que configuramos o projeto do CCNet. Quanto um script do MSBuild é executado pelo CCNet uma variável CCNetArtifactDirectory é definida com o caminho da pasta de artefatos então a tarefa do NCover ficou assim:</p>
<pre class="brush: xml;"><ncover ToolPath="$(CaminhoNCover)"         CommandLineExe="$(ComandoNUnit)"         CommandLineArgs="$(TestFolder)bin$(Configuration)UnitTests.dll"         WorkingDirectory="$(TestFolder)bin$(Configuration)"         CoverageFile="$(CCNetArtifactDirectory)$(ArquivoNCover)"         LogFile="$(ArquivoLogNCover)"         ExcludeAttributes="MSBuildDemo.Common.CoverageExcludeAttribute"         AssemblyList="@(CodeProjects->'%(FileName)')" /></pre>
<p>E as tarefas do NCover Explorer ficaram assim:</p>
<pre class="brush: xml;"><ncoverExplorer ToolPath="$(CaminhoNCoverExplorer)"     ProjectName="$(ProjectName)"     OutputDir="$(CCNetArtifactDirectory)"     CoverageFiles="$(CCNetArtifactDirectory)$(ArquivoNCover)"     SatisfactoryCoverage="80"     ReportType="3"     XmlReportName="ResumoCobertura.xml"     HtmlReportName="ResumoCobertura.html" />
<ncoverExplorer ToolPath="$(CaminhoNCoverExplorer)"     ProjectName="$(ProjectName)"     OutputDir="$(CCNetArtifactDirectory)"     CoverageFiles="$(CCNetArtifactDirectory)$(ArquivoNCover)"     SatisfactoryCoverage="80"     ReportType="4"     XmlReportName="RelatorioCoberturaPorClasse.xml"     HtmlReportName="RelatorioCoberturaPorClasse.html" /></pre>
<p>E as tarefas do NUnit foram alteradas assim:</p>
<pre class="brush: xml;"><createItem Include="$(CCNetArtifactDirectory)*.$(ArquivoNUnit)">
    <output TaskParameter="Include" ItemName="ExistingNUnitResults"/>
</createItem>

<delete Files="@(ExistingNUnitResults)"/>

<copy SourceFiles="@(NUnitResults)"     DestinationFolder="$(CCNetArtifactDirectory)"     ContinueOnError="true"/></pre>
<p>Um detalhe importante é que os nomes dos arquivos gerados pelo NUnit e pelo NCover devem ser iguais aos nomes dos arquivos dentro da tag files no projeto configurado dentro do ccnet.config. <strong>Utilizando o CCTray</strong> O CCTray é um aplicativo que instalamos e ele fica constantemente consultando o servidor do CCNet e nos avisa se algum build ou teste unitário foi quebrado. No dashboard do CCNet tem um link para fazer download do CCTray e novamente a instalação é muito simples. <img style="display: block; margin-left: auto; margin-right: auto; border: 0px none;" title="Download CCTray" src="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_01%20Jun.%2019%2021.33_thumb.gif" alt="Download CCTray" width="215" height="250" border="0" /> A configuração dele também é simples, após instalado, vá em File -> Settings -> Build Projects clique em Add para e depois em Add Server, aponte para o endereço onde está instalado o servidor depois em OK, agora é só selecionar o seu projeto. <strong>Customizando os relatórios do NCover Explorer</strong> Quando você baixa o NCover Explorer Extras, dentro do zip existe uma pasta Cruise Control .NET, copie os arquivos .xls que estão nessa pasta para a pasta webdashboard/xls onde foi instalado o seu CCNet. Depois a abra o arquivo dashboard.config e edite para que fique assim:</p>
<pre class="brush: xml;">...
<xslFileNames>
    ...
    <xslFile>xslNCoverExplorerSummary.xslxslFile>
    ...
</xslFileNames>
<xslReportBuildPlugin description="NCover Report" actionName="NCoverBuildReport" xslFileName="xslNCoverExplorer.xsl" /></pre>
<p>Entre no dashboard e clique em Force Build, depois que o build estiver completo e você entrar pelo dashboard nesse último build, você deverá ver um relatório como o abaixo:</p>
<p><a href="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_02%20Jun.%2020%2015.28_2.gif"><img style="display: block; margin-left: auto; margin-right: auto; border: 0px none;" title="Relatorio de testes" src="http://templecoding.com/wp-content/uploads/2010/06/ScreenHunter_02%20Jun.%2020%2015.28_thumb.gif" alt="Relatorio de testes" width="644" height="463" border="0" /></a></p>
<p><strong>Conclusão</strong></p>
<p>Pronto, agora sim você tem um projeto que é compilado automaticamente a cada check-in/commit, os testes unitários são executados também de forma automática e se por acaso o projeto parar de compilar ou algum teste unitário falhar o CCTray avisará com uma mensagem e inclusive fará o trabalho de dedo duro dizendo quem foi o usuário que fez o último commit com a falha.</p>
<p>O ideal é que todos do projeto tenham o CCTray e que a primeira, ou uma das primeiras atividades seja configurar o script para compilação automática.</p>
<p>Com isso você pode ter mais certeza, a cada commit, que seu código não irá causar problemas a outros e se causar, você terá uma resposta imediata, o que torna a correção do problema muito mais fácil.</p>
<!--:-->
