---
layout: post
title: CIS hardened K8S cluster deployment
description: 
tags: [CIS Kubernetes]
image:
---

## Architecture

![architecture.png](https://raw.githubusercontent.com/julienlevasseur/jekyll-theme-basically-basic/master/images/2018-05-24-cis-hardened-k8s-cluster-deployment/architecture.png)

> clusterSpec file support to be added

## Generation of the Terraform code from kops

```bash
kops create cluster --name=k8s-hardened.example.com --state=s3://k8s-hardened-tfstate --dns-zone=example.com --zones us-east-1b --zones us-east-1c --zones us-east-1d --image=ami-9462dbeb --out=. --target=terraform
I0523 13:53:12.577820   16311 create_cluster.go:1318] Using SSH public key: ~/.ssh/id_rsa.pub
I0523 13:53:13.332480   16311 create_cluster.go:472] Inferred --cloud=aws from zone "us-east-1b"
I0523 13:53:13.636571   16311 subnets.go:184] Assigned CIDR 172.20.32.0/19 to subnet us-east-1b
I0523 13:53:13.636595   16311 subnets.go:184] Assigned CIDR 172.20.64.0/19 to subnet us-east-1c
I0523 13:53:13.636608   16311 subnets.go:184] Assigned CIDR 172.20.96.0/19 to subnet us-east-1d
I0523 13:53:16.908772   16311 executor.go:91] Tasks: 0 done / 77 total; 31 can run
I0523 13:53:16.909985   16311 dnszone.go:242] Check for existing route53 zone to re-use with name "example.com"
I0523 13:53:17.015071   16311 dnszone.go:249] Existing zone "example.com." found; will configure TF to reuse
I0523 13:53:17.851508   16311 vfs_castore.go:731] Issuing new certificate: "ca"
I0523 13:53:18.908899   16311 vfs_castore.go:731] Issuing new certificate: "apiserver-aggregator-ca"
I0523 13:53:19.976941   16311 executor.go:91] Tasks: 31 done / 77 total; 26 can run
I0523 13:53:21.051639   16311 vfs_castore.go:731] Issuing new certificate: "kubelet"
I0523 13:53:21.362105   16311 vfs_castore.go:731] Issuing new certificate: "kube-proxy"
I0523 13:53:21.461494   16311 vfs_castore.go:731] Issuing new certificate: "kops"
I0523 13:53:21.821614   16311 vfs_castore.go:731] Issuing new certificate: "kubecfg"
I0523 13:53:21.952314   16311 vfs_castore.go:731] Issuing new certificate: "kubelet-api"
I0523 13:53:22.348586   16311 vfs_castore.go:731] Issuing new certificate: "apiserver-proxy-client"
I0523 13:53:22.361188   16311 vfs_castore.go:731] Issuing new certificate: "apiserver-aggregator"
I0523 13:53:22.371679   16311 vfs_castore.go:731] Issuing new certificate: "kube-scheduler"
I0523 13:53:22.516060   16311 vfs_castore.go:731] Issuing new certificate: "kube-controller-manager"
I0523 13:53:22.525978   16311 vfs_castore.go:731] Issuing new certificate: "master"
I0523 13:53:23.239116   16311 executor.go:91] Tasks: 57 done / 77 total; 18 can run
I0523 13:53:23.567858   16311 executor.go:91] Tasks: 75 done / 77 total; 2 can run
I0523 13:53:23.568794   16311 executor.go:91] Tasks: 77 done / 77 total; 0 can run
I0523 13:53:23.575171   16311 target.go:292] Terraform output is in .
I0523 13:53:23.896716   16311 update_cluster.go:291] Exporting kubecfg for cluster
kops has set your kubectl context to k8s-hardened.example.com

Terraform output has been placed into .
Run these commands to apply the configuration:
   cd .
   terraform plan
   terraform apply

Suggestions:
 * validate cluster: kops validate cluster
 * list nodes: kubectl get nodes --show-labels
 * ssh to the master: ssh -i ~/.ssh/id_rsa admin@api.k8s-hardened.example.com
 * the admin user is specific to Debian. If not using Debian please use the appropriate user based on your OS.
 * read about installing addons at: https://github.com/kubernetes/kops/blob/master/docs/addons.md.
```

A `kubernetes.tf` file has been created :

```bash
ls -l 
total 44
-rw-rw-r-- 1 user user   525 May 23 13:50 clusterSpec.yaml
drwxr-xr-x 2 user user  4096 May 23 13:53 data
-rw-r--r-- 1 user user 19250 May 23 13:53 kubernetes.tf

```

Let's create this k8s cluster !

## Kubernetes cluster terraformation

```bash
terraform plan

(...)

Plan: 39 to add, 0 to change, 0 to destroy.
```

```bash
terraform apply

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

(...)

Plan: 39 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

Apply complete! Resources: 39 added, 0 changed, 0 destroyed.

Outputs:

cluster_name = k8s-hardened.example.com
master_security_group_ids = [
    sg-xxxxxxxx
]
masters_role_arn = arn:aws:iam::xxxxxxxxxxxx:role/masters.k8s-hardened.example.com
masters_role_name = masters.k8s-hardened.example.com
node_security_group_ids = [
    sg-2e771b66
]
node_subnet_ids = [
    subnet-xxxxxxxx,
    subnet-xxxxxxxx,
    subnet-xxxxxxxx
]
nodes_role_arn = arn:aws:iam::xxxxxxxxxxxx:role/nodes.k8s-hardened.example.com
nodes_role_name = nodes.k8s-hardened.example.com
region = us-east-1
vpc_id = vpc-xxxxxxxx

```

Kubernetes dashboard installation :

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
```

DNS cluster access :

Get the master public ip :
```bash
aws ec2 describe-instances --filters=Name=instance-id,Values=i-xxxxxxxxxxxxxxxxx|grep PublicIpAddress|cut -d ':' -f2|grep -oE "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b"
xx.XX.xx.XX
```

Create a R53 recordset :

```hcl
data "aws_route53_zone" "selected" {
  name         = "example.com."
  private_zone = false
}

resource "aws_route53_record" "master" {
  zone_id = "${data.aws_route53_zone.selected.zone_id}"
  name    = "api.k8s-hardened.${data.aws_route53_zone.selected.name}"
  type    = "A"
  ttl     = "60"
  records = ["xx.XX.xx.XX"]
}
```

```bash
terraform apply
data.aws_route53_zone.selected: Refreshing state...

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + aws_route53_record.master
      id:                 <computed>
      allow_overwrite:    "true"
      fqdn:               <computed>
      name:               "api.k8s-hardened.example.com"
      records.#:          "1"
      records.4074151582: "xx.XX.xx.XX"
      ttl:                "60"
      type:               "A"
      zone_id:            "xxxxxxxxxxxxxx"


Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

aws_route53_record.master: Creating...
  allow_overwrite:    "" => "true"
  fqdn:               "" => "<computed>"
  name:               "" => "api.k8s-hardened.example.com"
  records.#:          "" => "1"
  records.4074151582: "" => "xx.XX.xx.XX"
  ttl:                "" => "60"
  type:               "" => "A"
  zone_id:            "" => "xxxxxxxxxxxxxx"
aws_route53_record.master: Still creating... (10s elapsed)
aws_route53_record.master: Still creating... (20s elapsed)
aws_route53_record.master: Still creating... (30s elapsed)
aws_route53_record.master: Still creating... (40s elapsed)
aws_route53_record.master: Creation complete after 46s (ID: xxxxxxxxxxxxxx_api.k8s-hardened.example.com._A)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

```

Validate the kubernetes cluster :

```bash
kops validate cluster --state=s3://k8s-hardened-tfstate
Using cluster from kubectl context: k8s-hardened.example.com

Validating cluster k8s-hardened.example.com

INSTANCE GROUPS
NAME      ROLE  MACHINETYPE MIN MAX SUBNETS
master-us-east-1b Master  m3.medium 1 1 us-east-1b
nodes     Node  t2.medium 2 2 us-east-1b,us-east-1c,us-east-1d

NODE STATUS
NAME        ROLE  READY
ip-172-20-127-213.ec2.internal  node  True
ip-172-20-59-172.ec2.internal master  True
ip-172-20-78-155.ec2.internal node  True

Your cluster k8s-hardened.example.com is ready
```