# How to destroy one specific resource from TF file in Terraform?

[Rajesh Kumar](https://www.devopsschool.com/blog/author/rajeshkumar/ "Posts by Rajesh Kumar") August 11, 2019 comments off

Spread the Knowledge

[](https://www.facebook.com/sharer/sharer.php?u=https%3A%2F%2Fwww.devopsschool.com%2Fblog%2Fhow-to-destroy-one-specific-resource-from-tf-file-in-terraform%2F "Facebook")

[](http://twitter.com/intent/tweet?text=How%20to%20destroy%20one%20specific%20resource%20from%20TF%20file%20in%20Terraform%3F&url=https%3A%2F%2Fwww.devopsschool.com%2Fblog%2Fhow-to-destroy-one-specific-resource-from-tf-file-in-terraform%2F "Twitter")[](http://reddit.com/submit?url=https%3A%2F%2Fwww.devopsschool.com%2Fblog%2Fhow-to-destroy-one-specific-resource-from-tf-file-in-terraform%2F&title=How%20to%20destroy%20one%20specific%20resource%20from%20TF%20file%20in%20Terraform%3F "Reddit")[](http://www.linkedin.com/shareArticle?mini=true&url=https%3A%2F%2Fwww.devopsschool.com%2Fblog%2Fhow-to-destroy-one-specific-resource-from-tf-file-in-terraform%2F&title=How%20to%20destroy%20one%20specific%20resource%20from%20TF%20file%20in%20Terraform%3F "Linkedin")[](https://mix.com/mixit?url=https%3A%2F%2Fwww.devopsschool.com%2Fblog%2Fhow-to-destroy-one-specific-resource-from-tf-file-in-terraform%2F "Mix")[](https://api.whatsapp.com/send?text=How%20to%20destroy%20one%20specific%20resource%20from%20TF%20file%20in%20Terraform%3F https%3A%2F%2Fwww.devopsschool.com%2Fblog%2Fhow-to-destroy-one-specific-resource-from-tf-file-in-terraform%2F "Whatsapp")[](https://www.blogger.com/blog_this.pyra?t&u=https%3A%2F%2Fwww.devopsschool.com%2Fblog%2Fhow-to-destroy-one-specific-resource-from-tf-file-in-terraform%2F&l&n=How%20to%20destroy%20one%20specific%20resource%20from%20TF%20file%20in%20Terraform%3F "Blogger Post")[](https://www.devopsschool.com/blog/how-to-destroy-one-specific-resource-from-tf-file-in-terraform/ "More")

Terraform destroy is a command that allows you to destroy either a full stack (based on your TF files), or single resources, using the -target option. You can even do:

```
$ terraform state list
$ terraform destroy -target RESOURCE_TYPE.NAME
$ terraform destroy -target RESOURCE_TYPE.NAME -target RESOURCE_TYPE2.NAME
$ terraform state list
```

**Option of skipping a resource while destroying terraform resources?**

```
$ terraform state list
$ terraform destroy -target=RESOURCE_TYPE.NAME -target=RESOURCE_TYPE2.NAME
$ terraform state list
```

**How to remove single resources, single instances of a resource, entire modules, and more items from the Terraform state?**

```
# Remove a Resource
$ terraform state rm module.foo.packet_device.worker[0]

# Remove a Module
$ terraform state rm module.foo
```

**How to delete all resources except one?**

```
# list all resources
terraform state list

# remove that resource you don't want to destroy
# you can add more to be excluded if required
terraform state rm <resource_to_be_deleted> 

# destroy the whole stack except above resource(s)
terraform destroy 
```