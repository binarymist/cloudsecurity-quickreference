# 3. Countermeasures {#countermeasures}

Revisit the [Countermeasures subsection](https://f0.holisticinfosecforwebdevelopers.com/chap03.html#starting-with-the-30000-foot-view-countermeasures) of the first chapter of [Fascicle 0](https://f0.holisticinfosecforwebdevelopers.com/).

As I briefly touch on in the [CSP Account Single User Root](#cloud-countermeasures-violations-of-least-privilege-csp-account-single-user-root) subsection, [Canarytokens](https://canarytokens.org/) are excellent tokens you can drop anywhere on your infrastructure. When an attacker opens one of these tokens, an email will be sent to a pre-defined email address with a specific message that you define. This provides an early warning that someone unfamiliar with your infrastructure is running things that do not normally get run. There are quite a few different tokens available and new ones are being added every so often. These tokens are very quick and free to generate, and can be dropped wherever you like on your infrastructure. [Haroon Meer](https://twitter.com/haroonmeer) discusses these on the [Network Security](https://binarymist.io/publication/ser-podcast-network-security/) show I hosted for Software Engineering Radio near the end of the episode.

### Shared Responsibility Model

The following responsibilities are those that you need to have a strong understanding of in order to establish a good level of security when operating in the Cloud.

#### CSP Responsibility 

There is not a lot you can do about this, just be aware of what you are buying into before you do so. [AWS for example](https://aws.amazon.com/compliance/shared-responsibility-model/) states: "_Customers retain control of what security they choose to implement to protect their own content, platform, applications, systems and networks, **no differently than they would for applications in an on-site** datacenter._"

#### CSP Customer Responsibility {#cloud-countermeasures-shared-responsibility-model-csp-customer-responsibility}

If you leverage the Cloud, make sure the following aspects of security are all at an excellent level:

1. People security: Discussed in Fascicle 0 under the People chapter
2. [Application security](https://f1.holisticinfosecforwebdevelopers.com/chap06.html#web-applications): Discussed in the Web Applications chapter of [Holistic Info-Sec for Web Developers, Fascicle 1](https://f1.holisticinfosecforwebdevelopers.com/). The move to application security was also discussed in my [Docker Security book](https://binarymist.io/publication/docker-security/)
3. Configuring the infrastructure and/or platform components: Usually CSP specific, but I cover some aspects in this chapter

The following is in response to the set of frequently asked questions under the [risks subsection](#cloud-identify-risks-shared-responsibility-model-csp-customer-responsibility) of CSP Customer Responsibility:

* **(Q)**: _As a software engineer, do I really care about physical network security and network logging?_  
   
   **(A)**: In the past, many aspects of [network security](#cloud-identify-risks-network-security) were the responsibility of the Network Administrators, but with the move to the Cloud, this has, to a large degree changed. The networks established (intentionally or not) between the components that we are leveraging and creating in the Cloud are a result of Infrastructure and Configuration Management, often (and rightly so) expressed as code, or specifically Infrastructure as Code (IaC). As discussed in the [Network Security](#cloud-identify-risks-network-security) subsection, this is now the responsibility of the Software Engineer  
   
* **(Q)**: _Surely "as a software engineer", I can just use TLS and that is the end of it?_  
   
   **(A)**: TLS is one very small area of network security. Its implementation as HTTPS and the PKI model is effectively [broken](https://f1.holisticinfosecforwebdevelopers.com/chap04.html#network-identify-risks-tls-downgrade). If TLS is your only saviour, putting it bluntly, you are without hope. The [Network chapter](https://f1.holisticinfosecforwebdevelopers.com/chap04.html#network) of Holistic Info-Sec for Web Developers, Fascicle 1, covers the tip of the network security ice berg, as network security is a huge topic, one that has many books written about it, along with other resources. These provide more in-depth coverage than I can provide as part of a holistic view of security for Software Engineers. Software Engineers must come to grips with the fact that they need to implement defence in depth  
   
* **(Q)**: _If the machine is compromised, then we give up on security, we aren't responsible for the network_  
   
   **(A)**: Please refer to the [VPS chapter](https://f1.holisticinfosecforwebdevelopers.com/chap03.html#vps) of Holistic Info-Sec for Web Developers, Fascicle 1, for your responsibilities as a Software Engineer in regards to "the machine". In regards to "the network", please refer to the [Network Security](#cloud-identify-risks-network-security) subsection  
   
* **(Q)**: _What is the difference between application security and network security? Aren't they just two aspects of the same thing?_  
   
   **(A)**: No, for application security, see the [Web Applications chapter](https://f1.holisticinfosecforwebdevelopers.com/chap06.html#web-applications) of Holistic Info-Sec for Web Developers, Fascicle 1. For network security, see the [Network chapter](https://f1.holisticinfosecforwebdevelopers.com/chap04.html#network) of Holistic Info-Sec for Web Developers, Fascicle 1. Again, as Software Engineers, you are now responsible for all aspects of information security  
   
* **(Q)**: _If I have implemented TLS for communication, have I fixed all of the network security problems?_  
   
   **(A)**: If you are still reading this, I'm pretty sure you know the answer, please share it with other Developers and Engineers as you receive the same questions

### CSP Evaluation {#cloud-countermeasures-csp-evaluation}

Once you have sprung the questions from the [CSP Evaluation](#cloud-identify-risks-csp-evaluation) subsection in the Identify Risks subsection on your service provider and received their answers, you will be in a good position to feed these into the following subsections.


1. Do you keep a signed audit log on which users performed which actions and when, via UIs and APIs?  
   
   On AWS you can enable [CloudTrail](https://aws.amazon.com/cloudtrail/) to log all of your API calls, command line tools, SDKs, and Console interactions. This will provide a good amount of visibility of who has been accessing the AWS resources and Identities
   
2. There is this thing called the shared responsibility model I have heard about between CSPs and their customers. Please explain what your role and my role is in the protection of mine and my customers data?  
   
   Make sure you are completely clear on who is responsible for which data, where and when. It is not a matter of if your data will be stolen, but more a matter of when. Know what your responsibilities are. As discussed in the Web Applications chapter of Holistic Info-Sec for Web Developers, under the [Data-store Compromise](https://f1.holisticinfosecforwebdevelopers.com/chap06.html#web-applications-identify-risks-management-of-application-secrets-data-store-compromise) subsection, Data-store Compromise is one of the 6 top threats facing New Zealand, and these types of breaches are happening daily.  
   
   Also consider data security insurance  
   
3. Do you encrypt all communications between servers within your data centres?  
   
   I have discussed in many places that we should aim to have all communications on any given network encrypted. Ideally end to end encrypted where possible (as discussed in the [End to End Encryption show](https://binarymist.io/publication/ser-podcast-end-to-end-encryption/) I hosted for Software Engineering Radio with guest Head of Cryptography Engineering at Tresorit, Peter Budai). Proprietary/serverless technologies can make this more difficult. If you are using usual machine instances, then in most cases, the CSPs infrastructure is logically not really any different than an in-house network, where you can encrypt your own communications.  
   
   AWS also provides [Virtual Private Cloud](https://aws.amazon.com/vpc/) (VPC), which you can build your networks within, including [Serverless](https://aws.amazon.com/serverless/) technologies. This allows for segmentation and isolation.  
   
   AWS also offers four different types of [VPN connections](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpn-connections.html) to your VPC  
   
4. Do you provide access to logs, if so what sort of access and to what sort of logs?  
   
   If you do not have access to logs, then you are flying blind, you have no idea what is happening around you. How much does the CSP strip out of the logs before they allow you to view them? It is really important to weigh up what you will have visibility to, and what you will not have visibility to, in order to work out where you may be vulnerable.  
   
   Can the CSP provide guarantees that they are taking care of those vulnerable areas? Make sure you are comfortable with the amount of visibility that you will and/or not have up front. Unless you make sure blind spots are covered, you could be unnecessarily opening yourself up to attack. Some of the CSPs log aggregators could be [flaky for example](https://read.acloud.guru/things-you-should-know-before-using-awss-elasticsearch-service-7cd70c9afb4f).   
   
   With the likes of machine instances and network components, you should be taking the same responsibilities that you would if you were self hosting. I addressed these in the VPS and Network chapters of Holistic Info-Sec for Web Developers, Fascicle 1, under the Lack of Visibility subsections.  
   
   In terms of visibility into the Cloud infrastructure, most decent CSPs provide the tooling, you just need to use it.  
   
   As mentioned in point 1 above and [Violations of Least Privilege](https://f1.holisticinfosecforwebdevelopers.com/chap06.html#web-applications-countermeasures-management-of-application-secrets-least-privilege) countermeasures, AWS provides **CloudTrail** to log API calls, Management Console actions, SDKs, CLI tools, and other AWS services. As usual, AWS has good [documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html) around what sort of log events are captured, what form they take, and the plethora of [services you can integrate](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-supported-services.html) with CloudTrail. As well as viewing and analysing account activity, you can [define AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/with-cloudtrail.html) functions to be run on the `s3:ObjectCreated:*` event that is published by S3 when CloudTrail drops its logs in an S3 bucket.  
   
   AWS **[CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)** can be used to collect and track your resource and application metrics. CloudWatch can also be used to react to collected events with the likes of Lambda functions to do your bidding.  
   
   There are also a collection of logging-specific items that you should review in the Logging subsection of the [CIS AWS Foundations document](https://d0.awsstatic.com/whitepapers/compliance/AWS_CIS_Foundations_Benchmark.pdf)  
   
5. What is your process around terminating my contract with you and/or moving to another CSP?  
   
   Make sure you have an [exit and/or migration](http://blog.sysfore.com/do-you-have-your-cloud-exit-plan-ready/) strategy planned as part of entering into an agreement with your chosen CSP. Make sure it is incorporated as part of the contract with your chosen CSP.  
   
   * What is the CSP is going to do to assist in terminating and/or migrating your data and services from the CSP. Consider how long it may take you to move your data at slow internet speeds if you have large amounts stored. Will you be able to use the CSPs proprietary [API based technique](http://searchcloudstorage.techtarget.com/opinion/The-need-for-a-cloud-exit-strategy-and-what-we-can-learn-from-Nirvanix) for migrating your data from the current CSP to a new CSP? If your current CSP goes out of business, you may only have two weeks to move everything and set-up shop with another cloud, as was the case with [Nirvanix](http://searchcloudstorage.techtarget.com/news/2240205813/Nirvanix-cloud-customers-face-worse-nightmares). The tighter you integrate your current CSP and leverage their proprietary services, the more work it will be to move. At the same time, the less you depend on your CSPs proprietary services, the [less benefit](http://www.theserverside.com/feature/Getting-out-is-harder-than-getting-in-The-importance-of-a-cloud-exit-strategy) you will get from them. This is why threat modelling is an essential part of discovering a strategy that works for your organisations requirements
   * How does the CSP deal with your data and services when your contract is terminated, does it lie around somewhere for some time? Ideally, be certain that it is completely purged so that it is not available on their network at all. If it remains there for a duration, is it discoverable by an attacker? Will they let you test this? If not, they are probably trying to hide something. Remember, often a greater number of attacks come from within the organisation rather than from external
   * Does the CSP have third parties that audit, test and certify the completeness of the termination/migration procedure  
   
6. Where do your servers, processes and data reside physically?  
   
   Do not assume that your data in the Cloud in another country is governed by the same laws as it is in your country. Make sure you are aware of the laws that apply to your data, depending on where it is held 
   
7. Who can view the data I store in the Cloud?  
   
   Technically, anyone can. In the case of AWS, they will not purposely disclose your data to anyone, unless required to by law. There are a few things you need to consider here such as:  
   
   * There are many ways for an attacker to get at your data illegally, and again, that does not exclude insiders, as we discuss throughout this chapter. Just as if your data was discovered in your own in-house network, if you fail to take the precautions discussed throughout this chapter, especially around least privilege, then, as in the [attack on Code Spaces](#cloud-identify-risks-violations-of-least-privilege) we discussed in the Violations of Least Privilege subsection, you may be equally open to exploitation
   * In many cases you will not know if your data has been released to authorities, we discussed this in the [Giving up Secrets](#cloud-identify-risks-cloud-service-provider-vs-in-house-giving-up-secrets) countermeasures subsection
   * Defence in depth, tells us that we need to [encrypt our data at rest](https://f1.holisticinfosecforwebdevelopers.com/chap06.html#web-applications-countermeasures-data-store-compromise), and in transit, at a minimum. With this taken care of, when our data does fall into the hands of those we do not want to have it, it will be of little to no use to them in its encrypted form. I also discuss this in depth in my [interview](https://binarymist.io/publication/ser-podcast-end-to-end-encryption/) with Head of Cryptography Engineering at Tresorit, Peter Budai on the topic of End to End Encryption  
     * **At rest**: For starters, do not neglect what is discussed in the Web Applications chapter of Holistic Info-Sec for Web Developers, Fascicle 1, around protecting sensitive information at the application level (in code that is). AWS for example provides [EC2 Instance Store Encryption](https://aws.amazon.com/blogs/security/how-to-protect-data-at-rest-with-amazon-ec2-instance-store-encryption/), which provides disk and file system encryption, encryption for EBS volumes, S3 buckets, and RDS. AWS also provides Elastic File System [(EFS)encryption](https://aws.amazon.com/about-aws/whats-new/2017/08/amazon-efs-now-supports-encryption-of-data-at-rest/). As usual, it is your responsibility to use these offerings
     * **In transit**: Most decent CSPs will provide options for TLS, and you should also be leveraging TLS in your applications
     * **In use**: This is still an area of research, some progress is being made though. [Ben Humphreys spoke](https://2016.chcon.nz/talks.html#1245) about this at New Zealand's hacker conference in Christchurch CHCon. A conference I helped co-found here in my home town  
   
8. What is your Service Level Agreement (SLA) for uptime?  
   
   Count this cost before signing up to the CSP  
   
9. Are you ISO/IEC 27001:2013 Certified? If so, what is within its scope?  
   
   AWS offers a list of their [compliance certificates](https://pages.awscloud.com/compliance-contact-us.html)  
   
10. Do you allow your customers to carry out regular penetration testing of production and/or test environments, while allowing the network to be in-scope?  
    
    You typically do not need to go through this process of requesting permission from your own company to carry out penetration testing, and if you do, there should be a lot fewer restrictions in place.  
    
    **[AWS](https://aws.amazon.com/security/penetration-testing)** allow customers to submit requests to penetration test to and from some AWS EC2 and RDS instance types that you own. All other AWS services are not permitted to be tested or tested from.  
    
    **[GCP](https://cloud.google.com/security/)** does not require penetration testers to contact them before beginning testing of their GCP hosted services, so long as they abide by the Acceptable Use Policy and the Terms of Service.  
    
    **[Heroku](https://devcenter.heroku.com/articles/pentest-instructions)** are happy for you to penetration test your own applications running on their PaaS. If you are performing automated security scans, you will need to give them two business days notice before you begin your testing.  
    
    **[Azure](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/29/pen-testing-from-azure-virtual-machines/)** allows penetration testing of your applications and services running in Azure, you just need to fill out their form. In order to use Azure to perform penetration testing on other targets, you do not need permission providing you are not DDoS testing.   
   
11. Do you have a bug bounty programme, if so, what are the details?  
    
    If the CSP is of a reasonable size and is not already running bug bounties, this is a sign that security may not be taken as seriously as you'd like.  
    
    **AWS** has a [bug bounty](https://hackerone.com/amazon-web-services) program.  
    
    **GCP** states that if a bug is found in the google infrastructure, the penetration tester is encouraged to submit it to their bug bounty program.  
    
    **Heroku** offers a [bug bounty](https://hackerone.com/heroku) program.  
    
    **Azure** offers a [bug bounty](https://hackerone.com/azure) program.

### [Cloud Service Provider vs In-house](https://binarymist.io/talk/saturn-2015-talk-does-your-cloud-solution-look-like-a-mushroom/) {#cloud-countermeasures-cloud-service-provider-vs-in-house}

It depends on the CSP, and other things about your organisation. Each CSP does things differently, it has strengths and weaknesses in different areas of the shared responsibility model, and has different specialities. It is governed by different people and jurisdictions (USA vs Sweden for example), and some are less security conscious than others. The largest factor in this question is your organisation. How security conscious and capable of implementing a secure cloud environment are your workers.

You can have a more secure cloud environment than any CSP if you decide to do so and have the necessary resources to build it. If you don't decide to and/or don't have the necessary resources, then most well known CSPs will probably be doing a better job than your organisation.

You need to consider what are your objectives for using the given CSPs services for. If you are creating and deploying applications, then your applications will be a weaker link in the security chain, this is a very common case and one that is often overlooked. To attempt to address application security, I wrote about this in the [Web Applications chapter](https://f1.holisticinfosecforwebdevelopers.com/chap06.html#web-applications) of Holistic Info-Sec for Web Developers, Fascicle 1.

Your attackers will attack your weakest area first. In most cases this is not your CSP, but directed at your organisation's people due to lack of knowledge, passion, engagement, or a combination of each. If you have a physical premises, this can also be an easy target. Usually application security follows closely after people security. This is why I include the Physical and People chapters in [Fascicle 0](https://f0.holisticinfosecforwebdevelopers.com) of [Holistic Info-Sec for Web Developers](https://www.holisticinfosecforwebdevelopers.com/), they are the most commonly overlooked.

#### Skills

The fate of your data and that of your customers is in your hands. If you have the resources to provide the necessary security then you are better off with an In-house cloud, if not, the opposite is true.  
If you go with an In-house cloud, you should have tighter control over the people creating and administering it. This is good if they have the necessary skills and experience, if not, then the opposite is true again.

#### EULA

You and any In-house cloud environment you establish is not subject to changing EULAs.

#### Giving up Secrets

If you are using an In-house cloud and find yourself in a place where you have made it possible for your customers secrets to be read, and you are being forced by the authorities to give up secrets, you will know about it and be able to react appropriately, invoke your incident response team(s) and procedures.

#### Location of Data

If you use an In-house cloud, you decide where services & data reside.

#### Vendor lock-in

You have to weigh the vendor benefits and possible cost savings versus how hard and/or costly it is to move away from them when you need to.

Many projects are locked into technology decisions, offerings, libraries, and services from the design stage, as they are unable to swap these at a later stage without incurring significant costs. If the offering that was chosen is proprietary, then it makes it all the more difficult to make a change if and when it makes sense to do so.

Some times it can cost more up front to go with an open (non-proprietary) offering because the proprietary offering has streamlined the development, deployment, and maintainability, that is the whole point of a proprietary offerings, right? Yet, sometimes the open offering can actually be the cheaper option, due to proprietary offerings incurring an additional learning or skills development cost for the teams and people involved.

Often technology choices are chosen because they are the "new shiny", as everyone else is using it, or there is a lot of buzz or hype around it.

**An analogy**: Do Software Developers write code that is untestable because it is cheaper to write? Many development shops do, I discussed test driven development (TDD) in the Process and Practises chapter of [Fascicle 0](https://f0.holisticinfosecforwebdevelopers.com/). I have [blogged](https://binarymist.io/tags/tdd/), [spoken and offered workshops](https://binarymist.io/talk/) on the topic of testability extensively. Writing untestable code is a short sighted approach. Code is read, revised, and extended many times more than it is written initially.

If you are putting all your cost savings in the initial code writing phase, and failing to consider all the times that modification will be attempted, then you are missing huge cost savings. Taking an initial hit up front to write testable code, specifically, code that has the properties of maintainability and extensibility defined by the [Liskov Substitution Principle](https://binarymist.io/tags/lsp/), will set you up so that the interface is not coupled to the implementation.

If you define your process correctly right up front, and make sure you can swap components (implementation) in and out at will, maintainability and extensibility are not just possible, but a pleasure.

**An example**: You do not make the decision up front that you are going to switch from running your JavaScript on one CSP's VM to another CSP's proprietary serverless technology in 5 years. In fact, you have no idea up front what you may switch to in 5 or 10 years time. If you avoid being locked into a proprietary technology (AWS Lambda for example), you will be able to move that code anywhere you want trivially in the future. This is a simple implementation swap. Just as professional Software Engineers do so with code to make it testable code, we should think seriously about doing the same with technology offerings. Just apply the same concept.

#### Possible Single Points of Failure

Following are some of countermeasures to single points of failure in the Cloud with the goal of creating redundancy on items that we can not do without:

* Load balanced instances and/or start-up scripts
* Docker [restart policy](https://docs.docker.com/engine/admin/start-containers-automatically/) and/or orchestration tools
* Multiple Availability Zones
* Multiple Regions
* Multiple Accounts
* Make it hard for an attacker to succeed
  * Long complex passwords, those that you can not remember and must store in a password database
  * Multi-factor authentication
  * Make sure you are collecting and storing login history in a safe place, this can be used to challenge attackers who have successfully logged in, as you will have their IP addresses and browser user-agent string
  * Third party authentication
  * Lots of other techniques

### Review Other Chapters {#cloud-countermeasures-review-other-chapters}

As I mentioned in the [Identify Risks](#cloud-identify-risks-review-other-chapters) Review Other Chapters subsection, please make sure you are familiar with the related concepts discussed.

### People

Most of these countermeasures are discussed in the People chapter of [Fascicle 0](https://f0.holisticinfosecforwebdevelopers.com/)

### Application Security

Full coverage in the [Web Applications chapter](https://f1.holisticinfosecforwebdevelopers.com/chap06.html#web-applications) of [Holistic Info-Sec for Web Developers, Fascicle 1](https://f1.holisticinfosecforwebdevelopers.com/).

### Network Security

Full coverage in the [Network chapter](https://f1.holisticinfosecforwebdevelopers.com/chap04.html#network) of [Holistic Info-Sec for Web Developers, Fascicle 1](https://f1.holisticinfosecforwebdevelopers.com/). There are also a collection of network-specific items that you should review in the Networking subsection of the [CIS AWS Foundations document](https://d0.awsstatic.com/whitepapers/compliance/AWS_CIS_Foundations_Benchmark.pdf).

### Violations of [Least Privilege](https://f1.holisticinfosecforwebdevelopers.com/chap06.html#web-applications-countermeasures-management-of-application-secrets-least-privilege) {#cloud-countermeasures-violations-of-least-privilege}

When you create IAM policies, grant only the permissions required to perform the task(s) necessary for given users. If the user needs additional permissions, then they can be added, rather than adding everything up front and potentially having to remove again at some stage. Adding as required, rather than removing as required, will cause much less friction technically and socially.

**For example, [in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege)**, you need to keep a close watch on which [permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_permissions.html) are assigned to [policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) that your groups and roles have applied, and subsequently, which groups and roles your users are in or part of.

This is the recommended sequence for granting least privilege in AWS, other CSPs will be similar:

1. First, work out which permissions a given user requires
2. Create or select an existing group or role
3. Attach policy to the group or role that has the permissions that your given user requires. You can select existing policies or create new ones
4. Add the given user to the group or role

Regularly review all of the IAM policies you are using, making sure only the required permissions (Services, Access Levels, and Resources) are available to the users and/or groups attached to the specific policies.

Enable [Multi Factor Authentication](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#enable-mfa-for-privileged-users) (MFA) on the root user, and all IAM users with console access, especially privileged users at a minimum. AWS provides the ability to mandate that users use MFA, you can do this by creating a new managed policy based on the AWS guidance to [Enable Your Users to Configure Their Own Credentials and MFA Settings](https://docs.aws.amazon.com/IAM/latest/UserGuide/tutorial_users-self-manage-mfa-and-creds.html). Attach the new policy to a group that you have created and add users that must use MFA to that group.  
This process was pointed out to me by Scott Piper during our [Cloud Security interview](https://binarymist.io/publication/ser-podcast-cloud-security/) by way of his [blog post](https://duo.com/blog/potential-gaps-in-suggested-amazon-web-services-security-policies-for-mfa) and generous Github pull request.

The [Access Advisor](https://aws.amazon.com/blogs/security/remove-unnecessary-permissions-in-your-iam-policies-by-using-service-last-accessed-data/) tab, is visible on the IAM console details page for Users, Groups, Roles, or Policies after you select a list item. This provides information about which services are accessible for any of your users, groups, or roles. This can also be helpful for auditing permissions that should not be available to any of your users who are part of the group, role or policy you selected.

The [IAM Policy Simulator](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html) is accessible from the IAM console. This is good for granular reporting on the permissions of your specific Users, Groups and Roles, filtered by service and actions.

[AWS Trusted Advisor](https://aws.amazon.com/premiumsupport/trustedadvisor/) should be run periodically to check for security issues. It is accessible from the [Console](https://console.aws.amazon.com/trustedadvisor/), CLI and API. Trusted Advisor has a collection of core checks and recommendations which are free to use. These include security groups, specific ports unrestricted, IAM use, MFA on root user, EBS and RDS public snapshots.

* **Running services as root**: Make sure that Docker containers are not running under the root account. There are full details in my [Docker Security book](https://binarymist.io/publication/docker-security/)
* **Configuration Settings Changed Ad Hoc**: One option is to have solid change control in place. [AWS Config](https://aws.amazon.com/config/) can assist with this. [AWS Config](https://docs.aws.amazon.com/config/latest/developerguide/) continuously monitors and records how the AWS resources were configured and how they have changed, including how they are related to each other. This enables you to assess, audit, and evaluate the configurations of your AWS resources, and have notifications sent to you when AWS Config detects a violation, including created, modified or deleted rules changes.  
   
   AWS Config records IAM policies assigned to users, groups, or roles, and EC2 security groups, including port rules. Changes to your configuration settings can trigger Amazon Simple Notification Service (SNS) notifications, which you can have sent to your personnel tasked with controlling changes to your configurations.  
   
   Your custom rules can be codified and therefore source controlled. AWS calls this Compliance as Code. I discussed AWS CloudTrail briefly in item 1 of the [CSP Evaluation](#cloud-countermeasures-csp-evaluation) countermeasures subsection. AWS Config is integrated with CloudTrail, which captures all API calls from AWS Config console or API, SDKs, CLI tools, and other AWS services. The information collected by CloudTrail provides insight on what request was made, from which IP address, by who, and when  
* **Machine Instance Access To Open**: Reduce your attack surface by disabling access to your machine instances from *any* source IP address

There are also a collection of IAM specific items that you should review in the Identity and Access Management subsection of the [CIS AWS Foundations document](https://d0.awsstatic.com/whitepapers/compliance/AWS_CIS_Foundations_Benchmark.pdf).

#### Machine Instance Single User Root

As part of the VPS and container builds, there should be [specific users created](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/managing-users.html) for specific jobs, every user within your organisation that needs VPS access should have their own user account on every VPS, including [SSH access](#cloud-countermeasures-storage-of-secrets-private-key-abuse-ssh) if required (ideally this should be automated). With Docker, I discussed how this is done in the `Dockerfile` in my [Docker Security book](https://binarymist.io/publication/docker-security/) and [blog post](https://binarymist.io/blog/2018/03/31/docker-security/#the-default-user-is-root).

Drive a [least privilege policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) around this, configuring a strong [password policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#configure-strong-password-policy) for your users, and implement [multi-factor authentication](https://aws.amazon.com/iam/details/mfa/), which will help with poor password selection. I discuss this in more depth in the [Storage of Secrets](#cloud-countermeasures-storage-of-secrets) subsection.

#### CSP Account Single User Root {#cloud-countermeasures-violations-of-least-privilege-csp-account-single-user-root}

As I discuss in the [Credentials and Other Secrets](#cloud-countermeasures-storage-of-secrets-credentials-and-other-secrets) Countermeasures subsection of this chapter, create multiple accounts with least privileges required for each; the root user should hardly ever be used. Create groups and attach restricted policies to them, then add the specific users to them.

Also as discussed in the [Credentials and Other Secrets](#cloud-countermeasures-storage-of-secrets-credentials-and-other-secrets-entered-by-people-manually) countermeasures subsection, there should be almost no reason to generate key(s) for the AWS Command Line Tools for the AWS account root user. But if you do, consider setting up notifications for when they are used. As usual, AWS has plenty of [documentation](https://aws.amazon.com/blogs/security/how-to-receive-notifications-when-your-aws-accounts-root-access-keys-are-used/)
on the topic.

Another idea is to set-up monitoring and notifications on activity of your AWS account root user. AWS [documentation](https://aws.amazon.com/blogs/mt/monitor-and-notify-on-aws-account-root-user-activity/) explains how to do this.

There are also a collection of monitoring specific items that you should review in the Monitoring subsection of the [CIS AWS Foundations document](https://d0.awsstatic.com/whitepapers/compliance/AWS_CIS_Foundations_Benchmark.pdf).

Another great idea is to generate an AWS key [Canarytoken](https://canarytokens.org/) from canarytokens.org, and put it somewhere more obvious than your real AWS key(s). When someone uses it, you will be automatically notified. I discussed these with Haroon Meer on the Software Engineering Radio [Network Security](https://binarymist.io/publication/ser-podcast-network-security/) podcast. [Jay](https://twitter.com/HeyJayza) also wrote a [blog post](http://blog.thinkst.com/2017/09/canarytokens-new-member-aws-api-key.html) on the thinkst blog on how you can set this up, and what the inner workings look like.

Also consider rotating your IAM access keys for your CSP services. AWS EC2, for example, provides [auto-expire, auto-renew](https://aws.amazon.com/blogs/security/how-to-rotate-access-keys-for-iam-users/) access keys when using roles.

### [Storage of Secrets](https://www.programmableweb.com/news/why-exposed-api-keys-and-sensitive-data-are-growing-cause-concern/analysis/2015/01/05) {#cloud-countermeasures-storage-of-secrets}

In this section I discuss some techniques to handle our sensitive information in a safe manner.

If you have "secrets" in source control or wikis, they are probably not secret. Remove them, and change the secret (password, key, what ever it is). [Github provides guidance](https://help.github.com/articles/removing-sensitive-data-from-a-repository/) on removing sensitive data from a repository.

Also consider using [git-crypt](https://github.com/AGWA/git-crypt).

Use different access keys for each service and application requiring them.

Use [temporary security credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp.html).

Rotate access keys.

#### Private Key Abuse

Following are techniques to better handle private keys.

##### SSH {#cloud-countermeasures-storage-of-secrets-private-key-abuse-ssh}

There are many ways to harden SSH as we discussed in the [SSH](https://f1.holisticinfosecforwebdevelopers.com/chap03.html#vps-countermeasures-disable-remove-services-harden-what-is-left-ssh) subsection of the VPS chapter of Holistic Info-Sec for Web Developers, Fascicle 1. Usually the issue will be specific to lack of knowledge, desire and a dysfunctional [culture](https://blog.binarymist.net/2014/04/26/culture-in-the-work-place/) in the work place. You will need to address the people issues before looking at basic SSH hardening techniques.

Ideally, SSH access should be reduced to a selected few. Most of the work we do now by SSHing should be automated. If you review the commands in history on most VPSs, the majority of the commands are either deployment or monitoring which should all be [automated](https://github.com/binarymist/aws-docker-host).

When you create an AWS EC2 instance you can create a key pair [using EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair) or you can [provide your own](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#how-to-generate-your-own-key-and-import-it-to-aws). Either way, to be able to log-in to your instance, you need to have provided EC2 with the public key of your key pair and specified it by name. 

Every user should have their [own key-pair](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html), the private part should always be private, kept in the users local `~/.ssh/` directory (not the server) with permissions `600` or more restrictive, and not shared on your developer wiki, or anywhere else for that matter. The public part can be put on every server that the user needs access to. There is no excuse for users not to have their own key pair, you can have up to five thousand key pairs per AWS region. AWS has [clear directions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) on how to create additional users and provide SSH access with their own key pairs.

For generic confirmation of the host's SSH key fingerprint when prompted before establishing the SSH connection, follow the procedure I laid out for [Establishing your SSH Servers Key Fingerprint](https://f1.holisticinfosecforwebdevelopers.com/chap03.html#vps-countermeasures-disable-remove-services-harden-what-is-left-ssh-establishing-your-ssh-servers-key-fingerprint) in the VPS chapter of Holistic Info-Sec for Web Developers, Fascicle 1, and make it organisational policy. We should never blindly accept key fingerprints. The key fingerprints should be stored in a relatively secure place, so that only trusted parties can modify them. I would like to see, as part of the server creation process, the entity (probably the wiki) that specifies the key fingerprints is automatically updated by something on the VPS that keeps watch of the key fingerprints. Something like [Monit](https://f1.holisticinfosecforwebdevelopers.com/chap03.html#vps-countermeasures-lack-of-visibility-proactive-monitoring-getting-started-with-monit), would be capable of the monitoring and executing a script to do this.

To SSH to an EC2 instance, you will have to view the console output of the keys being generated. You can see this **only for the first run** of the instance when it is being created, this can be seen by first fetching https://console.aws.amazon.com, then:

1. Click the "EC2" link
2. Click "Instances" in the left column
3. Click the instance name you want
4. Click the select button "Actions" and choose "Get System Log" (a.k.a. "Console Output")
5. In the console output, you should see the keys being generated. Record them

Then, to SSH to your EC2 instance, the command to use can be seen by fetching  
https://console.aws.amazon.com, then:

1. EC2
2. Instances
3. Select your instance
4. Click the Connect button for details

##### TLS {#cloud-countermeasures-storage-of-secrets-private-key-abuse-tls}

So, how do we stop baking secrets into our Docker images?

The easiest way is to avoid adding secrets to the process of building your images. You can add them at run time in several ways. If you have a look at Namespaces in my [Docker Security book](https://binarymist.io/publication/docker-security/), also discussed in my [Docker Security blog post](https://binarymist.io/blog/2018/03/31/docker-security/#namespaces-risks), we used volumes. This allows us to keep the secrets entirely out of the image and only include in the container as mounted host directories, rather than adding those secrets to the `Dockerfile`:

{id="docker-run-mitigating-private-key-abuse", title="Mitigate private key abuse via terminal", linenos=off}
    docker run -d -p 443:443 -v /host-path/star.mydomain.com.cert:/etc/nginx/certs/my.cert -v /host-path/star.mydomain.com.key:/etc/nginx/certs/my.key -e "mySecret=dirty little secret" nginx

An even easier technique is to just implement adding of secrets in the `docker-compose.yml` file, thus saving time when you run the container:

{id="docker-compose-mitigating-private-key-abuse", title="Mitigate private key abuse using docker-compose.yml", linenos=off}
    nginx:
        build: .
        ports:
            - "443:443"
        volumes:
            - /host-path/star.mydomain.com.key:/etc/nginx/ssl/nginx.key
            - /host-path/star.mydomain.com.cert:/etc/nginx/ssl/nginx.crt
            - /host-path/nginx.conf:/etc/nginx/nginx.conf
        env_file:
            - /host-path/secrets.env

Using the `env_file` we can hide our environment variables in the `.env` file.  
Our `Dockerfile` would now look like the following, even our config is volume mounted and will no longer reside in our image:

{id="dockerfile-no-private-key-abuse", title="Mitigate private key abuse using Dockerfile", linenos=off}
    FROM nginx

    # ...
    # ...

#### Credentials and Other Secrets {#cloud-countermeasures-storage-of-secrets-credentials-and-other-secrets}

Create multiple users with the least privileges required for each to do their job, discussed below.  
Create and enforce password policies, discussed below.

Oddly enough, in the AWS account root user story I mentioned in the [Risks](#cloud-identify-risks-storage-of-secrets-credentials-and-other-secrets) subsection, I had created a report detailing this as one of the most critical issues that needed addressing, several weeks before all but one person lost access.

If your business is in the Cloud, the account root user is one of your most valuable assets, do not share it with anyone, and only use it when it's essential.

##### Entered by People (manually) {#cloud-countermeasures-storage-of-secrets-credentials-and-other-secrets-entered-by-people-manually}

**Protecting against outsiders**

The most effective alternative to storing user-names and passwords in an insecure manner is to use a group or team password manager. There are quite a few offerings available with all sorts of different attributes. The following are some of the points you will need to consider as part of your selection process:

* Cost in terms of money
* Cost in terms of set-up and maintenance
* Closed or open source. If you care about security, which you must if you are considering a team password manager, it is important to see how secrets are handled. I need to be able to see how the code is written, and which [Key Derivation Functions](https://f1.holisticinfosecforwebdevelopers.com/chap06.html#web-applications-countermeasures-data-store-compromise-which-kdf-to-use) (KDFs) and [cyphers](https://f1.holisticinfosecforwebdevelopers.com/chap06.html#web-applications-identify-risks-cryptography-on-the-client) are used. If it is of high quality, we can have more confidence that our precious sensitive pieces of information are, in fact, going to be private
* Do you need a web client?
* Do you need a mobile client (iOS, Android)?
* What platforms does it need to support?
* Does it need to be able to manage secrets of multiple customers?
* Auditing of user actions? Who is accessing and changing what?
* Ability to be able to lock out users, when they leave the organisation, for example?
* Multi-factor authentication
* Options: Does it have all the features you need?
* Who is behind the offering? Are they well known for creating solid, reliable, secure solutions?

The following are my personal top three, with the first being my preference, based on research I performed for one of my customers recently. All the points above were considered for a collection of about ten team password managers that I reviewed:

1. [Pleasant Password Server](http://pleasantsolutions.com/PasswordServer/) (KeePass backed)
2. [Password Manager Pro](https://www.manageengine.com/products/passwordmanagerpro/msp/features.html)
3. [LastPass](https://www.lastpass.com/teams)

**Protecting against insiders**

The above alone is not going to stop an account take over if you are sharing the likes of the AWS account root user email and password, even if it is in a group password manager. As AWS has [already stated](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html), only use the root user for what is absolutely essential (remember: least privilege). This is usually just to create an Administrators group to which you attach the `AdministratorAccess` managed policy, then add any new IAM users to that group who require administrative access.

Once you have created IAM users within an Administrators group as mentioned above, these users should set up groups to which you attach further restricted managed policies such as a group for `PowerUserAccess`, a group for `ReadOnlyAccess`, a group for `IAMFullAccess`, progressively becoming more restrictive. Use the most restrictive group possible in order to achieve specific tasks, simply assigning users to the groups you have created.

Be sure to use multi-factor authentication.

&nbsp;

Your AWS users are not assigned access keys to use for programmatic access by default, do not create these unless you actually need them, and again consider least privilege. There should be almost no reason to create an [access key](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#lock-away-credentials) for the root user.

Configure [strong password policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#configure-strong-password-policy) for your users, make sure they are using personal password managers and know how to generate long complex passwords.

##### Entered by Software (automatically) {#cloud-countermeasures-storage-of-secrets-credentials-and-other-secrets-entered-by-software}

There are many places in software that require access to secrets, to communicate with services, APIs, datastores. Configuration and infrastructure management systems have a problem storing and accessing these secrets in a secure manner.

**HashiCorp [Vault](https://www.vaultproject.io/)**. The most fully featured of these tools, has the following attributes/features:

* [Open Source](https://github.com/hashicorp/vault) written in Go-Lang
* Deployable to any environment, including development machines
* Arbitrary key/value secrets can be stored of any type of data
* Supports cryptographic operations of the secrets
* Supports dynamic secrets, generating credentials on-demand for fine-grained security controls
* Auditing: Vault forces a mandatory lease contract with clients, which allows the rolling of keys, automatic revocation, along with multiple revocation mechanisms providing operators a break-glass for security incidents
* Non-repudiation
* Secrets protected in transit and at rest
* Not coupled to any specific configuration or infrastructure management system
* Can read secrets from configuration, infrastructure management systems and applications via its API
* Applications can query Vault for secrets to connect to services such as datastores, thus removing the need for these secrets to reside in configuration files (See the [Risks that Solution Causes](#cloud-risks-that-solution-causes-storage-of-secrets-credentials-and-other-secrets-entered-by-software) for the caveat)
* Requires multiple keys generally distributed to multiple individuals to read its encrypted secrets
* Check the [Secret Backends](https://www.vaultproject.io/docs/secrets/index.html) for integrations

**[Docker secrets](https://docs.docker.com/engine/swarm/secrets/)**

* Manages any sensitive data (including generic string or binary content up to 500 kb in size) that a [container needs at runtime](#cloud-countermeasures-storage-of-secrets-private-key-abuse-tls), but you do not want to [store in the image](#cloud-identify-risks-storage-of-secrets-private-key-abuse-tls), source control, or the host systems file-system as we did in the TLS section above
* Only available to Docker containers managed by Swarm (services). Swarm manages the secrets
* Secrets are stored in the Raft log, which is encrypted if using Docker 1.13 and higher
* Any given secret is only accessibly to services (Swarm managed container) that have been granted explicit access to the secret
* Secrets are decrypted and mounted into the container in an in-memory filesystem which defaults to `/run/secrets/<secret_name>` in Linux, `C:\ProgramData\Docker\secrets` in Windows

**[Ansible Vault](https://docs.ansible.com/ansible/latest/playbooks_vault.html)**

Ansible is an [Open Source](https://github.com/ansible/ansible/blob/devel/docs/docsite/rst/playbooks_vault.rst) configuration management tool, and has a simple secrets management feature.

* Ansible tasks and handlers can be encrypted
* Arbitrary files, including binary data can be encrypted
* From version 2.3 can encrypt single values inside YAML files
* Suggested workflow is to check the encrypted files into source control for auditing purposes

{#cloud-countermeasures-storage-of-secrets-credentials-and-other-secrets-entered-by-software-kms}
AWS **[Key Management Service](https://aws.amazon.com/kms/)** (KMS) 

* Encrypt up to 4 KB of arbitrary data (passwords, keys)
* Supports cryptographic operations of the secrets: encrypt and decrypt
* Uses Hardware Security Modules (HSM)
* Integrated with AWS CloudTrail to provide auditing of all key usage
* AWS managed service
* Create, import and rotate keys
* Usage via AWS Management Console, SDK and CLI

AWS offers **[Parameter Store](https://aws.amazon.com/ec2/systems-manager/parameter-store/)**

* Centralised store on AWS to manage configuration data, plain text, or encrypted secrets via AWS KMS
* All calls to the parameter store are recorded with AWS CloudTrail, supports access controls.

Also see the [Additional Resources](#additional-resources-cloud-countermeasures-storage-of-secrets-credentials-and-other-secrets-entered-by-software) section for other similar tools and resources.

### [Serverless](https://github.com/anaibol/awesome-serverless) {#cloud-countermeasures-serverless}

Serverless is another form of separation of concerns, or decoupling. Serverless is yet another attempt to coerce Software Developers into abiding by the Object Oriented (OO) [SOLID](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)) principles that the vast majority of Developers never quite understood. Serverless forces a microservice way of thinking.

Serverless mandates the reactive, event driven approach that insists that our code features stand alone without the tight coupling of many services that we often use. Serverless forces us to split our databases from our business logic. Serverless goes a long way to forcing us to write [testable code](https://binarymist.io/blog/2012/12/01/moving-to-tdd/), and as I have said so many times, testable code is good code, code that is easy to maintain and extend, thus abiding by the [Open/closed principle](https://en.wikipedia.org/wiki/Open/closed_principle).

Serverless raises the bar in terms of abstraction, but at the same time allows you to focus on the code, which as a Developer, is preferred.

With AWS Lambda, you only pay when your code executes, as opposed to paying for machine instances, or as with Heroku, the entire time your application is running on their compute stack, even if the application code is not executing. AWS Lambda and similar offerings allow granular costing, passing on cost savings due to many customers all using the same hardware.

AWS Lambda and similar offerings allow us to avoid thinking about machine/OS and language environment patching, compute resource capacity, or scaling. You are now trusting your CSP to do these things. There are [no maintenance windows](https://aws.amazon.com/lambda/faqs/#scalability) or scheduled downtimes. Lambda is also currently free for up to one million requests per month, and does not expire after twelve months. This in itself is quite compelling.

#### Third Party Services

When you consume third party services (APIs, functions, etc), you are, in essence, outsourcing whatever you send or receive from them. How is that service handling what you pass to it or receive from it? How do you know that the service is who or what you think it is, are you checking its TLS certificate? Is the data in transit encrypted? Just as I discuss below under [Functions](#cloud-countermeasures-serverless-functions), you are sending and receiving from a potentially untrusted service. This all increases the attack surface.

#### Perimeterless

This isn't much different from the [Fortress Mentality](https://f1.holisticinfosecforwebdevelopers.com/chap04.html#network-countermeasures-fortress-mentality) subsection discussed in the Network chapter of Holistic Info-Sec for Web Developers, Fascicle 1.

#### Functions {#cloud-countermeasures-serverless-functions}

With AWS Lambda, in addition to getting your application security right, you also need to fully understand the [Permissions Model](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html), apply it, and protect your API gateway with a key.

1. **[Application Security](https://f1.holisticinfosecforwebdevelopers.com/chap06.html#web-applications)**: No matter where your code is executing, you must have a good grasp on application security, no amount of sand-boxing, Dockerising, application firewalling, or anything else will protect you from poorly written applications if they are running.  
   
   With regards to help with consuming all the free and open source offerings, review the [Consuming Free and Open Source](https://f1.holisticinfosecforwebdevelopers.com/chap06.html#web-applications-countermeasures-consuming-free-and-open-source) subsection of Holistic Info-Sec for Web Developers, Fascicle 1. Snyk has a [Serverless](https://snyk.io/serverless) offering also. Every function you add increases attack surface and all the risks that come with integrating with other services. Keep your inventory control tight with your functions and consumed dependencies. Know which packages you are consuming and which known defects they have, know how many and which functions are in production.  
   
   Test removing permissions and see if everything still works. If it does, your permissions were too open, reduce them  
   
2. **IAM**: In regards to AWS Lambda, although it should be similar with other large CSPs, make sure you apply only privileges required, this way you will not be [violating the principle](#cloud-countermeasures-violations-of-least-privilege) of Least Privilege
    * AWS Lambda function [access to other AWS resources](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role):
      * Create an [IAM execution role](https://docs.aws.amazon.com/lambda/latest/dg/with-s3-example-create-iam-role.html) of type `AWS Service Roles`, grant the AWS Lambda service permissions to assume your role by choosing `AWS Lambda`
      * Attach the policy to the role as discussed in step 3 under [Violations of Least Privilege](#cloud-countermeasures-violations-of-least-privilege). Make sure to tightly constrain the `Resource`'s of the chosen policy. Use `AWSLambdaBasicExecuteRole` if your Lambda function only needs to write logs to CloudWatch, `AWSLambdaKinesisExecutionRoleAWS` if your Lambda function also needs to access Kinesis Streams actions, `AWSLambdaDynamoDBExecutionRole` if your Lambda function needs to access DynamoDB streams actions along with CloudWatch, and `AWSLambdaVPCAccessExecutionRole` if your Lambda function needs to access AWS EC2 actions along with CloudWatch
      * When you create your Lambda function, apply your Amazon Resource Name (ARN) as the value to the `role`
      * Each function accessing a data store should use a unique user and credentials with only the permissions necessary to do what that specific function needs to do, this honours least privilege, and also provides some level of auditing
    * Other AWS resources [access to AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#intro-permission-model-access-policy):
      * Permissions are added via function policies. Make sure these are granular and specific

3. [Use](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-setup-api-key-with-console.html) an **[API key](https://serverless.com/framework/docs/providers/aws/events/apigateway/#setting-api-keys-for-your-rest-api)**

#### DoS of Lambda Functions

AWS Lambda allows you to [throttle](https://docs.aws.amazon.com/lambda/latest/dg/concurrent-executions.html#concurrent-execution-safety-limit) the concurrent execution count. AWS Lambda functions being invoked asynchronously can handle bursts for approximately 15-30 minutes. Essentially, if the default is not right for you, then you need to define the policy, that is, set the reasonable limits. Make sure you do this!

Set [Cloudwatch alarms](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-functions.html) on [duration and invocations](https://docs.aws.amazon.com/lambda/latest/dg/monitoring-functions-metrics.html). These can even be sent to Slack.

Drive the creation of your functions the same way you would drive any other production quality code... with unit tests ([TDD](https://binarymist.io/blog/2012/12/01/moving-to-tdd/)). Follow that with integration testing of the function in a production-like test environment with all the other components in place. [You can](https://serverless.zone/unit-and-integration-testing-for-lambda-fc9510963003) mock, stub, and pass spies with the AWS:

* JavaScript SDK, using tools such as:
  * [aws-sdk-mock](https://www.npmjs.com/package/aws-sdk-mock)
  * [mock-aws](https://www.npmjs.com/package/mock-aws)
* Python SDK (boto3), using tools such as:
  * [placebo](https://github.com/garnaat/placebo)
  * [moto](https://github.com/spulec/moto)

Set up billing alerts.

Be careful not to create direct or indirect recursive function calls.

Use an application firewall as I discussed in the Web Application chapter under the [Insufficient Attack Protection](https://f1.holisticinfosecforwebdevelopers.com/chap06.html#web-applications-countermeasures-insufficient-attack-protection) subsection of the Web Applications chapter of Holistic Info-Sec for Web Developers, Fascicle 1. This may provide some protection if your rules are adequate.

Consider how important it is to scale compute to service requests. If it is more important to you to have a monthly fixed price, consider fixed price machine instances, then you know the costs upfront.

#### [Centralised logging of AWS Lambda](https://hackernoon.com/centralised-logging-for-aws-lambda-b765b7ca9152) Functions

You should also be sending your logs to an aggregator and not in your execution time. Whatever your function writes to stdout is captured by Lambda and sent to Cloudwatch Logs asynchronously. This means that consumers of the function will not take a latency hit and you will not take a cost hit. Cloudwatch Logs can then be streamed to AWS Elasticsearch which may or may not be [stable enough](https://read.acloud.guru/things-you-should-know-before-using-awss-elasticsearch-service-7cd70c9afb4f) for you. Other than that, there are not too many good options on AWS yet, beside sending to Lambda, which, of course, could also end up costing you compute and being another DoS vector. 

#### Frameworks

The following are intended to make deploying your functions to the Cloud easier:

**[Serverless](https://serverless.com/framework/)**, along with a large collection of [awesome-serverless](https://github.com/JustServerless/awesome-serverless) resources on github.

The Serverless framework currently has the following provider APIs:

* AWS
* Microsoft Azure
* IBM OpenWhisk
* GCP
* Kuberless

**[Claudia.JS](https://claudiajs.com/)**: Specific to AWS and only covers Node.js. Authored by Gojko Adzic, which, if you have been in the industry as a Software Engineer for long, is enough to sell it.

**[Zappa](https://www.zappa.io/)**: Specific to AWS and only covers Python.


### Infrastructure and Configuration Management

Storing infrastructure and configuration as code is an effective measure for many mundane tasks that people may still be performing that are prone to human error. This means we can sequence specific processes, debug them, source control them, and achieve repeatable processes that are far less likely to have security defects in them. This is provided that the automation is written by people who are sufficiently skilled and knowledgeable of the security topics involved. This also has the positive side effect of speeding processes up.

When an artefact is deployed, how do you know that it will perform the same in production that it did in development? This is what a staging environment is for. A staging environment will never be exactly the same as production unless your infrastructure is codified. This is another place where containers can help. With using containers, you can test the new software anywhere and it will run the same, providing its environment is identical to what you ship.

The container goes from the developer's machine once tested, to the staging environment, then to production. The staging environment in this case is less important than it used to be, and is just responsible for testing your infrastructure. All of this should of been built from source controlled infrastructure as code, so it is guaranteed to be repeatable.

1. Pick off repetitious, boring, prone-to-human-error and easily automatable tasks that your team(s) have been doing. Script and source control them
    * **Configuration management**: One of the grassroot types of tooling options required here is a configuration management tool. I have found Ansible to be excellent. If you use Docker containers, most of the configuration management is already taken care of in the `Dockerfile` as I detailed in my [Docker Security book](https://binarymist.io/publication/docker-security/) and [blog post](https://binarymist.io/blog/2018/03/31/docker-security/#the-default-user-is-root). The [`docker-compose.yml`](https://binarymist.io/blog/2018/03/31/docker-security/#nodegoat-docker-compose.yml) file, orchestration platforms and tooling take us to "infrastructure as code"
    * **Infrastructure management**: Terraform is one of the tools that can act as a simple version control for cloud infrastructure. One of my co-hosts (lead host Robert Blumen) on Software Engineering Radio ran an excellent [podcast on Terraform](http://www.se-radio.net/2017/04/se-radio-episode-289-james-turnbull-on-declarative-programming-with-terraform/)
    * Ultimately we want to achieve the maturity where we can have an entire front-end and back-end (if Web) deployment automated. There are many things this depends on. A deployment must come from a specific source branch that is production-ready. A production-ready branch is labelled as such because another branch leading into it has passed all the quality checks mentioned in the [Agile Development and Practises subsection](https://f0.holisticinfosecforwebdevelopers.com/chap06.html#process-and-practises-agile-development-and-practices) in the Process and Practises chapter of [Fascicle 0](https://f0.holisticinfosecforwebdevelopers.com/), as well as [continuous integration](https://binarymist.io/blog/2014/02/22/automating-specification-by-example-for-.net-web-applications/)
2. Once a few of the above tasks are done, you can start stringing them together in pipelines
3. Schedule execution of any/all of the above

### AWS

As mentioned in the [risks subsection](#cloud-identify-risks-aws) for AWS, the [CIS AWS Foundations document](https://d0.awsstatic.com/whitepapers/compliance/AWS_CIS_Foundations_Benchmark.pdf) is well worth following along with.

#### Password-less sudo

Add a password to the default user.

We have covered the people aspects, along with exploitation techniques of Weak Password Strategies, in the People chapter of [Fascicle 0](https://f0.holisticinfosecforwebdevelopers.com/)

We have covered the technical aspects of password strategies in the [Review Password Strategies](https://f1.holisticinfosecforwebdevelopers.com/chap03.html#vps-countermeasures-disable-remove-services-harden-what-is-left-review-password-strategies) subsection of the VPS chapter in [Holistic Info-Sec for Web Developers, Fascicle 1](https://f1.holisticinfosecforwebdevelopers.com/)

#### Additional Tooling {#cloud-countermeasures-aws-additional-tooling} 

* [Security Monkey](https://github.com/Netflix/security_monkey/): Monitors AWS and GCP accounts for policy changes, and alerts on insecure configurations, conceptually similar to AWS Config, as discussed in the [Violations of Least Privilege](#cloud-countermeasures-violations-of-least-privilege) countermeasures subsection. Security Monkey is free and open source. Although not strictly security related, the [Simian Army](https://github.com/Netflix/SimianArmy/wiki) tools from Netflix are also well worth mentioning if you are serious about doing things the right way in AWS. They include:
  * [Chaos Monkey](https://github.com/Netflix/SimianArmy/wiki/Chaos-Monkey)
  * [Janitor Monkey](https://github.com/Netflix/SimianArmy/wiki/Janitor-Home)
  * [Conformity Monkey](https://github.com/Netflix/SimianArmy/wiki/Conformity-Home)
* [CloudSploit](https://cloudsploit.com/): Aims to solve the problem of misconfigured AWS accounts with background scanning through hundreds of resources, settings, and activity logs looking for potential issues. Their blog also has some good resources on it. Scan reports include in-depth remediation steps. Has a free and paid hosted tiers. Auto scanning scheduling for the paid plans. Is open source on [github](https://github.com/cloudsploit/scans)
* [Amazon Inspector](https://console.aws.amazon.com/inspector/): At this time only targets EC2 instances. Inspector agent needs to be installed on all target EC2 instances
* [CloudMapper](https://github.com/duo-labs/cloudmapper) by Scott Piper for visualising your AWS environments. Along with his blog post at [duo.com](https://duo.com/blog/introducing-cloudmapper-an-aws-visualization-tool)
* [Awesome AWS](https://github.com/donnemartin/awesome-aws) has many useful resources
