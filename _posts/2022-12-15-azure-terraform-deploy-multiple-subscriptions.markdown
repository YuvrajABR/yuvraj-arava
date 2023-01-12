---
layout: post
title:  "Terraform deploy to multiple azure subscriptions"
date:   2022-12-16 12:02:06 +0530
categories: azuredevops
---
Deploying the azure infrastuture to mulitiple subscription from same terraform configuration can be
achieved by defining multiple provider blocks with alias.

## Pre-requsites

-  Permissions user id / app registration to subscriptions.

###### Below complete configuration, with two subcription creating resource group in each subscription.

{% highlight terraform %}
terraform {
 backend "azurerm" {
 }
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