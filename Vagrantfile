IMAGE_NAME = "ubuntu/focal64"
N = 1

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    provisioner = Vagrant::Util::Platform.windows? ? :ansible_local : :ansible

    config.vm.provider "virtualbox" do |v|
        v.memory = 6024
        v.cpus = 2
    end
      
    (1..N).each do |i|
        config.vm.define "zoo#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "192.168.50.#{i * 10}"
            node.vm.hostname = "zoo#{i}"
            node.vm.provision provisioner  do |ansible|
                ansible.playbook = "setup/zk-playbook.yaml"
                ansible.extra_vars = {
                    node_ip: "192.168.50.#{i * 10}",
                    node_id: "#{i}",
                    ansible_python_interpreter:"/usr/bin/python3",
                    nodes: N
                }
            end
        end
	end

end

