![[Pasted image 20220626015612.png]]

![[Pasted image 20220626015633.png]]

![[Pasted image 20220626020006.png]]


##### Deploy app to App Engine in GCP:
- `gcloud app deploy` : this command is used to deploy code for app engine.
		- Flags :
			- `--version=v2`
	
	- Working:
		1. It first creates a **deplyoment Packages**. Now this package is stored in the **Google cloud storage** 
		2. Now **Cloud build** which is a CI/CD service in google cloud requires the access to the package.
		3. 
```
inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ gcloud app deploy
Services to deploy:

descriptor:                  [/home/inderjitsinghblaggan/default-service/app.yaml]
source:                      [/home/inderjitsinghblaggan/default-service]
target project:              [blaggan-project]
target service:              [default]
target version:              [20220625t204206]
target url:                  [https://blaggan-project.uc.r.appspot.com]
target service account:      [App Engine default service account]

```

- We can see the app inside the **Services**  in App Engine in UI or we can use command: `gcloud app services list`
	- [gcloud app services cli doc link](https://cloud.google.com/sdk/gcloud/reference/app/services)
- To check the version of the app deployed we can use command: `gcloud app versions list`
- To get the url for the version we can use command: `gcloud app browse --version <VERSION.ID>`
- To check the instances part of the **Service from APP Engine** we can use command: `gcloud app instances list`
- To delete the services we can use command: ``gcloud app services delete service1`` 
```
inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ gcloud app deploy 
Services to deploy:

descriptor:                  [/home/inderjitsinghblaggan/default-service/app.yaml]
source:                      [/home/inderjitsinghblaggan/default-service]
target project:              [blaggan-project]
target service:              [default]
target version:              [20220625t205416]
target url:                  [https://blaggan-project.uc.r.appspot.com]
target service account:      [App Engine default service account]


Do you want to continue (Y/n)?  y

Beginning deployment of service [default]...
Uploading 1 file to Google Cloud Storage
100%
100%
File upload done.
Updating service [default]...done.     
Setting traffic split for service [default]...done.   
Deployed service [default] to [https://blaggan-project.uc.r.appspot.com]

You can stream logs from the command line by running:
  $ gcloud app logs tail -s default

To view your application in the web browser run:

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ gcloud app  services list
SERVICE: default
NUM_VERSIONS: 1

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ gcloud app versions list
SERVICE: default
VERSION.ID: 20220625t205416
TRAFFIC_SPLIT: 1.00
LAST_DEPLOYED: 2022-06-25T20:56:38+00:00
SERVING_STATUS: SERVING

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ gcloud app browse
Did not detect your browser. Go to this link to view your app:
https://blaggan-project.uc.r.appspot.com

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ curl https://blaggan-project.uc.r.appspot.com
My Default Service - V2inderjitsinghblaggan@cloudshell:~/default-service 

inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$ gcloud app instances list
SERVICE: default
VERSION: 20220625t205416
ID: 00c61b117c22dbbfbe2bbd1d16e37b1f43b593273bc3c6255ee4410c36416377d130d3002561d91886efe3ed9ed513f25cca19d3f9cdf98f82b4
VM_STATUS: N/A
VM_LIVENESS:
DEBUG_MODE:
inderjitsinghblaggan@cloudshell:~/default-service (blaggan-project)$
```