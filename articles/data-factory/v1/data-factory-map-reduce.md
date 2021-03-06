---
title: Chamar o Programa MapReduce da Data Factory do Azure
description: Saiba como processar dados executando programas MapReduce em um cluster HDInsight do Azure em uma Azure Data Factory.
author: dcstwh
ms.author: weetok
ms.reviewer: jburchel
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/10/2018
ms.openlocfilehash: f1f54e972d59d3de3b0f93b3150ee1150eb6f612
ms.sourcegitcommit: f611b3f57027a21f7b229edf8a5b4f4c75f76331
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/22/2021
ms.locfileid: "104786389"
---
# <a name="invoke-mapreduce-programs-from-data-factory"></a>Chamar Programas MapReduce da Data Factory
> [!div class="op_single_selector" title1="Atividades de transformação"]
> * [Atividade do hive](data-factory-hive-activity.md) 
> * [Atividade Pig](data-factory-pig-activity.md)
> * [Atividade MapReduce](data-factory-map-reduce.md)
> * [Atividade de streaming do Hadoop](data-factory-hadoop-streaming-activity.md)
> * [Atividade do Spark](data-factory-spark.md)
> * [Atividade de execução em lotes do Azure Machine Learning Studio (clássico)](data-factory-azure-ml-batch-execution-activity.md)
> * [Atividade do recurso de atualização do Azure Machine Learning Studio (clássico)](data-factory-azure-ml-update-resource-activity.md)
> * [Atividade de Procedimento Armazenado](data-factory-stored-proc-activity.md)
> * [Atividade do U-SQL da Análise Data Lake](data-factory-usql-activity.md)
> * [Atividade personalizada do .NET](data-factory-use-custom-activities.md)

> [!NOTE]
> Este artigo aplica-se à versão 1 do Data Factory. Se você estiver usando a versão atual do serviço Data Factory, consulte [ transformar dados usando a atividade MapReduce no Data Factory ](../transform-data-using-hadoop-map-reduce.md).


A atividade de MapReduce do HDInsight em um [pipeline](data-factory-create-pipelines.md) do Data Factory executa programas MapReduce [no seu próprio cluster](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) HDInsight baseado em Windows/Linux, ou em um [sob demanda](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). Este artigo se baseia no artigo sobre [atividades de transformação de dados](data-factory-data-transformation-activities.md) que apresenta uma visão geral da transformação de dados e as atividades de transformação permitidas.

> [!NOTE] 
> Se você estiver conhecendo o Azure Data Factory agora, leia a [Introdução ao Azure Data Factory](data-factory-introduction.md) e siga o tutorial [Criar seu primeiro pipeline de dados](data-factory-build-your-first-pipeline.md) antes de ler este artigo.  

## <a name="introduction"></a>Introdução
Um pipeline em uma fábrica de dados do Azure processa dados nos serviços de armazenamento vinculados utilizando serviços de computação vinculados. Ela contém uma sequência de atividades em que cada atividade executa uma operação de processamento específica. Este artigo descreve como usar atividade do HDInsight MapReduce.

Consulte [Pig](data-factory-pig-activity.md) e [Hive](data-factory-hive-activity.md) para obter detalhes sobre como executar scripts do Pig/Hive em um cluster HDInsight baseado no Windows/Linux a partir de um pipeline usando atividades do HDInsight Pig e Hive. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON para atividade do HDInsight MapReduce
Na definição do JSON para a atividade de HDInsight: 

1. Defina o **tipo** da **atividade** como **HDInsight**.
2. Especifique o nome da classe da propriedade **className**.
3. Especifique o caminho do arquivo JAR, incluindo o nome do arquivo para a propriedade **jarFilePath**.
4. Especifique o serviço vinculado que faz referência ao Armazenamento de Blob do Azure que contém o arquivo JAR para a propriedade **jarLinkedService**.   
5. Especifique quaisquer argumentos para o programa MapReduce na seção **argumentos**. Em runtime, você verá alguns argumentos extras (por exemplo: mapreduce.job.tags) da estrutura MapReduce. Para diferenciar seus argumentos com os argumentos MapReduce, considere usar opção e valor como argumentos, conforme mostrado no exemplo a seguir (- s, --input - output etc... são opções seguidas imediatamente por seus valores).

    ```JSON   
    {
        "name": "MahoutMapReduceSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                        "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "-s",
                            "SIMILARITY_LOGLIKELIHOOD",
                            "--input",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                            "--output",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                            "--maxSimilaritiesPerItem",
                            "500",
                            "--tempDir",
                            "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                        ]
                    },
                    "inputs": [
                        {
                            "name": "MahoutInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "MahoutOutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "MahoutActivity",
                    "description": "Custom Map Reduce to generate Mahout result",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-01-03T00:00:00Z",
            "end": "2017-01-04T00:00:00Z"
        }
    }
    ```
   Você pode usar a atividade do HDInsight MapReduce para executar qualquer arquivo de jar do MapReduce em um cluster do HDInsight. Na seguinte definição de JSON de exemplo de uma pipeline, a Atividade HDInsight é configurada para executar um arquivo JAR do Mahout.

