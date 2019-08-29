Vagrant.configure("2") do |config|

 config.vm.provider 'virtualbox' do |v|
   v.memory = 512
 end

# N for the number of nodes (lb/backends)
 N = 3
# MT for the number of Minio's tenants
 MT = 2
# SUBNET for the first three octets of the subnet (like 10.1.0)
 SUBNET = "10.1.0"
 (1..N).each do |i|
   config.vm.define "lb-#{i}" do |lb|
     lb.vm.box = "ubuntu/xenial64"
     lb.vm.hostname = "lb-#{i}"
     lb.vm.network :private_network, ip: "#{SUBNET}.1#{i}", netmask:"255.255.255.0"

   end
 end

 (1..N).each do |i|
   config.vm.define "back-#{i}" do |back|
     back.vm.box = "ubuntu/xenial64"
     back.vm.hostname = "back-#{i}"
     back.vm.network :private_network, ip: "#{SUBNET}.2#{i}", netmask:"255.255.255.0"

     if i == N
       back.vm.provision :ansible do |ansible|
         ansible.playbook = "init.yml"
         ansible.limit = "all"
         ansible.groups = {
         "backEnds" => ["back-[1:#{i}]"],
         "loadBalancers" => ["lb-[1:#{i}]"],
         }
         ansible.extra_vars = {
	   "host_subnet" => SUBNET,
           "host_count" => N,
           "minio_volume" => "/var/minio",
	   "minio_tenants" => MT,
           nginx_sites: { default: [ "listen 80", "server_name reddit", "location / { proxy_pass http://127.0.0.1:9292; }" ] } 
         }
       end
     end
   end
 end

end
