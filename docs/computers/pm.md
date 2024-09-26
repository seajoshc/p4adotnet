# Product Management

Make it work, then make it right, then make it fast. The engineers you work with will thank you.

## Launching a new SaaS product

Gatekeep your alpha/beta, do not go "open beta" until you are forced to. Talk to all of your "closed beta" customers regularly, know them by name and keep refining. Let more customers in if you aren't getting the feedback you need OR if things are booming and you know you are on to something. But be judicious if you expand the cohort, you must keep talking to whoever you let in.

Always be talkin to customers, duh.

Instrument the shit out of your product with an analytics tool. Currently a huge fan of Amplitude and Intercom.

Find PMF by asking users How upset would you be if the product went away?

- Very sad
- Sad
- Not sad at all

Then measure

- MAU / MAI, (Monthly actives over monthly instances aka tenants) - How many users per tenant shows adoption
- Pay attention to Churn at W2, W3, W4
- Have a funnel, know your funnel, analyze your funnel

Refine, refactor, invest deeper into every experience you build going forward. Likely taking on a ton of work at this point and letting things bake and getting customer guidance is now critical.

Have a 3-6 months roadmap and keep focusing, expand when you have clear signal for what's next AND your funnel is healthy at the top.

Start focusing on completing journeys/jobs-to-be-done. Make sure the product tells a story. One really solid usecase with a clear "we do X better than A, B, C" story is worth WAY more than 10 half assed usecases. Caveat here is you can get away with half assed (at least for a few years) if your new product is being built inside an already very successful/large company but you will still enjoy much better success if you focus :) (speaking from experience building a new SaaS product for developers at Atlassian)

### Developer Experience / Extensibility

Think about extensibility and developer experience with your product from day 1. If you're building something SaaS chances are you'll want this at some point (e.g. integrations); doubly so if you're a product sold to devs/eng. APIs, SDKs, CLI tools, etc. get it out early and refine it openly.

Carve out a DevEx for the path YOU WANT 3P developers to build towards. Figure out what that core experience is, the best way they can extend your platform, and hand hold them down it. Build your base.

### On integrations with dev tools

Trying to build lots of plugins/integrations for 3P developer systems/tools is hard and doesn't scale well. You will ship a lot of v1's and never come back to them if you try a "get names on the box" to attract customers approach. Invest in the most critical apps that will actually get used; build magic experiences for top requested integrations worth the most deals/seats. If you spend 2 eng time for 2 sprints on an app nobody uses for 12+ months just to potentially attract customers was it worth it? Name on the box helps much less compared to a strong "we do X,Y,Z better than all our competitors" pitch using just a few (or one!) really strong usecase.

## Things to emulate or learn from

### Articles and blog posts

Building and running modern software is hard.

- [DevOps is Bullshit](https://blog.massdriver.cloud/posts/devops-is-bullshit/)
- [Who should write the Terraform?](https://zwischenzugs.com/2022/08/08/who-should-write-the-terraform/)
- [Mind the platform execution gap](https://martinfowler.com/articles/platform-prerequisites.html)
- [Gardening platforms](https://docs.google.com/presentation/d/1cY95dRixFho0pMIlrEFcGL_XKVy9vnE4NGOD6TQMj50/view#slide=id.p) - the format is really obnoxious but there is some decent nuggets in there
- https://blog.jim-nielsen.com/2022/website-fidelity/
- Video on moving fast from YouTube

### SDK, API, Dev Docs

Good examples to emulate and learn from:

- https://news.ycombinator.com/item?id=32794330
- https://tailwindcss.com/docs/installation
- https://www.solidjs.com/tutorial/introduction_signals
- https://www.postgresql.org/docs/current/index.html
- https://docs.djangoproject.com/en/4.1/
- https://learn.microsoft.com/en-us/dotnet/api/system.linq.enumerable.distinct?view=netcore-3.0
- https://redis.io/commands/
