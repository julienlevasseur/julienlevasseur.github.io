
---
layout: post
title: Nomad Pack
description: A presentation of Nomad Pack
tags:
  - Nomad
  - Pack
  - Helm
  - Registry
---

# Nomad Pack

From Nomad Pack's project page:
> Nomad Pack is a templating and packaging tool used with [HashiCorp Nomad](https://www.nomadproject.io/).
> 
> Nomad Pack is used to:
> 
> - Easily deploy popular applications to Nomad
> - Re-use common patterns across internal applications
> - Find and share job specifications with the Nomad community

Basically, Nomad Pack is the equivalent of [Helm](https://helm.sh/) for Nomad.

It offers the concepts of Registries and Pack to manage applications with Nomad.

## Registry

A Nomad Pack Registry is a simple repository that stores Packs.
An example of registry is the [nomad-pack-community-registry](https://github.com/hashicorp/nomad-pack-community-registry/).

## Pack

A Nomad Pack is a set of templates and user defined variables which describes a Nomad job.

A Pack is comprised of:

- A `README.md` file containing a human-readable description of the pack, often including any -ependency * information.
- A `metadata.hcl` file containing information about the pack.
- A `variables.hcl` file that defines the variables in a pack.
- An optional `outputs.tpl` file that defines an output to be printed when a pack is deployed.
- A templates subdirectory containing the HCL templates used to render the jobspec.

And a `metadata.hcl` file that contains information about the pack, such as:
- "app {url}" - The HTTP(S) url to the homepage of the application to provide a quick reference to the - documentation and help pages.
- "pack {name}" - The name of the pack.
- "pack {description}" - A small overview of the application that is deployed by the pack.
- "pack {version}" - The version of the pack.
- "dependency {name}" - The dependencies that the pack has on other packs. Multiple dependencies can be - supplied.
- "dependency {source}" - The source URL for this dependency.

Here's a `metadata.hcl` example:
```hcl
app {
  url = "https://github.com/mikenomitch/hello_world_server"
}

pack {
  name = "hello_world"
  description = "This pack contains a single job that renders hello world, or a different greeting, to the screen."
  version = "0.3.2"
}
```

And final interesting thing to note: Templates are written using [Go Template](https://learn.hashicorp.com/tutorials/nomad/go-template-syntax) Syntax.

## Nomad Pack usage

As of November 2023, `nomad-pack` can be install from the [nightly build release](https://github.com/hashicorp/nomad-pack/releases/tag/nightly).

Once installed, you can validate that it's working fine:
```bash
$ nomad-pack
Welcome to Nomad Pack
Docs:
Version: v0.1.1-dev (7599a58)

Usage: nomad-pack [--version] [--help] [--autocomplete-(un)install] <command> [args]

Common commands
  plan         Dry-run a pack update to determine its effects
  render       Render the templates within a pack
  run          Run a new pack or update an existing pack
  destroy      Delete an existing pack
  info         Get information on a pack
  status       Get information on deployed packs

Other commands
  deps          Manage dependencies for pack.
  generate      Generate a sample nomad-pack registry, pack, or variable overrides file for a pack.
  list          List packs available in the local environment.
  registry      Add, delete, or list registries and packs in the local environment.
  stop          Stop a running pack
  version       Prints the version of Nomad Pack

```

This is nice, but without any Registry, we can't do much.

So, let's add the community registry:
```bash
$ nomad-pack registry add community-registry github.com/hashicorp/nomad-pack-community-registry
go-getter URL is github.com/hashicorp/nomad-pack-community-registry?depth=1
Registry successfully cloned at /Users/julien/Library/Caches/nomad/packs/nomad-pack-tmp
Processing pack entries at /Users/julien/Library/Caches/nomad/packs/nomad-pack-tmp
found pack entry alertmanager
Processing pack alertmanager@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/alertmanager@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/alertmanager@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:25.426991 +0000 UTC

found pack entry aws_ebs_csi
Processing pack aws_ebs_csi@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/aws_ebs_csi@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/aws_ebs_csi@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:25.657316 +0000 UTC

found pack entry aws_efs_csi
Processing pack aws_efs_csi@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/aws_efs_csi@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/aws_efs_csi@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:25.765715 +0000 UTC

found pack entry backstage
Processing pack backstage@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/backstage@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/backstage@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:25.919219 +0000 UTC

found pack entry boundary
Processing pack boundary@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/boundary@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/boundary@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:26.075292 +0000 UTC

found pack entry caddy
Processing pack caddy@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/caddy@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/caddy@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:26.205035 +0000 UTC

found pack entry ceph
Processing pack ceph@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/ceph@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/ceph@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:26.512265 +0000 UTC

found pack entry ceph_rbd_csi
Processing pack ceph_rbd_csi@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/ceph_rbd_csi@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/ceph_rbd_csi@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:26.775157 +0000 UTC

found pack entry chaotic_ngine
Processing pack chaotic_ngine@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/chaotic_ngine@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/chaotic_ngine@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:26.925311 +0000 UTC

found pack entry csi_openstack_cinder
Processing pack csi_openstack_cinder@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/csi_openstack_cinder@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/csi_openstack_cinder@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:27.10293 +0000 UTC

found pack entry ctfd
Processing pack ctfd@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/ctfd@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/ctfd@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:27.26639 +0000 UTC

found pack entry drone
Processing pack drone@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/drone@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/drone@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:27.387127 +0000 UTC

found pack entry faasd
Processing pack faasd@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/faasd@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/faasd@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:27.589333 +0000 UTC

found pack entry fabio
Processing pack fabio@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/fabio@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/fabio@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:27.73064 +0000 UTC

found pack entry grafana
Processing pack grafana@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/grafana@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/grafana@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:27.907119 +0000 UTC

found pack entry haproxy
Processing pack haproxy@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/haproxy@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/haproxy@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:28.072369 +0000 UTC

found pack entry hashicups
Processing pack hashicups@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/hashicups@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/hashicups@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:28.188947 +0000 UTC

found pack entry hello_world
Processing pack hello_world@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/hello_world@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/hello_world@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:28.334971 +0000 UTC

found pack entry influxdb
Processing pack influxdb@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/influxdb@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/influxdb@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:28.499258 +0000 UTC

found pack entry jaeger
Processing pack jaeger@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/jaeger@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/jaeger@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:28.653764 +0000 UTC

found pack entry jenkins
Processing pack jenkins@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/jenkins@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/jenkins@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:28.851195 +0000 UTC

found pack entry kibana
Processing pack kibana@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/kibana@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/kibana@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:29.056243 +0000 UTC

found pack entry loki
Processing pack loki@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/loki@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/loki@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:29.204561 +0000 UTC

found pack entry nextcloud
Processing pack nextcloud@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/nextcloud@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/nextcloud@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:29.352162 +0000 UTC

found pack entry nginx
Processing pack nginx@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/nginx@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/nginx@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:29.482206 +0000 UTC

found pack entry nomad_autoscaler
Processing pack nomad_autoscaler@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/nomad_autoscaler@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/nomad_autoscaler@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:29.619473 +0000 UTC

found pack entry nomad_ingress_nginx
Processing pack nomad_ingress_nginx@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/nomad_ingress_nginx@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/nomad_ingress_nginx@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:29.786746 +0000 UTC

found pack entry opentelemetry_collector
Processing pack opentelemetry_collector@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/opentelemetry_collector@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/opentelemetry_collector@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:30.060692 +0000 UTC

found pack entry outline
Processing pack outline@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/outline@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/outline@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:30.186383 +0000 UTC

found pack entry prometheus
Processing pack prometheus@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/prometheus@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/prometheus@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:30.341469 +0000 UTC

found pack entry prometheus_consul_exporter
Processing pack prometheus_consul_exporter@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/prometheus_consul_exporter@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/prometheus_consul_exporter@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:30.498987 +0000 UTC

found pack entry prometheus_node_exporter
Processing pack prometheus_node_exporter@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/prometheus_node_exporter@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/prometheus_node_exporter@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:30.631238 +0000 UTC

found pack entry prometheus_snmp_exporter
Processing pack prometheus_snmp_exporter@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/prometheus_snmp_exporter@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/prometheus_snmp_exporter@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:30.77235 +0000 UTC

found pack entry promtail
Processing pack promtail@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/promtail@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/promtail@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:30.910991 +0000 UTC

found pack entry rabbitmq
Processing pack rabbitmq@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/rabbitmq@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/rabbitmq@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:31.11904 +0000 UTC

found pack entry redis
Processing pack redis@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/redis@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/redis@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:31.295228 +0000 UTC

found pack entry simple_service
Processing pack simple_service@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/simple_service@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/simple_service@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:31.441228 +0000 UTC

found pack entry sonarqube
Processing pack sonarqube@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/sonarqube@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/sonarqube@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:31.585174 +0000 UTC

found pack entry tempo
Processing pack tempo@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/tempo@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/tempo@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:31.745235 +0000 UTC

found pack entry tfc_agent
Processing pack tfc_agent@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/tfc_agent@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/tfc_agent@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:31.917908 +0000 UTC

found pack entry traefik
Processing pack traefik@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/traefik@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/traefik@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:32.081226 +0000 UTC

found pack entry vector
Processing pack vector@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/vector@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/vector@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:32.185369 +0000 UTC

found pack entry wordpress
Processing pack wordpress@latest
Updating pack
Removing previous latest
Writing pack to /Users/julien/Library/Caches/nomad/packs/community-registry/latest/wordpress@latest
Loading cloned pack from /Users/julien/Library/Caches/nomad/packs/community-registry/latest/wordpress@latest
calculating SHA for latest
SHA 6bac78a905b7966ed93078cba1604fafe0fa2334 downloaded at UTC 2023-11-07 19:57:32.328345 +0000 UTC

  temp directory deleted
  Registry successfully added to cache.
    REGISTRY NAME    |  REF   | LOCAL REF |                    REGISTRY URL
---------------------+--------+-----------+-----------------------------------------------------
  community-registry | latest | 6bac78a9  | github.com/hashicorp/nomad-pack-community-registry
```

Each pack defines a set of variables that can be provided by the user. To get information on the pack and to see which variables can be passed in, run the info command:
```bash
$ nomad-pack info --registry=community-registry loki
Pack Name          loki
Description        Loki is a horizontally-scalable, highly-available, multi-tenant log aggregation system inspired by Prometheus.
Application URL    https://grafana.com/oss/loki/
Pack "loki" Variables:
	- "job_name" (string) - The name to use as the job name which overrides using the pack name.
	- "constraints" (list of object) - Constraints to apply to the entire job.
	- "region" (string) - The region where the job should be placed.
	- "version_tag" (string) - The docker image version. For options, see https://hub.docker.com/grafana/loki
	- "grpc_port" (number) - The Nomad client port that routes to the Loki.
	- "resources" (object) - The resource to assign to the Loki service task.
	- "rules_yaml" (string) - The Loki rules to pass to the task.
	- "datacenters" (list of string) - A list of datacenters in the region which are eligible for task placement.
	- "http_port" (number) - The Nomad client port that routes to the Loki.
	- "loki_yaml" (string) - The Loki configuration to pass to the task.
```

Values for these variables can be provided using the --var flag.
```bash
nomad-pack run hello_world --var message=hola
```

But, also via a variables file, which is more convenient especially for Packs with many variables:
```bash
$ cat pack-vars.hcl
message="bonjour"

$ nomad-pack run hello_world -f ./pack-vars.hcl
```

If you want to remove all of the resources deployed by a pack, run the destroy command with the pack name:
```bash
$ nomad-pack destroy hello_world
```

## Sources & Refs

- [https://github.com/hashicorp/nomad-pack](https://github.com/hashicorp/nomad-pack)
- [https://github.com/hashicorp/nomad-pack-community-registry](https://github.com/hashicorp/nomad-pack-community-registry)
- [https://learn.hashicorp.com/tutorials/nomad/go-template-syntax](https://learn.hashicorp.com/tutorials/nomad/go-template-syntax)
