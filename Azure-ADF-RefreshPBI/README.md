# Azure 
## Data Factory
### Refresh Power BI

#### Português
Este arquivo .json se copiado e adicionado a um pipeline no Azure Datafactory pode ser usado como um template para um refresh no Dataset do Power BI. 
Para adicionar o template ao Datafactory, basta clicar em "Code"

![image](https://user-images.githubusercontent.com/45773133/123710073-7c512300-d844-11eb-940b-bcfc76ce94bd.png)

e copiar esse código no lugar do código existente. É necessário também preencher os parâmetros do pipeline,

![image](https://user-images.githubusercontent.com/45773133/123711069-372df080-d846-11eb-8c68-5ca4cbe28e7a.png)

que deve ser feito após criar uma API REST do Power BI no ADD e adicionar os segredos na Azure KeyVaultalém de dar as devidas permissões para o DF no PBI . Para um passo-a-passo detalhado, você pode seguir o seguinte tutorial: https://datasavvy.me/2020/07/09/refreshing-a-power-bi-dataset-in-azure-data-factory/ , de onde se originou o código.


==================================================================================


#### English
The file .json can be added to a Azure Data Factory pipeline and used as a template to refresh Power BI Dataset.
To add the template into DataFactory, just click on "Code" 

![image](https://user-images.githubusercontent.com/45773133/123710073-7c512300-d844-11eb-940b-bcfc76ce94bd.png)

and overwrite the existing code. It is necessary fill in the pipeline parameters,

![image](https://user-images.githubusercontent.com/45773133/123711069-372df080-d846-11eb-8c68-5ca4cbe28e7a.png)

which should be done after creating your PBI API REST, add the secrets into an Azure KeyVault and grant the service principal to access PBI workspace contaiing the dataset. For a detailed step-by-step, you can follow the tutorial: https://datasavvy.me/2020/07/09/refreshing-a-power-bi-dataset-in-azure-data-factory/ , where this code was originated.

