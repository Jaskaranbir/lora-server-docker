# -*- mode: ruby -*-
# vi: set ft=ruby :

plugin_dependencies = [
  "vagrant-docker-compose",
  "vagrant-host-shell",
  "vagrant-vbguest"
]

needsRestart = false

# Install plugins if required
plugin_dependencies.each do |plugin_name|
  unless Vagrant.has_plugin? plugin_name
    system("vagrant plugin install #{plugin_name}")
    needsRestart = true
    puts "#{plugin_name} installed"
  end
end

# Restart vagrant if new plugins were installed
if needsRestart === true
  exec "vagrant #{ARGV.join(' ')}"
end

Vagrant.configure(2) do |config|
  config.vm.define :loravm do |loravm|
    loravm.vm.hostname = "lora"
    loravm.vm.box = "bento/ubuntu-16.04"

    loravm.vm.provider :virtualbox do |vb|
      vb.name = "lora_vm"
      vb.gui = false
      vb.memory = "1024"
      vb.cpus = 2

      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
	    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end

	loravm.vm.network :forwarded_port,
        guest: 8080,
        host: 8080,
        auto_correct: true

    # Run as non-login shell, sourcing it to /etc/profile instead of /root/.profile
    # Due to clashing configurations for vagrant and base box.
    # See: https://github.com/mitchellh/vagrant/issues/1673#issuecomment-28288042
    loravm.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

    # Automatically run during vagrant-vbguest install
    # loravm.vm.provision :shell, inline: "apt-get update"

    loravm.vm.provision :docker
    loravm.vm.provision :docker_compose,
        compose_version: "1.21.0",
        project_name: "lora",
        yml: ["/vagrant/docker-compose.yml"],
        # options: "--verbose",
        run: "always",
        # Comment out lines below to prevent rebuilding
        # Docker containers at every boot
        command_options: { build: "--force-rm" },
        rebuild: true

    loravm.vm.provision :shell,
        path: "scripts/deploy-wait_message.sh",
        run: "always",
        privileged: false

      # Automatically set current-dir to /vagrant on vagrant ssh
    config.vm.provision :shell,
        inline: "echo 'cd /vagrant' >> /home/vagrant/.bashrc"

    # ================> Post-Success-Deployment Message <=================
    loravm.vm.provision :host_shell,
        inline: "scripts/post-deploy_message.sh",
        run: "always"
  end
end
