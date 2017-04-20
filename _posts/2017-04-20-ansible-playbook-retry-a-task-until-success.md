---
layout: post
title: ansible playbook retry a task until success
description: This article describes how to set a task to retry for several times until success in ansible playbook.
tags: [ansible]
---

When writing ansible playbook, there is a scene when a task should be delayed for seconds to run. But you do not know how long it should be. In this case, we want to delay this task for seconds, and retry it for several times. 

Here is an example to archive that.

<!--more-->

{% highlight yaml %}
tasks:
  - shell: echo "do something"; exit 1
    register: command_result
    retries: 5
    delay: 10
    until: command_result | success
{% endhighlight %}
