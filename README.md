# Azure-ML-Case-Study

I designed two architectures for this case study. It should be noted that, system C is storing information which are not updated frequently. And System A and B can be treat as streaming data for real-time processing.

### Solution 1.0 -  simple Azure cloud solution 

- Detail
	- Train the model in Azure machine learning studio and publish the model as web service

	- Apply model as UDF in the stream analytic job, the stream analytics job will read data from three systems and predict with the model in real-time.

	- Blob Storage is used to store information from system C as reference blob for Stream Analytics. It also stored output result of stream analytics job.

	- Event Hub is used as messaging system to process streams of data from system A and B. (Apache Kafka)

- Pros and cons
	- Pros
		- it is light weighted and economical, we just need azure Services like blob storage, event hub, stream analytics job, machine learning to handle the whole real-time predict pipeline. Services like blob storage, stream analytics job are very cheap and practical services on Azure cloud.

		- Programming within stream analytics is in SQL-like language, which is easy to learn and powerful enough to handle this use case.
	- Cons
		- Lack of scalability for future rolls out, when the data is large in size, it is hard to query and analyse.
		
		- The update from system C is processed manually, but not automated.
	
		- Lack of data integration. When the data sources are widely different in future, it is better to have some data integration engines to connect and combine them to have more coherent consistent view on the whole business scenario.

		- Lack of interpretability. It is better to have a real-time visualization of the result if necessary.
		
		- Consider the security issue of cloud solution, we can try Azure IOT edge service to obtain better guarantee on the business data.

Further reference- https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-machine-learning-integration-tutorial

### Solution 2.0 – Advanced Azure Cloud solution

- Detail
	- Replace the blob storage with cosmos DB for storing the output blob. It has more advanced performance for querying JSON objects. And JSON is more human readable format compared with other format, which improves the interpretability of the output. 

	- Develop a web app with Azure Storage PHP Client Libraries, to automatically update the changes from system C or other configuration to the solution.

	- Add data warehousing to integrate data from blob storage and cosmos DB for reporting and analysis.

- Pros and cons
	- Pros

		- Better performance and scalability solution with visualization, automated updates/configuration and secure backup for further research.
	- Cons

		- It is an expensive but powerful solution.
		
		- The cost of Cosmos DB is much higher than blob storage, so we just use it to store the result or the result of the most recent month. We can use blob storage to store all the data for backup purpose and use mongo DB API to remove the old data in Cosmos DB regularly.



 