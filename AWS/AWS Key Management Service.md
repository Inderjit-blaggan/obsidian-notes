**Key Management Service.**

- **So anytime you hear encryption in AWS,**  you have to think about KMS obviously,  and it's a **way to easily control access to our data**  and **AWS will manage the encryption keys for us**.  
- It's fully integrated with IAM for authorization  and has seamless integration  into all the AWS services, pretty much.  
- So EBS, S3, Redshift, RDS, SSM  etc.  We can use the CLI or the SDK to interact with KMS.  
- So in KMS, **you have two types of key**.  

a. **Symmetric keys**
b. **Asymmetric keys**
![[Pasted image 20220521110140.png]]
- So the symmetric keys were the first offering of KMS.  **It's in single encryption key that will be used  to encrypt and decrypt information.**  And then all the service data are integrated with AWS KMS  use symmetric KMS keys.  
- It's necessary **if you wanna do envelope encryption**  and you **never actually get access to the KMS key itself**  or unencrypted, you must just send data into KMS  with a KMS API call to use it.  

- asymmetric key are new  and they can allow you to have **two kind of public keys.**  
- It's a **key pair**. So it's a **public key and a private key.**  
i. **The public key** will be used to encrypt  
ii. **Private key** will be used to decrypt.  

Now, this is very helpful  when you want to have encrypt, decrypt,  or sign and verify operations.  
- And you can **download the public key**,  that **means that you can encrypt stuff  from anywhere with a public key,**  and you can send the public key to people  that you don't trust in untrusted environments.  
- But once the **data is encrypted with the public key,  only the private key has the power  to decrypt your information.**  
- You still cannot use the private key unencrypted though.  You have to use the KMS API calls.  So the use case for an asymmetric key  is to have encryptions outside of AWS by users  who cannot directly call the KMS API for whatever reason.  


Now, you have **different types of KMS keys.**  
![[Pasted image 20220521110212.png]]
1. **Customer managed key**.  And this is the key you create directly in KMS.  
- So you can create, manage and use them.  
- You can enable, disable them.  You can enable a rotation policy  to rotate the key every year,  while the old key is still preserved, of course.  
- And you can add a key policy,  which is a resource policy for KMS keys.  And you can audit the key usage in CloudTrail.  
- This is the kind of keys you will **leverage  for envelope encryption.**  
- So these are the kind of keys that you manage yourself.  Therefore, they're called customer managed keys.  


2. **AWS managed keys**.  So these ones are used exclusively by AWS services  such as AWS/S3, AWS/EBS and so on.  
- And this is what you see in the UI  when you use the AWS managed keys.  So they are managed by AWS  and they're automatically rotated every three years.  
- You can view the key policy and you can audit in CloudTrail,  but you cannot leverage them  for your own encryption operations.  

3. **AWS owned keys**  and these are created and managed by AWS.  
- Used by some services to protect your resources.  And they can be used across multiple AWS accounts  but they're not in your accounts,  they're used by AWS internally.  
- And you cannot view, use, track or audit these keys  but AWS tells you that these keys actually exist.  
![[Pasted image 20220521110244.png]]
Okay. So now, if we want to have a **summary  of the different types of KMS keys**.  
- For the customer managed keys, you can manage metadata,  you can view metadata and you can manage it,  and you can also use for your accounts,  and you can have an automatic rotation every one year.  
- The AWS managed key can be used in your accounts, okay,  but you cannot manage them.  And they're required to be rotated every three years.  This is automatic.  
- And then the owned keys, you just know they exist  but you cannot use them or see them. 


