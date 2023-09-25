# Terraform best practices

Lock versions! Modules, providers, and Terraform itself. The devil you know is better than the devil you don't. Use either Docker `docker run -v $(pwd):/<dir name>/ -w /<dir name>/ --rm -it hashicorp/terraform:light <terraform command>` or `tfenv`. Use a shell alias/wrapper to make life easier for the version you standardize on.

Don't forget about .gitignore

```
terraform.tfstate.backup
terraform.tfvars
.terraform/
```

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
dc = "foo"
zones = ["a"]
subnet = "bar"
```

Locals can use expressions and resource arguments so they are dynamic. Used in modules. Do things like enforcement of tag names or bucket names. Consider the local a constant to be relied on. Be aware of cloud provider character limits.

```
locals{
    bucket_name = "${local.company_name}-protected-${local.suffix}-${var.dc}"
}

```

Do:
- keep all variables in one file
- use tfvars where necessary
- utilize env vars
- use for locals de-hardcoding one-time names, DRY
- keep things generic
- leave logic to modules, KISS
- avoid using locals outside of modules
- pass outputs to modules using `data`` sources

Don't:
- use multiple locals blocks if not totally necessary
- decentralize vars/tfvars
- ignore env vars

Ugly:
- Hardcoded variable values where not necessary

# References
- https://www.youtube.com/watch?v=U_CsR5ibrOI
left off at 19:48
