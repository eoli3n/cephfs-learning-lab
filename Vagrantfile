BOX_IMAGE = "generic/debian10"
NODE_COUNT = 3
RAM = 1024
CPU = 1
NODE_PREFIX = "ceph"

Vagrant.configure("2") do |config|
  # Provision NODE_COUNT nodes
  (1..NODE_COUNT).each do |i|
    HOSTNAME = NODE_PREFIX+"-#{i}"
    config.vm.define HOSTNAME do |subconfig|
      # Use BOX_IMAGE
      subconfig.vm.box = BOX_IMAGE
      # Set hostname
      subconfig.vm.hostname = HOSTNAME 
      # Create a private network
      subconfig.vm.network :private_network, ip: "10.0.0.#{i + 10}"
      subconfig.vm.provider :libvirt do |libvirt|
        # Add disks
        libvirt.storage :file, :size => '1G', :device => 'vdb'
        libvirt.storage :file, :size => '1G', :device => 'vdc'
        libvirt.storage :file, :size => '1G', :device => 'vdd'
        # Configure CPU and RAM
        libvirt.cpus = CPU
        libvirt.memory = RAM
      end
      # Configure hostnames in /etc/hosts
      (0..NODE_COUNT).each do |j|
         SUBHOSTNAME = NODE_PREFIX+"-#{j}"
         subconfig.vm.provision "shell", inline: <<-SHELL
           echo "10.0.0.#{j + 10} #{SUBHOSTNAME}" >> /etc/hosts
         SHELL
      end
    end
  end
  # Run on each VM
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ceph.yml"
  end
end
