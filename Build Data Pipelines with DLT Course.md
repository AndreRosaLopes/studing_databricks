# Introduction

![figure](DLT_course\medallion_arquiteture.png)


## Bronze
* Raw data
* Append operation or batch

```sql
CREATE OR REFRESH STREAMING TABLE orders_bronze
AS SELECT current_timestamp() processing_time, *
FROM cloud_files("${source}/orders", "json", map("cloudFiles.inferColumnTypes", "true"))
```

## Silver
* Preserves granularity
* Schema enforcement
* Data quality

## Gold
* Refined and aggregatedy
* ML, report and etc.

```sql
CREATE LIVE TABLE customer_counts_state
  COMMENT "Total active customers per state"
AS SELECT state, count(*) as customer_count, current_timestamp() updated_at
  FROM LIVE.customers_silver
  GROUP BY state
```

## Live Table (Materialized View)

Is a materialized view for the lakelhouse
![figure](DLT_course\DLT_what_is.png)

* A cada atualização do pipeline, os resultados da consulta são recalculados para refletir as mudanças nos conjuntos de dados upstream que podem ter ocorrido devido à conformidade, correções, agregações ou CDC em geral.
* Não deve ser modificado por operações externas ao Pipeline DLT (você obterá respostas indefinidas ou sua alteração será simplesmente desfeita).

```sql
CREATE OR REFRESH LIVE TABLE orders_by_date
AS SELECT date(order_timestamp) AS order_date, count(*) AS total_daily_orders
FROM LIVE.orders_silver
GROUP BY date(order_timestamp)
```
## Live View
```sql
CREATE LIVE VIEW subscribed_order_emails_v
  AS SELECT a.customer_id, a.order_id, b.email 
    FROM LIVE.orders_silver a
    INNER JOIN LIVE.customers_silver b
    ON a.customer_id = b.customer_id
    WHERE notifications = 'Y'
```

## Stream Table
Append only table
![figure](DLT_course\DLT_stream.png)
* As alterações no pipeline serão refletidas nos registros posteriores. 
* Pode executar operações na tabela fora do Pipeline DLT gerenciado (acrescentar dados, executar GDPR, etc).

