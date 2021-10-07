# OKD installation
1. create compute engine with
> - CentOS 7
> - 4 Core 16 GB
> - 100 GB HDD
> - name "okd"
2. shell to instance
3. edit "/etc/ssh/sshd_config"
```
$> sudo vi /etc/ssh/sshd_config
```
4. change "PubkeyAuthentication" to "yes"
```
set "PubkeyAuthentication" to "yes"
```
5. restart sshd
```
$> sudo systemctl restart sshd
```
6. try to shell from local notebook to verify configuration
7. on okd machine, generate public key for connected user
```
$> ssh-keygen -t rsa
```
8. copy generated public key to google metadata
```
/home/champillon/.ssh/id_rsa.pub
```
9. update software package
```
$> sudo yum -y update
```
10. install following components
```
$> sudo yum install -y wget git perl net-tools docker-1.13.1 bind-utils iptables-services bridge-utils openssl-devel bash-completion kexec-tools sos psacct python-cryptography python2-pip python-devel python-passlib java-1.8.0-openjdk-headless "@Development Tools"
```
11. install following components
```
$> sudo yum -y install python-passlib httpd-tools
```
12. install following components
```
$> sudo yum install -y epel-release
```
13. install following components
```
$> sudo yum install -y ansible
```
14. 
