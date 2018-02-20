---
title: Estilo de arquitetura de Big Data
description: "Descreve os benefícios, os desafios e as melhores práticas para arquiteturas de Big Data no Azure"
author: MikeWasson
ms.openlocfilehash: 4e8b58d5fa0f6a441d70e05ec7d6a0e668712563
ms.sourcegitcommit: b0482d49aab0526be386837702e7724c61232c60
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2017
---
# <a name="big-data-architecture-style"></a><span data-ttu-id="670b4-103">Estilo de arquitetura de Big Data</span><span class="sxs-lookup"><span data-stu-id="670b4-103">Big data architecture style</span></span>

<span data-ttu-id="670b4-104">Uma arquitetura de Big Data foi projetada para lidar com ingestão, processamento e análise de dados grandes ou complexos demais para sistemas de banco de dados tradicionais.</span><span class="sxs-lookup"><span data-stu-id="670b4-104">A big data architecture is designed to handle the ingestion, processing, and analysis of data that is too large or complex for traditional database systems.</span></span>

![](./images/big-data-logical.svg)

 <span data-ttu-id="670b4-105">Soluções de Big Data normalmente envolvem um ou mais dos seguintes tipos de carga de trabalho:</span><span class="sxs-lookup"><span data-stu-id="670b4-105">Big data solutions typically involve one or more of the following types of workload:</span></span>

- <span data-ttu-id="670b4-106">Processamento em lote de fontes Big Data em repouso.</span><span class="sxs-lookup"><span data-stu-id="670b4-106">Batch processing of big data sources at rest.</span></span>
- <span data-ttu-id="670b4-107">Processamento em tempo real de Big Data em movimento.</span><span class="sxs-lookup"><span data-stu-id="670b4-107">Real-time processing of big data in motion.</span></span>
- <span data-ttu-id="670b4-108">Exploração interativa de Big Data.</span><span class="sxs-lookup"><span data-stu-id="670b4-108">Interactive exploration of big data.</span></span>
- <span data-ttu-id="670b4-109">Análise preditiva e machine learning.</span><span class="sxs-lookup"><span data-stu-id="670b4-109">Predictive analytics and machine learning.</span></span>

<span data-ttu-id="670b4-110">A maioria das arquiteturas de Big Data inclui alguns ou todos os seguintes componentes:</span><span class="sxs-lookup"><span data-stu-id="670b4-110">Most big data architectures include some or all of the following components:</span></span>

- <span data-ttu-id="670b4-111">**Fontes de dados**: todas as soluções de Big Data começam com uma ou mais fontes de dados.</span><span class="sxs-lookup"><span data-stu-id="670b4-111">**Data sources**: All big data solutions start with one or more data sources.</span></span> <span data-ttu-id="670b4-112">Os exemplos incluem:</span><span class="sxs-lookup"><span data-stu-id="670b4-112">Examples include:</span></span>

    - <span data-ttu-id="670b4-113">Armazenamentos de dados de aplicativo, como bancos de dados relacionais.</span><span class="sxs-lookup"><span data-stu-id="670b4-113">Application data stores, such as relational databases.</span></span>
    - <span data-ttu-id="670b4-114">Arquivos estáticos produzidos por aplicativos, como arquivos de log do servidor Web.</span><span class="sxs-lookup"><span data-stu-id="670b4-114">Static files produced by applications, such as web server log files.</span></span>
    - <span data-ttu-id="670b4-115">Fontes de dados em tempo real, como dispositivos IoT.</span><span class="sxs-lookup"><span data-stu-id="670b4-115">Real-time data sources, such as IoT devices.</span></span>

- <span data-ttu-id="670b4-116">**Armazenamento de dados**: dados de operações de processamento em lote normalmente são armazenados em um repositório de arquivos distribuído que pode conter amplos volumes de arquivos grandes em vários formatos.</span><span class="sxs-lookup"><span data-stu-id="670b4-116">**Data storage**: Data for batch processing operations is typically stored in a distributed file store that can hold high volumes of large files in various formats.</span></span> <span data-ttu-id="670b4-117">Esse tipo de repositório geralmente é chamado *data lake*.</span><span class="sxs-lookup"><span data-stu-id="670b4-117">This kind of store is often called a *data lake*.</span></span> <span data-ttu-id="670b4-118">As opções para implementar esse armazenamento incluem contêineres de blobs ou Azure Data Lake Store no Armazenamento do Azure.</span><span class="sxs-lookup"><span data-stu-id="670b4-118">Options for implementing this storage include Azure Data Lake Store or blob containers in Azure Storage.</span></span> 

