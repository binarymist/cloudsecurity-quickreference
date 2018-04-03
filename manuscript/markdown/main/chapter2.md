# 2. Identify Risks {#identify-risks}

Some of the processes we described at the top level in the [30,000' View chapter](https://f0.holisticinfosecforwebdevelopers.com/chap03.html#starting-with-the-30000-foot-view) of [Fascicle 0](https://f0.holisticinfosecforwebdevelopers.com/toc.html) may be worth revisiting.

## Shared Responsibility Model {#cloud-identify-risks-shared-responsibility-model}

The shared responsibility model is one that many have not grasped or understood well. Let's look at the responsibilities of the parties involved.

### CSP Responsibility

The CSP takes care of the infrastructure, not the customer specific configuration of it. Due to the sheer scale of what they are building, the CSP is often able to build in good security controls, in contrast to the average system administrator, who has limited resources or ability to focus on security to the same degree.

Again, due to sheer scale, the average CSP has a concentrated group of good security professionals versus a business who's core focus is often not security related. CSPs provide good security mechanisms, but the customer has to know and care enough to use them.

CSPs who architect infrastructure, build components, frameworks, hardware, and platform software in most cases take security seriously and are doing a reasonable job.

### CSP Customer Responsibility {#cloud-identify-risks-shared-responsibility-model-csp-customer-responsibility}

CSP customers are expected to be responsible for their own security as it pertains to:

1. Their people working with the technology
2. [Application security](#web-applications), specific to shortcomings in people: lack of skills, experience, engagement, etc.
3. Configuring the infrastructure and/or platform components, again referencing people defects

All too often the customer's responsibility is neglected, which renders the Cloud no better for the customer in terms of security.

> The primary problem with the Cloud is this: customers have the misconception that someone else is taking care of all their security. That is not how the shared responsibility model works though. Yes, the CSP is probably taking care of infrastructure security, but other forms of security as listed above are even more important than before the shift to the Cloud. These items are now the lowest hanging fruit for the attacker.

The following are a set of questions (verbatim) I have been asked recently, and that I hear similar versions of frequently:

* _As a software engineer, do I really care about physical network security and network logging?_
* _Surely "as a software engineer", I can just use TLS and that is the end of it?_
* _If the machine is compromised, do we give up on security because we aren't responsible for the network?_
* _What is the difference between application security and network security? Aren't they just two aspects of the same thing?_
* _If I have implemented TLS for communication, have I fixed all of the network security problems?_

## CSP Evaluation {#cloud-identify-risks-csp-evaluation}

CSPs are constantly changing their terms and conditions, as well as many other components and aspects of what they offer. I have compiled a set of must-answer questions to quiz your CSP with as part of your threat modelling before (or even after) you sign their service agreement.  
Most of these questions were already part of my [Cloud vs In-house talk](https://binarymist.io/talk/saturn-2015-talk-does-your-cloud-solution-look-like-a-mushroom/) at the Saturn Architects conference I spoke at. I recommend using these as a basis for identifying risks that are important for you to consider. This should make you well armed to come up with countermeasures and think of any additional risks.

1. Do you keep a signed audit log of what actions users performed, and when, via UIs and APIs?  
   
   Both authorised and unauthorised users are more careful about the actions they take, or do not take, when they know their actions are being recorded and are potentially being watched  
   
2. How do you enact the shared responsibility model between CSPs and their customers? Please explain your role and my role in the protection of my and my customers data.  
   
   You will almost certainly not have complete control over the data you entrust to your CSP, but they will also not assume responsibility over the data you entrust to them, or how it is accessed. One example of this might be, how do you preserve secrecy for data at rest? For example, are you using the most [suitable KDF](#web-applications-countermeasures-data-store-compromise) and adjusting the number of iterations applied each year (as discussed in the [MembershipReboot](#web-applications-countermeasures-lack-of-authentication-authorisation-session-management-technology-and-design-decisions-membershipreboot) subsection of the Web Applications chapter) to the secrets stored in your data stores? The data you hand over to your CSP is no more secure than we discuss in the Management of Application Secrets subsections of the Web Applications chapter and in many cases has the potential to be less secure for some of the following reasons:  
   
   * An often encountered false assumption is that somehow the data you provide is safer by default on your CSP's network
   * Your CSP can be forced by governing authorities to give up the data you entrust to them, as we discuss in the [Giving up Secrets](#cloud-identify-risks-cloud-service-provider-vs-in-house-giving-up-secrets) subsection  
   
3. Do you encrypt all communications between servers within your data centres as well as your service providers?  
   
   How is your data encrypted in transit (as discussed in the Management of Application Secrets subsections of the Web Applications chapter)? In reality, you have no idea what paths it will take once in your CSPs possession, and could very well be intercepted without your knowledge.  
   
   * You have little to no control over the network path that the data you provide will travel on
   * There are more parties involved in your CSPs infrastructure than on your own network  
   
4. Do you provide access to logs, if so, what sort of access, and to what sort of logs?  
   
   Hopefully you will have easy access to any and all logs, just as you would if it was your own network. That includes hosts, routing, firewall, and any other service logs  
   
5. What is your process around terminating my contract with you and/or moving to another CSP?  
   
   No CSP is going to last forever, termination or migration is inevitable, it is just a matter of when  
   
6. Where do your servers, processes and data reside physically?  
   
   As we discuss a little later in the Cloud Services Provider vs In-house subsection of Countermeasures, your data is governed by different people and jurisdictions depending on where it physically resides. CSPs have data centres in different countries and jurisdictions, each having different data security laws


7. Who can view the data I store in the Cloud?  
   
   Who has access to view this data? What checks and controls are in place to make sure that this data cannot be exfiltrated?  
   
8. What is your Service Level Agreement (SLA) for uptime?  
   
   Make sure you are aware of what the uptime promises mean in terms of real time. Some CSPs will allow 99.95% uptime if you are running on a single availability zone, but closer to 100% if you run on multiple availability zones. Some CSPs do not have a SLA at all.  
   
   CSPs will often provide credits for the downtime, but these credits in many cases may not cover the losses you encounter during high traffic events  
   
9. Are you ISO/IEC 27001:2013 Certified? If so, what is within its scope?  
   
   If the CSP can answer this with a "everything" and prove it, they have done a lot of work to make this possible. This shows a certain level of commitment to their security posture. Just be aware, as with any certification, it is just that, it doesn't necessarily prove sound security  
   
10. Do you allow your customers to carry out regular penetration testing of production and/or test environments, and allow the network to be in-scope?  
    
    CSPs that allow penetration testing of their environments demonstrate that they embrace transparency and openness. If their networks stand up to penetration tests they obviously take security seriously. Ideally, this is what you are looking for. CSPs that do not permit penetration testing of their environments are usually trying to hide something. It may be that they know they have major insecurities, or a skills shortage in terms of security professionals. Worse, they may be unaware of where their security stature lies and are not willing to have their faults demonstrated  
   
11. Do you have bug bounty programmes running, if so, what do they look like?  
    
    This is another example if their programme is run well, it conveys that the CSP is open and transparent about their security faults and are willing to mitigate them as soon as possible

## [Cloud Service Provider vs In-house](https://speakerdeck.com/binarymist/does-your-cloud-solution-look-like-a-mushroom) {#cloud-identify-risks-cloud-service-provider-vs-in-house}

A question that I hear frequently is: "What is more secure, building and maintaining your own cloud, or trusting a CSP to take care of your security for you?". That is a defective question, as discussed in the [Shared Responsibility Model ](#cloud-identify-risks-shared-responsibility-model) subsections. There are [some aspects](#cloud-identify-risks-shared-responsibility-model-csp-customer-responsibility) of security that the CSP has no knowledge of, and only you as the CSP customer can better secure those areas.

Choosing a CSP means that you are depending on their security professionals to design, build, and maintain the infrastructure, frameworks, hardware and platforms. Usually large CSPs will do a decent job of this. Though, if you choose to design, build, and maintain your own in-house cloud, you will also be subject to the skills of your own engineers who have created the Cloud components you decide to use. You will ultimately be responsible for the following, along with other aspects of how these components fit together and interact with each other:

* General infrastructure
* Hardware
* Hosting
* Continuously hardening components and infrastructure
* Patching
* Network firewall routes and rules
* Network component logging
* NIDS
* Regular penetration testing
* Many other aspects covered in the VPS and Network chapters

In general, your engineers are going to have to be as good or better than those of any given CSP that you are comparing capabilities with, in order to achieve a similar level of security at the infrastructure level.

Trust is an issue with the Cloud since you do not have control of your data or the people that create and administer the Cloud environment you decide to use.

### Skills

In many cases smaller CSPs suffer from the same resourcing issues that many businesses do, with regards to having solid security skills and engagement in order for their workers to apply security in-house. To benefit from the Shared Responsibility Model of the CSP, it often pays to go with the larger CSPs.

### EULA

CSPs have the right to change their End User License Agreements (EULA) at any time. Do you actually read the EULA when you sign up for a cloud service?

### Giving up Secrets {#cloud-identify-risks-cloud-service-provider-vs-in-house-giving-up-secrets}

Hosting providers can be, and in many cases are [forced](http://www.stuff.co.nz/business/industries/67546433/Spies-request-data-from-Trade-Me) by governing authorities to [give up](https://www.stuff.co.nz/business/95116991/trade-me-fields-thousands-of-requests-for-member-information) your secrets and those of your customers. These are very unfortunate yet common scenarios, you may not even know it has happened.  
A New Zealand (NZ) media outlet, The NZ Herald [covered a story](http://www.nzherald.co.nz/business/news/article.cfm?c_id=3&objectid=11422509) where senior lawyers and the Privacy Commissioner told the Herald of their concerns of the practise which see companies coerced into giving up information to the police. Instead of seeking a legal order, police asked companies to hand over information to assist with the "maintenance of the law". They threatened them with prosecution if they told the person whom they are interested in, and accept the data with no record keeping to show how often requests are made. The request from the police carries no legal force at all, yet is regularly complied with.

### Location of Data

As touched on in the CSP Evaluation questions, CSPs are outsourcing services to several providers. They often don't have visibility themselves and sometimes the data is hosted in other jurisdictions where further control is lost.

### Vendor lock-in

This not only applies to the Cloud versus In-house, it also applies to open technologies in the Cloud versus closed/proprietary offerings.

There is a certain reliance on vendor guarantees, which are not usually an issue. However, the issue is more commonly our lack of fully understanding what part we play in the shared responsibility model.

What happens when you need to move from your current CSP? How much do you have invested in proprietary services such as [serverless](#cloud-identify-risks-serverless) offerings? What would it cost for your organisation to port to another CSP's environment? Are you getting so much benefit that it doesn't really matter? If you are thinking this way, then you could be missing many of the steps you should be taking as your part of the shared responsibility model. We investigate this further throughout this chapter. Serverless technologies really look great until you [measure](#cloud-identify-risks-serverless) the costs of [securing everything](#cloud-countermeasures-serverless). Weigh both the costs and benefits. 

### Possible Single Points of Failure

There are plenty of single points of failure in the Cloud, such as:

* Machine instance dies
* Docker container dies
* Availability Zone goes down
* Region goes down
* Multiple Regions go down
* Account Takeover occurs

## Review Other Chapters {#cloud-identify-risks-review-other-chapters}

There is a lot in common on the topic of Cloud security in the other chapters of this fascicle and Fascicle 0. 

This point in the chapter, is a good time to orient / reorient yourself with the related topics / concepts from the other chapters. From here on in, I will assume that you can apply the knowledge from the other chapters to the topic of Cloud security without me having to revisit large sections of it, specifically:

**[Fascicle 0](https://leanpub.com/holistic-infosec-for-web-developers/)**:

People chapter

* Ignorance
* Morale, Productivity and Engagement Killers
* Weak Password Strategies 

**This Fascicle**:

[VPS](#vps) chapter

* Forfeit Control thus Security
* Weak Password Strategies
* Root Logins
* SSH
* Lack of Visibility
* Docker
* Using Components with Known Vulnerabilities
* Lack of Backup

[Network](#network) chapter

* Fortress Mentality
* Lack of Visibility
* Data Exfiltration, Infiltration

[Web Applications](#web-applications) chapter

* Most / all of it

## People

You might ask what people have to do with cloud security. From my experience working as a consulting Architect, Engineer, and Security Professional for many organisations and their teams, has shown me, that in the majority of security incidents, reviews, tests and redesigns, the root cause of failures stems back to people defects. This is recognised as the number one issue of the [CSP Customer Responsibility](#cloud-identify-risks-shared-responsibility-model-csp-customer-responsibility) in the Shared Responsibility Model. As people, we are our own worst enemies. We can be the weakest and the strongest links in the security chain. The responsibility falls squarely in our own laps.

You will notice that most of the defects addressed in this chapter come down to people:

* Missing a step in a sequence (often performed manually rather than automatically)
* Lacking the required knowledge
* Lacking the required desire / engagement

## Application Security

On the Software Engineering Radio show, I hosted an interview with Zane Lackey on [Application Security](https://binarymist.io/publication/ser-podcast-application-security/), it is well worth listening to.

With the shift to the Cloud, AppSec has become more important than it used to be, recognised and discussed:

* Previously in this chapter by the number two issue of the [CSP Customer Responsibility](#cloud-identify-risks-shared-responsibility-model-csp-customer-responsibility) of the Shared Responsibility Model
* In the [Application Security](#vps-countermeasures-docker-application-security) subsection of Docker in the VPS chapter
* Entirely in the next chapter (Web Applications)

In general, as discussed in the [Shared Responsibility Model](#cloud-identify-risks-shared-responsibility-model), the dedicated security resources, focus, awareness, engagement of our major CSPs are usually greater than most organisations have access to. This pushes the target areas for the attackers further up the tree. People, followed by AppSec, represent the lowest hanging fruit for the attackers.

## Network Security {#cloud-identify-risks-network-security}

The network between the components you decide to use in the Cloud will almost certainly no longer be administered by your network administrator(s), but rather by you as a Software Engineer. Networks are now [expressed as code](#cloud-identify-risks-infrastructure-and-configuration-management), and because coding is one of your responsibilities as a Software Engineer, the network will more than likely be left to you to design and code for. You will need to have a good understanding of [Network Security](#network).

## Violations of [Least Privilege](#web-applications-countermeasures-management-of-application-secrets-least-privilege) {#cloud-identify-risks-violations-of-least-privilege}

The principle of Least Privilege is an essential aspect of defence in depth, stopping an attacker from progressing is essential.

The attack and demise of [Code Spaces](https://cloudacademy.com/blog/how-codespaces-was-killed-by-security-issues-on-aws-the-best-practices-to-avoid-it/) is a good example of what happens when least privilege is not kept up with. An unauthorised attacker gained access to the Code Spaces AWS console and deleted everything attached to their account. Code Spaces was no more, they could not recover.

In most organisations I have worked for as an architect or engineer, I have seen many cases of least privilege violations. We discuss this principle in many places through this entire book series. It is a concept that needs to become part of your very instinct. The principle of least privilege asserts that no actor should be given more privileges than is necessary to do their job.

Here are some examples of violating least privilege:

**[Fascicle 0](https://leanpub.com/holistic-infosec-for-web-developers/)**:

* Physical chapter: Someone has access to a facility that they do not need access to in order to do their job. For example, a cleaner having access to a server room, or any room where they could possibly exfiltrate or infiltrate anything
* People chapter: In a phishing attack, the attacker may have access to an email address to use for a FROM address, making an attack appear more legitimate. The attacker should not have access to an email address of an associate of their target, this violates least privilege

**This Fascicle**:

* VPS chapter: We discussed privilege escalation. This is a direct violation of least privilege, because after escalation, attackers now have additional privileges
* Network chapter: We discussed [lack of segmentation](#network-identify-risks-lack-of-segmentation). I also discussed this with [Haroon Meer](https://twitter.com/haroonmeer) on the [Network Security](https://binarymist.io/publication/ser-podcast-network-security/) show I hosted for Software Engineering Radio. This for example could allow an attacker that has gained control of a network to have unhindered access to all of an organisation's assets. This is often due to a monolithic network segment rather than having assets on alternative network segments with firewalls between them
* Web Applications chapter: We discuss setting up data-store accounts that only have privileges to query the stored procedures necessary for a given application, thus reducing the power that any possible SQL injection attack may have to carry out arbitrary commands. [Haroon Meer](https://twitter.com/haroonmeer) also discussed this as a technique for exfiltration of data in the [Network Security](https://binarymist.io/publication/ser-podcast-network-security/) podcast

Hopefully you are getting more clarity on least privilege, and its propensity to break down in a cloud environment. Some examples:

* **Running services as root**: A perfect example of this is running a docker container that does not specify a non-root user in its image. Docker will default to root if not configured otherwise, as discussed in the [Docker](#vps-identify-risks-docker-the-default-user-is-root) subsection of the VPS chapter
* **Configuration Settings Changed Ad Hoc**: Because there are so many features and configurations that can be easily modified, developers and administrators will modify them. For example, someone needs immediate access when we are in the middle of something else, that we quickly modify a permissions setting without realising we have just modified that permission for a group of other people as well. It is so easy to make ad hoc changes, they will be made
* **Machine Instance Access To Open**: Is an attacker permitted to access your machine instances from anywhere? If so, this is an additional attack surface

### Machine Instance Single User Root {#cloud-identify-risks-violations-of-least-privilege-machine-instance-single-user-root}

The [default](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/managing-users.html) on AWS EC2 instances is to have a single user (root). There is no audit trail with a bunch of developers all using the same login. When ever anything happens on any of these machine instances, it can only be attributed to user `ubuntu` on an Ubuntu AMI, `ec2-user` on a RHEL AMI, or `centos` on a Centos AMI. There are so many things wrong with this approach.

### CSP Account Single User Root {#cloud-identify-risks-violations-of-least-privilege-csp-account-single-user-root}

Sharing, and using the root user unnecessarily, as I discuss in the [Credentials and Other Secrets](#cloud-identify-risks-storage-of-secrets-credentials-and-other-secrets) subsections. In this case, the business owners lost their business.

## Storage of Secrets {#cloud-identify-risks-storage-of-secrets}

I have seen a lot of mishandling of sensitive information, some examples as follows.

### Private Key Abuse

Below are some of the ways I have seen private keys mishandled.

#### SSH {#cloud-identify-risks-storage-of-secrets-private-key-abuse-ssh}

[SSH](#vps-countermeasures-disable-remove-services-harden-what-is-left-ssh) key-pair auth is no better than password auth if it is abused in the following way, in-fact it may even be worse. I have seen some organisations who store a single private key with no pass-phrase for all of their EC2 instances in their developer wiki. All or many of the developers have access to this, with the idea being that they just copy the key from the wiki to their local `~/.ssh/`. There are a number of things wrong with this. 

* Private key is not private if it is shared amongst the team
* No pass-phrase, means no second factor of authentication
* Because there is only one user (single key-pair) being used on the VPSs, there is also no audit trail
* The weakest link is the weakest wiki password of all the developers, and we all know how weak that is likely to be, with a bit of reconnaissance, probably guessable in a few attempts without any password profiling tools. I have discussed this and demonstrated a collection of password profiling tools in the "Weak Password Strategies" subsection of the People chapter of [Fascicle 0](https://leanpub.com/holistic-infosec-for-web-developers/). Once the attacker has the weakest password, then they own all of the EC2 (if on AWS) instances, or any resource that is using key-pair authentication. If the organisation is failing this badly, then they almost certainly will not have any password complexity constraints on their wiki either

Most developers will also blindly accept what they think are the server key fingerprints without verifying them, which opens them up to a MItM attack, as discussed in the VPS chapter under the [SSH subsection](#vps-countermeasures-disable-remove-services-harden-what-is-left-ssh-establishing-your-ssh-servers-key-fingerprint). This quickly moves from just being a technical issue to a cultural one, where people are trained to accept that the server is who it says it is. The fact that they have to verify the fingerprint is essentially a step that gets in their way.

#### TLS {#cloud-identify-risks-storage-of-secrets-private-key-abuse-tls}

When Docker reads the instructions in the following `Dockerfile`, an image is created that copies your certificate, private key, and any other secrets you have declared, and adds them to an additional layer and forms the resulting image. Both `COPY` and `ADD` will bake what ever you are copying or adding into an additional layer or delta, as discussed in the [Consumption from Registries](#vps-countermeasures-docker-consumption-from-registries) Docker subsection in the VPS chapter. Whoever can access this image from a public or less public registry now has access to your certificate and even worse your private key.

Anyone can see how these images were built using these tools:

* [dockerfile-from-image](https://github.com/CenturyLinkLabs/dockerfile-from-image)
* [ImageLayers](https://imagelayers.io/)

The `ENV` command similarly adds the `dirty little secret` value as the `mySecret` key into the image layer.

{id="dockerfile-private-key-abuse", title="Private key abuse with Dockerfile", linenos=off}
    FROM nginx

    # ...
    COPY /host-path/star.mydomain.com.cert /etc/nginx/certs/my.cert
    COPY /host-path/star.mydomain.com.key /etc/nginx/certs/my.key
    ENV mySecret="dirty little secret"
    COPY /host-path/nginx.conf /etc/nginx/nginx.conf 
    # ...

### Credentials and Other Secrets {#cloud-identify-risks-storage-of-secrets-credentials-and-other-secrets}

Sharing accounts, especially super-user accounts on the likes of [machine instances](#cloud-identify-risks-violations-of-least-privilege-machine-instance-single-user-root) and worse, your CSP IAM account(s), and even worse still, the account [root user](#cloud-identify-risks-violations-of-least-privilege-csp-account-single-user-root). I have seen organisations where they only had the single default AWS account [root user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html) (you are given when you first sign up to AWS), shared amongst several teams of Developers and managers, and on the organisations wiki, which in itself is a big security risk. Subsequently, one such organisation had one of the business owners go rogue, and change the single password and locked everyone else out.

#### Entered by People (manually)

Developers and others putting user-names and passwords into company wikis, source control, or anywhere where there is a reasonably good chance that an unauthorised person will be able to view them with a little to moderate amount of persistence. This was discussed above in the [SSH](#cloud-identify-risks-storage-of-secrets-private-key-abuse-ssh) section. When you have a team of developers sharing passwords, the weakest link is usually very weak, and that is only if you are considering outsiders to be a risk. According to the study I discussed in the [Fortress Mentality](#network-identify-risks-fortress-mentality) subsection of the network chapter, this would be a mistake, with about half of the security incidents being carried out from inside of an organisation.

#### Entered by Software (automatically)

Whatever you use to get work done in the Cloud programmatically, you are going to need to authenticate the process at some point. I see a lot of passwords in configuration files, stored in:

* Source control
* [Dropbox](#network-identify-risks-data-exfiltration-infiltration-dropbox)
* Many other insecure mediums

This is a major insecurity.

## Serverless {#cloud-identify-risks-serverless}

Serverless is not serverless, but the idea is that as a Software Engineer, you do not think about the physical machines that your code will run on. You can also focus on small pieces of functionality without understanding all of the interactions and relationships of the code you write.

### Third Party Services

There is a lot of implicit trust put in third party services that components of your serverless architecture consume.

### Perimeterless

Any perimeters that you used to, or at least thought you had, are gone. We discussed this in the [Fortress Mentality](#network-identify-risks-fortress-mentality) subsection of the Network chapter.

### Functions

[Amazon](https://aws.amazon.com/serverless/) has [Lambda](https://aws.amazon.com/lambda/) which can run Java, C#, Python, Node.js.

[GCP](https://cloud.google.com/serverless/) has [Cloud Functions](https://cloud.google.com/functions/) which are JavaScript functions.

Azure has [Functions](https://azure.microsoft.com/en-us/services/functions/).

AWS's complexity alone causes a lot of Developers to just "get it working" if they are lucky, then push it through to production. Then as an adverse side effect, security is, in most cases, overlooked. With AWS Lambda, you need to first:

1. Pick your function from a huge collection
2. Pick the trigger (event) from one of the many AWS services available
3. Choose an API Gateway to allow invocation of your function from the Internet

What represents security when it comes to the Serverless paradigm?

The target areas for the attacker change, they just move closer to the application. In order of most important first, we have:

1. **[Application Security](#web-applications)**: Functions are still just code. Now that some other areas of infrastructure have become harder to compromise, attackers focus more on application security, and as usual, this is a weak area for most developers. Also consider the huge threat surface of depending on other open source consumables, as discussed in the Web Applications chapter "[Consuming Free and Open Source](#web-applications-identify-risks-consuming-free-and-open-source)" subsection
2. Identity and Access Management (**IAM**) and permissions. What permissions does an attacker have to execute in any given environment, including any and all services consuming and consumed by functions 
3. **API key**, a distant third

Rich Jones demonstrated what can happen if you fail at the above three points in AWS in his talk "[Gone in 60 Milliseconds](https://www.youtube.com/watch?v=YZ058hmLuv0)":

* Getting some exploit code into an S3 bucket via an injection vulnerability and passing a parameter that references the injected key value
* `/tmp` is writeable
* Persistence is possible if you keep the container warm
* Lots of other attack vectors

### DoS of Lambda Functions

The compute executing the functions you supply is short lived. With AWS, [containers are used](https://docs.aws.amazon.com/lambda/latest/dg/lambda-introduction.html) and reused providing your function runs at least once approximately every four minutes and thirty seconds, according to Rich Jones' talk. The idea of hardware DoS is less likely, but [billing DoS](https://thenewstack.io/zombie-toasters-eat-startup/) is a [real issue](https://sourcebox.be/blog/2017/08/07/serverless-a-lesson-learned-the-hard-way/).

AWS Lambda will, by default allow any given function a [concurrent execution limit](https://docs.aws.amazon.com/lambda/latest/dg/concurrent-executions.html#concurrent-execution-safety-limit) of 1000 per region. 

## Infrastructure and Configuration Management {#cloud-identify-risks-infrastructure-and-configuration-management}

The glaringly obvious risks with the management of configuration and infrastructure as code is the management of secrets, and most other forms of information security. "Huh?" I hear you say. Let me try and unpack that statement.

When you create and configure infrastructure as code, you are essentially combining many technical aspects: machine instances (addressed in the VPS chapter), networking (as addressed in the Network chapter), the Cloud, and your applications (as addressed in the Web Applications chapter), and bake them all into code to be executed. If you create security defects as part of the configuration or infrastructure, then lock them up in code, you will have the same consistent security defects each time that code is run. Hence,  Software Engineers now need to understand much more than they used to about security. We are now responsible for so much more than we used to be.

Now we will focus on a collection of the largest providers.

## AWS {#cloud-identify-risks-aws}

The AWS section is intended as overflow for items that have not been covered elsewhere in this chapter, and requires some attention.

The [CIS AWS Foundations document](https://d0.awsstatic.com/whitepapers/compliance/AWS_CIS_Foundations_Benchmark.pdf) is very useful in understanding some of the risks in AWS, as well as auditing options to determine if they exist currently. This document also includes countermeasures, including clear direction on how to apply them. This is well worth following along with as you read through this chapter.

AWS is continually announcing and releasing new products, features and configuration options. The attack surface just keeps expanding. AWS does an incredible job of providing security features and options for its customers, but,  just as the [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/) states, "_security in the Cloud is the responsibility of the customer_". AWS provides the security, you have to decide to use it and educate yourself on doing so. Obviously if you are reading this, you are already well down this path. If you fail to use, and configure correctly, what AWS has provided, your attackers will at the very minimum use your resources for evil, and you will foot the bill. Even more likely, they will attack and steal your business assets, and bring your organisation to its knees. 

### Password-less sudo

Password-less sudo. A low privileged user can operate with root privileges. This is essentially as bad as root logins.

%% https://serverfault.com/questions/615034/disable-nopasswd-sudo-access-for-ubuntu-user-on-an-ec2-instance

%% https://appsecday.com/schedule/hacking-aws-end-to-end/

%% Kiwicon 10 talk "Hacking AWS end to end". Slide-deck here: https://github.com/dagrz/aws_pwn/blob/master/miscellanea/Kiwicon%202016%20-%20Hacking%20AWS%20End%20to%20End.pdf, along with readme and code.

%% [Cognito](https://aws.amazon.com/cognito/)
