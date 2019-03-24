# Jenkins Prometheus Grafana

To get started, perform a git clone on this repository. Make sure you have [Vagrant installed](https://docs.vagrantup.com/v2/installation/), [VirtualBox](https://www.virtualbox.org/wiki/Downloads) or [LibVirt](https://libvirt.org/).

The repository will install for you Jenkins 2.164.1, Prometheus v2.8.0 , Grafana 6.0.2 .

Start with Git Clone
```
git clone https://github.com/gangsta/jenkins-prometheus-grafana
cd jenkins-prometheus-grafana
vagrant up - provider virtualbox
or
vagrant up - provider libvirt
```

Once vagrant is done provisioning the VMs run `vagrant status` to confirm all instances are running:

## Visit the web UI by browsing to:

```bash
Jenkins    http://192.168.33.10:8080/
Prometheus http://192.168.33.10:9090/
Grafana    http://192.168.33.10:3000/

"For Login Use: admin:admin"
```

## Add Cool Grafana Dashboard:
Login to Grafana, and add Dashboard with ID: 9964


## If you want to login into Vagrant VM run:
```bash
vagrant ssh
```

## When you're done, you can shut down the cluster using:

```bash
vagrant destroy -f
```

### If you liked this post please give it a "star" and share it to spread the knowledge.
Repo Is created by Karen Har-yan
Read my Blog Post about this setup: https://medium.com/@haryan/grafana-cool-dashboard-for-monitoring-jenkins-with-prometheus-46c2c015b5a1