- <span data-ttu-id="670b4-119">**Processamento em lote**: como os conjuntos de dados são muito grandes, geralmente uma solução de Big Data deve processar arquivos de dados usando trabalhos de lote de execução longa para filtrar, agregar e preparar os dados para análise.</span><span class="sxs-lookup"><span data-stu-id="670b4-119">**Batch processing**: Because the data sets are so large, often a big data solution must process data files using long-running batch jobs to filter, aggregate, and otherwise prepare the data for analysis.</span></span> <span data-ttu-id="670b4-120">Normalmente, esses trabalhos envolvem ler arquivos de origem, processá-los e gravar a saída para novos arquivos.</span><span class="sxs-lookup"><span data-stu-id="670b4-120">Usually these jobs involve reading source files, processing them, and writing the output to new files.</span></span> <span data-ttu-id="670b4-121">Opções incluem executar trabalhos de U-SQL no Azure Data Lake Analytics, usar trabalhos Hive, Pig ou de Mapear/Reduzir personalizados em um cluster HDInsight Hadoop ou usar programas de Java, Scala ou Python em um cluster HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="670b4-121">Options include running U-SQL jobs in Azure Data Lake Analytics, using Hive, Pig, or custom Map/Reduce jobs in an HDInsight Hadoop cluster, or using Java, Scala, or Python programs in an HDInsight Spark cluster.</span></span>

- <span data-ttu-id="670b4-122">**Ingestão de mensagens em tempo real**: se a solução inclui fontes em tempo real, a arquitetura deve incluir uma maneira de capturar e armazenar mensagens em tempo real para processamento de fluxo.</span><span class="sxs-lookup"><span data-stu-id="670b4-122">**Real-time message ingestion**: If the solution includes real-time sources, the architecture must include a way to capture and store real-time messages for stream processing.</span></span> <span data-ttu-id="670b4-123">Isso pode ser um armazenamento de dados simples, em que as mensagens de entrada são removidas para uma pasta para processamento.</span><span class="sxs-lookup"><span data-stu-id="670b4-123">This might be a simple data store, where incoming messages are dropped into a folder for processing.</span></span> <span data-ttu-id="670b4-124">No entanto, muitas soluções precisam de um repositório de ingestão de mensagens para atuar como buffer de mensagens e dar suporte a processamento de expansão, entrega confiável e outras semânticas de enfileiramento de mensagem.</span><span class="sxs-lookup"><span data-stu-id="670b4-124">However, many solutions need a message ingestion store to act as a buffer for messages, and to support scale-out processing, reliable delivery, and other message queuing semantics.</span></span> <span data-ttu-id="670b4-125">Opções incluem Hubs de Eventos do Azure, Hubs de IoT do Azure e Kafka.</span><span class="sxs-lookup"><span data-stu-id="670b4-125">Options include Azure Event Hubs, Azure IoT Hubs, and Kafka.</span></span>

- <span data-ttu-id="670b4-126">**Processamento de fluxo**: depois de capturar mensagens em tempo real, a solução deve processá-las filtrando, agregando e preparando os dados para análise.</span><span class="sxs-lookup"><span data-stu-id="670b4-126">**Stream processing**: After capturing real-time messages, the solution must process them by filtering, aggregating, and otherwise preparing the data for analysis.</span></span> <span data-ttu-id="670b4-127">Os dados de fluxo processados são gravados em um coletor de saída.</span><span class="sxs-lookup"><span data-stu-id="670b4-127">The processed stream data is then written to an output sink.</span></span> <span data-ttu-id="670b4-128">O Azure Stream Analytics oferece um serviço de processamento de fluxo gerenciado baseado em consultas SQL em execução perpétua que operam em fluxos não associados.</span><span class="sxs-lookup"><span data-stu-id="670b4-128">Azure Stream Analytics provides a managed stream processing service based on perpetually running SQL queries that operate on unbounded streams.</span></span> <span data-ttu-id="670b4-129">Você também pode usar tecnologias de streaming Apache de software livre, como Storm e Spark Streaming em um cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="670b4-129">You can also use open source Apache streaming technologies like Storm and Spark Streaming in an HDInsight cluster.</span></span>

