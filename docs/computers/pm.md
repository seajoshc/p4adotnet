# Product Management

## Lessons learned

Make it work, then make it right, then make it fast. The engineers you work with will thank you. 

Ladder up: data -> insights -> automation.

Gatekeep your alpha/beta, do not go "open beta" until you are forced to. Talk to all of your "closed beta" customers regularly, know them by name and keep refining. Let more customers in if you aren't getting the feedback you need OR if things are booming and you know you are on to something. But be judicious if you expand the cohort, you must keep talking to whoever you let in.

Consistent PMF surveying (regular pacing and measuring) throughout the pre-GA experience so you have data to reason about.

## Launching a new SaaS product

### Before GA

Always be talkin to customers, duh.

Instrument the shit out of your product with an analytics tool. Currently a huge fan of Amplitude.

Find PMF by asking users How upset would you be if the product went away?
- Very sad
- Sad
- Not sad at all

Then measure
- MAU / MAI, (Monthly actives over monthly instances aka tenants) - How many users per tenant shows adoption
- Pay attention to Churn at W2, W3, W4
- Have a funnel, know your funnel, analyze your funnel

Carve out your tentpoles, your core features. Have a story across them, have some happy path/golden path journies working well. Build the product and early customer base around that but begin investing into other features/ideas to see what people crave on top of the core experience.

Integrations/ecosystem think about it early. APIs, SDKs, CLI tools, etc. get it out early and refine it openly. Build a ton of thin/shallow integrations across a wide set of categories if your industry allows for it. You want names that attract your customers, then they will tell you what features they want from the integrations. If you can't go wide and shallow, go really deep in the top 2-3 category/tool/whatever you want to integrate with and make them killer features - this works if you can integrate well with a big player like Salesforce or Atlassian.

Carve out a DevEx for the path YOU WANT 3P developers to build towards. Figure out what that core experience is, the best way they can extend your platform, and hand hold them down it. Build your base.

Refine, refactor, invest deeper into every experience you build going forward. Likely taking on a ton of work at this point and letting things bake and getting customer guidance is now critical.

Have a roadmap but be ready to shift/pivot when a killer idea or feature request from several customers (or let's be real one really large) comes in...

Start focusing on completing journeys/jobs-to-be-done. Make sure the product tells a story.
