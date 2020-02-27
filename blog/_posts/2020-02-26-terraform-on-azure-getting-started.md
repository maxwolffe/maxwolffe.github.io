---
layout: goal
title: Terraform on Azure - The Absolute Basics
date_updated: 2020-02-26
---

Modern cloud providers put an unbelievably powerful set of tools in the hands of engineers, but it can be a real pain to set up and manage your own cloud infrastructure. [Terraform](http://terraform.io) is an open source tool developed by Hashi Corp which allows you to declare your infrastructure in code, manage it with version control, and deploy it to the cloud. In the following short tutorial, we'll go through setting up Terraform and we'll create a single resource on Azure using Terraform.

## Why Terraform?
Terraform solves a number of problems you and your organization might face:
1. **Provisioning cloud infrastructure is boring and error-prone**
   This is the reason I use Terraform. I find it to be the easiest way to spin up infrastructure for my personal tests and it allows me to skip past the Azure UI.
2. **Understanding your topology (especially if it's multi-cloud) is difficult.**
   You can define your whole topology in terraform, and using a single `plan` command, determine whether your live infrastructure is the same as the infrastructure which is defined in code.
3. **Topology and configurations drift between environments.**
   Keeping cloud infrastructure consistent in multiple environments (production and staging for example) is difficult if engineers are manually making changes using the provider portals. Terraform has tools to allow one to have different deployment sizes, such as a smaller deployment for staging than prodution, while keeping your deployment topology otherwise the same.
4. **Others!** [Read more here](https://www.terraform.io/intro/index.html)

# Diving In!
Now that we've discussed why you might want to use Terraform, let's dive into provisioning our first resource.

## Pre-requisites
In this section, we're going to get everything we need to run Terraform. The below is tailored towards Mac users and assumes that you have [homebrew](https://brew.sh/) installed.

1. [Install the Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).
    ```
    brew update && brew install azure-cli
    ```
2. Sign up for an Azure Account if you don't already have one. You can start with [a free account by using this promotion](https://azure.microsoft.com/en-us/free/).
3. Log into the azure command line. Terraform uses this CLI under the hood to make requests on your behalf:
    ```
    az login
    ```
4. Install Terraform.
    ```
    brew install terraform
    ```

## Creating a Resource Group

This section is going to cover creating the simplest piece of cloud infrastructure - a resource group. A resource group is an entity which holds references to all of the resources we create, such as VMs, CosmosDB instances, etc. You can read more about Resource Groups in [Terraform's excellent documentation](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html).

1. Create a terraform file in your working directory: `main.tf`. For this tutorial, we'll be making all of our changes within this file.
2. We're going to use Azure for this tutorial, so we start by configuring the Azure provider, which is the client library we'll use to interact with the Azure cloud platform. We'll pin the resource manager to a specific version.

Terraform files are written in the HashiCorp Configuration Language (HCL). Blocks, such as this [provider block](https://www.terraform.io/docs/configuration/providers.html), are the main unit of HCL. This particular block declares the provider and the version of the provider, which will be used throughout your terraform project.
```
file: main.tf

provider "azurerm" {
  version = "=1.36.0"
}
```
3. Let's check to make sure this worked by running our first terraform command, `init`. This command downloads the integrations necessary to start provisioning for your project. You should see something like the following:

```
$ terraform init

Initializing the backend...

Initializing provider plugins...
- Checking for available provider plugins...
- Downloading plugin for provider "azurerm" (hashicorp/azurerm) 1.36.0...

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```
4. Next lets create some resources! We'll use a [resource block](https://www.terraform.io/docs/configuration/resources.html) here to define the resource in our topology. The syntax of a resource block is `resource "provider_resource-type" "local_name"`, followed by configuration properties within the block. In the below example, we are defining an azure resource group with the name `"main"`, within the block we are naming the resource group `"mwolffe-resources"` and telling azure to provision this resource group in the `"US West 2"` region. Each resource type has different parameters. You can find a list of all resource types for Azure on the [Terraform website](https://www.terraform.io/docs/providers/azurerm/index.html).

```
file: main.tf

provider "azurerm" {
  version = "=1.36.0"
}

resource "azurerm_resource_group" "main" {
  name     = "mwolffe-resources"
  location = "West US 2"
}
```
5. Now that we have our first piece of infrastructure defined in code, let's create it on Azure!

`terraform plan` allows you to see the changes which Terraform will make on your behalf. It does this by querying Azure and comparing the state on Azure to the state we have defined in our file. Let's do that now.
```
$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_resource_group.main will be created
  + resource "azurerm_resource_group" "main" {
      + id       = (known after apply)
      + location = "westus2"
      + name     = "mwolffe-resources"
      + tags     = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

`terraform plan` doesn't actually create anything for us, it just shows us the changes we're going to make. Let's actually create those resources now, with `terraform apply`.

```
$ terraform apply

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_resource_group.main will be created
  + resource "azurerm_resource_group" "main" {
      + id       = (known after apply)
      + location = "westus2"
      + name     = "mwolffe-resources"
      + tags     = (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

azurerm_resource_group.main: Creating...
azurerm_resource_group.main: Creation complete after 1s [id=/subscriptions/xxx-xxx-xxx-xxx/resourceGroups/mwolffe-resources]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

Let's go to the Azure portal to make sure that resource group has actually been created for us!

- Visit ms.portal.azure.com
- Sign in
- Navigate to Resource Groups
- Observe your resource group and rejoice!

6. Let's clean up this new resource group by running `terraform destroy`. This will make sure we're not charged for item's we're not using.

## Conclusion
Hopefully you found this tutorial a fun and easy introduction to some basic Terraform concepts. I intend to follow this up with another very simple Azure example which creates a basic web server. If you are interested in learning more about Terraform I highly recommend [Jim Brikman's Terraform Up and Running](https://www.terraformupandrunning.com/). Please [tweet at me](https://twitter.com/MaxWolffe) or [send me a LinkedIn message](https://www.linkedin.com/in/maxwolffe/) if you have questions or comments! Thanks for reading.
