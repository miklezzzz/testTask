Vagrant.configure("2") do |config|

 config.vm.provider 'virtualbox' do |v|
   v.memory = 1024
   v.cpus = 3
 end

 (1..3).each do |i|
   config.vm.define "lb-#{i}" do |lb|
     lb.vm.box = "ubuntu/xenial64"
     lb.vm.hostname = "lb-#{i}"
     lb.vm.network :private_network, ip: "10.1.0.1#{i}", netmask:"255.255.255.0"

     lb.vm.provision "ansible" do |ansible|
       ansible.playbook = "playbooks/site.yml"
       ansible.groups = {
       "lb-#{i}" => ["loadBalancers"]
       }
     end
   end
 end

 (1..3).each do |i|
   config.vm.define "back-#{i}" do |back|
     back.vm.box = "ubuntu/xenial64"
     back.vm.hostname = "back-#{i}"
     back.vm.network :private_network, ip: "10.1.0.2#{i}", netmask:"255.255.255.0"

     back.vm.provision "ansible" do |ansible|
       ansible.playbook = "playbooks/site.yml"
       ansible.groups = {
       "back-#{i}" => ["backEnds"]
       }
     end
   end
 end

end
