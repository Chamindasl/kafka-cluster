# Kafka Multi Node Cluster on Ubuntu Focal64

## Objective
Setup a full blown multi node kafka cluster on Ubuntu Focal64 (20.04 LTS) in one click. Should be worked on Linux, Mac and Windows.

## Prerequisites
* Vagrant
* VirtualBox (or similar) as VM provider
* Ansible (Linux/Mac) or guest_ansible vagrant plugin (Widows) 
* Its 6GB * 3 = 18 GB is recommended. 2GB * 3 would be enough in most cases. 

## Setup
### Checkout or download the code
```
git clone https://github.com/Chamindasl/kafka-cluster.git
cd kafka-cluster
```

### Spin up the cluster
```
vagrant up
``` 

### Login to nodes
Password login is enabled on all 3 servers, or vagrant ssh 
```
ssh vagrant@zoo1
#pwd is vagrant 

or 

vargant ssh zoo1
```


### Verify
```
vagrant@zoo1:~$ systemctl status confluent*
● confluent-zookeeper.service - Apache Kafka - ZooKeeper
     Loaded: loaded (/lib/systemd/system/confluent-zookeeper.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2021-03-05 17:21:14 UTC; 7h ago
       Docs: http://docs.confluent.io/
   Main PID: 616 (java)
      Tasks: 49 (limit: 6951)
     Memory: 122.5M
     CGroup: /system.slice/confluent-zookeeper.service
             └─616 java -Xmx512M -Xms512M -server -XX:+UseG1GC -XX:MaxGCPauseMillis=20 -XX:InitiatingHeapOccupancyPercent=35 -XX:+ExplicitGCInvokesConcurrent -XX:MaxInlineLevel=>

Warning: some journal files were not opened due to insufficient permissions.
● confluent-kafka.service - Apache Kafka - broker
     Loaded: loaded (/lib/systemd/system/confluent-kafka.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2021-03-05 17:21:14 UTC; 7h ago
       Docs: http://docs.confluent.io/
   Main PID: 615 (java)
      Tasks: 71 (limit: 6951)
     Memory: 384.6M
     CGroup: /system.slice/confluent-kafka.service
             └─615 java -Xmx1G -Xms1G -server -XX:+UseG1GC -XX:MaxGCPauseMillis=20 -XX:InitiatingHeapOccupancyPercent=35 -XX:+ExplicitGCInvokesConcurrent -XX:MaxInlineLevel=15 ->

vagrant@zoo1:~$ kafka-topics --list --bootstrap-server zoo1:9092
```

## Configuration
Consider following configurations in Vagrantfile

### Change Image, VM Name or number of VMs
```
IMAGE_NAME = "ubuntu/focal64"
VM_NAME = "zoo"
N = 3
```

### CPU, Memory modifications
```
config.vm.provider "virtualbox" do |v|
    v.memory = 6024
    v.cpus = 2
end
```   


## Troubleshooting
Restart the zookeeper and kafka
```
sudo systemctl restart  confluent-zookeeper confluent-kafka
```