---
layout: post
title: Deploy a package, not just a tag, branch or binary
status: public
type: post
published: true
author: Sriram Narayan
---

In a section called *Principles and Practices of Build and Deployment Scripting*, [the CD book](http://continuousdelivery.com/) suggests to *Use Your Operating System’s Packaging Tools*.

“Deploying a bunch of files that need to be distributed across the filesystem is very inefficient and makes maintenance—in the form of upgrades, rollbacks, and uninstalls—extremely painful. This is why packaging systems were invented.”

We concur. This is a good example of cross-pollination of good ideas between development and operations. Software development teams are increasingly packaging their applications using application-specific ([maven](http://maven.apache.org/ref/3.1.0/maven-model/maven.html), [gem](http://guides.rubygems.org/specification-reference/), [wheel](https://pypi.python.org/pypi/wheel), [npm](https://npmjs.org/doc/cli/npm-install.html), [cpan](http://www.cpan.org/modules/INSTALL.html)…) or system ([rpm](http://rpm5.org/), [deb](http://www.debian.org/doc/manuals/debian-faq/ch-pkgtools.en.html), [nuget](http://docs.nuget.org/), [chocolatey](http://www.hanselman.com/blog/IsTheWindowsUserReadyForAptget.aspx)...) packaging tools. Doing so has several advantages:

1.   Built in versioning (e.g. [rpm](http://rpm5.org/), [deb](http://www.debian.org/doc/manuals/debian-faq/ch-pkgtools.en.html), [nuget](http://docs.nuget.org/)).
2.   Dependency management.
3.   Distribution via standardized repository layouts, metadata and protocol.
4.   Transactional install/upgrade/uninstall via package managers.
5.   Managing configuration files: OS packages know how to manage files that are expected to change after installation so you don't lose your changes when you uninstall or override when you upgrade. e.g. rpm’s [%config macro](http://www-uxsup.csx.cam.ac.uk/~jw35/docs/rpm_config.html).
6.   File verification: Packages have built-in checksums, which can be used for verification before a package is extracted and installed.
7.   Signature verification (e.g. [rpm](http://www.centos.org/docs/5/html/Deployment_Guide-en-US/s1-check-rpm-sig.html)).
8.   Idempotence - Issuing the same package install/upgrade/uninstall multiple times leaves the system in the same state as issuing it once.
9.   Auditability: OS packaging tools allows you to track [what package installed what file](http://unix.stackexchange.com/questions/4705/how-to-find-out-which-package-a-file-belongs-to) in your system (versus files appearing or being changed at will)
10.   Traceability - what repository did a package come from? (e.g. deb)
11.   Infrastructure automation tools like Chef, Puppet and Ansible have good support for popular package management systems. Traditionally used for provisioning servers, and environments, these tools are increasingly being used for [application provisioning](https://github.com/opscode/java-quick-start) - more on this point in a follow-up post.
There you go - eleven good reasons to start creating packages for your components and applications and deploying from packages rather than from a tag, branch or binary.

![](/images/blog/sriram-package1.png)

Thanks to [Danilo](http://www.dtsato.com/blog/about/), [Ram](http://twitter.com/sriramnrn) for their insights on this topic.


<div class="highlight">This post is also cross-posted <a href="http://www.thoughtworks.com/insights/blog/deploy-package-not-just-tag-branch-or-binary
">here</a>.</div>
