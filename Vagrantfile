# vim: set ft=ruby :
disk_controller = 'IDE' # MacOS. This setting is OS dependent. Details https://github.com/hashicorp/vagrant/issues/8105

Vagrant.configure("2") do |config|
  # Определите хэш для конфигурации виртуальных машин
  MACHINES = {
    "rpm1" => {
      :box_name => "almalinux/9",
      :cpus => 1,
      :memory => 1024,
      :ip => "192.168.57.20",
      :hostname => "rpm1.local",
    }#,
#    "rpm2" => {
#      :box_name => "almalinux/9",
#      :cpus => 1,
#      :memory => 1024,
#      :ip => "192.168.57.30",
#      :hostname => "rpm2.local",
#    }
  }

  # Проходим по каждой записи в хэше
  MACHINES.each do |name, machine|
   config.vm.define name do |machine_config|
    machine_config.vm.network "private_network", ip: machine[:ip]
    machine_config.vm.box = machine[:box_name]
    machine_config.vm.box_version = machine[:box_version]
    machine_config.vm.host_name = machine[:hostname]
      machine_config.vm.provider "virtualbox" do |v|
        v.memory = machine[:memory]
        v.cpus = machine[:cpus]
      end
    machine_config.vm.provision "shell", inline: <<-SHELL
       sed -i 's/^PasswordAuthentication.*$/PasswordAuthentication yes/' /etc/ssh/sshd_config
       systemctl restart sshd.service 
       yum makecache
    SHELL
    end
end
end