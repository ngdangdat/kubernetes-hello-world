Vagrant.configure("2") do |config|
  config.vm.synced_folder ".", "/vagrant", type: "nfs", nfs_udp: false
  $num_instances = 2
  # curl https://discovery.etcd.io/new?size=3
  $etcd_cluster = "node1=http://172.17.8.101:2380"
  (1..$num_instances).each do |i|
    config.vm.define "node#{i}" do |node|
      node.vm.box = "centos/7"
      node.vm.hostname = "node#{i}"
      ip = "172.17.8.#{i+100}"
      node.vm.network "private_network", ip: ip, type: "dhcp"
      node.vm.provider "virtualbox" do |vb|
        vb.memory = "3072"
        vb.cpus = 2
        vb.name = "node#{i}"
      end
	end
  end
end
