Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"  # Ubuntu 24.04 LTS
  config.vm.hostname = "ubuntu24"
  
  # Network configuration
  config.vm.network "private_network", ip: "192.168.56.10"
  
  # Shared folder - current directory to /vagrant in VM
  config.vm.synced_folder ".", "/vagrant"
  
  # VM settings
  config.vm.provider "virtualbox" do |vb|
    vb.name = "Ubuntu24-SSH"
    vb.memory = "2048"
    vb.cpus = 2
  end
  
  # Enable password authentication for root
  config.vm.provision "shell", inline: <<-SHELL
    # Set root password
    echo 'root:root123' | chpasswd
    
    # Enable root SSH login
    sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
    sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
    
    # Restart SSH service
    systemctl restart ssh
    
    echo "Ubuntu 24.04 VM is ready!"
    echo "SSH: ssh root@192.168.56.10 (password: root123)"
  SHELL
end