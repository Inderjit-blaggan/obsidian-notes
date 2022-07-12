`elastic`
hq9TtdwxX10a8VEFokh2z4Wc

![[Pasted image 20220705235945.png]]
b


![[Pasted image 20220706035851.png]]
a

![[Pasted image 20220706035918.png]]
c

![[Pasted image 20220706035952.png]]
a

![[Pasted image 20220706062206.png]]
Sharding enables us to scale the data volume. Adding more nodes to the cluster helps too, but only to a certain extent (unless there are no very large indices).

![[Pasted image 20220706062241.png]]
An index is divided into one or more shards, where each shard stores a part of the index' data.

![[Pasted image 20220706062313.png]]

An index will contain one shard for Elasticsearch >= 7.0.0.


![[Pasted image 20220706150241.png]]
Replication ensures that if a node goes down, data will be available on a different node (provided that the cluster consists of more than one node).


![[Pasted image 20220706150307.png]]
a

![[Pasted image 20220706150337.png]]
b


![[Pasted image 20220706150410.png]]
c


![[Pasted image 20220706150443.png]]
1

![[Pasted image 20220706150516.png]]
a