- <span data-ttu-id="670b4-130">**Armazenamento de dados analíticos**: muitas soluções de Big Data preparam dados para análise e então veiculam os dados processados em um formato estruturado que pode ser consultado usando ferramentas analíticas.</span><span class="sxs-lookup"><span data-stu-id="670b4-130">**Analytical data store**: Many big data solutions prepare data for analysis and then serve the processed data in a structured format that can be queried using analytical tools.</span></span> <span data-ttu-id="670b4-131">O armazenamento de dados analíticos usado para atender a essas consultas pode ser um data warehouse relacional estilo Kimball, como visto na maioria das soluções de BI (business intelligence) tradicionais.</span><span class="sxs-lookup"><span data-stu-id="670b4-131">The analytical data store used to serve these queries can be a Kimball-style relational data warehouse, as seen in most traditional business intelligence (BI) solutions.</span></span> <span data-ttu-id="670b4-132">Como alternativa, os dados podem ser apresentados por meio de uma tecnologia NoSQL de baixa latência, como HBase ou um banco de dados Hive interativo que oferece uma abstração de metadados sobre arquivos de dados no armazenamento de dados distribuído.</span><span class="sxs-lookup"><span data-stu-id="670b4-132">Alternatively, the data could be presented through a low-latency NoSQL technology such as HBase, or an interactive Hive database that provides a metadata abstraction over data files in the distributed data store.</span></span> <span data-ttu-id="670b4-133">O SQL Data Warehouse do Azure fornece um serviço gerenciado para armazenamento de dados em larga escala baseado em nuvem.</span><span class="sxs-lookup"><span data-stu-id="670b4-133">Azure SQL Data Warehouse provides a managed service for large-scale, cloud-based data warehousing.</span></span> <span data-ttu-id="670b4-134">O HDInsight dá suporte a Hive interativo, HBase e Spark SQL, que também pode ser usado para veicular dados para análise.</span><span class="sxs-lookup"><span data-stu-id="670b4-134">HDInsight supports Interactive Hive, HBase, and Spark SQL, which can also be used to serve data for analysis.</span></span>

- <span data-ttu-id="670b4-135">**Análise e relatório**: a meta da maioria das soluções de Big Data é gerar insights sobre os dados por meio de análise e relatórios.</span><span class="sxs-lookup"><span data-stu-id="670b4-135">**Analysis and reporting**: The goal of most big data solutions is to provide insights into the data through analysis and reporting.</span></span> <span data-ttu-id="670b4-136">Para capacitar os usuários a analisar os dados, a arquitetura pode incluir uma camada de modelagem de dados, como um cubo OLAP multidimensional ou um modelo de dados tabular no Azure Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="670b4-136">To empower users to analyze the data, the architecture may include a data modeling layer, such as a multidimensional OLAP cube or tabular data model in Azure Analysis Services.</span></span> <span data-ttu-id="670b4-137">Também pode dar suporte a business intelligence de autoatendimento, usando as tecnologias de modelagem e visualização do Microsoft Power BI ou do Microsoft Excel.</span><span class="sxs-lookup"><span data-stu-id="670b4-137">It might also support self-service BI, using the modeling and visualization technologies in Microsoft Power BI or Microsoft Excel.</span></span> <span data-ttu-id="670b4-138">Análise e relatórios também podem assumir a forma de exploração de dados interativos por cientistas de dados ou analistas de dados.</span><span class="sxs-lookup"><span data-stu-id="670b4-138">Analysis and reporting can also take the form of interactive data exploration by data scientists or data analysts.</span></span> <span data-ttu-id="670b4-139">Para esses cenários, muitos serviços do Azure dão suporte a blocos de anotações analíticos, como Jupyter, permitindo que esses usuários aproveitem suas habilidades existentes com Python ou R. Para exploração de dados em larga escala, você pode usar o Microsoft R Server, seja no modo autônomo ou com Spark.</span><span class="sxs-lookup"><span data-stu-id="670b4-139">For these scenarios, many Azure services support analytical notebooks, such as Jupyter, enabling these users to leverage their existing skills with Python or R. For large-scale data exploration, you can use Microsoft R Server, either standalone or with Spark.</span></span>

