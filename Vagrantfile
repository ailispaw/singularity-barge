# A dummy plugin for Barge to set hostname and network correctly at the very first `vagrant up`
module VagrantPlugins
  module GuestLinux
    class Plugin < Vagrant.plugin("2")
      guest_capability("linux", "change_host_name") { Cap::ChangeHostName }
      guest_capability("linux", "configure_networks") { Cap::ConfigureNetworks }
    end
  end
end

Vagrant.configure(2) do |config|
  config.vm.define "singularity-barge"

  config.vm.box = "ailispaw/barge"
  config.vm.box_version = ">= 2.7.3"

  config.vm.synced_folder ".", "/vagrant"

  config.vm.provision :shell do |sh|
    sh.inline = <<-EOT
      pkg install file
      pkg install singularity
    EOT
  end
end
