# kafka install cluster on 3 node
## This document is based on the how to running a Kafka cluster on 3 node on different hosts.
For this guide we running 3 nodes with name kafka-01, kafka-02, kafka-03 with the following IP-Address:
>- 192.168.27.201  kafka-01
>- 192.168.27.202  kafka-02
>- 192.168.27.203  kafka-03

1. At the first you need to open ports on OS firewall (for each node):
```
firewall-cmd --add-port={2181,9091,9092,9093,2888,3888}/tcp --permanent && firewall-cmd --reload
```
2. After that, you must installing java on your system (for each node):
```
yum install java java-devel
```
3. You can now added the following line at the `/etc/hosts` file (for each node):
```
cat <<EOF>> /etc/hosts
192.168.27.201  kafka-01
192.168.27.202  kafka-02
192.168.27.203  kafka-03
EOF
```
4. After that you need to install necessary packages for OS and update them (for each node):
```
yum -y install epel-release wget vim git curl net-tools yum-utils telnet && yum -y update
```
5. After that you need to download the latest kafka binary file from the server (for each node):
```
 wget https://downloads.apache.org/kafka/3.3.2/kafka_2.13-3.3.2.tgz
```
6. after that, decompress downloaded file and change the name of the directory to kafka(for each node):
```
tar -xvf kafka_*.tgz
rm -f kafka_*.tgz
mv -v kafka_* kafka
```
7. go to the directory and create the data directory for zookeeper (for each node):
```
cd kafka
mkdir -p data/zookeeper
```
8. change the value of the following keys into the config/zookeeper.properties file:
- for node 1 (192.168.27.201):
```
cat <<EOF >> config/zookeeper.properties
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# the directory where the snapshot is stored.
tickTime=2000
dataDir=/root/kafka/data/zookeeper
# the port at which the clients will connect
clientPort=2181
initLimit=5
syncLimit=2
# disable the per-ip limit on the number of connections since this is a non-production config
maxClientCnxns=300
# Disable the adminserver by default to avoid port conflicts.
# Set the port to something non-conflicting if choosing to enable this
admin.enableServer=false
# admin.serverPort=8080
server.1=kafka-01:2888:3888
server.2=kafka-02:2888:3888
server.3=kafka-03:2888:3888
EOF
```
- for node 2 (192.168.27.202):
```
cat <<EOF>> config/zookeeper.properties
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# the directory where the snapshot is stored.
tickTime=2000
dataDir=/root/kafka/data/zookeeper
# the port at which the clients will connect
clientPort=2181
initLimit=5
syncLimit=2
# disable the per-ip limit on the number of connections since this is a non-production config
maxClientCnxns=300
# Disable the adminserver by default to avoid port conflicts.
# Set the port to something non-conflicting if choosing to enable this
admin.enableServer=false
# admin.serverPort=8080
server.1=kafka-01:2888:3888
server.2=kafka-02:2888:3888
server.3=kafka-03:2888:3888
EOF
```
- for node 3 (192.168.27.203):
```
cat <<EOF >> config/zookeeper.properties
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# the directory where the snapshot is stored.
tickTime=2000
dataDir=/root/kafka/data/zookeeper
# the port at which the clients will connect
clientPort=2181
initLimit=5
syncLimit=2
# disable the per-ip limit on the number of connections since this is a non-production config
maxClientCnxns=300
# Disable the adminserver by default to avoid port conflicts.
# Set the port to something non-conflicting if choosing to enable this
admin.enableServer=false
# admin.serverPort=8080
server.1=kafka-01:2888:3888
server.2=kafka-02:2888:3888
server.3=kafka-03:2888:3888
EOF
```
9. Different echo lines can be made on each node:
- node 1 `(192.168.27.201)`:
```
echo '1' >> data/zookeeper/myid
```
- node 2 `(192.168.27.202)`:
```
echo '2' >> data/zookeeper/myid
```
- node 3 `(192.168.27.203)`:
```
echo '3' >> data/zookeeper/myid
```
10. 
