load 'config'

Vagrant.configure("2") do |config|
  # Provision NODE_COUNT nodes
  (1..NODE_COUNT).each do |i|
    hostname = HOSTNAME_PREFIX+"-#{i}"
    config.vm.define hostname do |subconfig|
      # Use BOX_IMAGE
      subconfig.vm.box = BOX_IMAGE
      # Set hostname
      subconfig.vm.hostname = hostname
      # Create a private network
      subconfig.vm.network :private_network, ip: "10.0.0.#{i + 10}"
      subconfig.vm.provider :libvirt do |libvirt|
        # Add disks
        letter = "a"
        (1..DISK_COUNT).each do |j|
          libvirt.storage :file, :size => DISK_SIZE, :bus => 'sata', :device => "sd" + letter
          letter.next!
        end
        # Configure CPU and RAM
        libvirt.cpus = CPU
        libvirt.memory = RAM
      end
      # Configure hostnames in /etc/hosts
      (1..NODE_COUNT).each do |j|
         subhostname = HOSTNAME_PREFIX+"-#{j}"
         subconfig.vm.provision "shell", inline: <<-SHELL
           echo "10.0.0.#{j + 10} #{subhostname}" >> /etc/hosts
         SHELL
      end
    end
  end
  # Run on each VM
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ceph.yml"
  end
end
