Vagrant.configure("2") do |config|
  config.vm.box = "spox/ubuntu-arm"
  config.vm.provision "shell", inline: <<-'SHELL'
    set -e
    export DEBIAN_FRONTEND=noninteractive
    # Wait for unattended-upgrades to release the lock
    while lsof /var/lib/dpkg/lock-frontend >/dev/null 2>&1; do
      echo "waiting for dpkg lock..."; sleep 5
    done
    apt-get update -y -qq
    apt-get install -y -qq ansible python3-apt --option "Dpkg::Options::=--force-confold"
  SHELL
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "provision.yml"
  end
  config.vm.define "master" do |master|
  config.vm.network "forwarded_port", guest: 80, host: 8080
    master.vm.hostname = "puppetmaster.local"
    master.vm.network "private_network", ip: "192.168.56.10"
    master.vm.provider "vmware_desktop"
  end
end