- <span data-ttu-id="670b4-140">**Orquestração**: a maioria das soluções de Big Data consiste em operações de processamento de dados repetidos, encapsuladas em fluxos de trabalho, que transformam dados de origem, movem dados entre várias origens e coletores, carregam os dados processados em um armazenamento de dados analíticos ou efetuam o push dos resultados diretamente para um relatório ou painel.</span><span class="sxs-lookup"><span data-stu-id="670b4-140">**Orchestration**: Most big data solutions consist of repeated data processing operations, encapsulated in workflows, that transform source data, move data between multiple sources and sinks, load the processed data into an analytical data store, or push the results straight to a report or dashboard.</span></span> <span data-ttu-id="670b4-141">Para automatizar esses fluxos de trabalho, você pode usar uma tecnologia de orquestração, como Azure Data Factory ou Apache Oozie e Sqoop.</span><span class="sxs-lookup"><span data-stu-id="670b4-141">To automate these workflows, you can use an orchestration technology such Azure Data Factory or Apache Oozie and Sqoop.</span></span>

<span data-ttu-id="670b4-142">O Azure inclui muitos serviços que podem ser usados em uma arquitetura de Big Data.</span><span class="sxs-lookup"><span data-stu-id="670b4-142">Azure includes many services that can be used in a big data architecture.</span></span> <span data-ttu-id="670b4-143">Eles se enquadram em aproximadamente duas categorias:</span><span class="sxs-lookup"><span data-stu-id="670b4-143">They fall roughly into two categories:</span></span>

- <span data-ttu-id="670b4-144">Serviços gerenciados, incluindo Azure Data Lake Store, Azure Data Lake Analytics, Data Warehouse do Azure, Azure Stream Analytics, Hub de Eventos do Azure, Hub IoT do Azure e Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="670b4-144">Managed services, including Azure Data Lake Store, Azure Data Lake Analytics, Azure Data Warehouse, Azure Stream Analytics, Azure Event Hub, Azure IoT Hub, and Azure Data Factory.</span></span>
- <span data-ttu-id="670b4-145">Tecnologias de software livre baseadas na plataforma Apache Hadoop, incluindo HDFS, HBase, Hive, Pig, Spark, Storm, Oozie, Sqoop e Kafka.</span><span class="sxs-lookup"><span data-stu-id="670b4-145">Open source technologies based on the Apache Hadoop platform, including HDFS, HBase, Hive, Pig, Spark, Storm, Oozie, Sqoop, and Kafka.</span></span> <span data-ttu-id="670b4-146">Essas tecnologias estão disponíveis no Azure no serviço Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="670b4-146">These technologies are available on Azure in the Azure HDInsight service.</span></span>

<span data-ttu-id="670b4-147">Essas opções não se excluem mutuamente e muitas soluções combinam tecnologias de software livre com serviços do Azure.</span><span class="sxs-lookup"><span data-stu-id="670b4-147">These options are not mutually exclusive, and many solutions combine open source technologies with Azure services.</span></span>

## <a name="when-to-use-this-architecture"></a><span data-ttu-id="670b4-148">Quando usar esta arquitetura</span><span class="sxs-lookup"><span data-stu-id="670b4-148">When to use this architecture</span></span>

<span data-ttu-id="670b4-149">Considere este estilo de arquitetura quando você precisar:</span><span class="sxs-lookup"><span data-stu-id="670b4-149">Consider this architecture style when you need to:</span></span>

- <span data-ttu-id="670b4-150">Armazenar e processar dados em volumes muito grandes para um banco de dados tradicional.</span><span class="sxs-lookup"><span data-stu-id="670b4-150">Store and process data in volumes too large for a traditional database.</span></span>
- <span data-ttu-id="670b4-151">Transformar dados não estruturados para análise e relatório.</span><span class="sxs-lookup"><span data-stu-id="670b4-151">Transform unstructured data for analysis and reporting.</span></span>
- <span data-ttu-id="670b4-152">Capturar, processar e analisar fluxos não associados de dados em tempo real ou com baixa latência.</span><span class="sxs-lookup"><span data-stu-id="670b4-152">Capture, process, and analyze unbounded streams of data in real time, or with low latency.</span></span>
- <span data-ttu-id="670b4-153">Usar Azure Machine Learning ou Serviços Cognitivos da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="670b4-153">Use Azure Machine Learning or Microsoft Cognitive Services.</span></span>

## <a name="benefits"></a><span data-ttu-id="670b4-154">Benefícios</span><span class="sxs-lookup"><span data-stu-id="670b4-154">Benefits</span></span>

