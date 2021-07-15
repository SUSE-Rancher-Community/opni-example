Vagrant.configure("2") do |config|
  config.vm.box = "opensuse/Leap-15.3.x86_64"
  config.vm.network "private_network", ip: "192.168.33.11"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 4  
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
  end
  config.vm.provision "shell", inline: <<-SHELL
    zypper update
    zypper --non-interactive in command-not-found nano tmux 
    zypper --non-interactive in gnu-netcat curl
    echo "Installing K3s"
    curl -sfL https://get.k3s.io | sh -
  SHELL
end