# 1. Asset Identification {#asset-identification}

I would encourage you to review the Asset Identification stage from the [30,000' View chapter](https://f0.holisticinfosecforwebdevelopers.com/chap03.html#starting-with-the-30000-foot-view) of [Fascicle 0](https://f0.holisticinfosecforwebdevelopers.com/toc.html), remove any assets that are not applicable and add any newly discovered ones.

## Productivity

Using IaaS, and even more so with PaaS, can provide great productivity gains. However, everything comes at a cost, as you can not gain productivity for free. Something will be scarified, usually that will be your security; which means you will no longer have control of your data.

Using cloud services can be a good thing especially for smaller businesses and startups. However, firstly decisions need to be made whether to use an external cloud provider or to create your own, as there are some very important considerations to be made. We will discuss these in the Identify Risks and Countermeasures subsections.

## Competitive Advantage

If you are a startup, be aware that the speed you initially enjoy with PaaS may not continue as your product moves from proof of concept to when the customers start to use your product or service. You may decide to be more cautious on your customer's behalf and your own intellectual property (IP) by bringing it in-house, or you may entrust it to a provider that takes security seriously, rather than just saying they do. We will be investigating these options through the Identify Risks subsection.

## Control

With regards to control of our own environments, we are blindly trusting huge amounts of IP to Cloud Service Providers (CSPs). In many instances, I have worked with customers who insist on putting everything in the Cloud without putting any thought into the security aspects of using the Cloud. Some have even commented how they are not concerned with the security risk. The problem is, they do not understand what is at risk. They may wonder why their competitor beats them to the market as their progress and plans are intercepted. Bruce Schneier's book, "Data and Goliath" is the best book I have read to date that reveals the problems of blindly yielding everything. It's an eye-opening canon of the sad reality of our risky habits and the results they will yield.

Whenever you see the word "trust", you are yielding control to the party you are trusting. When you trust another entity with your assets, you are giving them the power to control. Step back and think: Are your assets their primary concern, or is it maximising their profits by using you and/or your data as their asset?

If you decide to use an external cloud provider, you need to be aware that whatever goes into the Cloud is almost completely out of your control. You may not be able to remove your data once it is there, and have visibility into whether or not the existing data has really been removed from the Cloud.

## Data

If you are dealing with sensitive customer data, then you as the developer have an ethical and legal responsibility for it. Also, if you are putting sensitive data in the Cloud, then you could very well be shirking your responsibility. You may not even retain the legal ownership of it.

We will keep these assets in mind as we work through the rest of this chapter.
