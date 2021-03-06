---
title: Consultar em bancos de dados de nuvem com esquema diferente
description: como configurar consultas entre bancos de dados em partições verticais
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: how-to
author: MladjoA
ms.author: mlandzic
ms.reviewer: sstein
ms.date: 01/25/2019
ms.openlocfilehash: c507a4c618713ba83d25b9defa918092db1a3c8e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/19/2021
ms.locfileid: "92792082"
---
# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Consultar entre bancos de dados na nuvem com esquemas diferentes (visualização)
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

![Consultar tabelas em bancos de dados diferentes][1]

Bancos de dados particionados verticalmente usam diferentes conjuntos de tabelas diferentes bancos de dados. Isso significa que o esquema é diferente em bancos de dados diferentes. Por exemplo, todas as tabelas de inventário estão em um banco de dados, enquanto todas as tabelas relacionadas à contabilidade estão em um segundo banco de dados.

## <a name="prerequisites"></a>Pré-requisitos

* O usuário deve ter a permissão para ALTERAR QUALQUER FONTE DE DADOS EXTERNA. Essa permissão está incluída na permissão ALTERAR BANCO DE DADOS.
* As permissões para ALTERAR QUALQUER FONTE DE DADOS EXTERNA são necessárias para referenciar a fonte de dados subjacente.

## <a name="overview"></a>Visão geral

> [!NOTE]
> Ao contrário do particionamento horizontal, essas instruções DDL não dependem da definição de uma camada de dados com um mapa de fragmentos por meio da biblioteca de cliente do banco de dados elástico.
>

1. [CREATE MASTER KEY](/sql/t-sql/statements/create-master-key-transact-sql)
2. [CRIAR UMA CREDENCIAL NO ESCOPO DO BANCO DE DADOS](/sql/t-sql/statements/create-database-scoped-credential-transact-sql)
3. [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql)
4. [CREATE EXTERNAL TABLE](/sql/t-sql/statements/create-external-table-transact-sql)

## <a name="create-database-scoped-master-key-and-credentials"></a>Criar chave mestra do escopo do banco de dados e credenciais

A credencial é usada pela consulta elástica para se conectar aos bancos de dados remotos.  

```sql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'master_key_password';
CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
SECRET = '<password>'
[;]
```

> [!NOTE]
> Verifique se o `<username>` não inclui nenhum sufixo **" \@ ServerName"** .

## <a name="create-external-data-sources"></a>Criar fontes de dados externas

Sintaxe:

<External_Data_Source>:: = criar fonte de dados externa <data_source_name> com (tipo = RDBMS, local = ' <fully_qualified_server_name> ', DATABASE_NAME = ' <remote_database_name> ',  
    CREDENTIAL = <credential_name>) [;]

> [!IMPORTANT]
> O parâmetro TYPE deve ser definido como **RDBMS**.

### <a name="example"></a>Exemplo

O exemplo a seguir ilustra o uso da instrução CRIAR para fontes de dados externas.

```sql
CREATE EXTERNAL DATA SOURCE RemoteReferenceData
   WITH
      (
         TYPE=RDBMS,
         LOCATION='myserver.database.windows.net',
         DATABASE_NAME='ReferenceData',
         CREDENTIAL= SqlUser
      );
```

Para recuperar a lista de fontes de dados externas atuais:

```sql
select * from sys.external_data_sources;
```

### <a name="external-tables"></a>Tabelas externas

Sintaxe:

CRIAR tabela externa [database_name. [ schema_name ]. | schema_name. ] table_name  
    ({<column_definition>} [,... n]) {WITH (<rdbms_external_table_options>)}) [;]

<rdbms_external_table_options>:: = DATA_SOURCE = <External_Data_Source>, [SCHEMA_NAME = N ' nonescaped_schema_name ',] [OBJECT_NAME = N ' nonescaped_object_name ',]

### <a name="example"></a>Exemplo

```sql
CREATE EXTERNAL TABLE [dbo].[customer]
   (
      [c_id] int NOT NULL,
      [c_firstname] nvarchar(256) NULL,
      [c_lastname] nvarchar(256) NOT NULL,
      [street] nvarchar(256) NOT NULL,
      [city] nvarchar(256) NOT NULL,
      [state] nvarchar(20) NULL,
      DATA_SOURCE = RemoteReferenceData
   );
```

O exemplo a seguir mostra como recuperar a lista de tabelas externas do banco de dados atual:

```sql
select * from sys.external_tables;
```

### <a name="remarks"></a>Comentários

A consulta elástica estende a sintaxe de tabela externa existente para definir as tabelas externas que usam fontes de dados externas do tipo RDBMS. Uma definição de tabela externa para o particionamento vertical abrange os seguintes aspectos:

* **Esquema**: a DDL da tabela externa define um esquema que pode ser usado pelas consultas. O esquema fornecido na definição da tabela externa precisa corresponder ao esquema das tabelas no banco de dados remoto em que os dados reais são armazenados.
* **Referência de banco de dados remoto**: a DDL da tabela externa faz referência a uma fonte de dados externa. A fonte de dados externa especifica o nome do servidor e o nome do banco do dados remoto em que os dados reais da tabela são armazenados.

Com uma fonte de dados externa, como descrito na seção anterior, a sintaxe para criar tabelas externas é a seguinte:

A cláusula DATA_SOURCE define a fonte de dados externa (ou seja, o banco de dados remoto, no caso do particionamento vertical) que é usada para a tabela externa.  

