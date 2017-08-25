### Configuring FRINX ODL to interact with ConfD 

We have two hosts: 1) localhost and 2) confd host

FRINX ODL runs on localhost.

--------
Download FRINX ODL

unzip the downloaded file

go to the odl directory and run 

./bin/karaf

if you are running FRINX ODL for the first time check out our operations guide here:
https://frinx.io/frinx-documents/running-frinx-odl-distribution-for-the-first-time.html


I have the following NETCONF features installed:

frinx-user@root>
frinx-user@root>feature:list -i | grep netconf
odl-netconf-api                 | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-netconf-1.1.3-Boron-SR3.2_3_1-frinxodl            | OpenDaylight :: Netconf :: API                    
odl-netconf-mapping-api         | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-netconf-1.1.3-Boron-SR3.2_3_1-frinxodl            | OpenDaylight :: Netconf :: Mapping API            
odl-netconf-util                | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-netconf-1.1.3-Boron-SR3.2_3_1-frinxodl            |                                                   
odl-netconf-impl                | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-netconf-1.1.3-Boron-SR3.2_3_1-frinxodl            | OpenDaylight :: Netconf :: Impl                   
odl-config-netconf-connector    | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-netconf-1.1.3-Boron-SR3.2_3_1-frinxodl            | OpenDaylight :: Netconf :: Connector              
odl-netconf-netty-util          | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-netconf-1.1.3-Boron-SR3.2_3_1-frinxodl            | OpenDaylight :: Netconf :: Netty Util             
odl-netconf-client              | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-netconf-1.1.3-Boron-SR3.2_3_1-frinxodl            | OpenDaylight :: Netconf :: Client                 
odl-netconf-monitoring          | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-netconf-1.1.3-Boron-SR3.2_3_1-frinxodl            | OpenDaylight :: Netconf :: Monitoring             
odl-netconf-notifications-api   | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-netconf-1.1.3-Boron-SR3.2_3_1-frinxodl            | OpenDaylight :: Netconf :: Notification :: Api    
odl-netconf-notifications-impl  | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-netconf-1.1.3-Boron-SR3.2_3_1-frinxodl            | OpenDaylight :: Netconf :: Monitoring :: Impl     
odl-netconf-ssh                 | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-netconf-1.1.3-Boron-SR3.2_3_1-frinxodl            | OpenDaylight :: Netconf Connector :: SSH          
odl-netconf-tcp                 | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-netconf-1.1.3-Boron-SR3.2_3_1-frinxodl            | OpenDaylight :: Netconf Connector :: TCP          
odl-aaa-netconf-plugin          | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-netconf-1.1.3-Boron-SR3.2_3_1-frinxodl            | OpenDaylight :: AAA :: ODL NETCONF Plugin         
odl-netconf-connector-all       | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-controller-1.1.3-Boron-SR3.2_3_1-frinxodl         | OpenDaylight :: Netconf Connector :: All          
odl-netconf-connector           | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-controller-1.1.3-Boron-SR3.2_3_1-frinxodl         | OpenDaylight :: Netconf Connector :: Netconf Conne
odl-netconf-connector-ssh       | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-controller-1.1.3-Boron-SR3.2_3_1-frinxodl         | OpenDaylight :: Netconf Connector :: Netconf Conne
odl-netconf-topology            | 1.1.3-Boron-SR3.2_3_1-frinxodl | x         | odl-controller-1.1.3-Boron-SR3.2_3_1-frinxodl         | OpenDaylight :: Netconf Topology :: Netconf Connec



--------

copy the following yang files from xxx to {your_ODL_directory}/cache/



--------
install confd on a ubuntu 16.04.1 host (user: admin, password: admin)

Download Confd for Linux from the Cisco web page

admin@16:~$ ls
confd-basic-6.4.linux.x86_64.zip

Unzip the file

admin@16:~$ unzip confd-basic-6.4.linux.x86_64.zip 
Archive:  confd-basic-6.4.linux.x86_64.zip
   creating: confd-basic-6.4.linux.x86_64/
  inflating: confd-basic-6.4.linux.x86_64/confd-basic-6.4.doc.tar.gz  
  inflating: confd-basic-6.4.linux.x86_64/confd-basic-6.4.examples.tar.gz  
  inflating: confd-basic-6.4.linux.x86_64/confd-basic-6.4.libconfd.tar.gz  
  inflating: confd-basic-6.4.linux.x86_64/confd-basic-6.4.linux.x86_64.installer.bin  
  inflating: confd-basic-6.4.linux.x86_64/ConfD_Basic_License_Agreement_1.1.pdf  
