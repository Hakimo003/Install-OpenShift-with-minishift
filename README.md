# Install OpenShift with minishift

It's a Vagrant project to an advanced installation of OpenShift Origin with Ansible

## Prerequisites:

 * Install VirtualBox : https://www.virtualbox.org/wiki/Downloads
 * Install Vagrant :  https://www.vagrantup.com/downloads.html
 * Install Vagrant plugin :
  ** vagrant-hostmanager https://github.com/devopsgroup-io/vagrant-hostmanager
  ** landrush https://github.com/vagrant-landrush/landrush
 * start landrush dns :
```
 $ vagrant landrush start
```

## Infra-Install

```
  $ vagrant up
```

### Configure NFS server:

* Noeud NFS
```
  $ vagrant ssh nfs
  $ service nfs start
  $ service rpcbind start
  $ mkdir -p /exports/volumes/pv{1..10} && mkdir -p /exports/volumes/registry
  $ chown nfsnobody:nfsnobody /exports/volumes
  $ chown nfsnobody:nfsnobody /exports/volumes/pv{1..10}
    && chown nfsnobody:nfsnobody /exports/volumes/registry
  $ vim /etc/exports.d/openshift-WESCALE.exports

        /exports/volumes/registry  *(rw,root_squash,no_wdelay)
        /exports/volumes/pv1  *(rw,root_squash,no_wdelay)
        /exports/volumes/pv2  *(rw,root_squash,no_wdelay)
        /exports/volumes/pv4  *(rw,root_squash,no_wdelay)
        /exports/volumes/pv3  *(rw,root_squash,no_wdelay)
        /exports/volumes/pv5  *(rw,root_squash,no_wdelay)
        /exports/volumes/pv6 *(rw,root_squash,no_wdelay)
        /exports/volumes/pv7 *(rw,root_squash,no_wdelay)
        /exports/volumes/pv8 *(rw,root_squash,no_wdelay)
        /exports/volumes/pv9 *(rw,root_squash,no_wdelay)

  $ exportfs -rv
```

* Noeud ADMIN :

```
  $ vagrant ssh admin
  $ su - (password password)
  $ cd /home/vagrant && git clone https://github.com/openshift/openshift-ansible
  $ cd /home/vagrant/openshift-ansible && git checkout origin/release-3.7
  $ cd /home/vagrant/ && ./deploy.sh (password password)
```

### Verify the install :

* Noeud MASTER :

```
  $ oc login -u system:admin --config=/etc/origin/master/admin.kubeconfig
  $ oc get nodes
```
