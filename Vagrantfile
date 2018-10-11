# A dummy plugin for Barge to set hostname and network correctly at the very first `vagrant up`
module VagrantPlugins
  module GuestLinux
    class Plugin < Vagrant.plugin("2")
      guest_capability("linux", "change_host_name") { Cap::ChangeHostName }
      guest_capability("linux", "configure_networks") { Cap::ConfigureNetworks }
    end
  end
end

SINGULARITY_VERSION = "v3.0.0"

Vagrant.configure(2) do |config|
  config.vm.define "singularity-barge"

  config.vm.box = "ailispaw/barge"
  config.vm.box_version = ">= 2.8.0"

  config.vm.synced_folder ".", "/vagrant"

  config.vm.provision :shell, run: "always" do |sh|
    sh.inline = <<-EOT
      sudo pkg install git
      sudo pkg install make
      sudo pkg install squashfs
    EOT
  end

  config.vm.provision :shell do |sh|
    sh.privileged = false
    sh.inline = <<-EOT
      set -e

      git config --global http.sslCAinfo /etc/ssl/certs/ca-certificates.crt

      docker build --tag ailispaw/singularity /vagrant/singularity

      rm -rf ${HOME}/go/src/github.com/sylabs
      mkdir -p ${HOME}/go/src/github.com/sylabs
      cd ${HOME}/go/src/github.com/sylabs
      git clone https://github.com/sylabs/singularity.git
      cd singularity
      git checkout #{SINGULARITY_VERSION}

      docker run --rm -v $(pwd):$(pwd) ailispaw/singularity
    EOT
  end

  config.vm.provision :shell, run: "always" do |sh|
    sh.privileged = false
    sh.inline = <<-EOT
      cd ${HOME}/go/src/github.com/sylabs/singularity
      sudo make -C builddir install
    EOT
  end
end
