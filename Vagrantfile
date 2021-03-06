$master_script = <<SCRIPT
#!/bin/bash

apt-get install curl -y
REPOCM=${REPOCM:-cm5}
CM_REPO_HOST=${CM_REPO_HOST:-archive.cloudera.com}
CM_MAJOR_VERSION=$(echo $REPOCM | sed -e 's/cm\\([0-9]\\).*/\\1/')
CM_VERSION=$(echo $REPOCM | sed -e 's/cm\\([0-9][0-9]*\\)/\\1/')
OS_CODENAME=$(lsb_release -sc)
OS_DISTID=$(lsb_release -si | tr '[A-Z]' '[a-z]')
if [ $CM_MAJOR_VERSION -ge 4 ]; then
  cat > /etc/apt/sources.list.d/cloudera-$REPOCM.list <<EOF
deb [arch=amd64] http://$CM_REPO_HOST/cm$CM_MAJOR_VERSION/$OS_DISTID/$OS_CODENAME/amd64/cm $OS_CODENAME-$REPOCM contrib
deb-src http://$CM_REPO_HOST/cm$CM_MAJOR_VERSION/$OS_DISTID/$OS_CODENAME/amd64/cm $OS_CODENAME-$REPOCM contrib
EOF
curl -s http://$CM_REPO_HOST/cm$CM_MAJOR_VERSION/$OS_DISTID/$OS_CODENAME/amd64/cm/archive.key > key
apt-key add key
rm key
fi
apt-get update
export DEBIAN_FRONTEND=noninteractive
apt-get -q -y --force-yes install oracle-j2sdk1.7 cloudera-manager-server-db cloudera-manager-server cloudera-manager-daemons
service cloudera-scm-server-db initdb
service cloudera-scm-server-db start
service cloudera-scm-server start
SCRIPT

$hosts_script = <<SCRIPT
cat > /etc/hosts <<EOF
127.0.0.1       localhost

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

EOF
SCRIPT

Vagrant.configure("2") do |config|

  # Define base image
  config.vm.box = "hashicorp/precise64"
  #config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  # Manage /etc/hosts on host and VMs
  config.hostmanager.enabled = false
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = true
  config.hostmanager.ignore_private_ip = false


  config.vm.define :master do |master|
    master.vm.network :public_network    #, ip: "10.211.55.100"
    master.vm.hostname = "vm-cluster-node1"
    master.vm.provision :shell, :inline => $hosts_script
    master.vm.provision :hostmanager
    master.vm.provision :shell, :inline => $master_script
    master.vm.provider :vmware_fusion do |v|
      v.name = "vm-cluster-node1"
      v.vmx["memsize"] = "4096"
      v.gui = false
      v.vmx["vhv.enable"] = "TRUE" #Enable nested hypervisors to run x64 OS
      v.vmx["ethernet0.generatedAddress"] = nil #If the vmx file contains an auto-generated MAC address, remove it
      v.vmx["ethernet0.addressType"] = "static" #specify the MAC is static
      v.vmx["ethernet0.connectionType"] = "nat" #specify the NAT network
      v.vmx["ethernet0.present"] = "TRUE" #enable the NIC for the OS
      v.vmx["ethernet0.address"] = "00:0c:29:ca:d4:a0" #the MAC address
    end

  end

  config.vm.define :slave1 do |slave1|
    slave1.vm.box = "hashicorp/precise64"
    slave1.vm.network :public_network #    ip: "10.211.55.101"
    slave1.vm.hostname = "vm-cluster-node2"
    slave1.vm.provision :shell, :inline => $hosts_script
    slave1.vm.provision :hostmanager
    slave1.vm.provider :vmware_fusion do |v|
      v.name = "vm-cluster-node2"
      v.vmx["memsize"] = "2048"
      v.gui = false
      v.vmx["vhv.enable"] = "TRUE" #Enable nested hypervisors to run x64 OS
      v.vmx["ethernet0.generatedAddress"] = nil #If the vmx file contains an auto-generated MAC address, remove it
      v.vmx["ethernet0.addressType"] = "static" #specify the MAC is static
      v.vmx["ethernet0.connectionType"] = "nat" #specify the NAT network
      v.vmx["ethernet0.present"] = "TRUE" #enable the NIC for the OS
      v.vmx["ethernet0.address"] = "00:0c:29:ca:d4:a1" #the MAC address
    end

  end

  config.vm.define :slave2 do |slave2|
    slave2.vm.box = "hashicorp/precise64"
    slave2.vm.network :public_network # ip: "10.211.55.102"
    slave2.vm.hostname = "vm-cluster-node3"
    slave2.vm.provision :shell, :inline => $hosts_script
    slave2.vm.provision :hostmanager
    slave2.vm.provider :vmware_fusion do |v|
      v.name = "vm-cluster-node3"
      v.vmx["memsize"] = "2048"
      v.gui = false
      v.vmx["vhv.enable"] = "TRUE" #Enable nested hypervisors to run x64 OS
      v.vmx["ethernet0.generatedAddress"] = nil #If the vmx file contains an auto-generated MAC address, remove it
      v.vmx["ethernet0.addressType"] = "static" #specify the MAC is static
      v.vmx["ethernet0.connectionType"] = "nat" #specify the NAT network
      v.vmx["ethernet0.present"] = "TRUE" #enable the NIC for the OS
      v.vmx["ethernet0.address"] = "00:0c:29:ca:d4:a2" #the MAC address
    end

  end

  config.vm.define :slave3 do |slave3|
    slave3.vm.box = "hashicorp/precise64"
    slave3.vm.network :public_network # ip: "10.211.55.103"
    slave3.vm.hostname = "vm-cluster-node4"
    slave3.vm.provision :shell, :inline => $hosts_script
    slave3.vm.provision :hostmanager
    slave3.vm.provider :vmware_fusion do |v|
      v.name = "vm-cluster-node4"
      v.vmx["memsize"] = "2048"
      v.gui = false
      v.vmx["vhv.enable"] = "TRUE" #Enable nested hypervisors to run x64 OS
      v.vmx["ethernet0.generatedAddress"] = nil #If the vmx file contains an auto-generated MAC address, remove it
      v.vmx["ethernet0.addressType"] = "static" #specify the MAC is static
      v.vmx["ethernet0.connectionType"] = "nat" #specify the NAT network
      v.vmx["ethernet0.present"] = "TRUE" #enable the NIC for the OS
      v.vmx["ethernet0.address"] = "00:0c:29:ca:d4:a3" #the MAC address
    end

  end

end
