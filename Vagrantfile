manager_ip = "10.0.3.2"

Vagrant.configure(2) do |config|

  config.vm.define "manager" do |manager|
    manager.vm.hostname = "manager"
    manager.vm.box = "ubuntu/xenial64"
    manager.vm.network "private_network", ip: manager_ip

    manager.vm.provider :virtualbox do |v|
      v.cpus = 1
      v.memory = 512
    end
    manager.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning/manager.yml"
      ansible.extra_vars = {
          ansible_python_interpreter: "/usr/bin/python3",
          manager_ip: manager_ip
      }
    end
  end

  (1..2).each do |i|
    config.vm.define "worker-#{i}" do |worker|
      worker.vm.hostname = "worker-#{i}"
      worker.vm.box = "ubuntu/xenial64"
      worker.vm.network "private_network", ip: "10.0.3.#{i+2}"

      worker.vm.provider :virtualbox do |v|
        v.cpus = 1
        v.memory = 512
      end
      worker.vm.provision "ansible" do |ansible|
         ansible.playbook = "provisioning/worker.yml"
         ansible.extra_vars = {
             ansible_python_interpreter: "/usr/bin/python3",
             manager_ip: manager_ip
         }
       end
     end
  end
end