- <span data-ttu-id="670b4-155">**Opções de tecnologia**.</span><span class="sxs-lookup"><span data-stu-id="670b4-155">**Technology choices**.</span></span> <span data-ttu-id="670b4-156">Você pode combinar gerenciados serviços do Azure e tecnologias Apache em clusters HDInsight para aproveitar recursos ou investimentos em tecnologia existentes.</span><span class="sxs-lookup"><span data-stu-id="670b4-156">You can mix and match Azure managed services and Apache technologies in HDInsight clusters, to capitalize on existing skills or technology investments.</span></span>
- <span data-ttu-id="670b4-157">**Desempenho por meio de paralelismo**.</span><span class="sxs-lookup"><span data-stu-id="670b4-157">**Performance through parallelism**.</span></span> <span data-ttu-id="670b4-158">Soluções de Big Data aproveitam paralelismo, possibilitando soluções de alto desempenho dimensionadas para grandes volumes de dados.</span><span class="sxs-lookup"><span data-stu-id="670b4-158">Big data solutions take advantage of parallelism, enabling high-performance solutions that scale to large volumes of data.</span></span>
- <span data-ttu-id="670b4-159">**Escala elástica**.</span><span class="sxs-lookup"><span data-stu-id="670b4-159">**Elastic scale**.</span></span> <span data-ttu-id="670b4-160">Todos os componentes da arquitetura de Big Data dão suporte a provisionamento de expansão para que você possa ajustar sua solução para cargas de trabalho grandes ou pequenas e pagar somente pelos recursos que usa.</span><span class="sxs-lookup"><span data-stu-id="670b4-160">All of the components in the big data architecture support scale-out provisioning, so that you can adjust your solution to small or large workloads, and pay only for the resources that you use.</span></span>
- <span data-ttu-id="670b4-161">**Interoperabilidade com soluções existentes**.</span><span class="sxs-lookup"><span data-stu-id="670b4-161">**Interoperability with existing solutions**.</span></span> <span data-ttu-id="670b4-162">Os componentes da arquitetura de Big Data também são usados para processamento IoT e soluções de BI empresariais, permitindo que você crie uma solução integrada entre cargas de trabalho de dados.</span><span class="sxs-lookup"><span data-stu-id="670b4-162">The components of the big data architecture are also used for IoT processing and enterprise BI solutions, enabling you to create an integrated solution across data workloads.</span></span>

## <a name="challenges"></a><span data-ttu-id="670b4-163">Desafios</span><span class="sxs-lookup"><span data-stu-id="670b4-163">Challenges</span></span>

