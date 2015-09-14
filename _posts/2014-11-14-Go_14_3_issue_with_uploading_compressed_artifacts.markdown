---
layout: post
title: UPDATE&#58; Issue with uploading compressed artifacts in GoCD 14.3.0
status: public
type: post
published: true
author: GoCD Team
---


There was an [issue](https://github.com/gocd/gocd/issues/703) reported with respect to artifact uploads in the 14.3.0 release of GoCD.

### Issue
On GoCD server running with OpenJDK7, uploading compressed artifacts fails. The operation is reported as success in console-log on job details page but actual artifact does not get uploaded. Please note this does not affect uploading console-log itself or even a simple file as an artifact. This is an issue only with GoCD v14.3.0. Read further to know if you are affected by this defect.

### Who does this affect?
This defect would affect you only if your GoCD Server v14.3.0 is run using OpenJDK(jdk or jre) and the agent responsible for artifact upload is using a version of java other than the one used by GoCD server.

You are *unaffected* by this defect if:

- you use Sun/Oracle java to run GoCD Server

- you have SunEC extension installed for your OpenJDK [*EDIT*: on your GoCD server]

- Both your server and agent processes run using OpenJDK (not necessarily the same version) [*EDIT*: without <a href="http://en.wikipedia.org/wiki/Elliptic_curve_cryptography">ECC cipher suites</a>.
As explained in "What caused this?" section, this issue occurs when java on agent supports ECC cipher suites but java on server doesnot. You should definitely run the litmus test to be sure.]

#### Litmus test

You should run a pipeline which uploads a compressed file as an artifact to ensure you are not affected by this defect. If the artifact shows up on the artifacts tab of the job and can be successfully downloaded, you are safe from the defect.

### Workaround
We are aware that you have been waiting to try your hands on the new APIs and several bug fixes that came out in 14.3.0. 
Fortunately we have a few workarounds available to help you move ahead with the upgrade, meanwhile we would be working on finding the best possible fix for this issue. 
You could go for one of the workarounds listed below:

Options:

- Install SunEC extension on your OpenJDK setup. Get *sunec.jar* and place it under `jre/lib/ext/` folder. You must also place *libsunec.so* in `jre/lib/amd64/` folder. 

OR

- Install Oracle JRE on your GoCD server and switch to that.
	For GoCD server running on Windows, this means updating the system level environment variable `GO_SERVER_JAVA_HOME` to oracle jre home
	For Linux based installations, update `/etc/default/go-server` to set `JAVA_HOME` to oracle jre home.
	For Others, set the value of system level environment variable `JAVA_HOME` to oracle jre home.

OR

- Install the same version of java as you have on your agent. Update java used by GoCD server to appropriate value as suggested in the previous option.

At this point restart GoCD Server. Trigger your affected pipeline to ensure the artifact got uploaded successfully. This should resolve the issue.
If the issue persists even after applying the suggested workaround, do write to the go-cd mailing list reporting the same.

### What caused this?
I cannot speak about this without getting into some technical details. 
 
As a part of upgrading to Rails v4, we also had to upgrade bouncycastle-bcprov library. Earlier versions of GoCD used bouncycastle-bcprov v1.40. With that the cipher suites accepted by GoCD server would always be one of `SSL_RSA_WITH_RC4_128_SHA`, `SSL_RSA_EXPORT_WITH_RC4_40_MD5` or `SSL_RSA_WITH_RC4_128_MD5`.
GoCD 14.3.0 has moved on to use org.bouncycastle-bcprov v1.47. Bouncycastle v1.46 brought in support for [ECC cipher suites](http://tools.ietf.org/html/rfc4492). ECC cipher suites are made available by SunEC extension which is *not* packaged along with OpenJDK7 by default.
GoCD server is run using Jetty which uses bouncycastle crypto package (i.e. bcprov) for the handshake. At this point, instead of agreeing on the expected SSL & RSA based ciphers, one of the ECC ciphers get picked during the agent/server handshake. Jetty6 (yes, we are still using this!) does not work well with the modern cipher suites and causes issues like the one mentioned above. To tackle such scenarios, all supported ciphers suites apart from the three mentioned above are excluded from Jetty. Since, ECC cipher suites were not available on the server, they did not get excluded. However if your JVM has ECC ciphers available (by default or after applying one of the workarounds), GoCD would ensure they get excluded and make Jetty happy.

Jetty upgrade has been on the cards for a while, and that would possibly help resolve this. But that is a much involved task and hence works better as a long term solution. We need to evaluate other options as well which could fix the issue in the short term. 

Many thanks to [Vladimir Bormotov](https://github.com/bormotov) for reporting this issue and allowing us to use his setup to gather the required debug information.

As always, you could write to [go-cd](https://groups.google.com/forum/#!forum/go-cd) and [go-cd-dev](https://groups.google.com/forum/#!forum/go-cd-dev) mailing lists if you have any ideas/feedback/questions.

