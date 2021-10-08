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
set "PermitRootLogin" to "yes"
```
5. restart sshd
```
$> sudo systemctl restart sshd
```
6. try to shell from local notebook to verify configuration
7. on okd machine, generate public key for root
```
$> ssh-keygen -t rsa
```
8. copy generated public key to google metadata
```
/root/.ssh/id_rsa.pub
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
14. clone project for "openshift-ansible"
```
$> git clone https://github.com/openshift/openshift-ansible.git
$> cd openshift-ansible
$> git checkout release-3.11
```
15. get inventory config file
```
$> wget https://raw.githubusercontent.com/techmogun/okd-3.11-origin/master/inventory_wildcard_external
```
16. replace the following content in "inventory_wildcard_external"
```
"master.techmogun.local" to "okd"
"apps.master.192.168.10.5.xip.io" to "apps.passapong.dev"
```
17. add the following config to "inventory_wildcard_external"
```
openshift_public_hostname=okd.passapong.dev
openshift_master_api_port=8443
openshift_master_console_port=8443
```
18. set cloud dns to point at "okd" ip to subdomain "okd.passapong.dev"
19. run prerequisite test
```
$> sudo ansible-playbook -i inventory_wildcard_external playbooks/prerequisites.yml
```
20. run installation
```
$> sudo ansible-playbook -i inventory_wildcard_external playbooks/deploy_cluster.yml
```
21. change password to ocadmin
```
$> sudo htpasswd -b /etc/origin/master/htpasswd ocpadmin welcome1
$> sudo oc adm policy add-cluster-role-to-user cluster-admin ocpadmin
```
22. set okd vm to open 8443 port on google cloud firewall
23. access the admin console with ignore trusted cert. by open chrome in ignore-certificate-error
```
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --ignore-certificate-errors
```
24. access the admin url
```
https://okd.passapong.dev:8443/console/
```
