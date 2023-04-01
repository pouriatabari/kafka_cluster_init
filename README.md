# kafka install cluster on 3 node
## This document is based on the how to running a Kafka cluster on 3 node on different hosts.
For this guide we running 3 nodes with name kafka-01, kafka-02, kafka-03 with the following IP-Address:
- 192.168.27.201  kafka-01
- 192.168.27.202  kafka-02
- 192.168.27.203  kafka-03

1. At the first you need to open ports on OS firewall (for each node):
```
firewall-cmd --add-port={2181,9091,9092,9093,2888,3888}/tcp --permanent && firewall-cmd --reload
```
3. After that, you must installing java on your system (for each node):
```
yum install java java-devel
```
5. You can now added the following line at the ==/etc/hosts== file (for each node):
```
cat <<EOF>> /etc/hosts
192.168.27.201  kafka-01
192.168.27.202  kafka-02
192.168.27.203  kafka-03
EOF
```
7. After that you need to install necessary packages for OS and update them (for each node):
8. After that you need to download the latest kafka binary file from the server (for each node):
