****### Elastic Seach Stack
- This course focuses on Elasticsearch, but I want to take a moment to talk about a few  other technologies that are related to Elasticsearch.  Together with Elasticsearch, they form what’s referred to as the **Elastic Stack**, so let’s  talk a bit about that.  
- If you already know what the Elastic Stack is all about or if you just care about Elasticsearch,  then you are welcome to skip this lecture, but I do recommend that you stick around.  The Elastic Stack consists of technologies developed and maintained by the company behind  Elasticsearch.  We just talked about Elasticsearch, which is the heart of the Elastic Stack, meaning  that the technologies that I am about to tell you about, generally interact with Elasticsearch,  although it’s optional for some of them.  However, there is a strong synergy between the technologies, so they are frequently used  together for various purposes.  Alright, so let’s begin by talking about something called Kibana.  Kibana is an analytics and visualization platform, which lets you easily visualize data from  Elasticsearch and analyze it to make sense of it.  You can think of Kibana as an Elasticsearch dashboard where you can create visualizations  such as pie charts, line charts, and many others.  You can plot your website’s visitors onto a map and show traffic in real time, for instance.  You can aggregate website traffic by browser and find out which browsers are important  to support based on your particular audience.  Kibana is also where you configure change detection and forecasting that I mentioned  in the previous lecture.  Kibana also provides an interface to manage certain parts of Elasticsearch, such as authentication  and authorization.  Generally speaking, you can think of Kibana as a web interface to the data that is stored  within Elasticsearch.  It uses the data from Elasticsearch and basically just sends queries using the same REST API  that I previously mentioned.  It just provides an interface for building those queries and lets you configure how to  display the results.  This can save you a lot of time because you don’t have to implement all of this yourself.  You can build dashboards where you place a number of metrics and visualizations.  You can create a dashboard for system administrators that monitors the performance of servers,  such as CPU and memory usage.  You can then create a dashboard for developers which monitors the number of application errors  and API response times.  A third dashboard could be a dashboard with KPIs (being short for Key Points of Interest)  for management, keeping track of how the business performs, such as the number of sales, revenue, etc.  As you can see, you are likely to store a lot of different kinds of data in Elasticsearch,  apart from data that you want to search and present to your external users.  In fact, you might not even use Elasticsearch for implementing search functionality at all;  using it as an analytics platform together with Kibana, is a perfectly valid and common  use case as well.  On your screen now, you can see a couple of screenshots from the Kibana interface, giving  you an idea of what it looks like.  I cannot cover all of the features of Kibana here, because this is just a quick overview  of the Elastic Stack.  There is a public demo of Kibana though, which has some pre-configured dashboards and sample data.  So if you are curious to see what it looks like in action, then be sure to check that out.  I have attached a link to it to this lecture.  Next up, we have a tool named Logstash.  Traditionally, Logstash has been used to process logs from applications and send them to Elasticsearch,  hence the name.  That’s still a popular use case, but Logstash has evolved into a more general purpose tool,  meaning that Logstash is a data processing pipeline.  The data that Logstash receives, will be handled as events, which can be anything of your choice.  They could be log file entries, ecommerce orders, customers, chat messages, etc.  These events are then processed by Logstash and shipped off to one or more destinations.  A couple of examples could be Elasticsearch, a Kafka queue, an e-mail message, or to an  HTTP endpoint.  A Logstash pipeline consists of three parts - or stages - inputs, filters, and outputs.  Each stage can make use of a so-called plugin.  An input plugin could be a file, for instance, meaning that Logstash will read events from  a given file.  It could also be that we are sending events to Logstash over HTTP, or we could look up  rows from a relational database, or listen to a Kafka queue.  There are a lot of input plugins, so chances are that you will find what you need.  Okay, so while input plugins are how Logstash receives events, filter plugins are all about  how Logstash should process them.  Here we can parse CSV, XML, or JSON, for instance.  We can also do data enrichment, such as looking up an IP address and resolving its geographical  location, or look up data in a relational database.  An output plugin is where we send the processed events to.  Formally, those places are called stashes.  You can see a couple of examples on the right-hand side of the diagram.  So in a nutshell, Logstash receives events from one or more inputs, processes them, and  sends them to one or more stashes.  You can have multiple pipelines running within the same Logstash instance if you want to,  and Logstash is horizontally scalable.  A Logstash pipeline is defined in a proprietary markup format that is similar to JSON.  It’s not only a markup language, however, as we can also add conditional statements  and make a pipeline dynamic.  Let’s go through a simple example before moving on to other components of the Elastic Stack.  Suppose that we want to process access logs from a web server.  We can instruct Logstash to read the access log line by line and process each line as an event.  This is easy by using the input plugin named “file,” but as you will see later in this  lecture, there is a handy tool named Beats that is better for this task.  Once Logstash has received a line, we can process it.  A line from an access log file contains various pieces of information, but Logstash just receives  the line as a text string, so we need to parse this string to make some sense of it.  In other words, we need to structure this piece of unstructured data.  What we can do, is to write a so-called Grok pattern, which is basically like a regular  expression, to match pieces of text and store that within fields.  This means that we can have a field for the status code, one for the request path, another  for the IP address, and so on.  This is very useful, because wherever we want to send the processed events off to, we surely  don’t want to send a long text string.  Suppose that we want to send the log entries to Elasticsearch; we want to have designated  fields for each of the pieces of information within the documents that we add to Elasticsearch.  So for an access log entry, the document could look something like what you see on your screen now.  So that was an example of a common use case of Logstash; to receive access log entries,  process them, and send the results to Elasticsearch, or any other stash of your choice.  Of course Logstash can do much more than this, but this was just a quick overview.  Next up, let’s talk about a part of the Elastic Stack named X-Pack.  X-Pack is actually a pack of features that adds additional functionality to Elasticsearch  and Kibana.  It adds functionality in various feature areas, so let’s go through the most important ones.  First, we have security.  X-Pack adds both authentication and authorization to both Kibana and Elasticsearch.  In regards to authentication, Kibana can integrate with LDAP, Active Directory and other technologies  to provide authentication.  You can also add users and roles, and configure exactly what a given user or role is allowed to access.  This is very useful, as different people might need different privileges.  For example, a marketing department or management team should probably not be allowed to make  any changes to data, but should have read-only access to certain data of interest to them.  Next up, X-Pack enables you to monitor the performance of the Elastic Stack, being Elasticsearch,  Logstash, and Kibana.  Specifically, you can see CPU and memory usage, disk space, and many other useful metrics,  which enable you to stay on top of the performance and easily detect any problems.  What’s more, is that you can even set up alerting and be notified if something unusual happens.  Alerting is not specific to the monitoring of the Elastic Stack though, as you can set  up alerting for anything you want.  For example, you might want to be alerted if CPU or memory usage of your web servers  go through the roof, or if there is a spike in application errors.  Or perhaps you want to stay on top of suspicious user behavior, such as if a given user has  signed in from three different countries within the past hour.  You can then be notified by e-mail, Slack, or others when something goes wrong.  With reporting, you can export the Kibana visualizations and dashboards to PDF files.  You can generate reports on demand, or schedule them and then receive them directly within  your e-mail inbox.  You might want daily or weekly reports of your company’s key performance indicators,  or any useful information to engineers or system administrators.  Reports can also be triggered by specifying conditions, kind of as with alerting, so you  define rules for when the reports should be generated and delivered.  You can even have the reports generated with your own logo for a more professional look,  or perhaps for sharing reports with customers.  Apart from exporting visualizations and data as PDF files, you can also export data as  CSV files, which could be useful for importing data into a spreadsheet, for instance.  X-Pack is also what enables Kibana to do the machine learning that we talked about in the  previous lecture.  So basically, the functionality is provided by X-Pack, and the interface is provided by Kibana.  Let’s quickly recap on what you can do with machine learning.  First, we can do abnormality detection, such as detecting unusual changes in data based  on what the neural network believes is normal.  This can be tied together with alerting, so we could have a machine learning job watching  the number of daily visits to our website.  If there is a significant drop - or increase for that matter - this will be identified,  and we can optionally be notified by e-mail or something like that.  The other thing we can do, is to forecast future values.  This is especially useful for capacity planning, such as figuring out how many visitors our  website is going to get in the future.  This could be helpful for spinning up additional servers if not using auto scaling, or to have  more support agents available.  An example could be that a webshop gets a lot more traffic in both November and December  due to Black Friday and Christmas sales.  Next, let’s talk about a feature called Graph.  Graph is all about the relationships in your data.  An example could be that when someone is viewing a product on an ecommerce website, we want  to show related products on that page as well.  Or perhaps suggest the next song in a music playing app such as Spotify, based on what  the listener likes.  For example, if you like The Beatles, there is a good chance that you also like Pink Floyd.  But to make this work, it is important to distinguish between popular and relevant.  Suppose that a lot of people listen to Linkin Park and they also enjoy listening to Mozart  every now and then.  That does not suggest that the two are related, but the strong link between them is just caused  by the fact that they are both relatively popular.  For example, if you go out on the street and ask 10 people if they use Google, most of  them will say yes.  But that doesn’t mean that they have anything else in common; that’s just because Google  is so popular for all kinds of different people.  On the other hand, if you ask ten people if they use Stackoverflow, the ones that say  yes do have something in common, because Stackoverflow is specifically related to programming.  So essentially what we are looking for, is the uncommonly common, because that says something  about relevance and not popularity.  The point is that purely looking at the relationships in data without looking at relevance, can be misleading.  That’s why Graph uses the relevance capabilities of Elasticsearch when determining what is  related and what isn’t.  Graph exposes an API that you can use to integrate this into applications.  Apart from the aforementioned examples, another example could be to send out product recommendations  in a newsletter based on a person’s purchase history.  Graph also provides a plugin for Kibana where you can visualize your data as an interactive graph.  This is both very useful when you know what you are looking for, but it is perhaps even  more useful when you don’t.  The UI lets you drill down, navigate, and explore the relations in your data that you  maybe didn’t know existed.  So that’s a quick introduction to Graph.  It works out of the box, so you don’t need to index data in a specific way or change  anything to use it.  The last feature of X-Pack is one named SQL.  If you have worked with relational databases, then you are familiar with their query language,  being SQL.  In Elasticsearch, we query documents with a proprietary query language named the Query DSL.  This is essentially a JSON object defining the query.  The Query DSL is really flexible and you can do everything with it, but it might be a bit  verbose at times.  For developers who come from a background of working with relational databases, it would  be easier to just work with SQL, wouldn’t it?  That’s possible, so you can send SQL queries to Elasticsearch over HTTP, or alternatively  use the provided JDBC driver.  What Elasticsearch does, is to translate the SQL query into the Query DSL format behind  the scenes, so internally the query is handled the same way after that translation.  What’s cool, is that there is a Translate API where we can send SQL queries to, and  Elasticsearch will respond with the Query DSL that the SQL query was translated into.  So if you need some help writing the Query DSL query, you can write it as SQL, translate  it, and get a great starting point for your query.  That’s a great way to get started if you really want to stick with SQL.  Personally, I see this as a helper tool for development and for getting started.  Once you get familiar with the Query DSL, I do think that’s what you should use, but  that’s just my personal opinion.  And that’s it for X-Pack!  Finally, right?  Now we just have one thing left to cover; something called Beats.  Beats is a collection of so-called data shippers.  They are lightweight agents with a single purpose that you install on servers, which  then send data to Logstash or Elasticsearch.  There are a number data shippers - called beats - that collect different kinds of data  and serve different purposes.  For example, there is a beat named Filebeat, which is used for collecting log files and  sending the log entries off to either Logstash or Elasticsearch.  Filebeat ships with modules for common log files, such as nginx, the Apache web server,  or MySQL.  This is very useful for collecting log files such as access logs or error logs.  Another beat worth mentioning, is Metricbeat, which collects system-level and/or service metrics.  You can use it for collecting CPU and memory usage for the operating system, and any services  running on the system as well.  Metricbeat also ships with modules for popular services such as nginx or MySQL, so you can  monitor how they perform.  There are more beats available as you can see here, but Filebeat and Metricbeat are  the ones that are most commonly used, so be sure to check out the documentation in case  you need something other than we have just talked about.  Alright, so let’s put all of the pieces together now.  The center of it all is Elasticsearch which contains the data.  Ingesting data into Elasticsearch can be done with Beats and/or Logstash, but also directly  through Elasticsearch’s API.  Kibana is a user interface that sits on top of Elasticsearch and lets you visualize the  data that it retrieves from Elasticsearch through the API.  There is nothing Kibana does that you cannot build yourself, and all of the data that it  retrieves is accessible through the Elasticsearch API.  That being said, it’s a really powerful tool that will likely save you a lot of time,  as you probably won’t have to build your own dashboards from scratch.  Then we have X-Pack which enables additional features, such as machine learning for Elasticsearch  and Kibana, or management of Logstash pipelines in Kibana.  This is all referred to as the Elastic Stack.  You might, however, have heard of something called the ELK stack before.  This refers to Elasticsearch, Logstash, and Kibana, because these three tools are so frequently  used together.  The term originates from before there was something called Beats and X-Pack, creating  what is now known as the Elastic Stack.  So the Elastic Stack is a superset of the ELK stack, but these days you will mainly  refer to the Elastic Stack.  As you can hopefully see, covering all of this in a single course is nearly impossible,  as each of these tools provide so many capabilities, and if I were to include all of them in the  course, I would just barely scratch the surface.  Now that you know what the Elastic Stack is all about, let’s take a moment to talk about  some common architectures and how these technologies are used together.

