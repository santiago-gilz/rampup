$update = <<-SCRIPT
sudo dnf clean all
sudo rm -rf /var/cache/dnf
SCRIPT

Vagrant.configure("2") do |config|
    config.vm.box = "bento/centos-8.2"
    config.vm.provision "shell", inline: $update
    config.ssh.insert_key = false

    config.vm.define "api" do |machine|
        machine.vm.hostname = "api.movie-analyst.local"
        machine.vm.network "private_network", ip: "192.168.1.3"
        machine.vm.provider "virtualbox" do |v|
            v.name = "api"
        end
        machine.vm.provision "shell" do |s|
            s.path = "provision.sh"
            s.args   = "api"
        end
    end

    config.vm.define "ui" do |machine|
        machine.vm.hostname = "ui.movie-analyst.local"
        machine.vm.network "public_network", ip: "192.168.0.80"
        machine.vm.network "private_network", ip: "192.168.1.2"
        machine.vm.provider "virtualbox" do |v|
            v.name = "ui"
        end
        machine.vm.provision "shell" do |s|
            s.path = "provision.sh"
            s.args   = ["ui", "192.168.1.3"]
        end
        machine.vm.network "forwarded_port", guest: 3030, host: 8080
    end

    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 1
    end
end 
