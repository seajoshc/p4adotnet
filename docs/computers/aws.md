# AWS

[Establishing your best practice AWS environment](https://aws.amazon.com/organizations/getting-started/best-practices/) - the short and sweet bible on AWS account organization

Multi-region blog series
- [foundation with AWS security, networking, and compute services](https://aws.amazon.com/blogs/architecture/creating-a-multi-region-application-with-aws-services-part-1-compute-and-security/)
- [data and replication strategies](https://aws.amazon.com/blogs/architecture/creating-a-multi-region-application-with-aws-services-part-2-data-and-replication/)
- [application and management layers](https://aws.amazon.com/blogs/architecture/creating-a-multi-region-application-with-aws-services-part-3-application-management-and-monitoring/)

[DR architecture on AWS series](https://aws.amazon.com/blogs/architecture/tag/disaster-recovery-series/)

## Cognito

The two main components of Amazon Cognito are user pools and identity pools. User pools are user directories that provide sign-up and sign-in options for your app users. Identity pools enable you to grant your users access to other AWS services. You can use identity pools and user pools separately or together.

## Lambda

Default is synchronous, change invocation type to `Event` to switch to async

- when using async, failed requests go to the DLQ
- change invocation type to `DryRun` to do just that
