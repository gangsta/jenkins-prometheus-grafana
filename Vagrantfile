# -*- mode: ruby -*-
# vi: set ft=ruby :

$msg = <<MSG
------------------------------------------------------
Use Below URLS to access Jenkins, Prometheus, Grafana

LOGIN JENKINS: admin:admin
LOGIN GRAFANA: admin:admin

URLS:
 - Jenkins      - http://192.168.33.10:8080/
 - Prometheus   - http://192.168.33.10:9090/
 - Grafana      - http://192.168.33.10:3000/

------------------------------------------------------
MSG

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.define "vdjpg" do |node|
    node.vm.hostname = "vdjpg"
    node.vm.network "private_network", ip: "192.168.33.10"
    config.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 1
    end
    config.vm.provider "libvirt" do |vl|
      vl.memory = 2048
      vl.cpus = 1
    end
  end
  config.vm.provision "shell", inline: <<-SHELL
    echo ""
    echo "Installing base packages ...."
    echo ""
  	sudo yum install -y yum-utils device-mapper-persistent-data lvm2 >/dev/null 2>&1
    sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    echo ""
    echo "Installing docker packages ...."
    echo ""
    sudo yum install docker-ce git -y >/dev/null 2>&1
    sudo systemctl start docker
    echo ""
    echo "Downloading Docker Images ...."
    echo ""
    sudo docker run  --env JAVA_OPTS="-Djenkins.install.runSetupWizard=false" \
    -v /vagrant/jenkins/install.groovy:/usr/share/jenkins/ref/init.groovy.d/install.groovy \
    -v /vagrant/jenkins/jobs:/var/jenkins_home/jobs \
    -d --name jenkins -p 8080:8080 -p 50000:50000 jenkins/jenkins:2.164.1 >/dev/null 2>&1
    sudo docker run -d --name prometheus -p 9090:9090 prom/prometheus:v2.8.0 >/dev/null 2>&1
    sudo docker run -d -v /vagrant/provisioning/:/etc/grafana/provisioning --name grafana -p 3000:3000 grafana/grafana:6.0.2 >/dev/null 2>&1
    echo ""
    echo "Manage Docker Images ...."
    echo ""
    PROMID=$(sudo docker ps | grep prometheus | awk '{ print $1 }')
    sudo docker exec ${PROMID} sh -c 'printf "\n  - job_name: 'jenkins'\n    metrics_path: /prometheus\n    static_configs:\n    - targets: ['192.168.33.10:8080']\n" >> /etc/prometheus/prometheus.yml'
    sudo docker restart ${PROMID} >/dev/null 2>&1
    DOCKID=$(sudo docker ps | grep jenkins | awk '{ print $1 }')
    sudo docker exec ${DOCKID} sh -c '/usr/local/bin/install-plugins.sh prometheus' >/dev/null 2>&1
    sudo docker restart ${DOCKID} >/dev/null 2>&1
  SHELL
  config.vm.post_up_message = $msg
end
