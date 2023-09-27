
# Nexsus repository on ubuntu 22.04
----

*Install Nexus Repository manager with docker*

### Note: *Ubuntu EC2 up and running with at least t2.medium(4GB RAM), 2GB will not work*

- Set a HostName for Nexus Machine:
```bash
sudo hostnamectl set-hostname Nexus
```
- Update system:
```bash
sudo apt update -y && apt-get upgrade -y
```

- Install Docker-Compose:
```bash
sudo apt install docker
sudo apt install docker-compose -y
```

- Add current user to Docker group:
```bash
sudo usermod -aG docker $USER
```

- Create docker-compose.yml:
```bash
sudo nano docker-compose.yml 

# (Copy the below code high-lighted in yellow color)

version: "3"
services:
  nexus:
    image: sonatype/nexus3
    restart: always
    volumes:
      - "nexus-data:/sonatype-work"
    ports:
      - "8081:8081"
volumes:
  nexus-data: {}
```

- Now execute the compose file using Docker compose command to start Nexus Container:
```bash
sudo docker-compose up -d 
# -d means detached mode
```

- Make sure Nexus 3 is up and running
```bash
sudo docker-compose logs --follow
```

- Once you see the message, that's it. Nexus 3 is been setup successfully. Now press Control C and enter to come out of the above screen.

- How to get Nexus admin password?
- Now access Nexus UI by going to browser and enter public dns name with port 8081
- Now to go to browser --> http://change to_nexus_publicdns_name:8081

- We need to login to Nexus docker container to get Nexus admin password.

- Identify Docker container name
```bash
sudo docker ps
```

# Get admin password by executing below command
sudo docker exec -it it_nexus_1 cat /nexus-data/admin.password

# OR
sudo docker exec -it it_nexus_1 bash
cat /nexus-data/admin.password

# Please follow below steps for integrating Nexus 3 with Jenkins
https://www.cidevops.com/2018/06/jenkins-nexus-integration-how-to.html

#How to stop nexus container
sudo docker-compose down
----
```
### Official ubuntu repository

```
cat /etc/apt/sources.list

Ubuntu 22.04 LTS (Jammy Jellyfish) complete sources.list
sources.list
deb http://archive.ubuntu.com/ubuntu/ jammy main restricted universe multiverse
# deb-src http://archive.ubuntu.com/ubuntu/ jammy main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ jammy-updates main restricted universe multiverse
# deb-src http://archive.ubuntu.com/ubuntu/ jammy-updates main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse
# deb-src http://archive.ubuntu.com/ubuntu/ jammy-security main restricted universe multiverse

deb http://archive.ubuntu.com/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src http://archive.ubuntu.com/ubuntu/ jammy-backports main restricted universe multiverse

deb http://archive.canonical.com/ubuntu/ jammy partner
# deb-src http://archive.canonical.com/ubuntu/ jammy partner
```
----
### Add apt proxy on nexus server:

```
# Example:

# Distributian name
- jammy

# Add on remote storage section:
- http://archive.ubuntu.com/ubuntu/

# Export:
- http://192.168.200.11:8081/repository/apt-proxy/

```
----

```
# APT hosted : 

sudo apt-get update
sudo apt-get install gpg
sudo gpg --gen-key

# For example >>> 9890EBFCAE40AA9D9BD03ED9E734932B938F3E54

sudo gpg --list-keys
cd <path to the folder to import the key pair>
sudo gpg --armor --output public.gpg.key --export <gpg key Id>
sudo gpg --armor --output private.gpg.key --export-secret-key <gpg key Id>



# Define Name e.g. apt-hosted
# Define the Distribution e.g. bionic
# Put the private pgp key into Signing Key field, as described above
# Put the passphrase for the private signing key into the Passphrase field if you have defined one
# Pick a Blob store for Storage

apt-get update
apt-get install gnupg
apt-key add <full folder path in the container>/public.gpg.key

# Deploying Packages to Apt Hosted Repositories

curl -u "admin:admin123" -H "Content-Type: multipart/form-data" --data-binary "@./test.deb" "http://localhost:8081/repository/apt-hosted/"
```
----

# Python PYPI

```
https://help.sonatype.com/repomanager3/nexus-repository-administration/formats/pypi-repositories#PyPIRepositories-BrowsingPyPIRepositoriesandSearchingPackages
https://pip.pypa.io/en/stable/topics/configuration/#location


apt-get install python3-pip
pip3 install --upgrade pip
python3 -m pip install --upgrade pip

# Easy way 

pip install --index-url http://192.168.200.11:8081/repository/pypi-proxy/simple flask
pip install --index-url http://192.168.200.11:8081/repository/pypi-proxy/simple -v --trusted-host 192.168.200.11 flask


----------To build and upload this package's source distribution (sdist) to testpypi:-------------------------------------------------------

nano setup.cfg  # update the version number and package name
python3 -m pip install --user twine
python3 setup.py sdist
twine upload --repository testpypi dist/*
pip3 install the_name_of_your_test_package -i https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple


------------------------------------------------------------------------------------------------------------------------
pip config list -v

mkdir /etc/xdg/pip/
cd /etc/xdg/pip/ && nano /etc/xdg/pip/pip.conf

[global]
index-url = http://192.168.200.11:8081/repository/pypi-proxy/simple
trusted-host = 192.168.200.11



pip search example-package

--------------------------------------------------------------------------------------------------------------------------
```


