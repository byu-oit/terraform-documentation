# ![BYU logo](https://www.hscripts.com/freeimages/logos/university-logos/byu/byu-logo-clipart-128.gif) terraform-documentation
Documentation for Terraform usage and best practices

## Table of Contents
- [About](#about)
    - [What is Terraform?](#what-is-terraform)
    - [Who are we?](#who-are-we)
- [Getting Started](#getting-started)
    - [Requirements](#requirements)
    - [Try out Terraform](#try-out-terraform)
    - [Explore Established Deployment Patterns](#explore-established-deployment-patterns)
    - [See an Example](#see-an-example)
    - [Discover Modules](#discover-modules)
- [Standards and Best Practices](#standards-and-best-practices)
    - [S3 Backend](#s3-backend)
    - [GitHub Conventions](#github-conventions)
    - [Terraform Modules](#terraform-modules)
- [Get Involved](#get-involved)
    - [Connect with the Terraform Working Group](#connect-with-the-terraform-working-group)
    - [How to Contribute](#how-to-contribute)


## About


### What is Terraform?
For our purposes, [Terraform](https://github.com/hashicorp/terraform) is a declarative way to provision AWS resources. It can be used for infrastructure or deployable applications.

To use Hashicorp's own description, [Terraform](https://github.com/hashicorp/terraform) enables you to safely and predictably create, change, and improve infrastructure. It is an open source tool that codifies APIs into declarative configuration files that can be shared amongst team members, treated as code, edited, reviewed, and versioned.

You can think of [Terraform](https://github.com/hashicorp/terraform) as our successor to [handel](https://github.com/byu-oit/handel) and [handel-codepipeline](https://github.com/byu-oit/handel-codepipeline). It's more powerful and more flexible, but it requires some additional knowledge about the way AWS resources operate.
### Who are we?
We're the Terraform Working Group at BYU OIT. [Connect with us!](#connect-with-the-terraform-working-group)

## Getting Started
### Requirements
1. [Download and Install Terraform](https://www.terraform.io/downloads.html)
2. [Log into an AWS Account](https://github.com/byu-oit/BYU-AWS-Documentation#accessing-byu-aws-cli)

### Try out Terraform
Follow the steps at [`byu-oit/hello-world-api`](https://github.com/byu-oit/hello-world-api) to create and deploy a simple application.

### Explore Established Deployment Patterns
- [BYU OIT repositories with the `terraform-module` and `standard` topics](https://github.com/search?q=org%3Abyu-oit+topic%3Aterraform-module+topic%3Astandard&type=Repositories)
### See an Example
- TODO: Link to an actual deployable
- [BYU OIT repositories with the `terraform` topic](https://github.com/search?q=org%3Abyu-oit+topic%3Aterraform&type=Repositories)
### Discover Modules
- [BYU OIT repositories with the `terraform-module` topic](https://github.com/search?q=org%3Abyu-oit+topic%3Aterraform-module&type=Repositories)
- [Terraform's Module Registry](https://registry.terraform.io/)

## Standards and Best Practices
### S3 Backend
Use the [S3 Backend module](https://github.com/byu-oit/terraform-aws-backend-s3) to deploy the Backend resources (S3 Bucket and Dynamo Table) into your account. This only needs to be done once per account.
In each project's main.tf, include the following block at the top:
```
terraform {
  backend "s3" {
    bucket = "terraform-state-storage-<account_number>"
    lock_table = "terraform-state-lock-<account_number>"
    key = "my-cool-app.tfstate"
    region = "us-west-2"
  }
}
```

### GitHub Conventions
Terraform Modules are in [public GitHub repositories](https://byu.app.box.com/file/293393654658) using the naming pattern `terraform-<provider>-<module_name>`. They should be created from [the terraform module template](https://github.com/byu-oit/terraform-module-template) and have the [`terraform-module`](https://github.com/search?q=org%3Abyu-oit+topic%3Aterraform-module&type=Repositories) GitHub topic.

Deployable Terraform applications should be in private GitHub repositories. They should have the [`terraform`](https://github.com/search?q=org%3Abyu-oit+topic%3Aterraform&type=Repositories) GitHub topic.

### Terraform Modules
#### Types of Terraform Modules
| Type | GitHub Topics | Description | Example |
| --- | --- | --- | --- |
| Standard | [`standard` & `terraform-module`](https://github.com/search?q=org%3Abyu-oit+topic%3Aterraform-module+topic%3Astandard&type=Repositories) | High level pattern module with standards and best practices baked in and a few configurable variables | [standard-fargate](https://github.com/byu-oit/terraform-aws-standard-fargate) |
| Component | [`component` & `terraform-module`](https://github.com/search?q=org%3Abyu-oit+topic%3Aterraform-module+topic%3Acomponent&type=Repositories) | Grouped resources that make up a logic unit | [alb](https://github.com/byu-oit/terraform-aws-alb) |
| Helper | [`helper` & `terraform-module`](https://github.com/search?q=org%3Abyu-oit+topic%3Aterraform-module+topic%3Ahelper&type=Repositories) | Module that does not instantiate nor update resources, but provides information or reusable functions | [acs-info](https://github.com/byu-oit/terraform-aws-acs-info) |
| Tool | [`tool` & `terraform-module`](https://github.com/search?q=org%3Abyu-oit+topic%3Aterraform-module+topic%3Atool&type=Repositories) | One-off resources to be built manually | [bastion](https://github.com/byu-oit/terraform-aws-bastion) |

#### Module Structure
Our terraform modules follow (for the most part) the [module structure](https://www.terraform.io/docs/modules/index.html) defined by Terraform. See [the terraform module template](https://github.com/byu-oit/terraform-module-template) for the basic structure of our terraform modules.

#### [Module Sources](https://www.terraform.io/docs/modules/sources.html)
When using a terraform module you specify a `source` which tells Terraform where to find the source code for the module.

We have opted to use the [GitHub source](https://www.terraform.io/docs/modules/sources.html#github) method using `git` 
over HTTPS to pull the source code of our modules living in public GitHub repos. This is the easiest way to pull modules
from GitHub from within a CICD pipeline (aka CodeBuild) without having to do extra configuration. The downside to this 
is if you need to use a module residing in a private GitHub repo, you will then have to use a GitHub access token tied
to a user who has access to the private repo. 

## Get Involved
### Connect with the Terraform Working Group
The Terraform Working Group is a group of individuals from different teams working together on the best practices of using Terraform at BYU. There is no particular owner, product, nor project backing the Terraform Working Group.

- GitHub team: [Terraform Developers](https://github.com/orgs/byu-oit/teams/terraform-developers)
- Slack channel: [#terraform](https://byu-oit.slack.com/archives/CQ2BE663T)
- Terraform Working Group meeting notes: [On Box](https://byu.app.box.com/notes/565434185067?s=i0zy8v9aymtf0rhtd2ywpe1puldi8b2n)

### How to Contribute
Reach out on Slack at [#terraform](https://byu-oit.slack.com/archives/CQ2BE663T), or create GitHub issues and pull requests on existing repositories.

To create a new module, create a new repo from the [the terraform module template](https://github.com/byu-oit/terraform-module-template), name it [accordingly](#github-conventions), and share the repository with the [Terraform Developers](https://github.com/orgs/byu-oit/teams/terraform-developers) team on GitHub, which will allow you to request reviews from the [Terraform Developers](https://github.com/orgs/byu-oit/teams/terraform-developers).
