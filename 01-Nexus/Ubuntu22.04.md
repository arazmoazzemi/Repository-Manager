
# Nexsus repository on ubuntu 22.04
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



# APT hosted : >>

sudo apt-get update
sudo apt-get install gpg
sudo gpg --gen-key

# For example >> 9890EBFCAE40AA9D9BD03ED9E734932B938F3E54

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




-----------------------------------------------------------
-------############# PYPI ###########----------------------
-----------------------------------------------------------
https://help.sonatype.com/repomanager3/nexus-repository-administration/formats/pypi-repositories#PyPIRepositories-BrowsingPyPIRepositoriesandSearchingPackages
https://pip.pypa.io/en/stable/topics/configuration/#location


apt-get install python3-pip
pip3 install --upgrade pip
python3 -m pip install --upgrade pip


-----------------------------------easy way---------------------------------------------------------------------------

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