As cláusulas SCHEMA_NAME e OBJECT_NAME fornecem a capacidade de mapear a definição da tabela externa para uma tabela em um esquema diferente no banco de dados remoto ou para uma tabela com um nome diferente, respectivamente. Isso será útil se você quiser definir uma tabela externa para uma exibição de catálogo ou DMV em seu banco de dados remoto – ou em qualquer outra situação em que o nome da tabela remota já esteja sendo usado localmente.  

A instrução DDL a seguir remove uma definição existente da tabela externa do catálogo do local. Ela não afeta o banco de dados remoto.

```sql
DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  
```

**Permissões para CREATE/DROP EXTERNAL TABLE**: as permissões ALTER ANY EXTERNAL DATA SOURCE são necessárias para a DDL da tabela externa, o que também é necessário para fazer referência à fonte de dados subjacente.  

## <a name="security-considerations"></a>Considerações de segurança

Usuários com acesso à tabela externa têm acesso automaticamente a tabelas remotas subjacentes com a credencial fornecida na definição de fonte de dados externa. Você deve gerenciar cuidadosamente o acesso à tabela externa para evitar a elevação indesejada de privilégios por meio da credencial da fonte de dados externa. Permissões de SQL regulares podem ser usadas para o acesso de GRANT ou REVOKE a uma tabela externa como se ela fosse uma tabela normal.  

## <a name="example-querying-vertically-partitioned-databases"></a>Exemplo: consultando bancos de dados particionados verticalmente

A consulta a seguir executa uma junção de três vias entre as duas tabelas locais para pedidos e linhas da pedido e a tabela remota para clientes. Este é um exemplo do caso de uso de dados de referência para a consulta elástica:

```sql
    SELECT
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline,
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer
    JOIN orders
    ON c_id = o_c_id
    JOIN  order_line
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100
```

## <a name="stored-procedure-for-remote-t-sql-execution-sp_execute_remote"></a>Procedimento armazenado para a execução remota de T-SQL: sp\_execute_remote

A consulta elástica também apresenta um procedimento armazenado que fornece acesso direto ao banco de dados remoto. O procedimento armazenado é chamado [sp\_execute \_remote](/sql/relational-databases/system-stored-procedures/sp-execute-remote-azure-sql-database) e pode ser usado para executar procedimentos armazenados remotos ou código T-SQL no banco de dados remoto. Ele usa os seguintes parâmetros:

* Nome da fonte de dados (nvarchar): o nome da fonte de dados externa do tipo RDBMS.
* Consulta (nvarchar): a consulta T-SQL a ser executada no banco de dados remoto.
* Declaração de parâmetro (nvarchar) - opcional: cadeia de caracteres com definições de tipo de dados para os parâmetros usados no parâmetro Query (como sp_executesql).
* Lista de valores de parâmetro - opcional: lista separada por vírgulas de valores de parâmetro (como sp_executesql).

O sp\_execute\_remote usa a fonte de dados externa fornecida nos parâmetros de invocação para executar a instrução T-SQL especificada no banco de dados remoto. Ele usa a credencial da fonte de dados externa para a conexão com o banco de dados remoto.  

Exemplo:

```sql
    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse'
```

## <a name="connectivity-for-tools"></a>Conectividade de ferramentas

Você pode usar cadeias de conexão SQL Server regulares para conectar suas ferramentas de integração de BI e de dados a bancos de dado no servidor que tem a consulta elástica habilitada e tabelas externas definidas. Certifique-se de que o SQL Server tem suporte como uma fonte de dados para a ferramenta. Em seguida, consulte o banco de dados de consulta elástica e suas tabelas externas como qualquer outro banco de dados do SQL Server ao qual você se conectaria com a sua ferramenta.

## <a name="best-practices"></a>Práticas recomendadas

* Verifique se o banco de dados de ponto de extremidade de consulta elástica recebeu acesso ao banco de dados remoto habilitando o acesso para serviços do Azure em sua configuração de firewall do banco de dados SQL do Azure. Também verifique se a credencial fornecida na definição da fonte de dados externa pode fazer logon com êxito no banco de dados remoto e se ela tem as permissões para acessar a tabela remota.  
* A consulta elástica funciona melhor para consultas em que a maior parte da computação pode ser realizada nos bancos de dados remotos. Normalmente, você obtém o melhor desempenho de consulta com predicados de filtro seletivo que podem ser avaliados nos bancos de dados remotos ou com junções que podem ser executadas por completo no banco de dados remoto. Outros padrões de consulta podem precisar carregar grandes quantidades de dados do banco de dados remoto e podem ter um desempenho insatisfatório.

## <a name="next-steps"></a>Próximas etapas

* Para obter uma visão geral de consulta elástica, veja [Visão geral de consulta elástica](elastic-query-overview.md).
* Para obter um tutorial sobre particionamento vertical, veja [Introdução à consulta entre bancos de dados (particionamento vertical)](elastic-query-getting-started-vertical.md).
* Para um tutorial sobre particionamento horizontal (fragmentação), consulte [Introdução à consulta elástica para particionamento horizontal (fragmentação)](elastic-query-getting-started.md).
* Para sintaxe e amostras de consultas para dados particionados horizontalmente, consulte [Consultando dados particionados horizontalmente)](elastic-query-horizontal-partitioning.md)
* Consulte [sp\_execute \_remote](/sql/relational-databases/system-stored-procedures/sp-execute-remote-azure-sql-database) para um procedimento armazenado que executa uma instrução Transact-SQL em um único Banco de Dados SQL do Azure remoto ou um conjunto de bancos de dados que serve como fragmentos em um esquema de particionamento horizontal.

<!--Image references-->
[1]: ./media/elastic-query-vertical-partitioning/verticalpartitioning.png

<!--anchors-->