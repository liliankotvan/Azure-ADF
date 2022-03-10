# Azure-ADF-FileNameFilter

## (pt-br) Filtro de arquivos no Azure DataFactory

### Visão Geral  ![image](https://user-images.githubusercontent.com/45773133/148269556-bd635dd8-26f5-4f4a-bcb2-bc909e020ad6.png)

Realizar uma atividade de cópia de dados de arquivos .csv localizados em um Azure Blob Storage para uma tabela no BD do Azure SQL Database, utilizando o Azure DataFactory, um serviço de integração de dados da Azure, a nuvem da Microsoft. 

O container no Azure Blob Storage pode receber arquivos que não fazem parte da atividade de cópia, portanto é necessário filtrar os arquivos antes de executá-la.

### Arquitetura 

As etapas a serem desenvolvidas no Azure DataFactory são: 

*Listar os arquivos existentes no Azure Blob Storage.

*Filtrar os arquivos necessários.

*Copiar os dados de cada arquivo encontrado para a tabela no Azure SQL Database.

![image](https://user-images.githubusercontent.com/45773133/148269705-4dc1e909-9ed1-4ded-9709-6ad7a11cdcff.png)


### Atividades do Azure DataFactory  ![image](https://user-images.githubusercontent.com/45773133/148269626-43dae753-a5a3-4921-81e3-cec3c1f0207e.png)

#### 1) Get Metadata

Utilizada para listar os nomes dos arquivos dos blobs. Após adicionar o Dataset do blob que será listado ("CSVBLOB"), é necessário adicionar o argumento "Child Items" na Field list. Esta etapa fará a leitura de todos os arquivos no container especificado no Dataset.

![image](https://user-images.githubusercontent.com/45773133/148270846-cb0e0731-a31a-45ac-a3ec-d3162fcfde87.png)

A figura abaixo mostra o output da atividade, com a lista de arquivos do Blob:

![image](https://user-images.githubusercontent.com/45773133/148271129-410c1e7f-c968-48fb-85f0-cbcfd0912eff.png)


#### 2) Filter

Utilizada para filtrar os arquivos registrados na atividade anterior. 

Em Items, adicione o ChildItem da atividade GetMetadata ("@activity('Get Metadata').output.childItems"), assim a lista formada na atividade anterior poderá ser acessada.
Em Condition, coloque a condição do filtro ("@startswith(item().name,'Quadro')"). nesse caso, serão selecionados os arquivos que iniciam com a palavra "Quadro".

![image](https://user-images.githubusercontent.com/45773133/148271855-88f9d99b-e646-4183-ad3b-042ccd8b8573.png)

A figura abaixo mostra o output da atividade, com a lista de arquivos do Blob filtrada:

![image](https://user-images.githubusercontent.com/45773133/148271594-108fd77e-2bc9-40cb-aaea-0ff8fcadda70.png)


#### 3) ForEach

Utilizada para selecionar individualmente cada arquivo que foi listado na etapa anterior (Filter). 
Em Items, adicione os valores de saída da atividade anterior (Filter): "@activity('Filter').output.Value".

![image](https://user-images.githubusercontent.com/45773133/148272441-4a37219d-4ea7-431a-9211-5d1a402e3643.png)

Na aba Activities, clique no botao de edição e adicione a atividade Copy data.

![image](https://user-images.githubusercontent.com/45773133/148272923-39e8d30d-3daa-4ec8-bb2b-b439db8e995f.png)

#### 4) Copy data

Esta é a atividade onde será realizada a atividade de cópia dos dados do arquivo .csv (Azure Blob Storage) para a tabela no banco de dados (Azure SQL Database).

Na aba Source, deve ser adicionado o dataset do blob ("CSVBLOB") e no File path type escolher a opção Prefix. No campo do Prefix, escrever: @{item().name} , que vai selecionar o nome de cada item que for selecionado pelo ForEach.

![image](https://user-images.githubusercontent.com/45773133/148273804-f77a0d4e-04ef-4fd2-82ad-c5b2854f8a62.png)

Já na aba Sink deve ser adicionado o Dataset do Azure SQL Database, apontando para a tabela de destino.

![image](https://user-images.githubusercontent.com/45773133/148274796-2e4b066c-0784-4684-b3cf-b4322107ba35.png)

Após o preenchimento destas 2 abas, vá até a aba Mapping e clique no botão "Import Schemas". Aparecerá as colunas de origem (Azure Blob Storage) e de destino (Azure SQL Database). As colunas que estiverem com o mesmo nome serão conectadas, as que não tiverem um nome correspondente aparecerão indicadas e devem ser ajustadas até que não seja apontado mais nenhum erro (as colunas podem ser deletadas também, caso necessário)

![image](https://user-images.githubusercontent.com/45773133/148277210-d142b56e-2c1d-4b52-94d7-de7aabf88556.png)

### Dica

Para conseguir realizar o Mapping das colunas de entrada, pode ser feito a seguinte configuração temporária da aba Source:

![image](https://user-images.githubusercontent.com/45773133/148275303-96d3f6b9-34bb-476c-9a9c-171e5c055bd9.png)

O Dataset "CSVBLOB" deverá estar com um arquivo selecionado, para conseguir mapear os dados.

![image](https://user-images.githubusercontent.com/45773133/148276762-beabfa14-4a74-4acb-af9a-322e78cb088c.png)

Após o mapeamento, o dataset deve ser reconfigurado somente com o nome do container, deixando o nome do arquivo em branco. A aba Source da atividade Copy data deve estar conforme indicado inicialmente, com a opção Prefix.


### Como usar o arquivo no seu projeto

Este arquivo .json se copiado e adicionado a um pipeline no Azure Datafactory pode ser usado como um template. Para adicionar o template ao seu pipeline, crie no Azure DataFactory os Datasets: para o CSV ("CSVBLOB") e para a tabela no BD ("AzureSqlTable"). Após isso, crie um pipeline no Datafactory, clique em "Code"

![image](https://user-images.githubusercontent.com/45773133/148277588-7d985c12-0a32-4b77-b3e7-ea615d7c3442.png)

e substitua o código desse repo no lugar do código existente. 
