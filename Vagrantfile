load 'config'

Vagrant.configure("2") do |config|
  # Provision NODE_COUNT nodes
  (1..NODE_COUNT).each do |i|
    hostname = HOSTNAME_PREFIX+"#{i}"
    config.vm.define hostname do |subconfig|
      # Use BOX_IMAGE
      subconfig.vm.box = BOX_IMAGE
      # Set hostname
      subconfig.vm.hostname = hostname
      # Configure libvirt provider
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
      # Run ansible once each VM are up
      if i == NODE_COUNT
        subconfig.vm.provision :ansible do |ansible|
          # Disable default limit to connect to all the machines
          ansible.playbook = "00-install.yml"
          ansible.limit = "all"
          #ansible.verbose = "v"
        end
        subconfig.vm.provision :ansible do |ansible|
          ansible.groups = { 
            "leader" => [HOSTNAME_PREFIX + "1"],
            "followers" => [HOSTNAME_PREFIX + "[2:#{i}]"]
          }
          ansible.playbook = "01-cluster.yml"
          ansible.limit = "all"
          #ansible.verbose = "v"
        end
      end
    end
  end
end
