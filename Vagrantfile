# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
:inetRouter => {
        :box_name => "centos/7",
        #:public => {:ip => '10.10.10.1', :adapter => 1},
        :net => [
                   {ip: '192.168.255.1', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
                ]
  },
  :centralRouter => {
        :box_name => "centos/7",
        :net => [
                   {ip: '192.168.255.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
                   {ip: '192.168.0.1', adapter: 3, netmask: "255.255.255.240", virtualbox__intnet: "dir-net"},
                   {ip: '192.168.0.33', adapter: 4, netmask: "255.255.255.240", virtualbox__intnet: "hw-net"},
                   {ip: '192.168.0.65', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "mgt-net"},
                   {ip: '192.168.254.1', adapter: 6, netmask: "255.255.255.252", virtualbox__intnet: "o1-net"},
                   {ip: '192.168.253.1', adapter: 7, netmask: "255.255.255.252", virtualbox__intnet: "o2-net"},
                ]
  },
  
  :centralServer => {
        :box_name => "centos/7",
        :net => [
                   {ip: '192.168.0.2', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "dir-net"},
                   {adapter: 3, auto_config: false, virtualbox__intnet: true},
                   {adapter: 4, auto_config: false, virtualbox__intnet: true},
                ]
  },

  :office1Router => {
    :box_name => "centos/7",
    :net => [
               {ip: '192.168.254.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "o1-net"},
               {ip: '192.168.2.1', adapter: 3, netmask: "255.255.255.192", virtualbox__intnet: "dev-o1-net"},
               {ip: '192.168.2.65', adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "testservers-o1-net"},
               {ip: '192.168.2.129', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "managers-net"},
               {ip: '192.168.2.193', adapter: 6, netmask: "255.255.255.192", virtualbox__intnet: "hw-o1-net"}
            ]
  },

  :office1Server => {
    :box_name => "centos/7",
    :net => [
               {ip: '192.168.2.2', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "dev-o1-net"}
            ]
  },

  :office2Router => {
    :box_name => "centos/7",
    :net => [
              {ip: '192.168.253.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "o2-net"},
              {ip: '192.168.1.1', adapter: 3, netmask: "255.255.255.128", virtualbox__intnet: "dev-o2-net"},
              {ip: '192.168.1.129', adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "testservers-o2-net"},
              {ip: '192.168.1.193', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "hw-o2-net"}
            ]
  }, 
  
  :office2Server => {
    :box_name => "centos/7",
    :net => [
               {ip: '192.168.1.2', adapter: 2, netmask: "255.255.255.128", virtualbox__intnet: "dev-o2-net"}
            ]
  }
  
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s

        boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", ipconf
        end
        
        if boxconfig.key?(:public)
          box.vm.network "public_network", boxconfig[:public]
        end

        box.vm.provision "shell", inline: <<-SHELL
          mkdir -p ~root/.ssh
                cp ~vagrant/.ssh/auth* ~root/.ssh
        SHELL
        
        case boxname.to_s
        when "inetRouter"
		  box.vm.provision "ansible", run: "always", type: "ansible_local"  do |ansible|
			ansible.become = true
                        ansible.playbook = "inetRouter.yml"
                  end
#          box.vm.provision "shell", run: "always", inline: <<-SHELL
#            sysctl net.ipv4.conf.all.forwarding=1
#            iptables -t nat -A POSTROUTING ! -d 192.168.0.0/16 -o eth0 -j MASQUERADE
#            SHELL
        when "centralRouter"
                   box.vm.provision "ansible", run: "always", type: "ansible_local"  do |ansible|
                         ansible.become = true
                         ansible.galaxy_role_file = "requirements.yml"
                         ansible.galaxy_roles_path = "/etc/ansible/collections"
                         ansible.galaxy_command = "sudo ansible-galaxy collection install -r %{role_file} -p %{roles_path} --force" 
                         ansible.playbook = "centralRouter.yml"
                   end
#          box.vm.provision "shell", run: "always", inline: <<-SHELL
#            sysctl net.ipv4.conf.all.forwarding=1
#            echo "DEFROUTE=no" >> /etc/sysconfig/network-scripts/ifcfg-eth0 
#            echo "GATEWAY=192.168.255.1" >> /etc/sysconfig/network-scripts/ifcfg-eth1
#            systemctl restart network
#            SHELL
        when "centralServer"
                   box.vm.provision "ansible", run: "always", type: "ansible_local"  do |ansible|
                         ansible.become = true
                         ansible.galaxy_role_file = "requirements.yml"
                         ansible.galaxy_roles_path = "/etc/ansible/collections"
                         ansible.galaxy_command = "sudo ansible-galaxy collection install -r %{role_file} -p %{roles_path} --force"
                         ansible.playbook = "centralServer.yml"
                   end
#          box.vm.provision "shell", run: "always", inline: <<-SHELL
#            echo "DEFROUTE=no" >> /etc/sysconfig/network-scripts/ifcfg-eth0 
#            echo "GATEWAY=192.168.0.1" >> /etc/sysconfig/network-scripts/ifcfg-eth1
#            systemctl restart network
#            SHELL
#===================Office1	
			when "office1Router"
		  box.vm.provision "ansible", run: "always", type: "ansible_local"  do |ansible|
			ansible.become = true
			ansible.galaxy_role_file = "requirements.yml"
			ansible.galaxy_roles_path = "/etc/ansible/collections"
			ansible.galaxy_command = "sudo ansible-galaxy collection install -r %{role_file} -p %{roles_path} --force"
			ansible.playbook = "office1Router.yml"
		  end

			when "office1Server"
		  box.vm.provision "ansible", run: "always", type: "ansible_local"  do |ansible|
			ansible.become = true
			ansible.galaxy_role_file = "requirements.yml"
			ansible.galaxy_roles_path = "/etc/ansible/collections"
			ansible.galaxy_command = "sudo ansible-galaxy collection install -r %{role_file} -p %{roles_path} --force"
			ansible.playbook = "office1Server.yml"
		  end
			
			
#===================Office2			
			when "office2Router"
		  box.vm.provision "ansible", run: "always", type: "ansible_local"  do |ansible|
			ansible.become = true
			ansible.galaxy_role_file = "requirements.yml"
			ansible.galaxy_roles_path = "/etc/ansible/collections"
			ansible.galaxy_command = "sudo ansible-galaxy collection install -r %{role_file} -p %{roles_path} --force"
			ansible.playbook = "office2Router.yml"
		  end
			when "office2Server"
		  box.vm.provision "ansible", run: "always", type: "ansible_local"  do |ansible|
			ansible.become = true
			ansible.galaxy_role_file = "requirements.yml"
			ansible.galaxy_roles_path = "/etc/ansible/collections"
                        ansible.galaxy_command = "sudo ansible-galaxy collection install -r %{role_file} -p %{roles_path} --force"
			ansible.playbook = "office2Server.yml"
		  end
        end

      end

  end
  
  
end
