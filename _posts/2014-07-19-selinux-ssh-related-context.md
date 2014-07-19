---
layout: post
title: ssh相关的selinux安全上下文小记一则
description: ssh相关的selinux安全上下文未正确设置导致无法连接
tags: [Linux, ssh]
---

在配置使用pgssh，建立多主机信任关系过程中，遇到无法通过ssh验证的问题。追查多时，才发现机器居然，居然开启了selinux，将selinux关闭后再试，问题果然不再复现了。

继续追查selinux的具体原因，一开始以为是selinux布尔值的原因，尝试后无果；后来发现是安全上下文的问题，是家目录下的.ssh目录及其文件的安全上下文不对，执行`restorecon -R -v .ssh`命令恢复其安全上下文后，问题即解决。