admin@16:~$ ls
confd-basic-6.4.linux.x86_64  confd-basic-6.4.linux.x86_64.zip
admin@16:~$ cd confd-basic-6.4.linux.x86_64/
admin@16:~/confd-basic-6.4.linux.x86_64$ ls -al
total 17364
drwxr-xr-x 2 admin admin     4096 Apr 20 11:41 .
drwxr-xr-x 4 admin admin     4096 Aug 24 20:29 ..
-rw-r--r-- 1 admin admin  5776085 Mar 21 11:31 confd-basic-6.4.doc.tar.gz
-rw-r--r-- 1 admin admin   631635 Mar 22 12:02 confd-basic-6.4.examples.tar.gz
-rw-r--r-- 1 admin admin   478587 Mar 22 12:01 confd-basic-6.4.libconfd.tar.gz
-rwxr-xr-x 1 admin admin 10755987 Mar 22 12:02 confd-basic-6.4.linux.x86_64.installer.bin
-rw-r--r-- 1 admin admin   120335 Apr  6  2015 ConfD_Basic_License_Agreement_1.1.pdf

Create a new directory and run the installer

admin@16:~/confd-basic-6.4.linux.x86_64$ mkdir /home/admin/confd
admin@16:~/confd-basic-6.4.linux.x86_64$ ./confd-basic-6.4.linux.x86_64.installer.bin ~/confd
INFO  Unpacked confd-basic-6.4 in /home/admin/confd
INFO  Found and unpacked corresponding DOCUMENTATION_PACKAGE
INFO  Found and unpacked corresponding EXAMPLE_PACKAGE
INFO  Generating default SSH hostkey (this may take some time)
INFO  SSH hostkey generated
INFO  Environment set-up generated in /home/admin/confd/confdrc
INFO  ConfD installation script finished


The directory now contains all confd files


admin@16:~/confd-basic-6.4.linux.x86_64$ cd ../confd
admin@16:~/confd$ ls -al
total 336
drwxrwxr-x 13 admin admin   4096 Aug 24 20:30 .
drwxr-xr-x  5 admin admin   4096 Aug 24 20:30 ..
drwxr-xr-x  2 admin admin   4096 Mar 22 12:01 bin
-rw-r--r--  1 admin admin 182435 Mar 22 12:01 CHANGES
-rw-rw-r--  1 admin admin    572 Aug 24 20:30 confdrc
-rw-rw-r--  1 admin admin    538 Aug 24 20:30 confdrc.tcsh
drwxr-xr-x  4 admin admin   4096 Mar 21 11:31 doc
drwxr-xr-x  3 admin admin   4096 Mar 22 12:01 erlang
drwxr-xr-x  3 admin admin   4096 Mar 22 12:01 etc
drwxr-xr-x 20 admin admin   4096 Mar 22 12:02 examples.confd
drwxr-xr-x  2 admin admin   4096 Mar 22 12:01 include
drwxr-xr-x  3 admin admin   4096 Mar 22 12:01 java
-rw-r--r--  1 admin admin    648 Mar 22 12:01 KNOWN_ISSUES
drwxr-xr-x  5 admin admin   4096 Mar 22 12:02 lib
-rw-r--r--  1 admin admin  76279 Mar 22 12:01 LICENSE
drwxr-xr-x  6 admin admin   4096 Mar 21 11:24 man
-rw-r--r--  1 admin admin  11577 Mar 22 12:01 README
drwxr-xr-x  3 admin admin   4096 Mar 22 12:01 src
drwxr-xr-x  3 admin admin   4096 Mar 22 12:01 var
-rw-r--r--  1 admin admin    228 Mar 22 12:01 VERSION

Run confd

admin@16:~/confd$ cd bin
admin@16:~/confd/bin$ ./confd

Verify confd is running

