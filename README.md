# hcs-reporting

## Prerequisites

On a RHEL 8.6 server as root
    
```shell
# dnf install -y python38 git
# alternatives --set python /usr/bin/python3.8
```

Create a local user or use existing one:

```shell
# useradd hcs-user
# passwd hcs-user
Changing password for user hcs-user.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
```

## Installation

Run following commands as a local user:

```shell
$ git clone https://github.com/alexmatveyev/hcs-reporting.git && cd hcs-reporting/ && chmod +x hcs-collector

$ curl -LJ https://github.com/C-RH-C/crhc-cli/releases/download/1.10.11/crhc-linux-x64 -o crhc && chmod +x crhc
```

Download the offline token from https://console.redhat.com/openshift/token and proceed via cli as below:

```shell
$ ./crhc login --token eyJhbGciOiJIUzI1NiIsInR5cCIgOiAiSldUIiwia2lkIiA6ICJh.......
Authenticated and ready to go!
```

Setup hcs-collector with crhc-cli path and reporting base-dir

```shell
$ ./hcs-collector setup crhc-cli
setup crhc path
Please, type the path to the crhc-cli binary: /home/hcs-user/hcs-reporting/crhc
/home/hcs-user/hcs-reporting/crhc
the file is present
updating conf file

$ mkdir ../hcs_data

$ ./hcs-collector setup base-dir
setup basedir path
Please, type the path to the dir that will be used to store the data [~/hcs_data]: /home/hcs-user/hcs_data
/home/hcs-user/hcs_data
the dir is present
updating conf file

$ ./hcs-collector setup view
{
    "base_dir": "/home/hcs-user/hcs_data",
    "crhc_cli": "/home/hcs-user/hcs-reporting/crhc"
}
```

## Running report

```shell
$ ./hcs-collector collect
collect data
initial directory setup
/home/hcs-user/hcs-reporting/crhc
Cleaning the current cache files
removing the file /tmp/inventory.json
removing the file /tmp/swatch.json
removing the file /tmp/match_inv_sw.csv
Downloading and creating the new match info
dumping the inventory information to '/tmp/inventory.json', this can take some time to finish
dumping the subscription information to '/tmp/swatch.json'
File /tmp/match_inv_sw.csv created
File /tmp/issue_summary.log created


$ ./hcs-collector process

## RHEL On-Demand

Max Concurrent RHEL, referrent to ............: 2022-09
On-Demand, Physical Node .....................: 0
On-Demand, Virtual Node ......................: 4
Virtual Data Center, Virtual Node ............: 8
Unknown ......................................: 0

## RHEL Virtual Data Center

Virtual Data Center, Hypervisor ..............: 1
Virtual Data Center, Hypervisor Sockets ......: 1

## RHEL High Availability Add-On

High Availability, Physical Node .............: 0
High Availability, Virtual Node ..............: 3
```

## References

<https://github.com/C-RH-C/crhc-cli>
<https://github.com/C-RH-C/hcs-collector>
<https://github.com/alexmatveyev/hcs-collector/tree/vdc-and-virt-who>
