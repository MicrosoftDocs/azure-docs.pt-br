---
title: Mover dados do PostgreSQL usando Azure Data Factory
description: Saiba mais sobre como mover dados do banco de dados PostgreSQL usando o Azure Data Factory
author: linda33wj
ms.author: jingwang
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/10/2018
robots: noindex
ms.openlocfilehash: 8ca8770944a12c6e0dd3e4f95d286758ebee5f9e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100387267"
---
# <a name="move-data-from-postgresql-using-azure-data-factory"></a>Mover dados do PostgreSQL usando o Azure Data Factory
> [!div class="op_single_selector" title1="Selecione a versão do serviço Data Factory que você está usando:"]
> * [Versão 1](data-factory-onprem-postgresql-connector.md)
> * [Versão 2 (versão atual)](../connector-postgresql.md)

> [!NOTE]
> Este artigo aplica-se à versão 1 do Data Factory. Se você estiver usando a versão atual do serviço Data Factory, consulte [Conector do PostgreSQL na V2](../connector-postgresql.md).


Esse artigo explica como usar a Atividade de Cópia no Azure Data Factory para mover dados de um banco de dados PostgreSQL local. Ele se baseia no artigo [atividades de movimentação de dados](data-factory-data-movement-activities.md) , que apresenta uma visão geral da movimentação de dados com a atividade de cópia.