![[Pasted image 20220706051142.png]]
![[Pasted image 20220705231808.png]]


## ELK Stack
![[Pasted image 20220705231850.png]]



### Beat modules 
![[Pasted image 20220705231705.png]]

## Architecture of ELK:

Now that we have successfully installed Elasticsearch and Kibana, let's talk a bit about the architecture  of Elasticsearch.  
- So when we started up Elasticsearch, what actually happened, was that we started up  a node.  
- **A node** is **essentially an instance of Elasticsearch** that **stores data.**  To ensure that we can store many terabytes of data if we need to, we can run as many  nodes as we want.  
- Each **node will then store a part of our data.**  This way, we can store data on **multiple virtual or physical machines**, which enables us to  store many terabytes of data, even if each machine only has a disk capacity of a few  hundred gigabytes.  
- A **node refers to an instance of Elasticsearch and not a machine**, so you can run **any number  of nodes on the same machine.**  This means that on your development machine, you can start up five nodes if you want to,  without having to deal with virtual machines or containers.  
- That being said, you should typically separate things in a production environment so that  each node runs on a dedicated machine, a virtual machine, or within a container.  
- You might wonder how all of this is coordinated.  **How is the data distributed across the nodes**, and **how does Elasticsearch know where some  given data is stored?**  
	- We will discuss this in detail in the next couple of lectures, but the short answer to  the question is that each node belongs to what is called a cluster.  
	- A **cluster is a collection of related nodes** that **together contain all of our data**.  
	- We can have many clusters if we want to, but one is usually enough.  
	- Clusters are completely independent of each other by default.  
	- It is possible to perform cross-cluster searches, but it is not very common to do so.  
	- You might run multiple clusters that serve different purposes; for instance, you could  have a cluster for powering the search of an e-commerce application, and another for  Application Performance Management (abbreviated APM).  
	- The **reasons for splitting things** into **multiple clusters**, are **typically to separate things  logically, and to be able to configure things differently.**  
	- That being said, one cluster is typically enough, so we will be working with a single  cluster throughout this course.  
	
