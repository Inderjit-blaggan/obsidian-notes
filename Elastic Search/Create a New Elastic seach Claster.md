https://thoughts.t37.net/designing-the-perfect-elasticsearch-cluster-the-almost-definitive-guide-e614eabc1a87 : imp 
https://fdv.github.io/running-elasticsearch-fun-profit/ 
https://thoughts.t37.net/tagged/elasticsearch 
https://docs.rackspace.com/blog/clustered-elasticsearch-indexing-shard-and-replica-best-practices/


![[Pasted image 20220713235250.png]]![[Pasted image 20220713235634.png]]

- How did adding a shard on the node affected our performance, did we gain a lot of indexing per sec or did we gain just a little.
- And then we can continue adding more shard to see how many shards, that node can take before the permances starts demnishing ( This will get ideal shard capacity for node)
![[Pasted image 20220714000417.png]]

Monitor the nodes, CPU Spikes, Ram we had avaliable



![[Pasted image 20220714000642.png]]

![[Pasted image 20220714001030.png]]

![[Pasted image 20220714001224.png]]
![[Pasted image 20220714001255.png]]

![[Pasted image 20220714001519.png]]
A problem can arise when force merging a huge shard. If the shard size is > half of the disk size, you provably won’t be able to fully merge it, unless most of the data is made of deleted documents.
![[Pasted image 20220714001903.png]]
![[Pasted image 20220714002110.png]]


Multi-Node Benchmarking:

![[Pasted image 20220714002242.png]]


![[Pasted image 20220714002843.png]]



As a starting point:

-   Aim to keep the average shard size between a few GB and a few tens of GB. For use cases with time-based data, it is common to see shards in the 20GB to 40GB range.
-   Avoid the gazillion shards problem. The number of shards a node can hold is proportional to the available heap space. As a general rule, the number of shards per GB of heap space should be less than 20.


For instance, if you have 3 data nodes, you should have 3, 6 or 9 shards. This is very important. If, instead, you have 3 data nodes and decide to use 4 primary shards, then your indexing process will actually be slower than when using 3 shards, because 1 node (aka server) will have double the work and the other 2 will sit idle. 

As a rule of thumb for quick indexing, you should choose at least as many primary shards as the number of data nodes you have, assuming that the number of data nodes fits your use case.



### Ways to determine the ideal shard size 

#### 1. **Benchmarking**

The best way to determine the ideal shard size is by running experiments. Pick a server that is as identical as possible to your future production server and then:

1.  Configure a 1 node Elasticsearch cluster.
2.  Index your production data.
3.  Measure the indexing speed with a benchmarking tool (e.g. [Rally](https://github.com/elastic/rally)). 

If the ingestion time meets your SLAs then you’re fine and you know your future setup. After that, benchmark your queries. If they are fast enough, you’re good to go. 

Otherwise repeat the benchmarking with more shards or more nodes until you reach your SLAs. 

This is the most accurate way to determine the shard size for your use case. 

If this is too complicated, you can start with more shards and/or more nodes and scale down once you’re in production and know more about what you need. Read more about this method [here](https://www.elastic.co/de/blog/found-sizing-elasticsearch#start-big-scale-down). 

#### 2. Generic guidelines

Many times it is not possible to run experiments. Depending on your company, you might only get your servers provisioned once the cost can be estimated for the client.  

That’s why it’s good to also have some generic guidelines and best practices that others have worked with successfully. 

For logging, shard sizes between 10 and 50 GB usually perform well. 

For search operations, 20-25 GB is usually a good shard size. 

Another rule of thumb takes into account your overall heapsize. You should aim for having 20 shards per GB of heap – [as explained here](https://www.elastic.co/guide/en/elasticsearch/reference/current/size-your-shards.html#shard-count-recommendation).