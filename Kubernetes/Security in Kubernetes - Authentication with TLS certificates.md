[kode kloud Security PDF ]([Module 6 Kubernetes Security.pdf](file:///F:/Learning/Kubernetes/Module%206%20Kubernetes%20Security.pdf))

### Need for Certificates:
- We will look at the absolute basics of what TLS certificates are, why you need them and how you can configure certificates to secure associates or web servers.

- **A certificate** is **used to guarantee trust between two parties** during a transaction. For example, when a user tries to access a web server, TLS certificates ensure that the **communication between the user and the server is encrypted** and the server is who it says it is.

	- Let's take a look at a scenario:
	  ![[Pasted image 20220627130244.png]]
		- If a user were to access his online banking application, the credentials he types in would be sent in a plain text format. The hacker sniffing network traffic could easily retrieve the credentials and use it to hack into the user's bank account. Well, that's obviously not safe. So you must encrypt the data being transferred using encryption keys.
		- The **data is encrypted** using a **key**, which is basically a set of random numbers and alphabets.
		- You add the **random number to your data** and you **encrypt it into a format that cannot be recognized.** 
		- The data is then **sent to the server**. The hackers sniffing the network gets the data, but can't do anything with it. However, the same is the case with the **server receiving the data, it cannot decrypt the data without the key**.
		- So a **copy of the key must also be sent to the server** so that the server can decrypt and read the message, since the key is also sent over the same network. The attacker can sniff that as well and decrypt the data with it. 
		- This is known as symmetric encryption. It is a secure way of encryption, but since it uses the same key to encrypt and decrypt the data.
		- And since the key has to be exchanged between the sender and the receiver, there is a risk of a hacker gaining access to the key and decrypting that data.

- And that's where **asymmetric encryption** comes in. 
	- **Instead of using a single key to encrypt and decrypt data**, asymmetric encryption **uses a pair of keys**, a private key and a public key. Well, they are private and public keys, but for the sake of this example, we will call it a private key and a public lock.
	- A key which is only with me, so it's private. A lock that anyone can access. So it's public.
	- The trick here is if you **encrypt or lock that data with your lock**, you can only **open it with the associated key**. 
	- So your **private key must always be secure with you and not be shared with anyone else**. 
	- It's private. But the **lock is public and may be shared with others**, but they can only lock something with it no matter what is locked.
	- Using the public lock, it can only be unlocked by your private key.
	- Before we go back to our Web server example, let's look at an even simpler use case of securing essence, its access to servers using key pairs.
		1.  You have a server in your environment that you need access to. You don't want to use passwords as they're too risky.
		2. So you decide to use key pairs, you generate a public and private key pair. 
		3. You can do this by running the keygen command. It creates two files.
			- private key: id_rsa
			- public key: id_rsa.pub     Well, not a public key public lock.
	- You then **secure your server by locking down all access to it,** except through a door that is locked using your public lock.
	- It's usually done by adding an entry with your **public key into the servers.** `~/.ssh/authorised_keys file`
	- So you see, the **lock is public** and **anyone can attempt to break through.** But as long as no one gets their hands on your private key, which is safe with you on your laptop, no one can gain access to the server. When you try to stage, you specify the location of your private key in your message command.
	
	- What if you have other servers in your environment? How do you secure more than one server with your key pair?
		- Well, you can create copies of your public clock and place them on as many servers as you want. You can use the same private key to success into all of your servers securely.

	- What if other users need access to your servers?
		- Well, they can do the same thing. They can generate their own public and private key pairs as the only person who has access to those servers.
		- You can create an additional door for them and lock it with their public locks
		- Copy their public locks to all the servers, and now other users can access the servers using their private keys.


### Problem with Symmetric encryption:

- The problem we had earlier with symmetric encryption was that the `key` **used to encrypt data** had to be **sent to the server** over the **network along** with the **encrypted data.** And so there is a **risk of the hacker getting the key to decrypt the data**.
	- What if we could **somehow get the key to the server safely** and then the **server and client can safely continue communication** with each other **using symmetric encryption.**
	- To **securely transfer the symmetric key from the client to the server**, we use **asymmetric encryption**,

### Asymmetric Encryption
![[Pasted image 20220627125141.png]]

1. So we **generate a public and private key pair** on the server. We're going to refer to the public lock as public key going forward.
	- Now that you have got the idea, the keygen key then command was used earlier to create a pair of keys. Here we use the open SSL command to generate a private and public key pair.
		1. **private key** command: `openssl genrsa -out my-bank.key 1024`
		2. **public key** command:  `openssl rsa -in my-bank.key -pubout > mybank.pem`

2. When the **user first accesses the Web server** , he **gets the public key from the server** since the hacker is sniffing all traffic.
	- Let us assume **he too gets a copy of the public key**. We'll see what he can do with it. The user, in fact, the user's browser then **encrypts the symmetric key using the public key provided** by the server. The **symmetric key is now secure**.
	  
3. The user then **sends this to the server**. The hacker also gets a copy. The **server uses the private key to decrypt the message** and **retrieve the symmetric key** from it.
	- However, the hacker does not have the private key to decrypt and retrieve the symmetric key from the message received. 
	- The hacker only has the public key, which he can only lock or encrypt a message and not decrypt the message.

4. The **symmetric key is now safely available** only to the **user and the server** that can now use the **symmetric key to encrypt data and send to each other.**
	- The receiver can use the same symmetric key to decrypt data and retrieve information.
	- The hacker is left with the encrypted messages and public keys with which he can decrypt any data.

**With asymmetric encryption, we have successfully transferred the symmetric keys from the user to the server.**

### Need for CA Certificates

The hacker now looks for new ways to hack into your account, and so he realizes that the only way he can get your credential is by getting you to type it into a form he presents.

- So he creates a website that looks exactly like your bank's website.

- The design is the same. The graphics are the same. The website is a replica of the actual bank's website. He hosts the website on his own server.

- He wants you to think it's secure, too, so he generates his own set of public and private key pairs

and configures them on his web server.

- And finally, he somehow manages to tweak your environment or your network to route your requests going

to your bank's website to his servers.

- When you open up your browser and type the website address in, you see a very familiar page, the same

login page of your bank that you're used to seeing.

- So you, you go ahead and type in the username and password.

- You make sure you type in steps in the URL to make sure the communication is secure and encrypted.

- Your browser receives a key. You send encrypted symmetric key and then you send your credentials encrypted with the key, and the receiver decrypt the credentials with the same symmetric key.

- You've been communicating securely in an encrypted manner, but with the hackers sober as soon as you

send in your credentials, you see a dashboard that doesn't look very much like your bank's dashboard.

- What if you could look at the key you received from the server and say if it is a legitimate key from the real bank server, when the server sense the key, it does not send the key along.

- It sends a certificate that has the key in it. If you take a closer look at the certificate, you will see that it is like an actual certificate, but in a digital format, it has information about who the certificate is issued to the public key of that server, the location of that server, etc..

- On the right, you'll see the output of an actual certificate, every certificate has a name on it, the person or subject to whom the certificate is issued to. That is very important, as that is the field that helps you validate their identity.

- If this is for a web server, this must match what the user type thing in the URL on his browser. If the bank is known by any other names and if they like their users to access their application with the other names as well, then all those names should be specified in this certificate under the subject our native names section.

- But do you see anyone can generate a certificate like this?

- You could generate one for yourself saying your Google, and that's what the hacker did.

- In this case, he generated a certificate saying he is your bank's website. So how do you look at a certificate and verify if it is legit? That is where the most important part of the certificate comes into play. Who signed and issued the certificate?

- If you generated a certificate, then you will have to sign it by yourself. That is known as a self signed certificate. Anyone looking at the certificate you generated will immediately know that it is not a safe certificate because you have signed it. If you looked at the certificate you received from the hacker closely, you would have noticed that it was a fake certificate that was signed by the hacker himself.

- As a matter of fact, your browser does that for you. All of the web browsers are built in with a certificate validation mechanism wherein the browser checks the certificate received from the server and validates it to make sure it is legitimate.

- If it identifies it to be a fake certificate, then it actually warns you. So then how do you create a legitimate certificate for your web servers that the web browsers will trust?

How do you get your certificates signed by someone with authority?

- That's where certificate authorities or cars comes in. They're well known organizations that can sign and validate your certificates for you. Some of the popular ones are Symantec, which is third Comodo Global Sign, etc. The way this works is you generate a certificate signing request or CSR using the key you generated earlier and the domain name off your website.

- You can do this again using the open SSL command. This generates my Daschbach Dot CSR file, which is the certificate signing request that should be sent to the CIA for signing. It looks like this.

- The certificate authorities verify your details, and once it checks out, they signed the certificate

and send it back to you.

- You now have a certificate signed by SCA that the Brokers Trust.

Command to create CSR: openssl req -new -key my-banl.key -out my-bank.csr -subj "/C=US/ST=CA/O=MyOrg,Inc./CN=my-bank.com"

- If Hacker tried to get his certificate signed the same way, he would fail during the validation phase and his certificate would be rejected by the CIA.

- So the website that he's hosting won't have a valid certificate.

- The CW's use different techniques to make sure that you're the actual owner of that domain. You now have a certificate signed by a that the browsers trust.

But how do the browsers know that the CIA itself was legitimate? what if the certificate was signed by a fake CIA? In this case, our certificate was signed by Symantec. How would the browser know?

- Symantec is a valid CAA and that the certificate was in fact signed by Symantec and not by someone who

says they are Symantec. The certs themselves have a set of public and private key pairs. The certs use their private keys to sign the certificates. The public keys of all the keys are built into the browsers.

- The browser uses the public key of the CAA to validate that the certificate was actually signed by the CAA themselves.

- You can actually see them in the settings of your web browser under certificates. They're under Trusted Certs tab. Now, these are public figures that help us ensure the public websites we visit, like our banks, emails, etc. are legitimate.

However, they don't help you validate sites hosted privately say within your organization.

- For example, for accessing your payroll or internal email applications. For that, you can host your own private keys.

Most of these companies listed here have a private offering of their services and see a server that

you can deploy internally within your company.

- You can then have the public key of your internal C server installed on all your employees browsers and establish secure connectivity within your organization.
