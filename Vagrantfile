# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  #== Global config
  config.vm.provider "parallels" do |p, o|
    p.memory = "512"
  end

  config.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
  end

  ["vmware_fusion", "vmware_workstation"].each do |p|
    config.vm.provider p do |v|
      v.vmx["memsize"] = "512"
    end
  end

  config.vm.synced_folder ".", "/vagrant"

  #== Consul servers
  1.upto(3) do |i|
    config.vm.define "consul-server#{i}" do |server|
      private_ip = "192.168.121.#{i + 1}"

      server.vm.box = "puphpet/ubuntu1404-x64"
      server.vm.hostname = "consul-server#{i}"
      server.vm.network "private_network", ip: private_ip

      server.vm.provision "docker" do |d|
        d.run "cleung2010/consul",
          args: "--net host -v /vagrant/config:/etc/consul.d -p #{private_ip}:8300:8300 -p #{private_ip}:8301:8301 -p #{private_ip}:8301:8301/udp -p #{private_ip}:8302:8302 -p #{private_ip}:8302:8302/udp -p #{private_ip}:8400:8400 -p #{private_ip}:8500:8500 -p 172.17.42.1:53:8600/udp",
          cmd: "-config-dir /etc/consul.d -client #{private_ip} -advertise #{private_ip} -retry-join 192.168.121.2"
      end
    end
  end
end