## <a name="sample-on-github"></a>Exemplo no GitHub
Você pode baixar um exemplo para usar a atividade do MapReduce HDInsight em: [Exemplos do Data Factory no GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>Executando o programa de contagem de palavras
O pipeline neste exemplo executa o programa de contagem de palavras de mapa/redução no cluster do Azure HDInsight.   

### <a name="linked-services"></a>Serviços vinculados
Primeiro, crie um serviço vinculado para vincular o armazenamento do Azure que é usado pelo cluster do Azure HDInsight à fábrica de dados do Azure. Se você copiar/colar o código a seguir, não se esqueça de substituir o **nome da conta** e a **chave de conta** pelo nome e chave do Armazenamento do Azure. 

#### <a name="azure-storage-linked-service"></a>Serviço vinculado de armazenamento do Azure

```JSON
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
        }
    }
}
```

#### <a name="azure-hdinsight-linked-service"></a>Serviço vinculado do Azure HDInsight
Em seguida, você cria um serviço vinculado para vincular seu cluster do HDInsight do Azure para a fábrica de dados do Azure. Se você copiar/colar o código a seguir, substitua o **nome do cluster hdinsight** pelo nome do seu cluster hdinsight e altere os valores de nome de usuário e senha.   

```JSON
{
    "name": "HDInsightLinkedService",
    "properties": {
        "type": "HDInsight",
        "typeProperties": {
            "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
            "userName": "admin",
            "password": "**********",
            "linkedServiceName": "StorageLinkedService"
        }
    }
}
```

### <a name="datasets"></a>Conjunto de dados
#### <a name="output-dataset"></a>Conjunto de dados de saída
O pipeline neste exemplo não tem entradas. Especifique um conjunto de dados de saída para a atividade do HDInsight MapReduce. Esse conjunto de dados é apenas um conjunto fictício exigido para direcionar a agenda de pipeline.  

```JSON
{
    "name": "MROutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "fileName": "WordCountOutput1.txt",
            "folderPath": "example/data/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

### <a name="pipeline"></a>Pipeline
O pipeline neste exemplo tem apenas uma atividade que seja do tipo: HDInsightMapReduce. Algumas das propriedades importantes no JSON são: 

| Propriedade | Observações |
|:--- |:--- |
| type |O tipo deve ser definido como **HDInsightMapReduce**. |
| className |Nome da classe é: **wordcount** |
| jarFilePath |Caminho para o arquivo jar que contém a classe. Se você copiar/colar o código a seguir, não se esqueça de alterar o nome do cluster. |
| jarLinkedService |Serviço vinculado do Armazenamento do Azure que contém o arquivo jar. Esse serviço vinculado faz referência ao armazenamento que está associado ao cluster do HDInsight. |
| argumentos |O programa wordcount leva dois argumentos, uma entrada e uma saída. O arquivo de entrada é o davinci.txt. |
| frequency/interval |Os valores dessas propriedades correspondem ao conjunto de dados de saída. |
| linkedServiceName |refere-se ao serviço vinculado do HDInsight criado anteriormente. |

```JSON
{
    "name": "MRSamplePipeline",
    "properties": {
        "description": "Sample Pipeline to Run the Word Count Program",
        "activities": [
            {
                "type": "HDInsightMapReduce",
                "typeProperties": {
                    "className": "wordcount",
                    "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                    "jarLinkedService": "StorageLinkedService",
                    "arguments": [
                        "/example/data/gutenberg/davinci.txt",
                        "/example/data/WordCountOutput1"
                    ]
                },
                "outputs": [
                    {
                        "name": "MROutput"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "MRActivity",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2014-01-03T00:00:00Z",
        "end": "2014-01-04T00:00:00Z"
    }
}
```

## <a name="run-spark-programs"></a>Executar programas Spark
Você pode usar a atividade MapReduce para executar programas Spark no cluster HDInsight Spark. Consulte [Invoke Spark programs from Azure Data Factory (Invocar programas Spark da Azure Data Factory)](data-factory-spark.md) para obter detalhes.  

[developer-reference]: /previous-versions/azure/dn834987(v=azure.100)
[cmdlet-reference]: https://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: /previous-versions/azure/dn834987(v=azure.100)
[Azure Portal]: https://portal.azure.com

## <a name="see-also"></a>Consulte Também
* [Atividade do hive](data-factory-hive-activity.md)
* [Atividade Pig](data-factory-pig-activity.md)
* [Atividade de streaming do Hadoop](data-factory-hadoop-streaming-activity.md)
* [Invocar programas Spark](data-factory-spark.md)
* [Invocar scripts R](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/RunRScriptUsingADFSample)