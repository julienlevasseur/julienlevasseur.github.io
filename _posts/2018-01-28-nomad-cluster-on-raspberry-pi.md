---
title: "Nomad cluster on Raspberry Pi"
excerpt: "Nomad cluster on Raspberry Pi"
last_modified_at: 2018-01-28T17:21:01-05:00
tags: 
  - nomad
  - raspberry pi
  - consul
---

Hypriot image reference : https://github.com/hypriot/image-builder-rpi/releases

Consul installation:

```bash
curl -O https://releases.hashicorp.com/consul/1.0.3/consul_1.0.3_linux_arm.zip

unzip consul_1.0.3_linux_arm.zip

sudo mv consul /usr/local/bin/
sudo mkdir /var/opt/consul

sudo curl -o /lib/systemd/system/consul.service https://raw.githubusercontent.com/julienlevasseur/Config-files/master/consul/consul.service
sudo systemctl enable consul
sudo systemctl start consul
```

Nomad installation:
```bash
curl -O https://releases.hashicorp.com/nomad/0.7.1/nomad_0.7.1_linux_arm.zip

sudo apt-get update
sudo apt-get install golang unzip

unzip nomad
sudo mv nomad /usr/local/bin

rm nomad_0.7.1_linux_arm64.zip


sudo mkdir /etc/nomad/
sudo curl -o /etc/nomad/server.hcl https://raw.githubusercontent.com/julienlevasseur/Config-files/master/nomad/server.hcl
sudo curl -o /lib/systemd/system/nomad.service https://raw.githubusercontent.com/julienlevasseur/Config-files/master/nomad/nomad.service
sudo mkdir /var/opt/nomad
sudo systemctl enable nomad
sudo systemctl start nomad
```
