# http (Data Source)
- Link to doc: https://registry.terraform.io/providers/hashicorp/http/latest/docs/data-sources/http
- **Usecase**: incase we want to **retirive some data from https url** like json file or etc. 
- The `http` data source makes an HTTP GET request to the given URL and exports information about the response.

The given URL may be either an `http` or `https` URL. At present this resource can only retrieve data from URLs that respond with `text/*` or `application/json` content types, and expects the result to be UTF-8 encoded regardless of the returned content type header. 

```
# Datasource: EBS CSI IAM Policy get from EBS GIT Repo (latest)

data "http" "ebs_csi_iam_policy" {

  url = "https://raw.githubusercontent.com/kubernetes-sigs/aws-ebs-csi-driver/master/docs/example-iam-policy.json"

  # Optional request headers
  request_headers = {
    Accept = "application/json"
  }
}

output "ebs_csi_iam_policy" {
  value = data.http.ebs_csi_iam_policy.body
}
```