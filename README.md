---
services: hdinsight, cosmosdb
platforms: scala
author: blackmist
---

# Use Apache Kafka with Apache Spark on hdinsight

This is a basic example of using Apache Spark on HDInsight to stream data from Kafka to Azure Cosmos DB. This example uses Spark Structured Streaming and the [Azure Cosmos DB Spark Connector](https://github.com/Azure/azure-cosmosdb-spark). 

This example requires Kafka and Spark on HDInsight 3.6 in the same Azure Virtual Network. It also requires an Azure Cosmos DB SQL API database.

__NOTE__: Apache Kafka and Spark are available as two different cluster types. HDInsight cluster types are tuned for the performance of a specific technology; in this case, Kafka and Spark. To use both together, you must create an Azure Virtual network and then create both a Kafka and Spark cluster on the virtual network. For an example of how to do this using an Azure Resource Manager template, see [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet.json). For an example of using the template with this example, see [Use Apache Spark with Kafka on HDInsight (preview)](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-with-kafka).

## Deploy the Azure resources

This project includes a template that can be used to create these resources on your Azure subscription. To use the template, select the __Deploy to Azure__ button.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure-samples%2Fhdinsight-spark-scala-kafka-cosmosdb%2Fmaster%2Fresources%2Fcreate-linux-based-kafka-spark-cosmos-db.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

## Understand this example

This example uses a Scala application in a Jupyter notebook. The code in the notebook relies on the following pieces of data:

* __Kafka brokers__: The broker process runs on each workernode on the Kafka cluster. The list of brokers is required by the producer component, which writes data to Kafka.

* __Topic name__: The name of the topic that data is written to and read from. The default name used in the example is `tripdata`.

* __Cosmos DB endpoint__: The HTTPS endpoint used to connect to Cosmos DB.

* __Master key__: The key used to authenticate requests to the endpoint.

* __Database__: The name of the Cosmos DB SQL API database that data is written to.

* __Collection__: The document collection within the database.

* __PreferredRegions__: The preferred Azure regions to use when writing to the database.

__NOTE__: The notebooks contain information and links on how to obtain this information.

## To run this example

To use the example Jupyter notebooks, you must upload them to the Jupyter Notebook server on the Spark cluster. Use the following steps to upload the notebooks:

1. In your web browser, use the following URL to connect to the Jupyter Notebook server on the Spark cluster. Replace __CLUSTERNAME__ with the name of your Spark cluster.

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    When prompted, enter the cluster login (admin) and password used when you created the cluster.

2. From the upper right side of the page, use the __Upload__ button to upload the `Stream-taxi-data-to-Kafka.ipynb` file. Select the file in the file browser dialog and select __Open__. 

3. Find the __Stream-taxi-data-to-Kafka.ipynb__ entry in the list of notebooks, and select __Upload__ button beside it.

4. Once the file has uploaded, select the __Stream-taxi-data-to-kafka.ipynb__ entry to open the notebook. To load data into Kafka, follow the instructions in the notebook.

5. Repeat steps 1-3 to upload the `Stream-data-from-Kafka-to-Cosmos-DB.ipynb` document to Kafka. Once the file has uploaded, select the entry to open the notebook. Follow the instructions in the notebook to read the data from Kafka and store it into Cosmos DB.