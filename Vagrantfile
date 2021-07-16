Vagrant.configure("2") do |config|
  config.vm.box = "opensuse/Leap-15.3.x86_64"
  config.vm.network "private_network", ip: "192.168.33.11"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "8192"
    vb.cpus = 4  
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
  end
  config.vm.provision "shell", inline: <<-SHELL
    zypper update
    zypper --non-interactive in command-not-found nano tmux 
    zypper --non-interactive in gnu-netcat curl
    zypper --non-interactive in -t pattern apparmor
    echo "Installing K3s"
    curl -sfL https://get.k3s.io | sh -
    mkdir -p $HOME/.kube
    sudo cp -i /etc/rancher/k3s/k3s.yaml $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config
    export KUBECONFIG=$HOME/.kube/config
  SHELL
end