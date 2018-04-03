# 4. Risks that Solution Causes {#risks-that-solution-causes}

## Shared Responsibility Model

The risk is simply a lack of knowledge mainly from the speed at which technological solutions are changing, and the fact that you must keep up with it.

## CSP Evaluation {#cloud-risks-that-the-solution-causes-csp-evaluation}

There shouldn't be any risks when simply asking questions and analysing the responses. This is all part of helping you to build a better picture of where your current or prospective CSP is at on the security maturity model. Therefore, helping you to ascertain whether you should stick with them, select them as your CSP, and/or what your responsibility is to achieve the security maturity you require.

## Cloud Service Provider vs In-house {#cloud-risks-that-the-solution-causes-cloud-service-provider-vs-in-house}

In the [Countermeasures](#cloud-countermeasures-cloud-service-provider-vs-in-house) section I provided you a good number of points to consider, rather than outright solutions. These mostly depend on the specifics of your organisation, which you will have to weigh up. There is no one answer for all, you need to consider all options and make decisions based on what suites your organisation the best.

## People

Refer to the "Risks that Solution Causes" subsection of the People chapter of [Fascicle 0](https://f0.holisticinfosecforwebdevelopers.com).

## Application Security

Refer to the [Risks that Solution Causes](#web-applications-risks-that-solution-causes) subsection of the Web Applications chapter.

## Network Security

Refer to the [Risks that Solution Causes](#network-risks-that-solution-causes) subsection of the Network chapter.

## Violations of Least Privilege

Granting the minimum permissions required takes more work because you have to actually work out what is required.

* **Running services as root**: Removing permissions once a service has been running as root, may cause errors
* **Configuration Settings Changed Ad Hoc**: Because features and settings will be changed on an ad hoc basis, and change control, like security, is often seen as an annoyance. If it can be bypassed, it will be, and those changes will be forgotten.  
   
   AWS as many other CSPs provide many great tools to help us harden our configuration and infrastructure. If we decide not to take [our part](#cloud-countermeasures-shared-responsibility-model-csp-customer-responsibility) of the shared responsibility model seriously, then it is just time before we are compromised
* **Machine Instance Access To Open**: Locking the source IP address down to just one address that people can administer your machine instances from will make it difficult for workers outside a single office to connect to your machine instances

## Storage of Secrets

This involves a good sense of "smell" to sniff out all the possible leaking secrets. This sense may need to be developed.

### Private Key Abuse

The biggest issue I see in these situations is company culture. This needs to be addressed from both bottom up and top down.

#### SSH

Taking away your Developer's SSH access will not work unless the work that they would need SSH access for is automated. This is usually the best option anyway.

#### TLS

The Countermeasures just require some thought, establishing process, and not much more.

### Credentials and Other Secrets

It could be slightly inconvenient to maintain multiple users, rather than all users using a single user.

#### Entered by People (manually)

Password databases/managers can provide a huge improvement over resorting to storing secrets in insecure places such as unencrypted files, on post-it notes, and using the same or similar passwords across many accounts. If a password database is used correctly, all passwords will be unique and unable to be memorised.

There is risk to using a password database but not changing habits such as above, you may improve your security only slightly at the best. You must change the way you think about passwords and other secrets you enter manually.

There are [tools](https://github.com/denandz/KeeFarce) that can break password databases too. Understand how they do this and make sure you do not do what they require to succeed. The shorter the duration you have the password database running, the less likely an attack will be successful. Also configure the duration that a secret is in memory to the minimum possible.

#### Entered by Software (automatically) {#cloud-risks-that-solution-causes-storage-of-secrets-credentials-and-other-secrets-entered-by-software}

In order for an application or service to access the secrets provided by one of these tools, it must also be able to authenticate itself, which means we have replaced the secrets in the configuration file with another secret to access that initial secret, thus making the whole strategy not that much more secure, unless you are relying on obscurity. This is commonly known as the [secret zero](https://news.ycombinator.com/item?id=9453754) problem.

## Serverless

Many of the gains that attract people to the serverless paradigm are imbalanced by the extra complexities that require understanding in order to secure the integration of the components. There is a real danger that developers will fail to understand and implement all the security countermeasures required to achieve a similar security stand point they enjoyed when having their components in a less distributed manner and running in long-lived processes.

### Functions

API keys are great, but not so great when they reside in untrusted territory, which in the case of the web, is any time your users need access to your API. Anyone permitted to become a user has permission to send requests to your API.

Do not depend on the client side API keys for security, this is a very thin layer of defence. You can not protect API keys sent to a client over the Internet. Yes, we have TLS, but that will not stop an end user masquerading as someone else.

Also consider anything you put in source control, even if not public, already compromised. Your source control is only as strong as the weakest password of any given team member at best. You also have build pipelines that are often leaking, along with other leaky mediums such as people.

AWS as the largest CSP is a primary target for attackers.

The risk with application security is that it needs to be learned, and most developers currently do not have a great understanding of it.

### DoS of Lambda Functions

Some trial and error is probably necessary here, just make sure you err on the side of caution, or else you could be up for some large billing stress. This may not even be from an attacker, it could simply be due to your own coding or configuration mistakes, as already mentioned.

### Frameworks

These frameworks may lead the developer to think that the framework does everything for them, it does not. Although using a framework will abstract some operations, it is also another thing to learn.

## Infrastructure and Configuration Management

Time is required to codify and automate your infrastructure and configuration. Creating a repeatable process that continues to add the same bugs is a risk.

## AWS

### Additional Tooling

Relying on tooling alone to provide visibility on errors and defects is a risk.
