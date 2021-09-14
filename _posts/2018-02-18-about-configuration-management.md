---
title: "About configuration management"
excerpt: "About configuration management"
last_modified_at: 2018-02-18T15:35:01-05:00
tags: 
  - configuration-management
  - docker
  - consul
  - vault
  - 12-factor-app
---

When we talk about the [3rd factor](https://12factor.net/config) of the 12 factors app, we have to consider the best approach to manage the configuration.

For few years the privileged approach was to use a Configuration Management system such as Chef, Ansible or Salt. It's a wonderfull solution for bare-metal and IaaS infrastructures but with containered application
the limit of these tools has been reached.

It make no sense to run a client software on an application container to manage the config (Chef, Salt) or maintain complex inventories on the developper's workstation and having an ssh agent running on an application container (Ansible).
The objective of dockerizing apps is to keep them the more isolated and autonomous as possible. To reach this objective, the configuration management approach must be updated.

2 solutions comes in this updates approach :

* The dynamic configuration management
* The builded artifact

## Dynamic Configuration Management

![image_00](https://github.com/julienlevasseur/jekyll-theme-basically-basic/blob/post-2018-02-18-about-configuration-management/assets/images/posts/2018-02-18-about-configuration-management/image_00.png?raw=true)

The dynamic configuration management is usefull for dev or test envs. It produce a container with application's code but consider the configuration as a runtime set of variables.

It provide more flexibility to update the configuration and dissociate completely the code and the config.
[envconsul](https://github.com/hashicorp/envconsul) or [consul-template](https://github.com/hashicorp/consul-template) are amazing tools to set the configuration of the app at the container start.

Notice that this flexibility in configuration management limit the traceability of the code/config association behavior.
Thats why for prototyping, development or tests, this solution bring benefits. A feature team can release a micro-service directly as an application artifact (docker container) and the QA and automation team can test this application with different set of configuration without rebuilding it.

The right set of configuration can be tagged and this revision put in the orchestration solution to keep the association of them.

## Builded artifact

![image_01](https://github.com/julienlevasseur/jekyll-theme-basically-basic/blob/post-2018-02-18-about-configuration-management/assets/images/posts/2018-02-18-about-configuration-management/image_01.png?raw=true)

The builded artifact is a container that embed the code, the configuration and is versionned for the association of both.

This is the most reliable solution for production envs and continuous delivery.

It's less flexible than the first solution, but produce reliable artifacts ready to use and autonomous (eg: it doesn't require a consul and a vault in the production infrastructure).

With this approach, we use a basic image to retrieve the code and associated configuration. The temporary container can be tested and builded of all required tests passed.

The build gerenate a tagged artefact easy to track, thus when a bug will be discovered, it will be easier to know which version is in question (and so what is the code / config revisions pair causing the issue) and forensic and quality insurance can be made in shorter terms than retrieving the correct code revision, rebuilding the config state and providing a piece of infrastructure to host them.

Off course, the artifact must be treated as a sensible piece of software as he certainly contain secrets. But privates registries can easily bring a solution to that concern.

An example of pipeline for this approach can be:

![pipeline_01](https://github.com/julienlevasseur/jekyll-theme-basically-basic/blob/post-2018-02-18-about-configuration-management/assets/images/posts/2018-02-18-about-configuration-management/pipeline_01.png?raw=true)