- <span data-ttu-id="670b4-164">**Complexidade**.</span><span class="sxs-lookup"><span data-stu-id="670b4-164">**Complexity**.</span></span> <span data-ttu-id="670b4-165">Soluções de Big Data podem ser extremamente complexas, com vários componentes para lidar com a ingestão de dados de várias fontes de dados.</span><span class="sxs-lookup"><span data-stu-id="670b4-165">Big data solutions can be extremely complex, with numerous components to handle data ingestion from multiple data sources.</span></span> <span data-ttu-id="670b4-166">Pode ser um desafio criar, testar e solucionar problemas de processos de Big Data.</span><span class="sxs-lookup"><span data-stu-id="670b4-166">It can be challenging to build, test, and troubleshoot big data processes.</span></span> <span data-ttu-id="670b4-167">Além disso, pode haver um grande número de definições de configuração em vários sistemas que devem ser usados para otimizar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="670b4-167">Moreover, there may be a large number of configuration settings across multiple systems that must be used in order to optimize performance.</span></span>
- <span data-ttu-id="670b4-168">**Conjunto de qualificações**.</span><span class="sxs-lookup"><span data-stu-id="670b4-168">**Skillset**.</span></span> <span data-ttu-id="670b4-169">Muitas tecnologias de Big Data são altamente especializadas e usam frameworks e idiomas que não são típicos de arquiteturas de aplicativo mais gerais.</span><span class="sxs-lookup"><span data-stu-id="670b4-169">Many big data technologies are highly specialized, and use frameworks and languages that are not typical of more general application architectures.</span></span> <span data-ttu-id="670b4-170">Por outro lado, as tecnologias de Big Data estão gerando novas APIs que se baseiam em linguagens mais estabelecidas.</span><span class="sxs-lookup"><span data-stu-id="670b4-170">On the other hand, big data technologies are evolving new APIs that build on more established languages.</span></span> <span data-ttu-id="670b4-171">Por exemplo, a linguagem U-SQL no Azure Data Lake Analytics baseia-se em uma combinação de Transact-SQL e C#.</span><span class="sxs-lookup"><span data-stu-id="670b4-171">For example, the U-SQL language in Azure Data Lake Analytics is based on a combination of Transact-SQL and C#.</span></span> <span data-ttu-id="670b4-172">Da mesma forma, APIs com base em SQL estão disponíveis para Hive, HBase e Spark.</span><span class="sxs-lookup"><span data-stu-id="670b4-172">Similarly, SQL-based APIs are available for Hive, HBase, and Spark.</span></span>
- <span data-ttu-id="670b4-173">**Maturidade da tecnologia**.</span><span class="sxs-lookup"><span data-stu-id="670b4-173">**Technology maturity**.</span></span> <span data-ttu-id="670b4-174">Muitas das tecnologias usadas em Big Data estão em evolução.</span><span class="sxs-lookup"><span data-stu-id="670b4-174">Many of the technologies used in big data are evolving.</span></span> <span data-ttu-id="670b4-175">Embora tecnologias Hadoop centrais, como Hive e Pig, tenham se estabilizado, tecnologias emergentes, como Spark, apresentam grandes alterações e aprimoramentos a cada nova versão.</span><span class="sxs-lookup"><span data-stu-id="670b4-175">While core Hadoop technologies such as Hive and Pig have stabilized, emerging technologies such as Spark introduce extensive changes and enhancements with each new release.</span></span> <span data-ttu-id="670b4-176">Serviços gerenciados, como Azure Data Lake Analytics e Azure Data Factory, são relativamente jovens em comparação a outros serviços do Azure e provavelmente evoluirão ao longo do tempo.</span><span class="sxs-lookup"><span data-stu-id="670b4-176">Managed services such as Azure Data Lake Analytics and Azure Data Factory are relatively young, compared with other Azure services, and will likely evolve over time.</span></span>
- <span data-ttu-id="670b4-177">**Segurança**.</span><span class="sxs-lookup"><span data-stu-id="670b4-177">**Security**.</span></span> <span data-ttu-id="670b4-178">Soluções de Big Data normalmente se baseiam em armazenar todos os dados estáticos em um data lake centralizado.</span><span class="sxs-lookup"><span data-stu-id="670b4-178">Big data solutions usually rely on storing all static data in a centralized data lake.</span></span> <span data-ttu-id="670b4-179">Proteger o acesso a esses dados pode ser desafiador, especialmente quando os dados devem ser ingeridos e consumidos por vários aplicativos e plataformas.</span><span class="sxs-lookup"><span data-stu-id="670b4-179">Securing access to this data can be challenging, especially when the data must be ingested and consumed by multiple applications and platforms.</span></span>

## <a name="best-practices"></a><span data-ttu-id="670b4-180">Práticas recomendadas</span><span class="sxs-lookup"><span data-stu-id="670b4-180">Best practices</span></span>

- <span data-ttu-id="670b4-181">**Aproveitar o paralelismo**.</span><span class="sxs-lookup"><span data-stu-id="670b4-181">**Leverage parallelism**.</span></span> <span data-ttu-id="670b4-182">A maioria das tecnologias de processamento de Big Data distribui a carga de trabalho em várias unidades de processamento.</span><span class="sxs-lookup"><span data-stu-id="670b4-182">Most big data processing technologies distribute the workload across multiple processing units.</span></span> <span data-ttu-id="670b4-183">Isso exige que os arquivos de dados estáticos sejam criados e armazenados em um formato divisível.</span><span class="sxs-lookup"><span data-stu-id="670b4-183">This requires that static data files are created and stored in a splittable format.</span></span> <span data-ttu-id="670b4-184">Sistemas de arquivos distribuídos, como HDFS, podem otimizar o desempenho de leitura e gravação, e o processamento real é executado por vários nós de cluster em paralelo, o que reduz o tempo de trabalho geral.</span><span class="sxs-lookup"><span data-stu-id="670b4-184">Distributed file systems such as HDFS can optimize read and write performance, and the actual processing is performed by multiple cluster nodes in parallel, which reduces overall job times.</span></span>

