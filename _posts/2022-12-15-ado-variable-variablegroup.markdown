---
layout: post
title:  "Azure pipelines variable and variable groups"
date:   2022-12-15 12:02:06 +0530
categories: azuredevops
---
`variable` - Azure pipeline variable is scoped that pipeline (where its defined).

{% highlight yaml %}
  variables:
    - name: username
      value: "adminuser"
{% endhighlight %}

`variable group` - Azure pipeline variables group is group of variables that can be access the across all the pipelines in the project. varaible group is created from pipelines --> library

{% highlight yaml %}
  variables:
    - group: dev-env
{% endhighlight %}