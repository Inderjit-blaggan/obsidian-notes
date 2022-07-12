**CloudTrail** is a way to **get governance,  compliance and audit for your AWS Accounts.**  And **CloudTrail is enabled by default.**

![[Pasted image 20220521100524.png]]

-   This will **allow you to get a history of all the events  and API calls made within your AWS Accounts  by the Console, by the SDK, the CLI, other services on AWS,  and all these logs will be appearing in CloudTrail**. 
  
- You can also **put these logs from CloudTrail  into CloudWatch Logs or Amazon S3.**  And you can create the trail to be applied  to all regions or a single region,
-   If you want to accumulate all these history  of events accumulated across all the regions  into one specific for example, S3 Bucket. 
-   And when we use CloudTrail for example,  well say someone went ahead  and deleted something in AWS for example,  say that's an easy two instance was being terminated,  and you wanna figure out who did it?  Well, the answer is to look into CloudTrail  because CloudTrail we have that API call in it,  and we'll be able to get to the bottom of it  and understand who did what and when. 
-   So to summarize CloudTrail is in the middle,  and the actions of the SDK, the CLI or the Console  or even IAM Users and IAM Roles  or other services will be in the CloudTrail Console. 
-   We can look in it to inspect and audit what happened. 
-   And if you want to have all the events  for more than 90 days,  then we can send them into CloudWatch Logs  or we can send them into an S3 Buckets. 

So let me dive a little bit deeper for CloudTrail.

-    So we have **three kinds of events  that you can see in CloudTrail.** 

1.  **Management events**  and these **represents operations that are performed  on resources** in your AWS accounts. 

-   For example, whenever someone configure security  they will use the API code called IAM AttachRolePolicy  and this will appear in CloudTrail. 
-   If you create a subnets this will appear as well,  if you set up logging this will appear by default. 
-   Anything that modifies your resources  or your AWS Accounts will appear in CloudTrail. 
-   And by default,  trails are configured to log management events  no matter what. 

-   You can separate **two kinds of management events,** :

a. **Read Events** that **don't modify resources**  for example, someone is listing all the users in IAM,  or listing all the Institute instances in Ec2  these kinds of things. 

b. **Write Events**  that may **modify resources** for example,  someone deletes or tries to delete a DynamoDB table.  
- And obviously the Write Events  have probably a lot more importance  because they can wreck damages into your AWS infrastructure  whereas the Read Events  is just to get information which are silver important,  but maybe less destructive.  


2. **Data Event** so they're separate,  and by **default data events are not logged**  because they're high volume operations  
**so what are Data Events?**  
- Well you have Amazon S3 object-level activity  for example, GetObject, DeleteObjects, PutObjects  and as you can see this can be,  happening a lot on the S3 Buckets,  and so this is why they're not logged by default  and you have the option to separate again  Read and Write Events so a Read Events would be a GetObject,  whereas a Write Events would be a DeleteObject  or a PutObject.  
- Another kind of event you can have in a CloudTrail  are AWS Lambda function execution activities  so whenever someone uses the Invoke API  so you can get insights about how many times  the Lambda functions are being invoked.  
- And again, this could be really high volumes,  if your Lambda functions are executed a lot.  

3. **CloudTrail Insights Events.**  So when we have so many management events  across all types of services  and so many APS happening very very quickly  in your accounts,  it can be quite difficult to understand what looks odd,  what looks unusual and what doesn't.  And so this is where CloudTrail Insights comes in.  
- **So with CloudTrail Insights and you've to enable it**  and you have to pay for it,  **it will analyze your events  and try to detect unusual activity in your accounts.**  
- For example, in accurate resource provisioning,  hitting service limits,  bursts of AWS IAM actions,  gaps in periodic maintenance activity.  
- So the way it works is that,  **CloudTrail will analyze what's normal  management activities** look like to create a baseline,  and then it will continuously analyze anything  that is a Write type of events.  
- So **whenever something is changed  or tried to be changed**,  to **detect unusual patterns.**  So very simply, the management events  are going to be continuously  analyzed by CloudTrail Insights,  which will generate Insights Events  in case something is detected.  And so these anomalies, so these insights events will appear  in the CloudTrail console.  
![[Pasted image 20220521101859.png]]
- They will also be sent to Amazon S3 if you want to.  And an **EventBridge events,**  so in **CloudWatch event is going to be generated**  in case you need to **automate**  on top of these CloudTrail Insights,  for example to send an email.  So this is the idea behind CloudTrail Insights.  
 
  
  
