# -*- mode: ruby -*-
# vi: set ft=ruby :

# configs
$box_os = "ubuntu/trusty64"
$ipaddress = "192.168.250.101"
$box_provider = :virtualbox


$setupscript = <<END
  wget -q -O - https://jenkins-ci.org/debian/jenkins-ci.org.key | sudo apt-key add -
  sudo sh -c 'echo deb http://pkg.jenkins-ci.org/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
  sudo apt-get update
  sudo apt-get install -y jenkins
  sudo apt-get autoremove -y
END


Vagrant.configure(2) do |config|
  config.vm.box = $box_os
  config.vm.provider $box_provider do |vb|
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
  end

# box
  config.vm.define "jenkins", primary: true do |jenkins|
    jenkins.vm.hostname = "j1"
    jenkins.vm.network :private_network, ip: $ipaddress
    jenkins.vm.provision :shell, inline: $setupscript
    jenkins.vm.post_up_message = "Go to #{$ipaddress}:8080 in your browser to complete configuration."
  end

  config.vm.provider $box_provider do |vb|
     vb.memory = "1024"
   end

end
