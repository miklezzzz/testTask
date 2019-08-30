#############################
Test task for IQ
############################

1. Use "vagrant up" for the win.

2. Use "ansible lb-2 -m shell -a 'sudo wget https://dl.min.io/client/mc/release/linux-amd64/mc -q -O /usr/sbin/mc; sudo chmod u+x /usr/sbin/mc; sudo chown vagrant:root /usr/sbin/mc; j=1; for i in $(ls /etc/default/minio*);do  mc config host add minio$j http://127.0.0.1 `sudo grep ACCESS $i | cut -d  = -f2` `sudo grep SECRET $i | cut -d = -f2`; mc admin user add minio$j userforread MegaPassword123; mc admin policy set minio$j readonly user=userforread; mc admin user add minio$j userforreadwrite MegasPassword456; mc admin policy set minio$j readwrite user=userforreadwrite; j=$((j+1)); done' -i dynamic_inventory.py" to create canned users.

3. Use "ansible localhost -c local -m shell -a 'mc config host add minio2 http://10.1.0.31:8082 userforreadwrite MegasPassword456'" to set mc host at the localhost.

4. Use "ansible localhost -c local -m shell -a 'mc mb minio2/testbucket; mc cp ./test.jpg minio2/testbucket'" to upload test.jpeg into Minio's bucket.