Você pode copiar dados de um armazenamento de dados local do PostgreSQL para qualquer armazenamento de dados de coletor com suporte. Para obter uma lista de repositórios de dados com suporte como coletores da atividade de cópia, confira os [armazenamentos de dados com suporte](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Atualmente, o data factory dá suporte à movimentação de dados de um banco de dados PostgreSQL para outros armazenamentos de dados, mas não à movimentação de dados de outros armazenamentos de dados para um banco de dados PostgreSQL.

## <a name="prerequisites"></a>Pré-requisitos

O serviço Data Factory dá suporte à conexão com fontes PostgreSQL locais usando o Gateway de Gerenciamento de Dados. Consulte o artigo [movendo dados entre pontos locais e na nuvem](data-factory-move-data-between-onprem-and-cloud.md) para saber mais sobre o Gateway de gerenciamento de dados e obter instruções passo a passo de como configurar o gateway.

O gateway é requerido mesmo que o banco de dados PostgreSQL esteja hospedado em uma VM IaaS do Azure. Você pode instalar o gateway na mesma VM IaaS do armazenamento de dados ou em uma VM diferente, desde que o gateway possa conectar o banco de dados.

> [!NOTE]
> Consulte [Solucionar problemas de gateway](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) para ver dicas sobre como solucionar os problemas relacionados à conexão/gateway.

## <a name="supported-versions-and-installation"></a>Instalação e versões com suporte
Para o Gateway de Gerenciamento de Dados se conectar ao banco de dados PostgreSQL, instale o [Provedor de dados Ngpsql para PostgreSQL](https://go.microsoft.com/fwlink/?linkid=282716) com uma versão entre a 2.0.12 e a 3.1.9 no mesmo sistema que o Gateway de Gerenciamento de Dados. Há suporte para o PostgreSQL versão 7.4 e superior.

## <a name="getting-started"></a>Introdução
Você pode criar um pipeline com atividade de cópia que mova dados de um repositório de dados local PostgreSQL usando diferentes ferramentas/APIs.

- A maneira mais fácil de criar um pipeline é usar o **Assistente de cópia**. Confira [Tutorial: Criar um pipeline usando o Assistente de Cópia](data-factory-copy-data-wizard-tutorial.md) para ver um breve passo a passo sobre como criar um pipeline usando o Assistente de cópia de dados.
- Você também pode usar as ferramentas abaixo para criar um pipeline:
  - Visual Studio
  - Azure PowerShell
  - Modelo do Azure Resource Manager
  - API do .NET
  - API REST

    Confira o [tutorial de atividade de cópia](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) para obter instruções detalhadas para criar um pipeline com uma atividade de cópia.

Ao usar as ferramentas ou APIs, você executa as seguintes etapas para criar um pipeline que move dados de um armazenamento de dados de origem para um armazenamento de dados de coletor:

1. Criar **serviços vinculados** para vincular repositórios de dados de entrada e saída ao seu data factory.
2. Criar **conjuntos de dados** para representar dados de entrada e saída para a operação de cópia.
3. Criar um **pipeline** com uma atividade de cópia que usa um conjunto de dados como uma entrada e um conjunto de dados como uma saída.

Ao usar o assistente, as definições de JSON para essas entidades do Data Factory (serviços vinculados, conjuntos de dados e o pipeline) são automaticamente criadas para você. Ao usar ferramentas/APIs (exceto a API .NET), você define essas entidades do Data Factory usando o formato JSON. Para obter um exemplo com definições de JSON para entidades do Data Factory que são usadas para copiar dados de um armazenamento de dados PostgreSQL local, confira a seção [Exemplo de JSON: Copiar dados do PostgreSQL para o Blob do Azure](#json-example-copy-data-from-postgresql-to-azure-blob) deste artigo.

As seções que se seguem fornecem detalhes sobre as propriedades JSON que são usadas para definir entidades do Data Factory específicas para um armazenamento de dados PostgreSQL:

## <a name="linked-service-properties"></a>Propriedades do serviço vinculado
A tabela a seguir fornece a descrição para elementos JSON específicas para o serviço de vinculado do PostgreSQL.

| Propriedade | Descrição | Obrigatório |
| --- | --- | --- |
| type |A propriedade tipo deve ser definida como: **OnPremisesPostgreSql** |Sim |
| Servidor |Nome do servidor PostgreSQL. |Sim |
| Banco de Dados |Nome do banco de dados PostgreSQL. |Sim |
| esquema |Nome do esquema no banco de dados. O nome do esquema diferencia maiúsculas de minúsculas. |Não |
| authenticationType |Tipo de autenticação usado para se conectar ao banco de dados PostgreSQL. Os valores possíveis são: Anonymous, Basic e Windows. |Sim |
| Nome de Usuário |Especifique o nome de usuário se você estiver usando a autenticação Basic ou Windows. |Não |
| password |Especifique a senha da conta de usuário que você especificou para o nome de usuário. |Não |
| gatewayName |O nome do gateway que o serviço Data Factory deve usar para se conectar ao banco de dados PostgreSQL local. |Sim |

## <a name="dataset-properties"></a>Propriedades do conjunto de dados
Para obter uma lista completa das seções e propriedades disponíveis para definir conjuntos de dados, confira o artigo [Criando conjuntos de dados](data-factory-create-datasets.md). As seções como structure, availability e policy de um conjunto de dados JSON são similares para todos os tipos de conjunto de dados.

A seção typeProperties é diferente para cada tipo de conjunto de dados e fornece informações sobre o local dos dados no armazenamento de dados. A seção typeProperties de um conjunto de dados do tipo **RelationalTable** (que inclui o conjunto de dados do PostgreSQL) tem as seguintes propriedades:

| Propriedade | Descrição | Obrigatório |
| --- | --- | --- |
| tableName |Nome da tabela na instância do banco de dados PostgreSQL à qual o serviço vinculado se refere. O tableName diferencia maiúsculas de minúsculas. |Não (se **query** de **RelationalSource** for especificado) |

## <a name="copy-activity-properties"></a>Propriedades da atividade de cópia
Para obter uma lista completa das seções e propriedades disponíveis para definir atividades, confia o artigo [Criando pipelines](data-factory-create-pipelines.md). As propriedades, como nome, descrição, tabelas de entrada e saída, e política, estão disponíveis para todos os tipos de atividades.

Por outro lado, as propriedades disponíveis na seção typeProperties da atividade variam de acordo com cada tipo de atividade. Para a atividade de cópia, elas variam de acordo com os tipos de fonte e coletor.

Quando a fonte é do tipo **RelationalSource** (que inclui o PostgreSQL), as seguintes propriedades estão disponíveis na seção typeProperties:

| Propriedade | Descrição | Valores permitidos | Obrigatório |
| --- | --- | --- | --- |
| Consulta |Utiliza a consulta personalizada para ler os dados. |Cadeia de caracteres de consulta SQL. Por exemplo: `"query": "select * from \"MySchema\".\"MyTable\""`. |Não (se **tableName** de **dataset** for especificado) |

> [!NOTE]
> Os nomes de esquema e tabela diferenciam maiúsculas de minúsculas. Coloque-os em `""` (aspas duplas) na consulta.

**Exemplo:**

 `"query": "select * from \"MySchema\".\"MyTable\""`

## <a name="json-example-copy-data-from-postgresql-to-azure-blob"></a>Exemplo de JSON: copiar dados do PostgreSQL para o Blob do Azure
Este exemplo fornece definições de JSON de exemplo que você pode usar para criar um pipeline usando o [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) ou [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Eles mostram como copiar dados do banco de dados PostgreSQL para o Armazenamento de Blobs do Azure. No entanto, os dados podem ser copiados para qualquer um dos coletores declarados [aqui](data-factory-data-movement-activities.md#supported-data-stores-and-formats) usando a Atividade de Cópia no Azure Data Factory.

> [!IMPORTANT]
> Este exemplo fornece snippets de JSON. Ele não inclui instruções passo a passo para criar o data factory. Confira o artigo [Mover dados entre fontes locais e a nuvem](data-factory-move-data-between-onprem-and-cloud.md) para obter instruções passo a passo.

O exemplo tem as seguintes entidades de data factory:

1. Um serviço vinculado do tipo [OnPremisesPostgreSql](data-factory-onprem-postgresql-connector.md#linked-service-properties).
2. Um serviço vinculado do tipo [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Um [conjunto de dados](data-factory-create-datasets.md) de entrada do tipo [RelationalTable](data-factory-onprem-postgresql-connector.md#dataset-properties).
4. Um [conjunto de dados](data-factory-create-datasets.md) de saída do tipo [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
5. O [pipeline](data-factory-create-pipelines.md) com a Atividade de Cópia que usa [RelationalSource](data-factory-onprem-postgresql-connector.md#copy-activity-properties) e [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

O exemplo copia dados de um resultado de consulta no banco de dados PostgreSQL para um blob a cada hora. As propriedades JSON usadas nesses exemplos são descritas nas seções após os exemplos.

Como uma primeira etapa, configure o gateway de gerenciamento de dados. As instruções estão no artigo [Mover dados entre fontes locais e a nuvem](data-factory-move-data-between-onprem-and-cloud.md) .

**Serviço vinculado do PostgreSQL:**

```json
{
    "name": "OnPremPostgreSqlLinkedService",
    "properties": {
        "type": "OnPremisesPostgreSql",
        "typeProperties": {
            "server": "<server>",
            "database": "<database>",
            "schema": "<schema>",
            "authenticationType": "<authentication type>",
            "username": "<username>",
            "password": "<password>",
            "gatewayName": "<gatewayName>"
        }
    }
}
```
**Serviço vinculado do armazenamento de Blob do Azure:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
        }
    }
}
```
**Conjunto de dados de entrada do PostgreSQL:**

O exemplo supõe que você criou uma tabela "MyTable" no PostgreSQL e que ela contém uma coluna chamada "timestamp" para dados de série temporal.

A configuração `"external": true` informa o serviço Data Factory que o conjunto de dados é externo ao Data Factory e não é produzido por uma atividade no Data Factory.

```json
{
    "name": "PostgreSqlDataSet",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "OnPremPostgreSqlLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

**Conjunto de resultados de saída de blob do Azure:**

Os dados são gravados em um novo blob a cada hora (frequência: hora, intervalo: 1). O caminho de pasta e nome de arquivo para o blob são avaliados dinamicamente com base na hora de início da fatia que está sendo processada. O caminho da pasta usa as partes ano, mês, dia e horas da hora de início.

```json
{
    "name": "AzureBlobPostgreSqlDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/postgresql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Pipeline com Atividade de cópia:**

O pipeline contém uma Atividade de Cópia configurada para usar os conjuntos de dados de entrada e saída, e está agendada para ser executada por hora. Na definição JSON do pipeline, o tipo **source** está definido como **RelationalSource** e o tipo **sink** está definido como **BlobSink**. A consulta SQL especificada para a propriedade **query** seleciona os dados da tabela public.usstates no banco de dados PostgreSQL.

```json
{
    "name": "CopyPostgreSqlToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "select * from \"public\".\"usstates\""
                    },
                    "sink": {
                        "type": "BlobSink"
                    }
                },
                "inputs": [
                    {
                        "name": "PostgreSqlDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobPostgreSqlDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "PostgreSqlToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
## <a name="type-mapping-for-postgresql"></a>Mapeamento de tipo para PostgreSQL
Conforme mencionado no artigo [atividades de movimentação de dados](data-factory-data-movement-activities.md) , a atividade de cópia executa conversões automáticas dos tipos de origem nos tipos de coletor com a seguinte abordagem de duas etapas:

1. Converter de tipos de fonte nativos para o tipo .NET
2. Converter do tipo .NET para o tipo de coletor nativo

Ao mover os dados para o PostgreSQL os seguintes mapeamentos são usados do tipo do PostgreSQL para o tipo do .NET.

| Tipo de banco de dados PostgreSQL | Aliases de PostgresSQL | Tipo de .NET Framework |
| --- | --- | --- |
| abstime | |Datetime |
| BIGINT |int8 |Int64 |
| bigserial |serial8 |Int64 |
| bit [(n)] | |Byte[], String |
| bit varying [ (n) ] |varbit |Byte[], String |
| boolean |bool |Booliano |
| box | |Byte[], String |
| bytea | |Byte[], String |
| character [(n)] |char [(n)] |String |
| character varying [(n)] |varchar [(n)] |String |
| cid | |String |
| cidr | |String |
| circle | |Byte[], String |
| date | |Datetime |
| daterange | |String |
| double precision |float8 |Double |
| inet | |Byte[], String |
| intarry | |String |
| int4range | |String |
| int8range | |String |
| inteiro |int, int4 |Int32 |
| interval [fields] [(p)] | |Timespan |
| json | |String |
| jsonb | |Byte[] |
| line | |Byte[], String |
| lseg | |Byte[], String |
| macaddr | |Byte[], String |
| money | |Decimal |
| numeric [(p, s)] |decimal [(p, s)] |Decimal |
| numrange | |String |
| oid | |Int32 |
| caminho | |Byte[], String |
| pg_lsn | |Int64 |
| point | |Byte[], String |
| polygon | |Byte[], String |
| real |float4 |Único |
| SMALLINT |int2 |Int16 |
| smallserial |serial2 |Int16 |
| serial |serial4 |Int32 |
| text | |String |

## <a name="map-source-to-sink-columns"></a>Mapear origem para colunas de coletor
Para saber mais sobre mapeamento de colunas no conjunto de dados de origem para colunas no conjunto de dados de coletor, confira [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md) (Mapeamento de colunas de conjunto de dados no Azure Data Factory).

## <a name="repeatable-read-from-relational-sources"></a>Leitura repetida de fontes relacionais
Ao copiar dados de armazenamentos de dados relacionais, lembre-se da capacidade de repetição para evitar resultados não intencionais. No Azure Data Factory, você pode repetir a execução de uma fatia manualmente. Você também pode configurar a política de repetição para um conjunto de dados de modo que uma fatia seja executada novamente quando ocorrer uma falha. Quando uma fatia é executada novamente, seja de que maneira for, você precisa garantir que os mesmos dados sejam lidos não importa quantas vezes uma fatia seja executada. Consulte [leitura repetida de fontes relacionais](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Desempenho e Ajuste
Veja o [Guia de desempenho e ajuste da Atividade de Cópia](data-factory-copy-activity-performance.md) para saber mais sobre os principais fatores que afetam o desempenho da movimentação de dados (Atividade de Cópia) no Azure Data Factory, além de várias maneiras de otimizar esse processo.
