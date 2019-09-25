# A dummy plugin for Barge to set hostname and network correctly at the very first `vagrant up`
module VagrantPlugins
  module GuestLinux
    class Plugin < Vagrant.plugin("2")
      guest_capability("linux", "change_host_name") { Cap::ChangeHostName }
      guest_capability("linux", "configure_networks") { Cap::ConfigureNetworks }
    end
  end
end

SINGULARITY_VERSION = "v3.4.1"

Vagrant.configure(2) do |config|
  config.vm.define "singularity-barge"

  config.vm.box = "ailispaw/barge"
  config.vm.box_version = ">= 2.8.0"

  config.vm.synced_folder ".", "/vagrant"

  config.vm.provision :shell, run: "always" do |sh|
    sh.inline = <<-EOT
      pkg install git
      pkg install make
      pkg install squashfs
    EOT
  end

  config.vm.provision :shell do |sh|
    sh.privileged = false
    sh.inline = <<-EOT
      set -e

      git config --global http.sslCAinfo /etc/ssl/certs/ca-certificates.crt

      docker build --tag ailispaw/singularity:builder /vagrant/singularity

      rm -rf ${HOME}/go/src/github.com/sylabs
      mkdir -p ${HOME}/go/src/github.com/sylabs
      cd ${HOME}/go/src/github.com/sylabs
      git clone https://github.com/sylabs/singularity.git
      cd singularity
      git checkout #{SINGULARITY_VERSION}

      docker run --rm -v $(pwd):$(pwd) ailispaw/singularity:builder

      # Build a Barge Package for singularity
      source /etc/os-release
      PKG_DIR=/opt/pkg/${VERSION}
      sudo mkdir -p ${PKG_DIR}
      TMP_DIR=/tmp/singularity
      mkdir -p ${TMP_DIR}

      cd ${HOME}/go/src/github.com/sylabs/singularity
      sudo make DESTDIR=${TMP_DIR} -C builddir install
      sudo busybox tar -zc -f ${PKG_DIR}/barge-pkg-singularity-${VERSION}.tar.gz -C ${TMP_DIR} .
      sudo rm -rf ${TMP_DIR}
    EOT
  end

  config.vm.provision :shell, run: "always" do |sh|
    sh.inline = <<-EOT
      pkg install singularity
    EOT
  end
end
