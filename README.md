# dockerized-packetfence
PacketFence running inside a Docker container

## Image details

This image is based on the [official ubuntu 12.04 (precise) docker hub image](https://hub.docker.com/_/ubuntu/).

The recipe install packetfence, its dependencies and supervisor.

## Runtime

The container will run __supervisord__ as main process in order to manage the execution of several services needed by packetfence.

This may sound like breaking the [Docker best practices](https://docs.docker.com/articles/dockerfile_best-practices/#run-only-one-process-per-container), but in this case the complete packetfence service must be considered as a unique service (although we know that is composed of other microservices). Packetfence interacts tightly with several processes and he wants to have the control, so it is not possible to have them in separate containers.

Mysql is the only service that might be separated, but at the moment it's included to keep the Packetfence configuration simply.  In future releases it will be in a separate container and with data volumes.


## Deploy

Docker-compose takes care of the deploy process.

Once the container has been built, docker-compose runs it with this custom options:

- __privileged__: packetfence requires several privileges, like namespaces management. Because of this, the container has to be executed as privileged.

- __host network__: the container will be launched with full access to the host network stack. This is another requisite for packetfence.

- __kernel modules as volume__: at any point, packetfence accesses to /lib/modules/<linux-version>-generic. So it has to be accessible in the container.

The _launch command__ for the container is the __supervisor__ daemon. __Supervisor__ will take care of running packetfence and mysqld.

## Ok, how do I run it?

In `data/packetfence`, execute:

`docker-compose up`

# Extra: Vagrant machine

I used a vagrant environment to test this software. I decided to include the Vagrantfile to let anybody recreate the environment and play with packetfence.

## Requirements

- vagrant
- virtualbox

## Instructions to create VM:

Download base box

`vagrant box add ubuntu-14.04 https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box`

Start the machine

`vagrant up`

The first time, vagrant will provision the machine following the provision script embedded in the Vagrantfile.

## Network

Vagrant will create 3 virtual private networks. The virtual machine will be connected to them with the following IPs: `eth1 -> 192.168.40.4`, `eth2 -> 192.168.50.4`, `eth3 -> 192.168.55.4`.

When configuring packetfence, you will be able to select which interface acts as _inline_, which as _management_, etc.

## Run docker container with packetfence

In the virtual machine, go to `/data/packetfence` and execute `docker-compose up`
