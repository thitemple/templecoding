---
layout: post
title: Testes unitários no BizTalk com o BizUnit
categories:
- Desenvolvimento
tags:
- biztalk
- bizunit
- testes
status: publish
type: post
published: true
meta:
  _edit_last: '2'
  _revision-control: a:1:{i:0;s:8:"defaults";}
  dsq_thread_id: '715871781'
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
Hoje vou falar um pouco sobre como fazer "testes unitários" no BizTalk. Tenho que usar as aspas pra dizer testes unitários porque o BizTalk, sendo uma ferramenta de integração, só podemos fazer testes integrados. Mas a unidade nesse caso, pode ser definida como sendo cada integração diferente.

O <a href="https://github.com/vintem/OrderProcessing">código está disponivel no github</a>.

Vou começar de uma forma bem simples, apenas apresentando o BizUnit. Primeiro a definição como diz no site do próprio BizUnit:
<blockquote>BizUnit enables automated tests to be rapidly developed. BizUnit is a flexible and extensible declarative test framework targeted that rapidly enables the automated testing of distributed systems, for example it is widely used to test BizTalk solutions. BizUnit is fully extensible. Its approach is to enable test cases to be constructed from generic reusable test steps, test cases are defined in XML which allows them to be auto-generated and also enables the ‘fixing up’ of Url’s for different environments, e.g. test, staging and production environments. Defining test cases in XML enables test cases to be auto-generated.</blockquote>
A primeira coisa que preciso dizer, é que apesar do BizUnit funcionar, parece ser um projeto open source meio parado, quero dizer, não encontrei (até o momento desse post) nem o código fonte disponivel para baixa-lo. E como a maioria dos projetos open source, a documentação não é o forte. Por exemplo, já na definição do framework diz que podemos definir os casos de testes em arquivos XML, isso é verdade, mas também podemos fazer isso em código, no C#, como qualquer outro teste unitário. E é isso que vou fazer nesse exemplo.

O cenário para esse teste será bem simples, um pedido terá origem como um arquivo XML em uma pasta e caso ele tenha um valor total maior que 1000 será direcionado para uma pasta de pedidos especiais, senão, ele ira para uma pasta de pedidos regulares.

<a href="http://templecoding.com/wp-content/uploads/2012/06/fluxo_pedido1.png"><img class="aligncenter size-full wp-image-443" title="Fluxo de um pedido" src="http://templecoding.com/wp-content/uploads/2012/06/fluxo_pedido1.png" alt="" width="626" height="302" /></a>

O que queremos com um teste unitário para esse caso, é que ele possa ser executado diversas vezes de uma forma simples e verificável.

O nosso esquema XML também vai manter a simplicidade:
<pre>	00001
	Thiago Temple
	2012-06-05T00:00:00
	600</pre>
Para o projeto no BizTalk apenas vou gerar o Schema para essa mensagem, gerar um Distinguished Field no campo Total para poder fazer o roteamento na própria porta.

Veja como ficou a solução:

<a href="http://templecoding.com/wp-content/uploads/2012/06/visual_studio_solution1.png"><img class="aligncenter size-full wp-image-448" title="visual_studio_solution" src="http://templecoding.com/wp-content/uploads/2012/06/visual_studio_solution1.png" alt="" width="282" height="251" /></a>

E os filtros nas portas:

<a href="http://templecoding.com/wp-content/uploads/2012/06/Send_standard_port_filter1.png"><img class="aligncenter size-medium wp-image-449" title="Send_File_StandardOrder_Filter" src="http://templecoding.com/wp-content/uploads/2012/06/Send_standard_port_filter1-300x258.png" alt="" width="300" height="258" />
</a><a href="http://templecoding.com/wp-content/uploads/2012/06/Send_special_port_filter1.png"><img class="aligncenter size-medium wp-image-450" title="Send_File_SpecialOrder_Filter" src="http://templecoding.com/wp-content/uploads/2012/06/Send_special_port_filter1-300x260.png" alt="" width="300" height="260" /></a>

Com tudo explicado, vamos ao teste. O que vamos verificar nesse teste é que ao salvar um arquivo na pasta de entrada, o mesmo deverá ser roteado para a pasta correta. Portanto criei dois arquivos com dados de teste, um com valor maior de 1000 chamado SpecialOrder.xml e outro com um valor menor de 1000 chamado StandardOrder.xml.

Para o teste estou usando o MSTest mas também é possivel usar o NUnit facilmente.