admin@16:~/confd/bin$ ps -ef | grep confd
admin     3974     1  7 20:30 ?        00:00:00 /home/admin/confd/lib/confd/erts/bin/confd -K false -B -MHe true -- -root /home/admin/confd/lib/confd -progname confd -- -home /home/admin -- -smp disable -boot confd -delayed-detach -noshell -noinput -stacktrace_depth 24 -shutdown_time 30000 -conffile /home/admin/confd/etc/confd/confd.conf -max_fds 1024 -detached-fd 4
admin     3990  3466  0 20:31 pts/0    00:00:00 grep --color=auto confd
admin@16:~/confd/bin$ 



--------
confd is using 2022 as the default port for NETCONF
create a tunnel from localhost (127.0.0.1) to port 2022 on confd host (192.168.0.100)
this session needs to stay active for as long as you connect to the confd host on port 2022


Gerhards-MacBook-Pro:frinxit gwieser$ ssh admin@192.168.0.100 -L 2022:127.0.0.1:2022
admin@192.168.0.100's password: 
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-92-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

110 packages can be updated.
0 updates are security updates.


Last login: Thu Aug 24 20:26:35 2017 from 192.168.0.174
admin@16:~$ 
admin@16:~$ 

--------
test connectivity from localhost to confd host on port 2022 via tunnel

Gerhards-MacBook-Pro:~ gwieser$ ssh admin@localhost -p 2022 -s netconf
The authenticity of host '[localhost]:2022 ([::1]:2022)' can't be established.
RSA key fingerprint is SHA256:dpcwCrr87NuqIiRrHdERbksKcmVLiicZTZ39tNu0W78.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[localhost]:2022' (RSA) to the list of known hosts.
admin@localhost's password: 
<?xml version="1.0" encoding="UTF-8"?>
<hello xmlns="urn:ietf:params:xml:ns:netconf:base:1.0">
<capabilities>
<capability>urn:ietf:params:netconf:base:1.0</capability>
<capability>urn:ietf:params:netconf:base:1.1</capability>
<capability>urn:ietf:params:netconf:capability:writable-running:1.0</capability>
<capability>urn:ietf:params:netconf:capability:candidate:1.0</capability>
<capability>urn:ietf:params:netconf:capability:confirmed-commit:1.0</capability>
<capability>urn:ietf:params:netconf:capability:confirmed-commit:1.1</capability>
<capability>urn:ietf:params:netconf:capability:xpath:1.0</capability>
<capability>urn:ietf:params:netconf:capability:url:1.0?scheme=ftp,sftp,file</capability>
<capability>urn:ietf:params:netconf:capability:validate:1.0</capability>
<capability>urn:ietf:params:netconf:capability:validate:1.1</capability>
<capability>urn:ietf:params:netconf:capability:rollback-on-error:1.0</capability>
<capability>urn:ietf:params:netconf:capability:with-defaults:1.0?basic-mode=explicit&amp;also-supported=report-all-tagged</capability>
<capability>urn:ietf:params:netconf:capability:yang-library:1.0?revision=2016-06-21&amp;module-set-id=335e86ef56ac92cac3ea4fd81ce3c81d</capability>
<capability>http://tail-f.com/ns/netconf/extensions</capability>
<capability>http://tail-f.com/ns/aaa/1.1?module=tailf-aaa&amp;revision=2015-06-16</capability>
<capability>http://tail-f.com/ns/kicker?module=tailf-kicker&amp;revision=2017-03-16</capability>
<capability>http://tail-f.com/yang/acm?module=tailf-acm&amp;revision=2013-03-07</capability>
<capability>http://tail-f.com/yang/common-monitoring?module=tailf-common-monitoring&amp;revision=2013-06-14</capability>
<capability>http://tail-f.com/yang/confd-monitoring?module=tailf-confd-monitoring&amp;revision=2013-06-14</capability>
<capability>http://tail-f.com/yang/netconf-monitoring?module=tailf-netconf-monitoring&amp;revision=2016-11-24</capability>
<capability>urn:ietf:params:xml:ns:yang:iana-crypt-hash?module=iana-crypt-hash&amp;revision=2014-08-06&amp;features=crypt-hash-sha-512,crypt-hash-sha-256,crypt-hash-md5</capability>
<capability>urn:ietf:params:xml:ns:yang:ietf-inet-types?module=ietf-inet-types&amp;revision=2013-07-15</capability>
<capability>urn:ietf:params:xml:ns:yang:ietf-netconf-acm?module=ietf-netconf-acm&amp;revision=2012-02-22</capability>
<capability>urn:ietf:params:xml:ns:yang:ietf-netconf-monitoring?module=ietf-netconf-monitoring&amp;revision=2010-10-04</capability>
<capability>urn:ietf:params:xml:ns:yang:ietf-netconf-notifications?module=ietf-netconf-notifications&amp;revision=2012-02-06</capability>
<capability>urn:ietf:params:xml:ns:yang:ietf-yang-library?module=ietf-yang-library&amp;revision=2016-06-21</capability>
<capability>urn:ietf:params:xml:ns:yang:ietf-yang-types?module=ietf-yang-types&amp;revision=2013-07-15</capability>
<capability>urn:ietf:params:xml:ns:netconf:base:1.0?module=ietf-netconf&amp;revision=2011-06-01</capability>
<capability>urn:ietf:params:xml:ns:yang:ietf-netconf-with-defaults?module=ietf-netconf-with-defaults&amp;revision=2011-06-01</capability>
</capabilities>
<session-id>27</session-id></hello>]]>]]>

