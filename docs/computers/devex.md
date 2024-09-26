# Developer experience

idk this will be something someday

## REST

- Tablestakes way for users to consume your product programmatically or provide users opportunities to extend/enhance your product
- Customers understand this, REST has been around forever and people get it

## GraphQL

- It's a hurdle for customers still in 2024; might change over time

## Webhooks

- Somewhat the standard along with REST
- Let's customers "listen" for certain events in your product
  todo summarize https://github.com/standard-webhooks/standard-webhooks/blob/main/spec/standard-webhooks.md

Start thin, get fuller as necessary. 20kb max size.

HMAC for signing is pretty standard. This plus HTTPS to guarantee sender + integrity of payload. Treat pre shared keys like secrets duh.

## CLI

- Devs, DevOps, SRE, platform, systems people all love CLIs
- You can drop them into CI systems and other workflows, they are portable

## kubectl

- Cam across this recently with CoreWeave, they use kubectl as a client which is pretty rad
- Obviously a super narrow usecase but inspirational
