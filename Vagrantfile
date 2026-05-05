# -*- mode: ruby -*-
# vi: set ft=ruby :
#
# Vagrantfile voor Apple Intel (x86_64) + VirtualBox
# Bevat: Ubuntu 26.04, RockyLinux 10, RHEL 10
#
# Vereisten:
#   - Vagrant >= 2.3.x  (https://www.vagrantup.com)
#   - VirtualBox >= 7.0 (https://www.virtualbox.org)
#
# Gebruik:
#   vagrant up                     # alle VMs starten
#   vagrant up ubuntu2604          # alleen Ubuntu starten
#   vagrant up rockylinux10        # alleen RockyLinux starten
#   vagrant up rhel10              # alleen RHEL starten
#   vagrant ssh ubuntu2604         # inloggen op Ubuntu
#   vagrant halt                   # alle VMs stoppen
#   vagrant destroy -f             # alle VMs verwijderen
#
# NOOT RHEL 10:
#   RHEL vereist een geldig Red Hat account + subscription.
#   Stel de volgende omgevingsvariabelen in voor registratie:
#     export RHEL_USERNAME="jouw-redhat-email"
#     export RHEL_PASSWORD="jouw-redhat-wachtwoord"
#   Of pas de provisioning sectie hieronder aan.
#   Alternatief: gebruik de AlmaLinux 10 box (RHEL-compatibel, gratis).
#
# NOOT Ubuntu 26.04:
#   Als de box nog niet beschikbaar is op Vagrant Cloud, gebruik dan:
#     ubuntu/noble64  (Ubuntu 24.04 LTS) als tijdelijk alternatief.

Vagrant.configure("2") do |config|

  # ─────────────────────────────────────────────
  # Globale instellingen
  # ─────────────────────────────────────────────
  config.vm.box_check_update = true

  # Gedeelde map uitschakelen als VirtualBox Guest Additions ontbreken
  config.vm.synced_folder ".", "/vagrant", disabled: false

  # ─────────────────────────────────────────────
  # VM 1: Ubuntu 26.04 LTS
  # ─────────────────────────────────────────────
  config.vm.define "ubuntu2604" do |ubuntu|
    ubuntu.vm.hostname = "ubuntu2604"

    # Probeer Ubuntu 26.04 box; als niet beschikbaar gebruik 24.04
    # Controleer beschikbaarheid op: https://app.vagrantup.com/ubuntu
    ubuntu.vm.box = "ubuntu/oracular64"
    # ubuntu.vm.box = "ubuntu/noble64"   # <- fallback: Ubuntu 24.04 LTS

    ubuntu.vm.network "private_network", ip: "192.168.56.10"

    ubuntu.vm.provider "virtualbox" do |vb|
      vb.name   = "ubuntu-2604"
      vb.memory = 2048
      vb.cpus   = 2
      vb.gui    = false

      # Optioneel: schijfgrootte aanpassen (vereist vagrant-disksize plugin)
      # ubuntu.disksize.size = "20GB"
    end

    ubuntu.vm.provision "shell", inline: <<-SHELL
      echo "=== Ubuntu 26.04 provisioning ==="
      apt-get update -y
      apt-get upgrade -y
      apt-get install -y \
        curl \
        wget \
        git \
        vim \
        net-tools \
        htop \
        unzip
      echo "=== Ubuntu provisioning klaar ==="
    SHELL
  end

  # ─────────────────────────────────────────────
  # VM 2: Rocky Linux 10
  # ─────────────────────────────────────────────
  config.vm.define "rockylinux10" do |rocky|
    rocky.vm.hostname = "rockylinux10"

    # Controleer beschikbaarheid op: https://app.vagrantup.com/rockylinux
    rocky.vm.box = "rockylinux/10"

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
      dnf install -y \
        curl \
        wget \
        git \
        vim \
        net-tools \
        htop \
        unzip \
        epel-release
      echo "=== Rocky Linux provisioning klaar ==="
    SHELL
  end

  # ─────────────────────────────────────────────
  # VM 3: RHEL 10
  # ─────────────────────────────────────────────
  # OPTIE A: Officiële RHEL box (vereist Red Hat account)
  #   Registreer op: https://developers.redhat.com (gratis developer sub)
  #   Voeg toe aan Vagrant: vagrant login (met Red Hat credentials)
  #   Box naam: redhat/rhel-10 (controleer op Vagrant Cloud)
  #
  # OPTIE B: AlmaLinux 10 (gratis RHEL-compatibel alternatief, aanbevolen)
  #   Verwijder commentaar bij almalinux regel hieronder.
  # ─────────────────────────────────────────────
  config.vm.define "rhel10" do |rhel|
    rhel.vm.hostname = "rhel10"

    # OPTIE A: Officiële RHEL 10 box
    rhel.vm.box = "generic/rhel10"
    # rhel.vm.box = "redhat/rhel-10"   # alternatieve naam

    # OPTIE B: AlmaLinux 10 (aanbevolen als je geen RHEL licentie hebt)
    # rhel.vm.box = "almalinux/10"

    rhel.vm.network "private_network", ip: "192.168.56.12"

    rhel.vm.provider "virtualbox" do |vb|
      vb.name   = "rhel-10"
      vb.memory = 2048
      vb.cpus   = 2
      vb.gui    = false
    end

    rhel.vm.provision "shell", env: {
      "RHEL_USERNAME" => ENV["RHEL_USERNAME"] || "",
      "RHEL_PASSWORD" => ENV["RHEL_PASSWORD"] || ""
    }, inline: <<-SHELL
      echo "=== RHEL 10 provisioning ==="

      # Registreer bij Red Hat Subscription Manager (alleen voor echte RHEL)
      if [ -n "$RHEL_USERNAME" ] && [ -n "$RHEL_PASSWORD" ]; then
        echo "Registreren bij Red Hat..."
        subscription-manager register \
          --username="$RHEL_USERNAME" \
          --password="$RHEL_PASSWORD" \
          --auto-attach || echo "Registratie mislukt of al geregistreerd"
      else
        echo "RHEL_USERNAME / RHEL_PASSWORD niet ingesteld, registratie overgeslagen."
        echo "Stel in: export RHEL_USERNAME=... && export RHEL_PASSWORD=..."
      fi

      dnf update -y
      dnf install -y \
        curl \
        wget \
        git \
        vim \
        net-tools \
        htop \
        unzip
      echo "=== RHEL 10 provisioning klaar ==="
    SHELL
  end

end