**how do you create a KMS key?**  
![[Pasted image 20220521110305.png]]
- So you have what's called a key material origin  and it cannot be changed after creation.
- So you have to define it at creation time.  So you have the KMS, which is the AWS KMS key material  that means that you are going to have KMS  automatically create, generate and manage the key  in its own key store to see what happens  when you go into KMS and start to create a key.  
- But you have a second option called External.  In that case, while you import the key material directly  into the KMS key, and you are responsible for securing  and managing this key material outside of AWS. Okay?  
- So maybe you just wanna create it outside  and then import it into this KMS and then be done,  and delete it on your end.  Or you just wanna keep a copy in KMS  and therefore, you're responsible obviously,  for the copy outside of KMS.  
- Now, the last option is to use a custom key store  called AWS CloudHSM.  And this allows you to create the key material directly  within your HSM cluster and manage the key material there.  
- So let's consider how it works  when you have a custom key store,  which is backed by CloudHSM.  
- So you create your CloudHSM cluster as a custom key store  and there was a Dart integration with KMS.  
- That means that when KMS creates key  while the key materials are going to be stored  in your HSM cluster that you own and manage.  
- That means that the keys live within your HSM cluster.  
- They are only managed by KMS and used by KMS.  And all the cryptographic operations  will be directly performed in the HSM. 
- So this is the architecture.  You have your CloudHSM cluster with two AZs  and it is directly connected to KMS.  
![[Pasted image 20220521110527.png]]
- So once you have at least two HSM for high availability  then you can integrate with KMS  and then the users can still use the KMS API  to view, create and managed keys,  but behind the scenes,  you know for sure that KMS  will be using your CloudHSM cluster  for all the cryptographic operations  and for storing and retrieving these keys.  

**So why do we do this?**  
- Well, the **use case** is that **you need to have direct control  over the HSMs for higher security  or for risk security requirements,** at least.  
- And then the KMS keys probably  maybe they want to be in a dedicated HSM  or maybe you need to have a regulatory requirement  to have FIPS 140-2 level 3 as level of security  because KMS was only validated at level 


**External KMS key source**,  then you can bring your own key using your own key material.  - And **you are** **really going to be responsible**  for the **key material's security, availability,  and durability outside of AWS.**  
- So it must be a **256-bit symmetric key.**  
- So you cannot import asymmetric material for now  and it cannot be used with custom key store.  It has to be used on its own.  
- For this, if you want to have KMS key rotation, of course,  because the key source is external,  then you have to rotate it on your own.  So automate rotation is not supported.  

**So how does that work?**  

![[Pasted image 20220521110642.png]]
1. Well, the **user will create a KMS key in KMS**  and say that it's a **symmetric type of key  and that the source is external**.  
2. Now, the **key envelope is created**  but the **key material is not**.  
3. So then, **you download the public key and an import token**,  and you're going to use the public key  with your externally generated key material  to create an encryption, thanks to the public key.  
4. Now this **private key that has been encrypted  can be sent back to the KMS service  using also the import token  and then KMS will use some decryption mechanism**  to get the key material and store it  in your KMS key in there.  So this is the process to import external keys in KMS.  
 
- Okay. Lastly, so you **need to know  about KMS Multi-Region Keys.**  So it's possible for you to create a key,  for example, on us-east-1,  and have it replicated across multiple regions.  So they will have the same key material  and actually the same key ID  but it will be in different regions.  
  ![[Pasted image 20220521110753.png]]
- So the use cases are amazing because thanks to this,  well, you have the **same key in multiple regions  and that means that you can encrypt in one region  and decrypt in the other regions using the same KMS key ID.**  
- So you don't need to re-encrypt data  or make cross-region API calls.  
- So they will have the same key ID, the same key material,  the same automatic rotation and so on.  
- But these keys are not global keys. Okay?  It's this principle that there is a primary key  in one region and all the others are replicas.  
- Each key can be managed independently.  And although you can only have one primary key at a time,  it's possible for you to promote one replica  into their own primary key.  
- So the **use cases for multi-region keys  is going to be Disaster Recovery, Global Data Management,**  
- for example, for DynamoDB Global Tables,  Active-Active Application that span multiple region  or distributing, signing applications and so on. 
![[Pasted image 20220521110838.png]]