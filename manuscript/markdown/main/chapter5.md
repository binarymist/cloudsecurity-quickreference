# 5. Costs and Trade-offs {#costs-and-trade-offs}

## Shared Responsibility Model

My hope is that I have provided you enough visibility of your responsibility throughout this chapter and that you will have a good understanding of what you need to do to keep your environments as secure as is required for your particular business model.

## CSP Evaluation

There is quite a bit to be done here, but it all depends on the answers you receive from your CSP. I have provided scenarios and given many points to consider in the [Countermeasures](#cloud-countermeasures-csp-evaluation) section. You can evaluate these now if you have not already.

## Cloud Service Provider vs In-house

There is not a lot to add to the [Risks that Solution Causes](#cloud-risks-that-the-solution-causes-cloud-service-provider-vs-in-house) and [Countermeasures](#cloud-countermeasures-cloud-service-provider-vs-in-house) subsections.

## People

Refer to the [Costs and Trade-offs subsection](https://f0.holisticinfosecforwebdevelopers.com/chap08.html#leanpub-auto-ssm-costs-and-trade-offs-1) of the People chapter of Holistic Info-Sec for Web Developers,[Fascicle 0](https://f0.holisticinfosecforwebdevelopers.com).

## Application Security

Refer to the [Costs and Trade-offs subsection](https://f1.holisticinfosecforwebdevelopers.com/chap06.html#web-applications-costs-and-trade-offs) of the Web Applications chapter of Holistic Info-Sec for Web Developers, [Fascicle 1](https://f1.holisticinfosecforwebdevelopers.com).

## Network Security

Refer to the [Costs and Trade-offs subsection](https://f1.holisticinfosecforwebdevelopers.com/chap04.html#network-costs-and-trade-offs) of the Network chapter of Holistic Info-Sec for Web Developers, [Fascicle 1](https://f1.holisticinfosecforwebdevelopers.com).


## Violations of Least Privilege

It is worth investing the effort to make sure only the required user permissions are granted. As discussed, there are tools you can use to help speed this process up and make it more accurate.

* **Running services as root**: Always start with the minimum permissions possible and add if necessary, it is far easier to add than to remove
* **Configuration Settings Changed Ad Hoc**: Remember detection works where prevention fails. Where your change control fails, because it is decided not to use it, you need something to detect changes and notify someone who cares. For this, there are also other options specifically designed to perform this function. For a collection of such tools, review the [Tooling](#cloud-countermeasures-aws-additional-tooling) sections.  
   
   You need to have these tools set up so that they are [continually auditing](https://blog.cloudsploit.com/the-importance-of-continual-auditing-in-the-cloud-8d22e0554639) your infrastructure and notifying the person(s) responsible for issues resolution, rather than having people continually manually reviewing settings, permissions, and so forth
* **Machine Instance Access To Open**: Set-up a bastion host and lock the source IP address down to the public facing IP address of your bastion host required to access your machine instances. I discussed locking the source IP address down in the [Hardening SSH subsection](https://f1.holisticinfosecforwebdevelopers.com/chap03.html#vps-countermeasures-disable-remove-services-harden-what-is-left-ssh) of the VPS chapter of Holistic Info-Sec for Web Developers, Fascicle 1.  
   
   Your bastion host will be hardened as discussed throughout the VPS chapter. All authorised workers can VPN to the bastion host and SSH from there, or just SSH tunnel from wherever they are through the bastion host via port forwarding to any given machine instances.  
   
   If you have Windows boxes you need to reach, you can tunnel RDP through your SSH tunnel, see my[blog post about this](https://binarymist.io/blog/2010/08/26/installation-of-ssh-on-64bit-windows-7-to-tunnel-rdp/).  
   
   Rather than tunnelling, another option SSH gives us (using the `-A` option) is to hop from the bastion host to your machine instances by forwarding the private key. This does include the risk that someone could gain access to your forwarded SSH agent connection, thus being able to use your private key while you have an SSH connection established. `ssh-add -c` can provide some protection with this.  
   
   If you do decide to use the `-A` option, then you are essentially considering your bastion host as a trusted machine. I commented on the `-A` option in the [Tunnelling SSH](https://f1.holisticinfosecforwebdevelopers.com/chap03.html#vps-countermeasures-disable-remove-services-harden-what-is-left-ssh-tunneling-ssh) subsection of the VPS chapter of Holistic Info-Sec for Web Developers, Fascicle 1. There is plenty of good [documentation](https://cloudacademy.com/blog/aws-bastion-host-nat-instances-vpc-peering-security/) on setting up the bastion host in AWS. AWS provides some [Best Practices](https://docs.aws.amazon.com/quickstart/latest/linux-bastion/architecture.html#best-practices) for security on bastion hosts, and also [discusses](https://aws.amazon.com/blogs/security/how-to-record-ssh-sessions-established-through-a-bastion-host/) recording the SSH sessions that your users establish through a bastion host for auditing purposes

## Storage of Secrets

Credential and key theft are right up there with the most common attacks. This is not the place to skimp on costs.

### Private Key Abuse

I have discussed company culture and techniques for bringing change in various [talks](https://binarymist.io/talk/agilenz-2014-talk-how-to-increase-software-developer-productivity/), [blog posts](https://blog.binarymist.net/2014/04/26/culture-in-the-work-place/#effecting-change), etc. The following is an extract from my "Culture in the work place" blog post:

Org charts, do not show how influence takes place in a business. In reality, businesses do not function through the organizational hierarchy but through its hidden social networks.

People do not resist change or innovation, but they resist the insecurity created by change beyond their influence.

Have you heard the argument that "the quickest way to introduce a new approach is to mandate its use"?

A level of immediate compliance may be achieved, but the commitment will not necessarily be achieved (Fearless Change 2010).

If you want to bring change, the most effective way is from the bottom up. In saying that, bottom-up takes longer and it is harder. No pain, no gain, or as my wife puts it, it's the difference between making an instant coffee and making an espresso.

Top-down change is imposed on people, and tries to make change occur quickly and deals with problems such as rejection and rebellion only if necessary.
Bottom-up change triggered from a personal level focused by first obtaining trust, loyalty, and respect from serving in a servant leadership approach, earning the right to speak (have you served your time, done the hard yards)?

Because the personal relationship and involvement is not usually present with top-down, people will appear to be doing what you mandated, but secretly, still doing things the way they always have done.

The most effective way to bring change is on a local and personal level once you have built good relationships of trust.

#### SSH

By automating the work that is usually done manually by a developer with SSH access, you are investing a little time in order to do a mundane job that once automated, can be done many times without requiring the concentration and time of a human. This can represent a huge cost savings, as well as increasing your security.

#### TLS

No trade-offs required.

### Credentials and Other Secrets

It may may cost you a little to set-up and maintain additional accounts. This is essential if you want to retain ownership of your CSP account and resources.

#### Entered by People (manually)

It costs little to do research on the most suitable team password database for you, and to implement it. I provided you a good selection of factors to consider when reviewing and making your selection in the [Countermeasures](#cloud-countermeasures-storage-of-secrets-credentials-and-other-secrets-entered-by-people-manually) subsection.

#### Entered by Software (automatically)

Security is, in part, a deception. By embracing defence in depth, we make it harder to break into systems, which just means it takes longer, and someone may have to think a little harder. There is no totally secure system. You decide how much it is worth investing to slow your attackers down. If your attacker is 100% determined and well resourced, they will likely compromise you eventually, no matter what you do.

## Serverless

Securing Serverless is currently hard because the interactions between components are all in the open and on different platforms, which requires different sets of users and privileges. Trust boundaries are disparate.

At the same time, Serverless does push the developer into creating more testable components.

If you are going to go Serverless, which I admit does include some very compelling reasons to do so, make sure you invest heavily into implementing the [Countermeasures](#cloud-countermeasures-serverless) I have discussed.

### Functions

My hope is that after consuming this book series, you will be in a much better place to apply application security, understand how the Permissions Model works and be able to apply it.

### DoS of Lambda Functions

Yes, you need to spend some time here up front making sure you configure your duration and invocation counts conservatively, as well as setting alarms.

### Frameworks

If you have time to learn another framework then do so, this may take some trial and error to know whether the framework provides the right level of abstraction for you, while still providing enough breakout to obtain the low level control you may need.

## Infrastructure and Configuration Management

You will need to weigh up the life of your project/product against the cost of codifying parts of your infrastructure and configuration. In most cases projects will benefit from some initial outlay in this, the pay off will usually be realised reasonably quickly.

## AWS

### Additional Tooling

Understand the underlying issues, errors and defects that the tooling options operate on, and do not depend solely on tools to inform you of problems.
