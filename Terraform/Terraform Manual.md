## Terraform:
#terraform_manual 


#### Terraform Version syntax and descriptions:
| Command                 | Descriptions                                                                                                                                    | 
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | 
| ``version = "~> 3.70"`` | This means it can take 3.70, or later sub version of major 3.x version like 3.8,3.9 etc. The 4.x .. versions dont qualify as per this notations |    
| `version = ">= 3.70"`   | This will support all the version more than 3.70 like 3.80 , 4.x, 5.x etc                                                                       |  
|                         |                                                                                                                                                 |    

```
# Terraform Settings Block using the ~> symbol
# Recommended production grade setup using ~> over >=

terraform {
  required_version = ">= 1.0.0"
  required_providers {
    aws = {
      source = "hashicorp/aws"
      version = "~> 3.70"
    }
    kubernetes = {
      source = "hashicorp/kubernetes"
      version = "~> 2.7"
    }    
  }
}
```