##### How do we create  a cluster?  **What actually happened when we started up the node, was that a cluster was formed automatically**.  

- **When a node starts up,** it will **either join an existing cluster** if configured to do so,  or it **will create its own cluster consisting of just that node.**  
- An Elasticsearch node will always be part of a cluster, even if there are no other nodes.  
- There are some problems with only having a single node in terms of availability and scalability,  but for development purposes, it is perfectly fine to have a cluster consisting of a single  node.  

##### How data  is organized and stored in Elastic Search?
- **Each unit of data that you store within your cluster** is called a **document.**  
	- **Documents are JSON objects containing** whatever data you desire.  
	- When you **index a document**, the **original JSON object that you sent to Elasticsearch is stored**  along with **some metadata that Elasticsearch uses internally**.  
	![[Pasted image 20220706045109.png]]
		- **Example:** If you want to store a person, your object could look something like this.  As you can see, the object contains two fields, being "name" and "country."  
		- You can add any fields that you want, so you are in total control of this object.  
		- To the right, you can see an example of how this object would be stored within Elasticsearch.  
	- The JSON object that we send to Elasticsearch is **stored within a field named "_source,"**  and Elasticsearch then stores some metadata together with that object.  

##### So how are documents organized, you might wonder?  
- The **answer is within indices**.  
	- **Every document within Elasticsearch**, is stored within an **index.**  
	- An **index groups documents** together logically, as well as provide configuration options that  are related to scalability and availability, which we will take a look at a bit later.  
	- An index is therefore a collection of documents that have similar characteristics and are  logically related.  
		- **For example,** the document that you saw a moment ago, could be stored within an index named  "people." 
		- We could then have a different index named "departments," containing  a number of departments, each being stored as a document.  
		- An index may contain as many documents as you want, so there is no hard limit.  
		- When we get to **searching for data**, you will see that **we specify the index that we want  to search for documents,** meaning that **search queries are actually run against indices.**  

