# RAC RDBMS 21c Software Install

This Ansinble playbook is for Oracle Grid Infrastructure Installation, Oracle RAC Installation and Create RAC database 21c 64-bit on Oracle Linux 7 (OL7) 64-bit.
![img](https://miro.medium.com/max/1400/1*Dn-ENgHGeaJk8kpJXE_Sdw.png)

## Database Installation Guide for Linux
https://docs.oracle.com/en/database/oracle/oracle-database/21/ladbi/index.html

### software download page
Required Oracle Software: Download the Oracle software from OTN or MOS depending on your support status. Oracle binaries are staged from the "edelivery: Oracle Database 21c Software (64-bit)". They have to be manually downloaded and made available for this article to apply 
```
http://www.oracle.com/technetwork/indexes/downloads/index.html
```

### Setup:
 * OS: OEL 7.5 
 * Ansible: ansible 2.7.6
 * Database Version: Oracle 21.3 Linux64

## Master Playbook:
oracleGInRAC21c.yml

There are three roles with this playbook: 

roles                  | tasks
---------------------- | ---------------------------------
1 racgi21c_install     | # **To Install Oracle Grid Infrastructure Installation**
2 racdbsoft21c_install | # **To Install Oracle RAC software installation**
3 racdb21c_create      | # **To create Two Node RACDB ~21C**
4 racgi_ss_21c_install | # **Oracle Grid Infrastructure for a Standalone Server


### To Install Oracle RAC software installation:
> Enable role 2, disable role 1, role3 and role4 [ Pre-step: Role 1 should be completed ]

```
[root@oel75 ansible]# cat oracleGInRAC21c.yml
- hosts: ora-x1
  user: root

  roles:
   #- racdb21c_create
   - racdbsoft21c_install
   #- racgi21c_install
   #- racgi_ss_21c_install
```
## Tree Structure for this playbook is 

```
[root@oel75 ansible]# tree roles/racdbsoft21c_install
roles/racdbsoft21c_install
├── files
│   ├── 19cEE_SoftOnly.rsp.del
│   └── silent-19clsnr-config.rsp
├── samplerunnote.txt
├── tasks
│   ├── main.yml
│   ├── racdbsoft19c_exerootn1.yml
│   ├── racdbsoft19c_exerootn2.yml
│   ├── racdbsoft19c_installvalidate.yml
│   ├── racdbsoft19c_presoftinstall.yml
│   └── racdbsoft19c_softinstall.yml
├── templates
│   ├── 19cEE_SoftOnly.rsp.j2
│   ├── 19cRACEE_SoftOnly.rsp.j2
│   ├── dbsoft19c.rsp.del
│   ├── dbsoft19c.rsp.j2
│   ├── oracle-database-preinstall-19c-1.0-1.el7.x86_64.rpm
│   ├── oracle-database-preinstall-21c-1.0-1.el7.x86_64.rpm
│   ├── silent_listen_config.rsp
│   └── silent_listen_config.rsp.j2
└── vars
    └── main.yml

```
## Summary commands: 

1. Clone this repository:
    git clone https://github.com/asiandevs/oracle21c.git
    
2. Stage the following Oracle Software on the control machine

Oracle Database 21c Grid Infrastructure (21.3) for Linux x86-64
     - LINUX.X64_213000_grid_home.zip
Oracle Database 21c (19.3) for Linux x86-64 
     - LINUX.X64_213000_db_home.zip

3. Configure an Ansible inventory file (example as below) 
```
[root@oel75 ansible]# cat ansible.cfg | grep inventory
inventory = ./inventory
[root@oel75 ansible]# cat inventory
[ora-x1]
192.168.56.102

[ora-x2]
192.168.56.103

[oel75]
192.168.56.101

[dbservers]
192.168.56.102
192.168.56.103
```
Note: Modify variables based on you setup or your requirements. 

4. Run the playbook role "oracleGInRAC21c.yml"
```
ansible-playbook oracleGInRAC21c.yml  
```