O método **`cloud_files()`** permite que o [Auto Loader](https://docs.databricks.com/ingestion/auto-loader/index.html) seja usado nativamente com SQL.

```sql
CREATE OR REFRESH STREAMING TABLE orders_bronze
AS SELECT current_timestamp() processing_time, *
FROM cloud_files("${source}/orders", "json", map("cloudFiles.inferColumnTypes", "true"))
```
Constraints:
```sql
CREATE OR REFRESH STREAMING TABLE orders_silver
(CONSTRAINT valid_date EXPECT (order_timestamp > "2021-01-01") ON VIOLATION FAIL UPDATE)
COMMENT "Append only orders with valid timestamps"
TBLPROPERTIES ("quality" = "silver")
AS SELECT timestamp(order_timestamp) AS order_timestamp, * EXCEPT (order_timestamp, _rescued_data)
FROM STREAM(LIVE.orders_bronze)
```
More constraints:
```sql
CREATE STREAMING TABLE customers_bronze_clean
(CONSTRAINT valid_id EXPECT (customer_id IS NOT NULL) ON VIOLATION FAIL UPDATE,
CONSTRAINT valid_operation EXPECT (operation IS NOT NULL) ON VIOLATION DROP ROW,
CONSTRAINT valid_name EXPECT (name IS NOT NULL or operation = "DELETE"), -- just for tracking it!!!
CONSTRAINT valid_address EXPECT (
  (address IS NOT NULL and 
  city IS NOT NULL and 
  state IS NOT NULL and 
  zip_code IS NOT NULL) or
  operation = "DELETE"),
CONSTRAINT valid_email EXPECT (
  rlike(email, '^([a-zA-Z0-9_\\-\\.]+)@([a-zA-Z0-9_\\-\\.]+)\\.([a-zA-Z]{2,5})$') or -- regex
  operation = "DELETE") ON VIOLATION DROP ROW)
AS SELECT *
  FROM STREAM(LIVE.customers_bronze)
```



## First pipeline

1. Write down a notebook
2. Build a pipeline (with one or more notebooks)
3. Start button...

![figure](DLT_course\DES_PRD.png)

## Dependencies

![figure](DLT_course\Dependencies.png)

## Data quality
Expectations...
![figure](DLT_course\Expectations.png)

## Pipeline's UI - Operations

![alt text](DLT_course\UI_pipe1.png)

Full refresh - becarefull: source data may not be avalable any more...

Start >> Refresh

You can set pipelines.reset.allowed = false (to avoid full refresh)

cli

## Log events

![alt text](DLT_course\Logs_events.png)

## Parameters

* Reusabillity/ Mantenability

![alt text](DLT_course\parameters.png)

## Change Data Capture
APPLY CHANGES INTO to use with CDC.

```sql
CREATE OR REFRESH STREAMING TABLE customers_silver;

APPLY CHANGES INTO LIVE.customers_silver
  FROM STREAM(LIVE.customers_bronze_clean)
  KEYS (customer_id)
  APPLY AS DELETE WHEN operation = "DELETE"
  SEQUENCE BY timestamp                           -- like "order by"
  COLUMNS * EXCEPT (operation, _rescued_data)
```

![alt text](DLT_course\CDC.png)

## Managment
![alt text](DLT_course\Managment.png)

# LAB_01

### Notas Adicionais sobre a Configuração do Pipeline
Aqui estão algumas notas sobre as configurações do pipeline acima:

- **Modo de pipeline** - Isso especifica como o pipeline será executado. Escolha o modo com base nos requisitos de latência e custo.
  - Os pipelines `Triggered` são executados uma vez e, em seguida, desligados até a próxima atualização manual ou agendada.
  - Os pipelines `Continuous` são executados continuamente, ingerindo novos dados à medida que eles chegam.
- **Bibliotecas do Notebook** - Embora esses documentos sejam Notebooks Databricks padrão, a sintaxe SQL é especializada em declarações de tabela DLT. Exploraremos a sintaxe no exercício que se segue.
- **Local de armazenamento** - Este campo opcional permite que o usuário especifique um local para armazenar logs, tabelas e outras informações relacionadas à execução do pipeline. Se não for especificado, o DLT gerará automaticamente um diretório.
- **Catálogo e Esquema de Destino** - Esses parâmetros são necessários para disponibilizar dados fora do pipeline.
- **Modo de Cluster**, **Mínimos Workers**, **Máximos Workers** - Esses campos controlam a configuração do Workers para o cluster subjacente que processa o pipeline. Aqui, definimos o número de Workers como 1 porque **o uso do DLT com o Unity Catalog requer pelo menos um Workers**.
- **`Variáveis de Configuração`** - Os pares chave-valor que adicionamos aqui serão passados para os notebooks usados no pipeline. Veremos a variável que estamos usando, **`source`**, na próxima lição. Observe que as chaves diferenciam maiúsculas de minúsculas.

### Python vs SQL
| Python | SQL | Notas |
|--------|--------|--------|
| API Python | API SQL Proprietária |  |
| Sem verificação de sintaxe | Tem verificações de sintaxe| No Python, se você executar uma célula de notebook DLT sozinha, ele mostrará um erro, enquanto no SQL, ele verificará se o comando é sintaticamente válido e informará. Em ambos os casos, as células individuais do notebook não devem ser executadas para pipelines DLT. |
| Uma nota sobre imports | Nenhuma | O módulo dlt deve ser importado explicitamente em suas bibliotecas de notebook Python. Em SQL, esse não é o caso. |
| Tabelas como DataFrames | Tabelas como resultados de consulta | A API Python DataFrame permite várias transformações de um conjunto de dados ao encadear várias chamadas de API juntas. Em comparação com o SQL, as mesmas transformações devem ser salvas em tabelas temporárias à medida que são transformadas. |
|`@dlt.table()`  | Declaração `SELECT` | Em SQL, a lógica principal da sua consulta, contendo as transformações que você faz em seus dados, está contida na declaração `SELECT`. Em Python, as transformações de dados são especificadas quando você configura opções para @dlt.table().  |
| `@dlt.table(comment = `"Comentário Python",`table_properties = {"quality": "silver"})` | `COMMENT` "Comentário SQL"       `TBLPROPERTIES ("quality" = "silver")` | Esta é a maneira como você adiciona comentários e propriedades de tabela em Python vs. SQL |
| __`Metaprogramação Python`__ | N/A | Você pode usar funções internas Python com Delta Live Tables para criar programaticamente várias tabelas para reduzir a redundância de código.


# Python

Import:
```python
import dlt
import pyspark.sql.functions as F

source = spark.conf.get("source")
```
Creating a bronze table:
```python
@dlt.table
def orders_bronze():
    return (
        spark.readStream
            .format("cloudFiles")                            # --> Autoloader
            .option("cloudFiles.format", "json")
            .option("cloudFiles.inferColumnTypes", True)
            .load(f"{source}/orders")
            .select(
                F.current_timestamp().alias("processing_time"), 
                "*"
            )
    )
```


```python
@dlt.table(
    comment = "Append only orders with valid timestamps",
    table_properties = {"quality": "silver"})
@dlt.expect_or_fail("valid_date", F.col("order_timestamp") > "2021-01-01")
def orders_silver():
    return (
        dlt.read_stream("orders_bronze")
            .select(
                "processing_time",
                "customer_id",
                "notifications",
                "order_id",
                F.col("order_timestamp").cast("timestamp").alias("order_timestamp")
            )
    )
```
```python
@dlt.table
def orders_by_date():
    return (
        dlt.read("orders_silver")
            .groupBy(F.col("order_timestamp").cast("date").alias("order_date"))
            .agg(F.count("*").alias("total_daily_orders"))
    )
```
```python
@dlt.table
@dlt.expect_or_fail("valid_id", "customer_id IS NOT NULL")
@dlt.expect_or_drop("valid_operation", "operation IS NOT NULL")
@dlt.expect("valid_name", "name IS NOT NULL or operation = 'DELETE'")
@dlt.expect("valid_adress", """
    (address IS NOT NULL and 
    city IS NOT NULL and 
    state IS NOT NULL and 
    zip_code IS NOT NULL) or
    operation = "DELETE"
    """)
@dlt.expect_or_drop("valid_email", """
    rlike(email, '^([a-zA-Z0-9_\\\\-\\\\.]+)@([a-zA-Z0-9_\\\\-\\\\.]+)\\\\.([a-zA-Z]{2,5})$') or 
    operation = "DELETE"
    """)
def customers_bronze_clean():
    return (
        dlt.read_stream("customers_bronze")
    )
```
```python
dlt.create_target_table(
    name = "customers_silver")

dlt.apply_changes(
    target = "customers_silver",
    source = "customers_bronze_clean",
    keys = ["customer_id"],
    sequence_by = F.col("timestamp"),
    apply_as_deletes = F.expr("operation = 'DELETE'"),
    except_column_list = ["operation", "_rescued_data"])
```
```python
@dlt.table(
    comment="Total active customers per state")
def customer_counts_state():
    return (
        dlt.read("customers_silver")
            .groupBy("state")
            .agg( 
                F.count("*").alias("customer_count"), 
                F.first(F.current_timestamp()).alias("updated_at")
            )
    )
```

### View

```python
@dlt.view
def subscribed_order_emails_v():
    return (
        dlt.read("orders_silver").filter("notifications = 'Y'").alias("a")
            .join(
                dlt.read("customers_silver").alias("b"), 
                on="customer_id"
            ).select(
                "a.customer_id", 
                "a.order_id", 
                "b.email"
            )
    )
```

# Modos de Execução Delta Live Tables

Podemos acionar a execução de pipelines DLT em dois modos: Triggered e contínuos.


Um modo de atualizar 
```python
DA.dlt_data_factory.load(continuous=True, delay_seconds=0)
```

# Pipeline event logs
Para leitura, deve ser executado com um cluster que esteja no modo de acesso compartilhado ou com um Datawarehouse SQL.

O DLT armazena informações de registro em uma tabela especial chamada `event_log`. Para acessar os dados de registro desta tabela, você deve usar a função com valor de tabela (TVF) **`event_log`**. 

Criando a View


## Audict
```sql
SELECT timestamp, details:user_action:action, details:user_action:user_name
  FROM pipeline_event_log
  WHERE event_type = 'user_action'
```

## Last update ID's
```sql
DECLARE OR REPLACE VARIABLE latest_update_id STRING;
SET VARIABLE latest_update_id =
(SELECT origin.update_id
    FROM pipeline_event_log
    WHERE event_type = 'create_update'
    ORDER BY timestamp DESC LIMIT 1);
```

## Lineage
```sql
SELECT details:flow_definition.output_dataset, details:flow_definition.input_datasets 
  FROM pipeline_event_log
WHERE event_type = 'flow_definition' AND 
      origin.update_id = latest_update_id
```

## Data quality
```sql
SELECT row_expectations.dataset as dataset,
       row_expectations.name as expectation,
       SUM(row_expectations.passed_records) as passing_records,
       SUM(row_expectations.failed_records) as failing_records
FROM
  (SELECT explode(
            from_json(details :flow_progress :data_quality :expectations,
                      "array<struct<name: string, dataset: string, passed_records: int, failed_records: int>>")
          ) row_expectations
   FROM pipeline_event_log
   WHERE event_type = 'flow_progress' AND 
         origin.update_id = latest_update_id
  )
GROUP BY row_expectations.dataset, row_expectations.name
```
