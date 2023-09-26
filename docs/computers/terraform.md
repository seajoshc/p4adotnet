# Terraform best practices

Lock versions! Modules, providers, and Terraform itself. The devil you know is better than the devil you don't. Use either Docker `docker run -v $(pwd):/<dir name>/ -w /<dir name>/ --rm -it hashicorp/terraform:light <terraform command>` or `tfenv`. Use a shell alias or bash wrapper to make life easier for the version you standardize on!

Don't forget about .gitignore

```
terraform.tfstate.backup
terraform.tfvars
.terraform/
```

Organize based on what is within your control. You cannot control reorgs, for example.

Tag every resource liberally. Owner/contact, service role, environment, biz unit, managed_by=Terraform, etc. No tags, no merge!

Use remote state. Seems obvious, basically a requirement when operating as a team. Secured and backed up! Use state lock to prevent collisions. Don't commit state to git...

- Use a `data source` with TF remote state to share things like IDs across state files. Example:

```
# stateful service outputs a SQL DB id
output "sqldb_id" {
  value       = azurerm_sql_database.example.id
  description = "Database ID"
}

# stateless service references that ID for configuration
data "terraform_remote_state" "dev_sqldb" {
  backend = "azurerm"

  config = {
    storage_account_name = "terraformsa"
    container_name       = "terraformstate"
    key                  = "development/sqldb.tfstate"
  }
}

# now using in a file
data.terraform_remote_state.dev_sqldb.outputs.sqldb_id
```

Look at using community modules instead of rolling your own first. Don't reinvent the wheel, look for the most downloaded/used modules. That said, you need to know what is getting created and it shouldn't be a mystery when using community modules. If filling in 50+ parameters (e.g. EKS module), including for features you may not use, it's ok to take a step back and consider building your own module to make it more maintainable and understandable.

When you do create your own modules, strive to keep them clean and stateless.

Apply coding best practices. KISS, DRY, linting, formatting, pull requests with reviewers before merging and CI.

Do:

- Use git for managing TF files
- KISS & DRY yet Human & Clean (humans read and maintain Terraform)
- Functional programming & idempotency

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

- use multiple locals blocks if not totally necessary, hard to track and maintain
- decentralize vars/tfvars, keep them in one file for easier maintenance
- ignore env vars

Ugly:

- Hardcoded variable values where not necessary

## Execution

Do:

- use remote execution
- use TF apply with a plan file
- setup a TF timeout so you aren't waiting forever when something doesn't go right

Don't:

- execute locally, keep state where it's supposed to be

Ugly:

- execute locally and Ctrl+C to fubar your state

## Practices enforcement

- Tag resources
- Linting (`tflint`) & formatting (`terraform fmt`)
- Clean and reusable
- Documentation
- More complex things like notify a FinOps team if expected spend is over a threshold

Do:

- pre-merge linters, formatters, and logical checks
- GitHub actions/CI pipeline checks
- Slack bot drift reporter
- main branch = actual environment

Security issues and non-compliant configs if you don't have any practices enforcement.

## Structure

Huge state files have a big blast radius, you want small state files. Best:

```
modules/
  my-cool-service/
    global/
    regional/
root-modules/
  my-cool-service/
    development/
      global/
      us-west-2/
    staging/
      global/
      us-west-2/
    production/
      alpha/
        global/
        us-east-2/
        us-west-2/
      beta/
        global/
        us-east-2/
        us-west-2/
      gamma/
        global/
        us-east-2/
        us-west-2/
  an-equally-cool-service/
    development/
      global/
      us-west-2/
    staging/
      global/
      us-west-2/
    production/
      global/
      us-west-2/
```

Good:
```
root-modules/
  my-cool-service/
    development/
        us-west-2/
    staging/
        us-west-2/
    production/
        us-west-2/
  an-equally-cool-service/
    development/
        us-west-2/
    staging/
        us-west-2/
    production/
        us-west-2/
```

## Workspaces

Use-cases:

- different environments (??? double check this, think Hashi says not to use for this)
- different regions
- Same config, different customers/tenants
- test a set of changes before modifying prod

Use a TF wrapper to make sure you're using the right workspace!

# Tools

Atlantis

- Handles TF lock
- Manages state and history
- Ensures main branch is the actual environment state.

Others

- TFLint
- TFsec
- Infracost
- Inframap
- ValidIaC
- Terratest
- Terratag
- Terragrunt
- Checkov
- KICS
- Super-Linter

# References

- https://www.youtube.com/watch?v=U_CsR5ibrOI
- https://blog.gitguardian.com/infrastructure-as-code-security-best-practices-cheat-sheet-included/
- https://spacelift.io/blog/terraform-state
- https://substrate.tools/blog/terraform-best-practices-for-reliability-at-any-scale
