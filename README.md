# ![BYU logo](https://www.hscripts.com/freeimages/logos/university-logos/byu/byu-logo-clipart-128.gif) terraform-documentation
Documentation for Terraform usage and best practices

## Table of Contents
- [About](#about)
    - [What is Terraform?](#what-is-terraform)
    - [Who are we?](#who-are-we)
- [Getting Started](#getting-started)
    - [Requirements](#requirements)
    - [Try out Terraform](#try-out-terraform)
    - [Deploy a Pipeline](#deploy-a-pipeline)
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
2. [Connect to GitHub with SSH](https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)
3. [Log into an AWS Account](https://github.com/byu-oit/BYU-AWS-Documentation#accessing-byu-aws-cli)

### Try out Terraform
We're going to create and deploy a simple application.
1. Create new working directory

    `mkdir my-project && cd my-project`

2. Initialize Terraform in this directory   

    `terraform init`
3. Create main.tf
    
    (Hello, World!)
4. Deploy using `terraform apply`
5. (Observe deployed app)
6. Make a change
7. Redeploy using `terraform apply`
8. (Observe deployed app)
9. `terraform destroy`

Let's talk about what you just did. You just used Terraform to provision X, Y, and Z AWS resources. Further, we made a trivial change that we then rolled out using Terraform.

To make that change, Terraform uses a state file. That file ...

Our simple application stored the state file on your local machine. Let's do it the right way.

First we need to cleanup the mess we made when we did it wrong

`rm stuff`

Now we can add the config that tells Terraform to use the Backend 

1. Add S3 Backend to main.tf
2. `terraform init`

    Note: If you get an error, it's probably because nobody has ever used a Terraform S3 Backend with this account yet. Don't worry, it's easy to set this up. Follow [these instructions](#s3-backend).

`terraform apply`

(Observe deployed app)

`terraform destroy`

### Deploy a Pipeline
### Explore Established Deployment Patterns
(Link to "standard" modules)
### See an Example
(Link to an actual deployable)
### Discover Modules
(Link to #terraform-modules, Terraform's module registry)

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
Terraform Modules are in [public GitHub repositories](https://byu.app.box.com/file/293393654658) using the naming pattern `terraform-<provider>-<module_name>`. They should be created from [the terraform module template](https://github.com/byu-oit/terraform-module-template) and have the `terraform-module` GitHub topic.

Deployable Terraform applications should be in private GitHub repositories. They should have the `terraform` GitHub topic.

### Terraform Modules
#### Types of Terraform Modules
| Type | GitHub Topics | Description | Example |
| --- | --- | --- | --- |
| Standard | `standard` & `terraform-module` | High level pattern module with standards and best practices baked in and a few configurable variables | [standard-fargate](https://github.com/byu-oit/terraform-aws-standard-fargate) |
| Component | `component` & `terraform-module` | Grouped resources that make up a logic unit | [alb](https://github.com/byu-oit/terraform-aws-alb) |
| Helper | `helper` & `terraform-module` | Module that does not instantiate nor update resources, but provides information or reusable functions | [acs-info](https://github.com/byu-oit/terraform-aws-acs-info) |
| Tool | `tool` & `terraform-module` | One-off resources to be built manually | [bastion](https://github.com/byu-oit/terraform-aws-bastion) |

#### Module Structure
Our terraform modules follow (for the most part) the [module structure](https://www.terraform.io/docs/modules/index.html) defined by Terraform. See [the terraform module template](https://github.com/byu-oit/terraform-module-template) for the basic structure of our terraform module.

## Get Involved
### Connect with the Terraform Working Group
The Terraform Working Group is a group of individuals from different teams working together on the best practices of using Terraform at BYU. There is no particular owner, product, nor project backing the Terraform Working Group.

- GitHub team: [Terraform Developers](https://github.com/orgs/byu-oit/teams/terraform-developers)
- Slack channel: [#terraform](https://byu-oit.slack.com/archives/CQ2BE663T)
- Terraform Working Group meeting notes: [On Box](https://byu.app.box.com/notes/565434185067?s=i0zy8v9aymtf0rhtd2ywpe1puldi8b2n)

### How to Contribute
Reach out on Slack at [#terraform](https://byu-oit.slack.com/archives/CQ2BE663T), or create GitHub issues and pull requests on existing repositories.

To create a new module, create a new repo from the [the terraform module template](https://github.com/byu-oit/terraform-module-template), name it [accordingly](#github-conventions), and share the repository with the [Terraform Developers](https://github.com/orgs/byu-oit/teams/terraform-developers) team on GitHub, which will allow you to request reviews from the [Terraform Developers](https://github.com/orgs/byu-oit/teams/terraform-developers).
