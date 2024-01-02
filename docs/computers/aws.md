# AWS

[Establishing your best practice AWS environment](https://aws.amazon.com/organizations/getting-started/best-practices/) - the short and sweet bible on AWS account organization

## Cognito

The two main components of Amazon Cognito are user pools and identity pools. User pools are user directories that provide sign-up and sign-in options for your app users. Identity pools enable you to grant your users access to other AWS services. You can use identity pools and user pools separately or together.

## Lambda

Default is synchronous, change invocation type to `Event` to switch to async

- when using async, failed requests go to the DLQ
- change invocation type to `DryRun` to do just that
