# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Vagrantfile voor Apple Intel (x86_64) + VirtualBox
# Bevat: Ubuntu 24.04 LTS, RockyLinux 10, AlmaLinux 10 (RHEL 10-compatibel)
#
# ┌─────────────────────────────────────────────────────────┐
# │  WAAROM DEZE BOX KEUZES?                                │
# │                                                         │
# │  Ubuntu 26.04  → geen VirtualBox box beschikbaar.      │
# │                  Canonical stopt met Vagrant boxes.     │
# │                  bento/ubuntu-24.04 is de beste keuze. │
# │                                                         │
# │  RHEL 10       → geen publieke Vagrant Cloud box.      │
# │                  Red Hat distribueert RHEL niet vrij.  │
# │                  AlmaLinux 10 is 1:1 RHEL-compatibel  │
# │                  en heeft een officiële VirtualBox box. │
# └─────────────────────────────────────────────────────────┘
#
# Vereisten:
#   - Vagrant >= 2.3.x  (https://www.vagrantup.com)
#   - VirtualBox >= 7.0 (https://www.virtualbox.org)
#
# EERSTE KEER: voeg boxes toe met de amd64 architecture flag:
#   vagrant box add bento/ubuntu-24.04   --provider=virtualbox --architecture=amd64
#   vagrant box add bento/rockylinux-10  --provider=virtualbox --architecture=amd64
#   vagrant box add almalinux/10         --provider=virtualbox --architecture=amd64
#
# Gebruik:
#   vagrant up                    # alle VMs starten
#   vagrant up ubuntu2404         # alleen Ubuntu starten
#   vagrant up rockylinux10       # alleen RockyLinux starten
#   vagrant up almalinux10        # alleen AlmaLinux (RHEL-compat) starten
#   vagrant ssh ubuntu2404        # inloggen op Ubuntu
#   vagrant halt                  # alle VMs stoppen
#   vagrant destroy -f            # alle VMs verwijderen

Vagrant.configure("2") do |config|

  # ─────────────────────────────────────────────
  # Globale instellingen
  # ─────────────────────────────────────────────
  config.vm.box_check_update = true
  config.vm.synced_folder ".", "/vagrant", disabled: false

  # ─────────────────────────────────────────────
  # VM 1: Ubuntu 24.04 LTS
  # Box: bento/ubuntu-24.04 (aanbevolen door HashiCorp, ondersteunt VirtualBox amd64)
  # ─────────────────────────────────────────────
  config.vm.define "ubuntu2404" do |ubuntu|
    ubuntu.vm.hostname = "ubuntu2404"
    ubuntu.vm.box = "bento/ubuntu-24.04"

    ubuntu.vm.network "private_network", ip: "192.168.56.10"

    ubuntu.vm.provider "virtualbox" do |vb|
      vb.name   = "ubuntu-2404"
      vb.memory = 2048
      vb.cpus   = 2
      vb.gui    = false
    end

    ubuntu.vm.provision "shell", inline: <<-SHELL
      echo "=== Ubuntu 24.04 provisioning ==="
      apt-get update -y
      apt-get upgrade -y
      apt-get install -y curl wget git vim net-tools htop unzip
      echo "=== Ubuntu provisioning klaar ==="
    SHELL
  end

  # ─────────────────────────────────────────────
  # VM 2: Rocky Linux 10
  # Box: bento/rockylinux-10 (officiële bento box, VirtualBox amd64)
  # ─────────────────────────────────────────────
  config.vm.define "rockylinux10" do |rocky|
    rocky.vm.hostname = "rockylinux10"
    rocky.vm.box = "bento/rockylinux-10"

    rocky.vm.network "private_network", ip: "192.168.56.11"

    rocky.vm.provider "virtualbox" do |vb|
      vb.name   = "rockylinux-10"
      vb.memory = 2048
      vb.cpus   = 2
      vb.gui    = false
    end

    rocky.vm.provision "shell", inline: <<-SHELL
      echo "=== Rocky Linux 10 provisioning ==="
      dnf update -y
      dnf install -y curl wget git vim net-tools htop unzip epel-release
      echo "=== Rocky Linux provisioning klaar ==="
    SHELL
  end

  # ─────────────────────────────────────────────
  # VM 3: AlmaLinux 10 (RHEL 10 compatibel)
  # Box: almalinux/10 (officieel gepubliceerd door het AlmaLinux project)
  # Ondersteunt VirtualBox amd64 volledig.
  # AlmaLinux is 1:1 binair compatibel met RHEL 10 — zelfde packages,
  # zelfde kernel, zelfde gedrag. Enige verschil: gratis en zonder licentie.
  # ─────────────────────────────────────────────
  config.vm.define "almalinux10" do |alma|
    alma.vm.hostname = "almalinux10"
    alma.vm.box = "almalinux/10"

    alma.vm.network "private_network", ip: "192.168.56.12"

    alma.vm.provider "virtualbox" do |vb|
      vb.name   = "almalinux-10"
      vb.memory = 2048
      vb.cpus   = 2
      vb.gui    = false
    end

    alma.vm.provision "shell", inline: <<-SHELL
      echo "=== AlmaLinux 10 (RHEL-compatibel) provisioning ==="
      dnf update -y
      dnf install -y curl wget git vim net-tools htop unzip
      echo "=== AlmaLinux provisioning klaar ==="
    SHELL
  end

end