Começamos com a classe de teste para o caso regular que chamei de ProcessingStandardOrder.
<pre class="brush: csharp;">[TestClass]
    public class ProcessingStandardOrder
    {
        [TestMethod]
        public void StandardOrderShouldBeSavedInCorrectFolder()
        {
            var testCase = new TestCase { Name = "Test that standard order is saved in correct folder" };

            testCase.SetupSteps.Add(DeleteFilesStep());

            testCase.ExecutionSteps.Add(CreateFileStep());

            testCase.ExecutionSteps.Add(ValidateFileExits());

            var bizUnit = new BizUnit.BizUnit(testCase);
            bizUnit.RunTest();
        }

        private TestStepBase ValidateFileExits()
        {
            var validateFileExistsSTep = new FileReadMultipleStep
            {
                DirectoryPath = @"C:Biztalk InOutOrderProcessingStandard",
                SearchPattern = "*.xml",
                ExpectedNumberOfFiles = 1,
                Timeout = 3000,
                DeleteFiles = true
            };
            return validateFileExistsSTep;
        }

        private static CreateStep CreateFileStep()
        {
            var createImportTestFile = new CreateStep
            {
                CreationPath = @"C:Biztalk InOutOrderProcessingInStandardOrder.xml",
                DataSource = new FileDataLoader { FilePath = @"......OrderProcessing.TestsTestDataStandardOrder.xml" }
            };
            return createImportTestFile;
        }

        private static DeleteStep DeleteFilesStep()
        {
            var deleteFiles = new DeleteStep
            {
                FilePathsToDelete = new Collection { @"C:Biztalk InOutOrderProcessingIn*.xml", @"C:Biztalk InOutOrderProcessingStandard*.xml" }
            };
            return deleteFiles;
        }
    }</pre>
O codigo é bem simples. O importante é saber que o BizUnit sempre vai executar os SetupSteps antes dos ExecutionSteps e depois ainda vai executar todos os CleanupSteps. Criei um TestCase, limpei o diretório de entrada e de destino, copiei o meu arquivo com os dados de teste e verifiquei que o mesmo estava na pasta esperada.

O importante é notar o funcionamento do BizUnit. É preciso criar um TestCase e criar um BizUnit passando o teste. Veja também que no método ValidateFileExists eu adicionei um Timeout de 3000 milisegundos, isso é importante porque precisamos esperar que o arquivo seja processado pelo BizTalk, caso contrário é possivel que o teste rode antes que o BizTalk processe o arquivo e então o teste falhará.

Por fim a classe que testa o caso especial ProcessingSpecialOrder:
<pre class="brush: csharp;">[TestClass]
    public class ProcessingSpecialOrder
    {
        [TestMethod]
        public void SpecialOrderShouldBeSavedInCorrectFolder()
        {
            var testCase = new TestCase { Name = "Test that special order is saved in correct folder" };

            testCase.SetupSteps.Add(DeleteFilesStep());

            testCase.ExecutionSteps.Add(CreateFileStep());

            testCase.ExecutionSteps.Add(ValidateFileExits());

            var bizUnit = new BizUnit.BizUnit(testCase);
            bizUnit.RunTest();
        }

        private TestStepBase ValidateFileExits()
        {
            var validateFileExistsSTep = new FileReadMultipleStep
            {
                DirectoryPath = @"C:Biztalk InOutOrderProcessingSpecial",
                SearchPattern = "*.xml",
                ExpectedNumberOfFiles = 1,
                Timeout = 3000,
                DeleteFiles = true
            };
            return validateFileExistsSTep;
        }

        private static CreateStep CreateFileStep()
        {
            var createImportTestFile = new CreateStep
            {
                CreationPath = @"C:Biztalk InOutOrderProcessingInSpecialOrder.xml",
                DataSource = new FileDataLoader { FilePath = @"......OrderProcessing.TestsTestDataSpecialOrder.xml" }
            };
            return createImportTestFile;
        }

        private static DeleteStep DeleteFilesStep()
        {
            var deleteFiles = new DeleteStep
            {
                FilePathsToDelete = new Collection { @"C:Biztalk InOutOrderProcessingIn*.xml", @"C:Biztalk InOutOrderProcessingSpecial*.xml" }
            };
            return deleteFiles;
        }
    }</pre>
É isso, lembrando que o <a href="https://github.com/vintem/OrderProcessing">código está disponivel no github</a>.
