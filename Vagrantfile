# -*- mode: ruby -*-
# vi: set ft=ruby et sw=4 sts=4 ts=4:

VAGRANTFILE_API_VERSION = "2"
ENV['ANSIBLE_CONFIG'] = "provisioning/ansible.cfg"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

    config.ssh.insert_key = false

    config.vm.define "mon" do |mon|
        mon.vm.hostname = "mon"
        mon.vm.box = "minimal/centos7"
        mon.vm.box_url = "minimal/centos7"
        mon.vm.network :forwarded_port, guest: 22, host: 2222, id: "ssh"
        mon.vm.network :forwarded_port, guest: 80, host: 1080, id: "http"
        mon.vm.network :forwarded_port, guest: 443, host: 1443, id: "http"
        mon.vm.network "private_network", ip: "192.168.111.12"
    end

    config.vm.define "app", primary: true do |app|
        app.vm.hostname = "app"
        app.vm.box = "minimal/centos7"
        app.vm.box_url = "minimal/centos7"
        app.vm.network :forwarded_port, guest: 22, host: 2221, id: "ssh"
        app.vm.network :forwarded_port, guest: 80, host: 8080, id: "http"
        app.vm.network "private_network", ip: "192.168.111.11"
    end

    config.vm.box_check_update = false

    config.vm.provider "virtualbox" do |vb, override|
        vb.customize ["modifyvm", :id, "--cpus", "2"]
        vb.customize ["modifyvm", :id, "--memory", "1024"]
        vb.customize ["modifyvm", :id, "--usb", "off"]
        config.vm.box = "minimal/centos7"
    end

    config.vm.provision "ansible" do |ansible|
        #ansible.verbose = "vvvv"
        ansible.playbook = "provisioning/playbook.yml"
    end
end
