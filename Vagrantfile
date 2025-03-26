MACHINES = {
  :node1 => {
    :box_name => "ubuntu/jammy64",
    :vm_name => "node1",
    :cpus => 2,
    :memory => 1024,
    :ip => "192.168.57.11",
  },
  :node2 => {
    :box_name => "ubuntu/jammy64",
    :vm_name => "node2",
    :cpus => 2,
    :memory => 1024,
    :ip => "192.168.57.12",
  },
  :barman => {
    :box_name => "ubuntu/jammy64",
    :vm_name => "barman",
    :cpus => 1,
    :memory => 1024,
    :ip => "192.168.57.13",
  },
}

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxconfig[:vm_name]
      box.vm.network "private_network", ip: boxconfig[:ip]
      box.vm.provider "virtualbox" do |v|
        v.memory = boxconfig[:memory]
        v.cpus = boxconfig[:cpus]
      end

      ssh_pub_key = File.readlines("./id_rsa/id_rsa.pub").first.strip
      box.vm.provision "shell", inline: <<-SHELL
        echo #{ssh_pub_key} >> ~vagrant/.ssh/authorized_keys
        echo #{ssh_pub_key} >> ~root/.ssh/authorized_keys
        sudo sed -i 's/#PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
        systemctl restart sshd
      SHELL
    end
  end
end