**CloudTrail Event Retentions**
![[Pasted image 20220521102046.png]]
- So events by **default are stored for 90 days in CloudTrail**  and then afterwards they're deleted.  
- But sometimes, you may want to have events for longer  in case you want to go back  to something that happened maybe a year ago  for audit purposes.  
- And so to **keep events beyond this period  what you have to do is  that you have to log them to S3 so send them to S3**  and then you would use **Athena to analyze them**.  
- So very simply,  all your management events,  your data events and your insights events are going to go  into CloudTrail for 90 days retention period.  And then you would log those into your S3 Buckets  for long-term retention.  And when you're ready to analyze them,  you would use the Athena service  which is a server-less service to query data in S3  to find the events that you're interested in about  and learn more about them.  


**CloudTrail,  and some solution architectures**

1. **how to deliver files from CloudTrail to S3**
![[Pasted image 20220521103432.png]]


- So, we have CloudTrail,  and you can **set up a dump of the CloudTrail files  into S3 every five minutes**.
- And so, this encryption can be enabled.  
- And by default, it's going to be SSE-S3 encryption,  but you can set up manually SSE-KMS  for your CloudTrail files.  
- You could set up a lifecycle policy on your S3 buckets  to deliver files to the glacier tier  in case there are archives and you don't need  to have them around for a long time  or you just wanna archive them and make sure  that if you need to access them,  you can wait maybe for six, 12 hours  before you analyze these.  
- When a file is delivered into S3,  you can leverage S3 events  to notify either an SQS, an SNS topic or a Lambda function,  but you can also have CloudTrail,  deliver notifications directly into SNS  and from SNS, you could invoke SQS or Lambda.  
- So, this gives you a bunch of different options you  can have for this architecture.  

**You need to think about all the enhancements  we can have from S3.**
- **So, we can enable versioning**, to prevent accidental deletes.  
- We can have MFA, for delete protection.  
- We can have **S3 Lifecycle Policy,** as I said,  to move it to S3 IA or Glacier.  
- You can have **S3 Object Lock**  to make sure that the S3 objects are never deleted,  and will never be modified as well.  
- So, this could be very powerful.  You can enable encription such as SSE-S3 or SSE-KMS.  
- And you can even have a feature  to perform **CloudTrail Log File Integrity Validation**,  that will verify that the files delivered in S3  are exactly as they should be  and they have not been tempered with,  they have not been modified.  


2. **CloudTrail can be multi-account and multi-region.**
![[Pasted image 20220521103500.png]]
- So, say, we have **two accounts, Account A and Account B,**  and we have a security accounts  that we want to send the logs to.  
- So, we'll have CloudTrail in the first accounts,  and CloudTrail in the second accounts.  
- And we'll **set up an S3 bucket**,  that will be holding the logs of all these CloudTrails.  
- The thing is **CloudTrail needs  to be delivering these log files into the S3 bucket  of the security accounts.**  
- And so, the only way for us to do it,  is to **define an S3 bucket policy**,  and that **S3 bucket policy should allow CloudTrail  to deliver files into S3.**  
- And the cool thing we have here  is that the S3 bucket policy, first of all,  is necessary and very simple to maintain  for cross-account delivery.  
- The second is that if **account A needs to access  the logs from the Cloud Trail buckets**,  we can create, for example,  **a cross-account role and assume the role  in the security accounts,**  or we can **edit the bucket policy  to allow reads from the Account A.**  
- So, it's really up to you  to see the kind of architectures you need,  but it's really interesting here to see  that there's different options to provide the reads access  to S3 based on the use case.  
- And on top of it, we have demonstrated a good way  to have a security account in the middle  to keep all these cultural logs into a secure place.  And maybe the security account has a much tighter security  for user management and so on.  

