# Terraform best practices

Lock versions! Modules, providers, and Terraform itself. The devil you know is better than the devil you don't.

Organize based on what is within your control. You cannot control reorgs, for example.

Tag every resource liberally. Owner/contact, service role, environment, biz unit, managed_by=Terraform, etc. No tags, no merge!

Use remote state. Seems obvious, basically a requirement when operating as a team. Secured and backed up! Use state lock to prevent collisions.

Use community modules instead of rolling your own when possible. Don't reinvent the wheel, look for the most downloaded/used modules.

When you do create your own modules, strive to keep them clean and stateless.

Apply coding best practices. KISS, DRY, linting, formatting, pull requests with reviewers before merging and CI.

## Variables and locals

Variables, their purpose is for settings for module configuration. Make sure to set a default or validation! If used just once, set a default. If used per region/etc, use tfvars file instead.

```
variable "az_names" {
    type = list(string)
    default = ["use-west-2a"]
}

tfvars:
aws_account = "xxx"
aws_region = "us-west-2"
zones = ["a"]
subnet = "foo"
```

Locals can use expressions and resource arguments so they are dynamic. Used to do things like enforcement tag names or bucket names.

```

```

Do:
- keep all variables in one file
- use tfvars where necessary
- utilize env vars
- use for locals de-hardcoding one-time names, DRY
- keep things generic


left off at 17:03
