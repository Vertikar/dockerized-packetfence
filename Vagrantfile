$provision = <<SCRIPT
sudo su
curl -sSL https://get.docker.com/ | sh
apt-get install -y python-pip python-dev
pip install glances
pip install docker-compose
echo 'Provision script finish.'
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.define "vm0" do |vm0|
    vm0.vm.box = "ubuntu-14.04"
    vm0.vm.network "private_network", ip: "192.168.40.4"
    vm0.vm.network "private_network", ip: "192.168.50.4"
    vm0.vm.network "private_network", ip: "192.168.55.4"
    vm0.vm.synced_folder "data/", "/data"
    vm0.vm.provider :virtualbox do |vb|
      vb.memory = 4096
      vb.cpus = 2
    end
    vm0.vm.provision  "shell", inline: $provision
  end

end
