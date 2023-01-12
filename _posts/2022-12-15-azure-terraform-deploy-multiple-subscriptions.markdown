---
layout: post
title:  "Terraform deploy to multiple azure subscriptions"
date:   2022-12-16 12:02:06 +0530
categories: azuredevops
---
Deploying the azure infrastuture to mulitiple subscription from same terraform configuration can be
achieved by defining multiple provider blocks with alias.

## Pre-Requsites

-  App registration/Azure login user must be provided with permissions for both subscriptions.

Assign alias as below with name of you choice, here defining `alias` as `dev`
{% highlight terraform %}
provider "azurerm" {
  alias           = "dev"
  subscription_id = "XXXXXXXX-47ef-47ec-b60a-XXXXXXXXXXX"
  features {}
}
{% endhighlight %}

Similarly, for other subscription assinging `alias` as `prod`.
{% highlight terraform %}
provider "azurerm" {
  alias           = "prod"
  subscription_id = "XXXXXXX-413a-4d05-ba10-XXXXXXXXXXX"
  features {}     
}
{% endhighlight %}

We have done defining the provider for both the subscription, Referencing the subscription with provider name as below.

Referencing the `dev` subscription with `provider = azurerm.dev` attribute as below.

{% highlight terraform %}
#creating resource group in dev subscription
resource "azurerm_resource_group" "dev" {
  provider = azurerm.dev
  name     = "azeus-demo-dev-rg"
  location = "East US"
}
{% endhighlight %}

Likewise, referencing the `prod` subscription with `provider = azurerm.prod` attribute as below.

{% highlight terraform %}
#creating resource group in dev subscription
resource "azurerm_resource_group" "prod" {
  provider = azurerm.prod
  name     = "azeus-demo-prod-rg"
  location = "East US"
}
{% endhighlight %}

Below complete configuration, with two subcription creating resource group in each subscription.

{% highlight terraform %}
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=3.0.0"
    }
  }
}

provider "azurerm" {
  alias           = "dev"
  subscription_id = "XXXXXXXX-47ef-47ec-b60a-XXXXXXXXXXX"
  features {}
}

provider "azurerm" {
  alias           = "prod"
  subscription_id = "XXXXXXX-413a-4d05-ba10-XXXXXXXXXXX"
  features {}     
}

resource "azurerm_resource_group" "dev" {
  provider = azurerm.dev
  name     = "azeus-demo-dev-rg"
  location = "East US"
}

resource "azurerm_resource_group" "prod" {
  provider = azurerm.prod
  name     = "azeus-demo-prod-rg"
  location = "East US"
}
{% endhighlight %}