3. **Solution architecture for alerting**: 
![[Pasted image 20220521104319.png]]
- Say, you want to **create alerts,  when certain API calls are done.**  
- So, you have CloudTrail,  and you can stream all these events into CloudWatch Logs.  And then from CloudWatch Logs,  you can open up a bunch of use case.  
- And one of the things you can do with **CloudWatch logs**  is to **create a metric filter**,  and then a **CloudWatch Alarm on top of it**.  And this filter will basically filter  for the API call you're looking for.  Maybe you're looking for a terminate instance,  so you create a metric filter for terminate instance.  And then if that filter passes through,  then you would have the metric incremented by one.  
- And maybe your CloudWatch alarm is set up to be triggered  whenever the metric filter is equals to one,  in which case, the CloudWatch Alarm can deliver  to an SNS topic  and from that SNS topic, you could have a Lambda function,  you could have an SQS, you could have multiple things,  but this will be a way to create alerts for API calls.  
- Now, these log metric filters  can be detecting also a high level of API happening.  It doesn't have to be a specific API.  
- So, you can be counting the occurrences of a specific API,  for example, TerminateInstances,  or count the API calls per user,  or detect a high level of denied API calls,  maybe, someone is trying to breach into your security.  
- So, it's not just for one API call,  you can be creating a metrics filter  and CloudWatch alarms is for counting them  or detecting high level of unusual activity.  

4. **CloudTrail supports Organizational Trail**.  
![[Pasted image 20220521104338.png]]
- So, if you have AWS Organizations,  for example, you have a management accounts,  and then you have several  or used with other member accounts,  then it is possible for you  to set up a Organizational Trail directly  in the management accounts,  because it's very important for you to remember,  this trail is created in the management accounts,  and not in the children accounts, in the member accounts  of your organization.  
- Now what's happening is that **once you create this trail,  then any events into all the member accounts  will be monitored by this trail directly,**  which is very easy to understand.  
- So, that means that any accounts in the Prod OU  or the Dev OU will be monitored.  And from there, while you can send the information  from a trail into an S3 buckets,  that will be again in your management accounts.  
- And this S3 buckets  will contain a certain kind of organization,  where as you see, the **suffix of the S3 prefix represents  the account number** that is being monitored by CloudTrial.  
- Finally, we have CloudTrial but how can we react  to events happening in CloudTrial the fastest?  
- Overall **cloud trial may take up to 15 minutes  to deliver your events.**  And so you need to know that.  
![[Pasted image 20220521104404.png]]
- But anyway, we have CloudWatch events.  And CloudWatch events can be triggered  for any API call happening in CloudTrail,  you can set it up,  and it's going to be the fastest, most reactive way.  
- If you do **CloudTrail deliver in CloudWatch Logs**,  the events are going to be streamed  but there may be some time before they're delivered.  
- And here we can create a metric filter,  not only to to one event  where CloudWatch events does digital fine,  but you can also set up a metric filter  to analyze occurrences and detect anomalies  and so you're maybe looking  for many events at the same time.  So, this is more powerful.  
- Now we have CloudTrail delivery in S3.  And here the events  are going to be delivered every five minutes  and you can analyze the log integrity,  you can deliver cross accounts, long term storage,  and you could look at these logs holistically  maybe using something like Athena and QuickSight.  
- And so, CloudTrail delivery  in S3 is going to be less reactive but more comprehensive.  So, the idea is that based on the needs you have,  you can have different solution, either CloudWatch event  CloudWatch Logs or S3,  and none of them is bad or is great,  it just depends on what you're trying to achieve.  All right, that's it for CloudTrail,  I hope that was eye opening  and I will see you in the next section.