##### Alright, let's take a short moment to recap on the key takeaways from this lecture.  
![[Pasted image 20220706045018.png]]
- An Elasticsearch cluster is a collection of nodes, which are responsible for storing data.  
- A node refers to a running instance of Elasticsearch, which can be running on a physical or virtual  machine, or within a Docker container, for instance.  
- Data is stored as documents, with a document being a unit of information.  A document can represent anything you want.  A few examples are a person, company, product, review, event, etc.  
- Each document belongs to an index, which is a way to logically group together related  documents.  There is more to it than that, but we will get to that later.


#### Query the Cluster health using the Dev Tools from Management
1. Click on the left side menu bar and go to Management section and click on Dev Tools.
2. Add the query shown in the bellow screenshot to get the information about the cluster : `GET /_cluster/health`
![[Pasted image 20220706045615.png]]


#### Inspect the nodes in the cluster
- [Elastic seach doc for node API nodes](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-nodes-info.html)
- [Elastic seach doc for cat/nodes Api](https://www.elastic.co/guide/en/elasticsearch/reference/current/cat-nodes.html_
- use the following http query to get information about the nodes `GET /_cat/nodes?v`
	- `?` symbolises **paramters** and here `v` will add the descrition for field of output.
	![[Pasted image 20220706050025.png]]



### Inspect the indices in the ELK cluster
- use the following http query to get information about the nodes `GET /_cat/indices?v`
	- `?` symbolises **paramters** and here `v` will add the descrition for field of output.
	- ![[Pasted image 20220706050413.png]]
- To check all the **system level indices** we can use `v&expand_wildcards=all`
	- ![[Pasted image 20220706050707.png]]

### Inspecting shards in the ELK Cluster

- use the following http query to get information about the nodes `GET /_cat/shards?v`
	- `?` symbolises **paramters** and here `v` will add the descrition for field of output.
	![[Pasted image 20220706081852.png]]


## Creating a index in elastic search



- use the http query to create the index `PUT /my-index-000001`
```
PUT /my-index-000001
{
  "settings": {
    "number_of_replicas": 2,
    "number_of_shards": 2
  }
}
```


## Deleting a index in elastic search

- use the http query to delete the index `DELETE /my-index-000001`


## Adding a document to index
- An index is not much fun without any documents, so let's add some.  
- More formally, we will **"index" some documents**, which is the more correct terminology to use,  but you might hear me use both phrases interchangeably.  
- We can index a document by sending a POST request to an endpoint consisting of the index  name, a slash, and "_doc." 
Let's type that out.  We then need to define the document within the request body as a JSON object.  The whole object is what Elasticsearch will add as the document, and you can add any valid JSON.  Since our index is named "products," we should add a document that represents a product.  I will add one consisting of three fields; "name," "price," and "in_stock."  


```
POST /my-index-000001-000001/_doc
{
  "name" : "Coffee Maker",
  "price": 64,
  "in_stock": 10
}
```
![[Pasted image 20220707153644.png]]

- Great, the document has now been added to the "products" index.  
	- There are two things I want to point out within the results.  
		- First, notice the object named "_shards." This object tells us how many shards succeeded  and failed to store the document.  In our case everything went well, and the document was stored on three shards.  
			- But why three?  Remember how we configured our index to contain two shards and two replica shards.  
			- The document was added to one of the two shards, being a primary shard, and it was then replicated  to its replica shards.  
			- So the document was added to the primary shard and its two replica shards, together forming  a replication group.  
		- The next thing I want to point out, is the "_id" key which contains an identifier  for the document.  This identifier was generated automatically because we didn't specify one when we added  the document.

- We can do that if we want to, so let's try to **add another document with an ID of 100**.  
	- To specify an ID, we first need to change the HTTP verb to "PUT," since this is a convention  for REST APIs.  Then we just need to append a forward slash followed by the ID to the endpoint.  Within the query results, we can see that the "_id" key does indeed contain the  value that we specified.  Note that the identifier is just a string value and does not have to be an integer as  in this example.  
- Define the id by our self
```
PUT /my-index-000001-000001/_doc/100
{
  "name" : "Toaster",
  "price": 29,
  "in_stock": 4
}
```
![[Pasted image 20220707153814.png]]

- Another thing to note, is that we didn't even have to create the index in advance.  There is a setting named ``"action.auto_create_index"`` that can be configured at the cluster level,  which specifies if indices should be created automatically.  
- The setting defaults to "true," meaning that if we try to add a document to a non-existing  index, the index will be created, after which the document will be added to it.  
- It's best practice to explicitly create indices, but this behavior is convenient in  development mode.  Now that you know how to add documents to an index, let's try retrieving them.


## Retrieving documents by ID

- Let's try to retrieve one of the documents that we just added. 
- We will need to know its  ID, and since I didn't make a copy of the auto-generated ID for one of the documents,  I will retrieve the one with an ID of 100. 
- Since we will be retrieving a resource, we  will use the "GET" HTTP verb. 
- The endpoint will actually be the same as when we added  the document, so this is another example of how the HTTP verb sometimes specifies the  action that should be performed.  
```
GET /<index>/_doc/<id>
```
![[Pasted image 20220707154832.png]]
- Running the query, we can see that Elasticsearch returns the document that we added. The JSON  object that we specified when adding the document, is returned to us under the "_source" key.  If the document is not found, the "found" key will be set to "false," and the "_source"  key will not be part of the results.  That's how easy it is to retrieve a specific document, provided that you know its ID!  Later in the course you will see how to perform searches for documents matching some criteria,  but that's a topic for a bit further down the road.


## Updating a Document

- Now that we have some documents within our index, let's take a look at how we can update them.  
- Suppose that a product has been sold, and we therefore need to decrease the "in_stock"  field by one.  
- We can do that by sending a POST request to the Update API, following the document's  ID.  
- Let me just type that out.  Within the request body, we should add a ``"doc"`` object consisting of key-value pairs.  
- The keys are the field names, and the values are the new field values.  Let's set the "in_stock" field to 3, where it used to be 4.  The document has now been updated, which we can tell because the "result" key has  a value of "updated." This key might also have a value of "noop" if the update did  not result in a change, i.e. if we supplied the same value as the document currently contained  for the field.  
```
POST /products/_update/100
{
  "doc": {
    "in_stock" : 3
  }
}
```
- Let's retrieve it to confirm that the "in_stock" field now has a value of 3.  Indeed the field value has been updated as we expected.  Apart from updating existing fields, we can also add new fields to existing documents.  
- We do that in exactly the same way, so let's make a copy of the query we just wrote and  define a new field.  The new field will be a "tags" field containing an array of string values.  Note that I could just have added the "tags" key to the first query that we ran.  Any field that does not exist already is added, so the result would be the same.  Let's quickly verify that the document now contains a field named "tags."  
```
POST /products/_update/100
{
  "doc": {
    "tags": ["electronics"]
  }
}
```
- Indeed we now see the "tags" field within the "_source" key.  

	# How updation of the document works  internally.  
	- It might surprise you that **Elasticsearch documents are immutable.**  
	- What this means, is that **documents cannot be changed.**  
	- But didn't we just change an existing document?  Actually we didn't.  **We replaced it.**  
	- The **Update API** just took care of a couple of things for us.  Specifically, **it retrieved the document, changed its fields according to our specification,**  and **reindexed the document with the same ID,** effectively replacing it.  
	- So to be clear, the existing document was not updated with any new fields or values;  it was replaced entirely.  It just appeared as if the document was updated.  
	- We could have done the exact same thing by first retrieving the existing document, then  modify its fields, and then finally replace the document that we retrieved in the first  place.  What the Update API does, is just save us some work.  
	- On top of that, it is also more efficient, because it limits the number of network round  trips that are required.  Everything happens with a single request within the shard on which the document is stored.  
	- If we were to do this ourselves, we would need to send two requests instead of one,  and thereby incur some additional network latency and overhead.  That was just a bit of background knowledge for you.
![[Pasted image 20220707160627.png]]


## Scripting and updating the document values with logic

- In the previous lecture, we reduced the "in_stock" field by one, which we did by explicitly specifying  its new value.  Wouldn't it be cool if we could subtract the value by one, without needing to know  the current value for the document?  That is, if we could do everything in one go, instead of having to retrieve the document  first, subtract the field value by one, and then update the document.  We can do exactly this with so-called "**scripted updates.**" 
- Elasticsearch supports scripting,  which **enables you to write custom logic while accessing a document's values**.  
- In this example, we can **easily increase or decrease the "in_stock" field's value**.  
- That's probably the simplest possible example, so that's the one I will go with, because  scripting is a relatively big topic that we will cover later in the course.  
- I do want to mention, however, that you can also embed "if" statements and such, so  you can think of scripting as a way to write custom code that looks similar to many programming  languages.  
- To use a script, we will actually use the Update API as we did in the previous lecture,  but just specify a "script" object within the request body.  Within this object, we should add a "source" key and provide our script as its value.  I will just write out the script and explain it afterwards.  
	- ``"ctx"`` is a variable, and is short for ``"context."`` We can access the source document  through its ``"_source"`` property, which gives us an object containing the document's fields.  In this case we accessed the "in_stock" field and subtracted one from it.  
	- If we wanted to increase the value instead, we would simply write ``"++"`` at the end.  
		```	
		POST /products/_update/100
		{
		  "script": {
		    "source": "ctx._source.in_stock--"
		  }
		}		  
		```
	- In this example I added the script on a single line, which is ideal for very simple scripts.  If you have more advanced scripts, however, you can use three double quotes at the beginning  and end of the script instead of one.  
	- This marks the string as a multiline string, allowing your script to span multiple lines.  You will see this at the end of this lecture.  For now, let's go ahead and run the query that we just wrote.  
	- If we retrieve the document, we should see that the "in_stock" field equals two instead  of three.  
	- We can also do assignments instead, so let's assign a value of ten to the field.  The value should now be set to ten, so let's check.  And indeed it is.  
		```
		POST /products/_update/100
		{
		  "script": {
		    "source": "ctx._source.in_stock = 20"
		  }
		}
		```
			
	- We can also **define parameters within the request**.  Of course this is not very useful when we define the query within the Console tool.  However, if we were sending the query from an application of some sort, then we could  make the parameter dynamic based on some logic.  
	- For example, if someone purchases four toasters, we could reduce the value of the "in_stock"  field by four instead of just one. 
	-  Let's do just that and pass in a parameter and give it a value of four.  
	- We define parameters within an option named ``"params,"`` which is an object consisting  of key-value pairs, where each pair corresponds to a parameter name and value.  With the parameter in place, let's modify our script to make use of it.  
       ```
       POST /products/_update/100
		{
		  "script": {
		    "source": "ctx._source.in_stock-= params.quantity",
		    "params": {
		      "quantity": 4
		    }
		  }
		}
       ```
       	
	- Let's run the query and verify that the field value is reduced by four, meaning that  it should be set to six.  As you can see, the result is what we expected.  
	- Before ending this lecture, I want to mention two other possible values for the **"result"  key** that is specified within the query results, apart from ``"updated."`` 
	- The key may also  contain the value ``"noop."`` In the previous lecture, I told you that this is the case  **if we try to update a field value with its existing value.**  
	- The **same is not the case with scripted updates;** if we set a field value within a script, the  **result will always equal** ``"updated,"`` even if no field values actually changed.  
	- There are two exceptions to this, both being if we explicitly set the operation within  the script.  
	- For example, we can write a script that ignores a given document based on a condition.  This is done by setting the ``"op"`` property on the ``"ctx"`` variable to ``"noop."``  You can see an example of such a script on your screen.  
		- What this script does, is to only reduce the value of the "in_stock" field by one if  it has a value other than zero.  
		- If the value is zero, no changes are applied to the document, and the "result" key  will be set to "noop" within the response.  Otherwise it will be set to "updated."  
		  
		- Note that you can write a similar script that just inverts the conditional statement and  only reduces the field value if it evaluates to "true."  The difference between these two scripts, is that the one to the right always yields  a result of "updated," regardless of whether or not the field value was actually changed.  This is not the case for the one on the left.  So if it's important for you to detect if nothing was changed, then that's the way  to go.  Actually, you can also set the operation to "delete," which will cause the document  to be deleted instead.  This will set the value of the "result" key to "deleted" within the results.  This is rarely useful though, as you can also delete documents that match a given query,  as you will see later in this section, so this should only be used for situations where  you need to use scripting to determine if a document should be deleted.  You can see an example of such a query on your screen.  The query decreases the value of the "in_stock" field by one as long as it is less than or  equal to one.  This effectively means that the product will be removed whenever the last product is sold.  That's just one example of how this functionality can be used.  If you want to try out these queries, you can find them within the GitHub repository.  That's how you can update a document with a script.