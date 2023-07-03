# Guide to Install Uptime Kuma into Oracle Linux
[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Donation](https://img.shields.io/static/v1?label=Donation&message=❤️&style=social)](https://github.com/soranoo/Donation)

This guide also works for other CentOS~

## ⚓ Requirements

- Oracle Cloud Infrastructure (OCI) account
- Basic knowledge of OCI
- Tool to connect to OCI instance (e.g. PuTTY)

## 🚀 Get started

1. Create a new OCI instance

   ### Basic Setup

   [![image](docs\imgs\basic-config.png)](docs\imgs\basic-config.png)

   Please feel free to adjust the setup to your needs.

   ### Network Configuration

   The instance needs to be reachable from the internet.

   You have to create a new virtual cloud network (VCN) with a public subnet and a public IP address if you don't have one yet. Just like the following:
   [![image](docs\imgs\networking-config.png)](docs\imgs\networking-config.png)

   ### SSH Key

   You need to create a new SSH key pair or use an existing one. You can find a guide [here](https://docs.cloud.oracle.com/en-us/iaas/Content/GSG/Tasks/creatingkeys.htm).

   In this guide I will use the key OCI created.
   [![image](docs\imgs\ssh-setup.png)](docs\imgs\ssh-setup.png)
   Please don't forget to download the private key and save it in a safe place. You will need it later.

   ### Create Instance

   After checking everything, click the "Create" button and wait for the instance to be created.

   ### Here is my setup (Reference Only)

   [![image](docs\imgs\my-setup.png)](docs\imgs\my-setup.png)

2. Setup Virtual Cloud Network(VCN) Security List

   ### Add Ingress Rules

   You need to add the following ingress rules to the security list of the VCN you created.
   | Stateless | Source Type | Source CIDR | IP Protocol | Source Port Range | Destination Port Range | Description |
   | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
   | No | CIDR | 0.0.0.0/0 | TCP | All | 22 | Allow SSH Connections |
   | No | CIDR | 0.0.0.0/0 | TCP | All | 3001 | Uptime Kuma Default Port |

   Result:
   [![image](docs\imgs\ingress-rules-setup.png)](docs\imgs\ingress-rules-setup.png)

3. Connect to the instance

   ### Connect to the instance via SSH

   You can use any SSH client you like. I will use CMD in this guide.

   ```bash
   ssh -i <path-to-private-key> opc@<public-ip-address>
   ```

   ### Install Docker

   Update the instance and install Docker.

   ```bash
   $ sudo yum update -y
   ```

   Install Docker

   ```bash
   $ sudo yum install -y yum-utils

   $ sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

   $ sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
   ```

   Start Docker

   ```bash
   $ sudo systemctl start docker
   ```

   ### Install Uptime Kuma

   Create a new directory for Uptime Kuma

   ```bash
   $ mkdir uptime-kuma
   ```

   Cd into the directory

   ```bash
   $ cd uptime-kuma
   ```

   Setup `docker-compose.yml`

   ```bash
   $ nano docker-compose.yml
   ```

   Copy and paste the https://github.com/louislam/uptime-kuma/blob/master/docker/docker-compose.yml into the file.

   Exit and save the file by pressing `Ctrl + X` and then `Y`.

   Start Uptime Kuma

   ```bash
   $ docker compose up -d
   ```

   The first time you start Uptime Kuma, it will take a while to download the images. After that, you can access Uptime Kuma via `http://<public-ip-address>:3001`.

## 👾 Helpful Commands

1. Reset Uptime Kmuma Password
   ```bash
   $ docker exec -it uptime-kuma npm run reset-password
   ```

## 📚 References

1. [Install Docker Engine on CentOS](https://docs.docker.com/engine/install/centos/)
2. [docker/docker-compose.yml](https://github.com/louislam/uptime-kuma/blob/master/docker/docker-compose.yml)