----------

Mount the confd node "CONFD1" in ODL

curl -X PUT \
  http://127.0.0.1:8181/restconf/config/network-topology:network-topology/topology/topology-netconf/node/CONFD1 \
  -H 'authorization: Basic YWRtaW46YWRtaW4=' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -H 'postman-token: 09065151-1c48-6b24-cf31-b5ad7c8f2f5b' \
  -d '{
  "node": [
    {
      "node-id": "CONFD1",
      "netconf-node-topology:host": "127.0.0.1",
      "netconf-node-topology:port": 2022,
      "netconf-node-topology:keepalive-delay": "10",
      "netconf-node-topology:tcp-only": false,
      "netconf-node-topology:username": "admin",
      "netconf-node-topology:password": "admin",
      "netconf-node-topology:connection-timeout-millis": "1000",
      "netconf-node-topology:default-request-timeout-millis": "1000",
      "netconf-node-topology:yang-module-capabilities":{
	      "capability": [
	       "http://tail-f.com/yang/common?module=tailf-common&amp;revision=2017-03-08",
	       "http://tail-f.com/yang/common?module=tailf-cli-extensions&amp;revision=2017-03-08",
	       "http://tail-f.com/yang/common?module=tailf-meta-extensions&amp;revision=2017-03-08"
	      ]
      }
    }
  ]
}
'

-------------
check config data store in ODL

curl -X GET \
  http://127.0.0.1:8181/restconf/config/network-topology:network-topology/topology/topology-netconf \
  -H 'accept: application/yang.data+json' \
  -H 'authorization: Basic YWRtaW46YWRtaW4=' \
  -H 'cache-control: no-cache' \
  -H 'postman-token: f58bd9df-d584-19a8-f24b-5eb9c3381637' \
  -d '{
  "input":{
  }
}'

your output should look similar to the following:

{
    "topology": [
        {
            "topology-id": "topology-netconf",
            "node": [
                {
                    "node-id": "CONFD1",
                    "netconf-node-topology:tcp-only": false,
                    "netconf-node-topology:host": "127.0.0.1",
                    "netconf-node-topology:yang-module-capabilities": {
                        "capability": [
                            "http://tail-f.com/yang/common?module=tailf-meta-extensions&amp;revision=2017-03-08",
                            "http://tail-f.com/yang/common?module=tailf-common&amp;revision=2017-03-08",
                            "http://tail-f.com/yang/common?module=tailf-cli-extensions&amp;revision=2017-03-08"
                        ]
                    },
                    "netconf-node-topology:port": 2022,
                    "netconf-node-topology:connection-timeout-millis": 1000,
                    "netconf-node-topology:username": "admin",
                    "netconf-node-topology:password": "admin",
                    "netconf-node-topology:default-request-timeout-millis": 1000,
                    "netconf-node-topology:keepalive-delay": 10
                }
            ]
        }
    ]
}


---------
let's check if the node is present in the operational data store in ODL. 


curl -X GET \
  http://127.0.0.1:8181/restconf/operational/network-topology:network-topology/topology/topology-netconf \
  -H 'accept: application/yang.data+json' \
  -H 'authorization: Basic YWRtaW46YWRtaW4=' \
  -H 'cache-control: no-cache' \
  -H 'postman-token: a3fc9816-45cd-8128-0a91-16f2df45dd49' \
  -d '{
  "input":{
  }
}'