- <span data-ttu-id="670b4-185">**Dados de partição**.</span><span class="sxs-lookup"><span data-stu-id="670b4-185">**Partition data**.</span></span> <span data-ttu-id="670b4-186">Geralmente, o processamento de lote ocorre conforme uma agenda recorrente &mdash; por exemplo, semanal ou mensal.</span><span class="sxs-lookup"><span data-stu-id="670b4-186">Batch processing usually happens on a recurring schedule &mdash; for example, weekly or monthly.</span></span> <span data-ttu-id="670b4-187">Arquivos de dados de partição e estruturas de dados como tabelas, com base em períodos de temporais que correspondem à agenda de processamento.</span><span class="sxs-lookup"><span data-stu-id="670b4-187">Partition data files, and data structures such as tables, based on temporal periods that match the processing schedule.</span></span> <span data-ttu-id="670b4-188">Isso simplifica a ingestão de dados e o agendamento de trabalho, além de tornar mais fácil solucionar problemas de falhas.</span><span class="sxs-lookup"><span data-stu-id="670b4-188">That simplifies data ingestion and job scheduling, and makes it easier to troubleshoot failures.</span></span> <span data-ttu-id="670b4-189">Além disso, o particionamento de tabelas usadas em consultas Hive, U-SQL ou SQL pode melhorar significativamente o desempenho da consulta.</span><span class="sxs-lookup"><span data-stu-id="670b4-189">Also, partitioning tables that are used in Hive, U-SQL, or SQL queries can significantly improve query performance.</span></span>

- <span data-ttu-id="670b4-190">**Aplicar semântica de esquema na leitura**.</span><span class="sxs-lookup"><span data-stu-id="670b4-190">**Apply schema-on-read semantics**.</span></span> <span data-ttu-id="670b4-191">Usar um data lake permite combinar o armazenamento de arquivos em vários formatos, sejam estruturados, semiestruturados ou não estruturados.</span><span class="sxs-lookup"><span data-stu-id="670b4-191">Using a data lake lets you to combine storage for files in multiple formats, whether structured, semi-structured, or unstructured.</span></span> <span data-ttu-id="670b4-192">Use semântica de *esquema na leitura*, que projeta um esquema nos dados quando os dados estão sendo processados, não quando estão armazenados.</span><span class="sxs-lookup"><span data-stu-id="670b4-192">Use *schema-on-read* semantics, which project a schema onto the data when the data is processing, not when the data is stored.</span></span> <span data-ttu-id="670b4-193">Isso integra flexibilidade à solução e evita gargalos durante a ingestão de dados causados pela verificação de tipo e a validação de dados.</span><span class="sxs-lookup"><span data-stu-id="670b4-193">This builds flexibility into the solution, and prevents bottlenecks during data ingestion caused by data validation and type checking.</span></span>

- <span data-ttu-id="670b4-194">**Processar dados no local**.</span><span class="sxs-lookup"><span data-stu-id="670b4-194">**Process data in-place**.</span></span> <span data-ttu-id="670b4-195">Soluções de BI tradicionais geralmente usam um processo ETL (extração, transformação e carregamento) para mover dados para um data warehouse.</span><span class="sxs-lookup"><span data-stu-id="670b4-195">Traditional BI solutions often use an extract, transform, and load (ETL) process to move data into a data warehouse.</span></span> <span data-ttu-id="670b4-196">Com maiores volumes de dados e uma maior variedade de formatos, soluções de Big Data geralmente usam variações de ETL, como TEL (transformação, extração e carregamento).</span><span class="sxs-lookup"><span data-stu-id="670b4-196">With larger volumes data, and a greater variety of formats, big data solutions generally use variations of ETL, such as transform, extract, and load (TEL).</span></span> <span data-ttu-id="670b4-197">Com essa abordagem, os dados são processados no armazenamento de dados distribuídos, transformando-os na estrutura necessária, antes de mover os dados transformados para um armazenamento de dados analíticos.</span><span class="sxs-lookup"><span data-stu-id="670b4-197">With this approach, the data is processed within the distributed data store, transforming it to the required structure, before moving the transformed data into an analytical data store.</span></span>

