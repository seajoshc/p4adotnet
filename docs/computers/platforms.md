# On platforms

As a product manager working on software products for a tech company, platforms are something I think about a lot.

## Links
- https://learn.microsoft.com/en-gb/platform-engineering/about/principles

## Mind the platform execution gap

[Notes from this Martin Fowler article](https://martinfowler.com/articles/platform-prerequisites.html)

A platform is a foundation of self-service APIs, tools, services, knowledge and support which are arranged as a compelling (internal) product.

The purpose of a developer productivity platform is to allow teams who build end-user products concentrate on their core mission.

An internal platform team will usually take tools and services offered by cloud providers and other vendors and host, adapt or extend them to make them conveniently available to their software developer colleagues. The aim is not to reinvent commercially available functionality (the world does not need another homegrown Kubernetes) but to bridge the gap between what you can buy and what is really needed (your teams may appreciate a simplified Kubernetes experience that takes advantage of assumptions about your infrastructure and makes it easier to manage).

A what-it-is-for rather than a how-it-is-made view of platform is preferable because offering platform services to internal teams is an institutionalised approach to reducing friction. It is incumbent upon platform engineers to keep an open mind about the best way to reduce that friction. Some days that will be provisioning infrastructure. Other days it might be making a build script a little easier to use or facilitating a workshop to help a team to define their SLOs.

When it goes wrong, problems with the platform are passed directly onto the entire software development organisation.

Your users appreciate consistency, stability and dependability over a stream of new features. 

Deprecation is a fundamental part of the platform product lifecycle, and failure to consider it may undermine the business benefits you hoped to gain by offering it in the first place.

Anything that prevents developers from smoothly using your platform, whether a flaw in API usability or a gap in documentation, is a threat to the successful realisation of the business value of the platform. Prioritise developer experience.

Platform products require customer empathy, product ownership and intelligent measurement, just like other kinds of product.

[Users] need to appreciate it, understand it and be aware of its features.

You have access to lots of feedback/communication internally, use it but also be ready for a lot of blunt/candid feedback. Harness your champions, encourage them to evangelize and always have a group of champions on your side you meet with regularly. You still need a roadmap and need to communicate it. Don't forget to market internally as well, just as important for awareness.

Maintaining goodwill and trust is key to adoption and continued usage.