your output should look similar to:


{
    "topology": [
        {
            "topology-id": "topology-netconf",
            "node": [
                {
                    "node-id": "controller-config",
                    "netconf-node-topology:available-capabilities": {
                        "available-capability": [
 							[output ommitted]
                        ]
                    },
                    "netconf-node-topology:host": "127.0.0.1",
                    "netconf-node-topology:unavailable-capabilities": {},
                    "netconf-node-topology:connection-status": "connected",
                    "netconf-node-topology:port": 1830
                },
                {
                    "node-id": "CONFD1",
                    "netconf-node-topology:available-capabilities": {
                        "available-capability": [
                            "urn:ietf:params:netconf:capability:validate:1.0",
                            "http://tail-f.com/yang/acm?module=tailf-acm&revision=2013-03-07",
                            "http://tail-f.com/yang/common?module=tailf-common&revision=2017-03-08",
                            "urn:ietf:params:xml:ns:yang:ietf-netconf-with-defaults?module=ietf-netconf-with-defaults&revision=2011-06-01",
                            "http://tail-f.com/yang/common-monitoring?module=tailf-common-monitoring&revision=2013-06-14",
                            "urn:ietf:params:netconf:capability:with-defaults:1.0?basic-mode=explicit&also-supported=report-all-tagged",
                            "urn:ietf:params:xml:ns:yang:ietf-inet-types?module=ietf-inet-types&revision=2013-07-15",
                            "urn:ietf:params:netconf:capability:rollback-on-error:1.0",
                            "urn:ietf:params:netconf:base:1.1",
                            "urn:ietf:params:netconf:base:1.0",
                            "urn:ietf:params:xml:ns:yang:ietf-netconf-monitoring?module=ietf-netconf-monitoring&revision=2010-10-04",
                            "urn:ietf:params:xml:ns:yang:ietf-netconf-acm?module=ietf-netconf-acm&revision=2012-02-22",
                            "urn:ietf:params:xml:ns:yang:ietf-yang-types?module=ietf-yang-types&revision=2013-07-15",
                            "urn:ietf:params:netconf:capability:confirmed-commit:1.0",
                            "urn:ietf:params:netconf:capability:candidate:1.0",
                            "urn:ietf:params:netconf:capability:confirmed-commit:1.1",
                            "urn:ietf:params:netconf:capability:yang-library:1.0?revision=2016-06-21&module-set-id=335e86ef56ac92cac3ea4fd81ce3c81d",
                            "urn:ietf:params:netconf:capability:validate:1.1",
                            "urn:ietf:params:xml:ns:yang:ietf-yang-library?module=ietf-yang-library&revision=2016-06-21",
                            "urn:ietf:params:xml:ns:yang:iana-crypt-hash?module=iana-crypt-hash&revision=2014-08-06",
                            "http://tail-f.com/yang/confd-monitoring?module=tailf-confd-monitoring&revision=2013-06-14",
                            "http://tail-f.com/ns/aaa/1.1?module=tailf-aaa&revision=2015-06-16",
                            "http://tail-f.com/ns/netconf/extensions",
                            "http://tail-f.com/yang/common?module=tailf-meta-extensions&revision=2017-03-08",
                            "urn:ietf:params:netconf:capability:writable-running:1.0",
                            "http://tail-f.com/yang/common?module=tailf-cli-extensions&revision=2017-03-08",
                            "urn:ietf:params:xml:ns:netconf:base:1.0?module=ietf-netconf&revision=2011-06-01",
                            "http://tail-f.com/ns/kicker?module=tailf-kicker&revision=2017-03-16",
                            "http://tail-f.com/yang/netconf-monitoring?module=tailf-netconf-monitoring&revision=2016-11-24",
                            "urn:ietf:params:netconf:capability:url:1.0?scheme=ftp,sftp,file",
                            "urn:ietf:params:netconf:capability:xpath:1.0",
                            "urn:ietf:params:xml:ns:yang:ietf-netconf-notifications?module=ietf-netconf-notifications&revision=2012-02-06"
                        ]
                    },
                    "netconf-node-topology:host": "127.0.0.1",
                    "netconf-node-topology:unavailable-capabilities": {},
                    "netconf-node-topology:connection-status": "connected",
                    "netconf-node-topology:port": 2022
                }
            ]
        }
    ]
}



