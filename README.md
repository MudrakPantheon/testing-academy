# Testing-Academy enviroment

The purpose of this repository is to create a fully functional virtual enviroment:
 - running on Kubernetes Cluster,
 - with private docker registry,
 - Jenkins with self-update job triggered by change in git repository,
 - ingress with MetalLB(solution for local use of K8s),
 - bash script to automate the start-up process.


## You will need:

#### Hardware:

x | Recommended | Minimum
----------- | ----------- | ----------- 
Available RAM | 16 GB | 8 GB
CPUs | 8 cores | 4 cores

#### Software:

 - [Vagrant](https://www.vagrantup.com/downloads)
 - [Virtualbox](https://www.virtualbox.org/wiki/Downloads)

 #### Resources

 Here you can find some resources that should help you understand how tools, that I used, work.

 - [What is Kubernetes?](https://youtu.be/cC46cg5FFAM)
 - [What is Jenkins?](https://youtu.be/2w-_JOK96Uc?t=81) 
 - [What is Ansible?](https://youtu.be/fHO1X93e4WA)
 - [Ansible Vault](https://docs.ansible.com/ansible/latest/cli/ansible-vault.html)

## Configuration

### 1. Vagrantfile

Here you can edit the hardware requirements. On top of the Vagrantfile you can see couple of variables:

`_NODES` = number of nodes to create,

`_RAM` = amount of RAM in MB for each of the node,

`_CPUS` = number of CPU cores to attach to each node.

```yaml
# Image configuration - DO NOT CHANGE
# https://app.vagrantup.com/boledovicpantheon/boxes/testing-academy-22.04_LTS
IMAGE_NAME = "boledovicpantheon/testing-academy-22.04_LTS" 
IMAGE_VERSION = "0.1.0" 

# Worker config
WORKER_COUNT = 2 # Do not change unless you need to
WORKER_RAM = 2048
WORKER_CPUS = 2

# Master config
MASTER_COUNT = 1 # DO NOT CHANGE
MASTER_RAM = 8192
MASTER_CPUS = 4
```

The worker node serves as a slave - it will be executing almost all tasks.

The master node serves as a control node, thats why it needs more RAM and CPUs. 

If you changed the number of worker nodes, you also need to add an entry in `${project_dir}/ansible/inventory/hosts.yaml`. 

### 2. IP adresses

> You can change them, but is not recommended. 

These are automatically generated by Vagrant:

DNS | IP address
--- | ---
node-1 | 192.168.56.101
node-2 | 192.168.56.102
master-1 | 192.168.56.11 

These ones are automatically configured in K8s cluster, if you want to access them on local machine, follow steps below.

DNS | IP address | Port
--- | --- | ---
example.jenkins.com | 192.168.56.240 | 80
docker.registry.com | 192.168.56.11 | 5000

### 3. Passwords

As this project is for testing purposes only, we use the same username and password everywhere.

**Username:** *user*

**Password :** *password*

## Running the enviroment

> Start-up should take no longer than 30 minutes, but it depends on your hardware capacity and internet connection.

1. Clone repository to your local machine:

`git clone https://github.com/boledovicpantheon/testing-academy.git` 


2. From inside the repository on your terminal, run `vagrant up`

3. Add DNS entry to your local machine with IP `192.168.56.240` and DNS name `example.jenkins.com`.
This way you will be able to access Jenkins controller on your local machine. It runs on port 80, so no need to specify it.

#### Optional:

Private docker registry is accesible only inside cluster, if you want to access it from your local machine also, you need to add IP 
`192.168.56.11` and DNS name `docker.registry.com` to your DNS records.
Don't forget that docker registry runs on port 5000! 


