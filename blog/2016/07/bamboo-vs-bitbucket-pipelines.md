+++
date = "2016-07-23"
title = "Bamboo vs Bitbucket Pipelines"
math = "true"
categories = "test"
+++

Atlassian recently announced that their Software as a Service (SaaS) version of Bamboo, known as 'Bamboo Cloud', the Continuous Integration solution was to be discontinued, I assume as a result of poor sales (I have seen very little demand with clients vs. the on-premises version).

This came as a surprise, as it was only recently released (May 24, 2016) and when Bitbucket Pipelines was announced at AtlasCamp 2016.

it seemed as if this was to be offered as a replacement, however this is not the case, as it's feature set is very different to Bamboo and does not cater for organisations which require that their Intellectual Property be stored internally, in the same region as their business and managed with the companies Active Directory (AD).

It is worth noting the differences in the function sets between the different solutions:

| Feature                                   | Bamboo On-Premise | Bamboo Cloud (Discontinued) | Bitbucket Pipelines |
|-------------------------------------------|-------------------|-----------------------------|---------------------|
| Integration with Bitbucket Cloud          | ✅                | ✅                          | ✅                  |
| Integration with Bitbucket Server         | ✅                | ❌                          | ❌                  |
| Run Custom Remote Agents                  | ✅                | ✅                          | ❌                  |
| Run EC2 Remote Agents                     | ✅                | ✅                          | ❌                  |
| Access Local Resources on Company Network | ✅                | ❌                          | ❌                  |
| Control how/where data is stored          | ✅                | ❌                          | ❌                  |
| CVS/Perforce Repository Support           | ✅                | ❌                          | ❌                  |
| Active Directory/LDAP Support             | ✅                | ❌                          | ❌                  |

\*This is being worked on as part of the [SAML Implementation](https://jira.atlassian.com/browse/ID-80)

The difference in feature sets means that many companies choose to host a Bamboo On-Premises instance either within their network or hosted on a cloud provider such as Microsoft Azure or Amazon Web Services (AWS) and connected to their company Active Directory through a VPN or through Atlassian Crowd or another SSO solution.

As with many of the Atlassian applications, you can mix and match your Cloud and On Premises versions with Bamboo - leading to a hybrid Cloud which is commonplace, using the Cloud versions of Bitbucket and JIRA with an On Premises instance of Bamboo, which allows access to internal test/dev environments.

So will Bitbucket Pipelines match your development teams' needs?

Bitbucket Pipelines uses AWS-hosted Docker containers, hosted within Atlassian's infrastructure, with the following limits: 2GB RAM/2 hours. It is free and allows for tasks to be performed by reading a Docker Image (with .Dockerfile) from a private or public Docker repository (such as the Docker Hub) and then running commands such as builds/deploys through these using scripts.<

The Pipelines system also follows Atlassians' Add-on strategy and outsources the responsibility of providing extended services to third-party suppliers, this means that it is an extensible system, but you are reliant on using/buying services through another supplier on top of Atlassian.

The current feature set is as follows:

* Perform any tasks with Docker Containers available from Docker Repositories (Build and Deploy Tools available)
* Build a number of languages out of the box including Node.js, Java, Ruby, Python, PHP, Golang, Haskell, Perl, Clojure and Erlang
* Deploy to Amazon Web Services (S3, CodeDeploy, Elastic Beanstalk)
* Deploy to Read The Docs
* Deploy to NPM (private), Artifactory or Nexus

Broadly, if you are working on more recent development platforms and using Cloud services to host your continuously integrated/deployed applications and use Bitbucket Cloud, then Bitbucket Pipelines offers a turnkey solution to building and deploying.

Bitbucket Pipelines is currently free while in Beta, si if your team has not already used DevOps practices, it can give staff an opportunity to become more familiar with these tools, which turbocharge development. It is also possible to build Microsoft code (think C# or Azure projects), but this requires some extra work as Docker runs on Linux with Bitbucket Pipelines and Microsoft's support is a bit ropey - there is a [demonstration](https://azure.microsoft.com/en-gb/blog/continuous-integration-delivery-of-web-apps-from-atlassian-bitbucket/) which offloads the build to Kudu, the Azure SCM solution.