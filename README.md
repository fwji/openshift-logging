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

7. download the install-logging inventory file to the openshift-ansible directory, update the ip address to your environment

8. Establish a link to the admin.kubeconfig that openshift-ansible expects
```console
root@bar:~/workspace/openshift-ansible$ ln -s /root/workspace/openshift.local.clusterup/kube-apiserver/admin.kubeconfig /etc/origin/master/admin.kubeconfig
```

9. Run ansible playbook to install EFK stack on openshift, this will take a few minutes to complete
```console
root@bar:~/workspace/openshift-ansible$ ansible-playbook -i install-logging.inventory playbooks/openshift-logging/config.yml -e openshift_logging_install_logging=true
```

10. Verify Installation
```console
root@bar:~/workspace/openshift-ansible$ ln -s /root/workspace/openshift.local.clusterup/kube-apiserver/admin.kubeconfig /etc/origin/master/admin.kubeconfig
oc get pods -n openshift-logging
NAME READY STATUS RESTARTS AGE
logging-curator-1-x5jbw 1/1 Running 0 1m
logging-es-data-master-cuyorw4k-1-8hjnf 2/2 Running 0 1m
logging-fluentd-5xwbl 1/1 Running 0 1m
logging-kibana-1-4cmgq 2/2 Running 0 2m
```

## Deploy application to openshift

1. Create an admin user
```console
root@bar:~/workspace/openshift-ansible$ oc login -u system:admin
root@bar:~/workspace/openshift-ansible$ oc adm policy add-cluster-role-to-user cluster-admin admin
```

2. Create a new project
```console
root@bar:~/workspace/openshift-ansible$ oc new-project myproject --description="My first project" --display-name="My Project"
```

3. Switch to myproject
```console
root@bar:~/workspace/openshift-ansible$ oc project myproject
```

4. Download the docker image from docker hub
```console
root@bar:~/workspace/openshift-ansible$ oc new-app "frankji/daytrader" --name daytrader
```

5. Verify the pod is up and running
```console
root@bar:~/workspace/openshift-ansible$ oc get pods
NAME                READY     STATUS    RESTARTS   AGE
daytrader-1-5nn6b   1/1       Running   0          3h
```

## Setup Kibana Dashboard