- <span data-ttu-id="670b4-198">**Equilibrar custos de tempo e utilização**.</span><span class="sxs-lookup"><span data-stu-id="670b4-198">**Balance utilization and time costs**.</span></span> <span data-ttu-id="670b4-199">Para trabalhos de processamento em lotes, é importante considerar dois fatores: custo unitário de nós de computação e custo por minuto de usar esses nós para concluir o trabalho.</span><span class="sxs-lookup"><span data-stu-id="670b4-199">For batch processing jobs, it's important to consider two factors: The per-unit cost of the compute nodes, and the per-minute cost of using those nodes to complete the job.</span></span> <span data-ttu-id="670b4-200">Por exemplo, um trabalho em lotes pode levar oito horas com quatro nós de cluster.</span><span class="sxs-lookup"><span data-stu-id="670b4-200">For example, a batch job may take eight hours with four cluster nodes.</span></span> <span data-ttu-id="670b4-201">No entanto, pode ser que o trabalho use todos os quatro nós somente durante as primeiras duas horas, sendo apenas dois nós necessários depois disso.</span><span class="sxs-lookup"><span data-stu-id="670b4-201">However, it might turn out that the job uses all four nodes only during the first two hours, and after that, only two nodes are required.</span></span> <span data-ttu-id="670b4-202">Nesse caso, executar todo o trabalho em dois nós aumentaria o tempo total do trabalho, mas não o duplicaria, de modo que o custo total seria menor.</span><span class="sxs-lookup"><span data-stu-id="670b4-202">In that case, running the entire job on two nodes would increase the total job time, but would not double it, so the total cost would be less.</span></span> <span data-ttu-id="670b4-203">Em alguns cenários de negócios, mais tempo de processamento pode ser preferível ao custo mais alto de usar recursos de cluster subutilizados.</span><span class="sxs-lookup"><span data-stu-id="670b4-203">In some business scenarios, a longer processing time may be preferable to the higher cost of using under-utilized cluster resources.</span></span>

- <span data-ttu-id="670b4-204">**Separar os recursos de cluster**.</span><span class="sxs-lookup"><span data-stu-id="670b4-204">**Separate cluster resources**.</span></span> <span data-ttu-id="670b4-205">Ao implantar clusters HDInsight, você normalmente alcança um melhor desempenho provisionando recursos de cluster separados para cada tipo de carga de trabalho.</span><span class="sxs-lookup"><span data-stu-id="670b4-205">When deploying HDInsight clusters, you will normally achieve better performance by provisioning separate cluster resources for each type of workload.</span></span> <span data-ttu-id="670b4-206">Por exemplo, embora clusters do Spark incluam Hive, se você precisar executar amplo processamento com Hive e Spark, deverá considerar implantar clusters Spark e Hadoop dedicados separados.</span><span class="sxs-lookup"><span data-stu-id="670b4-206">For example, although Spark clusters include Hive, if you need to perform extensive processing with both Hive and Spark, you should consider deploying separate dedicated Spark and Hadoop clusters.</span></span> <span data-ttu-id="670b4-207">Da mesma forma, se você estiver usando HBase e Storm para processamento de fluxo de baixa latência e Hive para processamento em lotes, considere clusters separados para Storm, HBase e Hadoop.</span><span class="sxs-lookup"><span data-stu-id="670b4-207">Similarly, if you are using HBase and Storm for low latency stream processing and Hive for batch processing, consider separate clusters for Storm, HBase, and Hadoop.</span></span>

- <span data-ttu-id="670b4-208">**Orquestrar a ingestão de dados**.</span><span class="sxs-lookup"><span data-stu-id="670b4-208">**Orchestrate data ingestion**.</span></span> <span data-ttu-id="670b4-209">Em alguns casos, aplicativos de negócios existentes podem gravar arquivos de dados para processamento em lote diretamente em contêineres do Azure Storage Blob, em que podem ser consumidos pelo HDInsight ou pelo Azure Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="670b4-209">In some cases, existing business applications may write data files for batch processing directly into Azure storage blob containers, where they can be consumed by HDInsight or Azure Data Lake Analytics.</span></span> <span data-ttu-id="670b4-210">No entanto, você geralmente precisará orquestrar a ingestão de dados de fontes de dados externas ou locais para o data lake.</span><span class="sxs-lookup"><span data-stu-id="670b4-210">However, you will often need to orchestrate the ingestion of data from on-premises or external data sources into the data lake.</span></span> <span data-ttu-id="670b4-211">Use um fluxo de trabalho de orquestração ou um pipeline, como aqueles compatíveis com Azure Data Factory ou Oozie, para fazer isso de maneira previsível e gerenciável centralmente.</span><span class="sxs-lookup"><span data-stu-id="670b4-211">Use an orchestration workflow or pipeline, such as those supported by Azure Data Factory or Oozie, to achieve this in a predictable and centrally manageable fashion.</span></span>

- <span data-ttu-id="670b4-212">**Limpar dados confidenciais cedo**.</span><span class="sxs-lookup"><span data-stu-id="670b4-212">**Scrub sensitive data early**.</span></span> <span data-ttu-id="670b4-213">O fluxo de trabalho de ingestão de dados deve remover dados confidenciais no início do processo para evitar armazená-los no data lake.</span><span class="sxs-lookup"><span data-stu-id="670b4-213">The data ingestion workflow should scrub sensitive data early in the process, to avoid storing it in the data lake.</span></span>