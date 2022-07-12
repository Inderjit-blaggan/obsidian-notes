![[Pasted image 20220616191641.png]]

###### gcloud commands for cli:

- `gcloud info` : this is used to get the info about the config file etc
```
inderjitsinghblaggan@cloudshell:~ (blaggan-project)$ gcloud info
Google Cloud SDK [388.0.0]

Installation Properties: [/usr/lib/google-cloud-sdk/properties]
User Config Directory: [/tmp/tmp.ORAUJSn58Y]
```
-  `gcloud auth list` : used to get details about the account logged in.
- `gcloud config list` : command used to list the configuration for gcloud sdk
```
inderjitsinghblaggan@cloudshell:~ (blaggan-project)$ gcloud config list
[accessibility]
screen_reader = True
[component_manager]
disable_update_check = True
[compute]
gce_metadata_read_timeout_sec = 30
[core]
account = inderjitsinghblaggan@gmail.com
disable_usage_reporting = True
project = blaggan-project
[metrics]
environment = devshell

```

Install the below components for auto completion of commands and flags in gcloud:![[Pasted image 20220616204515.png]]
- Run command: `gcloud beta interactive`: this will open interactive shell and we will get suggestion for each command and flag


###### To customize your cloudshell when it loads.

1. Create a file called `.customize_enviroment`
2. Add bash script to install the packages you want inside your cloudshell.

##### Service Accounts:
![[Pasted image 20220617010725.png]]

Creating a Service account using CLI:

1. To list the service account in the project, use command: `gcloud iam service-accounts list`
![[Pasted image 20220617011956.png]]

2. Create a Service account: ``gcloud iam service-accounts create blaggan-test --display-name='blaggan-service-account'

3. Now attach the permissions to the service account: `gcloud projects add-iam-policy-binding blaggan-project --member 'serviceAccount:blaggan-test@calm-shadow-319519.iam.gserviceaccount.com'   --role 'roles/storage.objectViewer'`

```
inderjitsinghblaggan@cloudshell:~ (calm-shadow-319519)$ gcloud iam service-accounts list
DISPLAY NAME: Compute Engine default service account
EMAIL: 107577702373-compute@developer.gserviceaccount.com
DISABLED: False

inderjitsinghblaggan@cloudshell:~ (calm-shadow-319519)$ gcloud iam service-accounts create blaggan-test --display-name='blaggan-service-account'
Created service account [blaggan-test].

inderjitsinghblaggan@cloudshell:~ (calm-shadow-319519)$ gcloud iam service-accounts list
DISPLAY NAME: blaggan-service-account
EMAIL: blaggan-test@calm-shadow-319519.iam.gserviceaccount.com
DISABLED: False

DISPLAY NAME: Compute Engine default service account
EMAIL: 107577702373-compute@developer.gserviceaccount.com
DISPLAY NAME: Compute Engine default service account
EMAIL: 107577702373-compute@developer.gserviceaccount.com
DISABLED: False

inderjitsinghblaggan@cloudshell:~ (calm-shadow-319519)$ gcloud projects add-iam-policy-binding blaggan-project --member 'serviceAccount:blaggan-test@calm-shadow-319519.iam.gserviceaccount.com'   --role 'roles/storage.objectViewer'
Updated IAM policy for project [blaggan-project].
bindings:
- members:
  - serviceAccount:service-631467083632@compute-system.iam.gserviceaccount.com
  role: roles/compute.serviceAgent
- members:
  - serviceAccount:631467083632-compute@developer.gserviceaccount.com
  - serviceAccount:631467083632@cloudservices.gserviceaccount.com
  role: roles/editor
- members:
  - user:inderjitsinghblaggan@gmail.com
  role: roles/owner
- members:
  - serviceAccount:blaggan-test@calm-shadow-319519.iam.gserviceaccount.com
  role: roles/storage.objectViewer
etag: BwXhlhrk0iw=
version: 1

```
