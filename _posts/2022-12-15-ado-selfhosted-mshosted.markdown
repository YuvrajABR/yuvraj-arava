---
layout: post
title:  "Azure pipelines self hosted and microsoft hosted agents"
date:   2022-12-15 12:02:06 +0530
categories: azuredevops
---
`Purpose of agent` - Pipeline tasks to be executed needs compute, this compute is nothing but an agent in azure devops, multiple agents group together is agent pool.

`Microsoft hosted agent` - Pipeline agent/compute is managed by Microsoft.

Agents are defined as below in yaml azure pipelines.

{% highlight yaml %}
  Pool:
    vmImage: windows-latest
{% endhighlight %}

`Self hosted agent` - Pipeline agent/compute is managed by user.

{% highlight yaml %}
  Pool:
    demands: 'selfhostedpool'
    - Agent.Name -eq dotnet-build-vm
{% endhighlight %}


