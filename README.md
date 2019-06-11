# openshift-logging

## Prerequisites

1. REHL or CentOS
2. Docker installed
3. oc 3.11 binary installed https://www.okd.io
4. root user access
5. Python installed
6. Ansible installed

## Install Openshift_Logging

1. make a new directory called workspace
```console
root@bar:~$ mkdir workspace
```

2. go to the workspace dircetory
```console
root@bar:~$ cd workspace
```

3. create a new oc cluster
```console
root@bar:~/workspace$ oc cluster up --public-hostname=192.168.122.45.nip.io
```

4. verify a folder named 'openshift.local.clusterup' is created
```console
root@bar:~/workspace$ ls
openshift.local.clusterup
```

5. git clone openshift-ansible
```console
root@bar:~/workspace$ git clone git@github.com:openshift/openshift-ansible.git
```

6. Checkout 3.11 release
```console
root@bar:~/workspace$ cd openshift-ansible
root@bar:~/workspace/openshift-ansible$ git checkout release-3.11
```

7. download the install-logging inventory file
