#### To Get information about Elastic Search Cluster
- We can query elastic search using the `GET`  http methord

| Arguments                              | Descriptions                                     |
| ------------------------------------ | ------------------------------------------------ |
| `-u`                                 | Username and curl will prompt for password later |
| `-X GET`                             | HTTP verb used for the query                     |
| `-H "Content-Type:application/json"` |                                                  |
| ` -d '{ "query": { "match_all": {} } }'`    |                                                  |


	- we can also pass the password in command using : `curl -X GET -u <Username>:<Password>  https://my-deployment-e21db5.es.us-central1.gcp.cloud.es.io`
```
v@DESKTOP-JE0IJPU:~$ curl -X GET -u elastic  https://my-deployment-e21db5.es.us-central1.gcp.cloud.es.io
Enter host password for user 'elastic':
{
  "name" : "instance-0000000000",
  "cluster_name" : "1a77652e47d744c2ac48166f0cd5b774",
  "cluster_uuid" : "hDAvYX2tTcmdbYcIvI2y_A",
  "version" : {
    "number" : "8.2.3",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "9905bfb62a3f0b044948376b4f607f70a8a151b4",
    "build_date" : "2022-06-08T22:21:36.455508792Z",
    "build_snapshot" : false,
    "lucene_version" : "9.1.0",
    "minimum_wire_compatibility_version" : "7.17.0",
    "minimum_index_compatibility_version" : "7.0.0"
  },
  "tagline" : "You Know, for Search"
}
```


#### To seach for a particular index
- Usage: `<Elastic seach url>/index/_search`
```
v@DESKTOP-JE0IJPU:~$ curl -X GET -u elastic -H "Content-Type:application/json"  https://my-deployment-e21db5.es.us-central1.gcp.cloud.es.io/products/_search
Enter host password for user 'elastic':
{"error":{"root_cause":[{"type":"index_not_found_exception","reason":"no such index [products]","resource.type":"index_or_alias","resource.id":"products","index_uuid":"_na_","index":"products"}],"type":"index_not_found_exception","reason":"no such index [products]","resource.type":"index_or_alias","resource.id":"products","index_uuid":"_na_","index":"products"},"status":404}v@DESKTOP-JE0IJPU:~$
```

### Query for a particular index with json passed for filteration

![[Pasted image 20220706053957.png]]
![[Pasted image 20220706054055.png]]
![[Pasted image 20220706054131.png]]

```
v@DESKTOP-JE0IJPU:~$ curl -X GET -u elastic -H "Content-Type:application/json"  https://my-deployment-e21db5.es.us-central1.gcp.cloud.es.io/products/_search   -d '{ "query": { "match_all": {} } }'
Enter host password for user 'elastic':
{"error":{"root_cause":[{"type":"index_not_found_exception","reason":"no such index [products]","resource.type":"index_or_alias","resource.id":"products","index_uuid":"_na_","index":"products"}],"type":"index_not_found_exception","reason":"no such index [products]","resource.type":"index_or_alias","resource.id":"products","index_uuid":"_na_","index":"products"},"status":404
```


![[Pasted image 20220706055009.png]]