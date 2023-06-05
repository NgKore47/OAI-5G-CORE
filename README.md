# OAI-5G-CORE

#### In OAI-5G-CORE, we have to configure the following files:
- `/docker-compose/docker-compose-basic-nrf.yaml`
	- changing the version of images from `v1.4.0 --> develop`
	- ***changing MCC, MNC, SD, SST, TAC*** --> setting `MCC=001` and `MNC=01` is important.
	- ***adding Integrity/ciphering algorithms:*** `INT_ALGO_LIST` and `CIPH_ALGO_LIST` to secure the communication/(user data and signaling messages) between the UE and the 5G Core network.
	- changing default DNS
- `/docker-compose/docker-compose-basic-vpp-nrf.yaml`
	- changing the version of images from `v1.4.0 --> develop`

> NOTE: Here is the list of all the changes that needs to be done on fresh OAI-5G-CORE
```patch
diff --git a/docker-compose/docker-compose-basic-nrf.yaml b/docker-compose/docker-compose-basic-nrf.yaml
index 1872e83..05fad08 100644
--- a/docker-compose/docker-compose-basic-nrf.yaml
+++ b/docker-compose/docker-compose-basic-nrf.yaml
@@ -4,7 +4,7 @@ services:
         container_name: "mysql"
         image: mysql:5.7
         volumes:
-            - ./database/oai_db2.sql:/docker-entrypoint-initdb.d/oai_db.sql
+            - ./database/oai_db.sql:/docker-entrypoint-initdb.d/oai_db.sql
             - ./healthscripts/mysql-healthcheck2.sh:/tmp/mysql-healthcheck.sh
@@ -22,7 +22,7 @@ services:
                 ipv4_address: 192.168.70.131
     oai-udr:
         container_name: "oai-udr"
-        image: oai-udr:v1.4.0
+        image: oaisoftwarealliance/oai-udr:develop
@@ -53,7 +53,7 @@ services:
                 ipv4_address: 192.168.70.136
     oai-udm:
         container_name: "oai-udm"
-        image: oai-udm:v1.4.0
+        image: oaisoftwarealliance/oai-udm:develop
@@ -81,7 +81,7 @@ services:
                 ipv4_address: 192.168.70.137
     oai-ausf:
         container_name: "oai-ausf"
-        image: oai-ausf:v1.4.0
+        image: oaisoftwarealliance/oai-ausf:develop
@@ -108,7 +108,7 @@ services:
                 ipv4_address: 192.168.70.138
     oai-nrf:
         container_name: "oai-nrf"
-        image: oai-nrf:v1.4.0
+        image: oaisoftwarealliance/oai-nrf:develop
@@ -122,32 +122,32 @@ services:
                 ipv4_address: 192.168.70.130
     oai-amf:
         container_name: "oai-amf"
-        image: oai-amf:v1.4.0
+        image: oaisoftwarealliance/oai-amf:develop
         environment:
             - TZ=Europe/paris
             - INSTANCE=0
             - PID_DIRECTORY=/var/run
-            - MCC=208
-            - MNC=95
+            - MCC=001
+            - MNC=01
             - REGION_ID=128
             - AMF_SET_ID=1
-            - SERVED_GUAMI_MCC_0=208
-            - SERVED_GUAMI_MNC_0=95
+            - SERVED_GUAMI_MCC_0=001
+            - SERVED_GUAMI_MNC_0=01
             - SERVED_GUAMI_REGION_ID_0=128
             - SERVED_GUAMI_AMF_SET_ID_0=1
             - SERVED_GUAMI_MCC_1=460
             - SERVED_GUAMI_MNC_1=11
             - SERVED_GUAMI_REGION_ID_1=10
             - SERVED_GUAMI_AMF_SET_ID_1=1
-            - PLMN_SUPPORT_MCC=208
-            - PLMN_SUPPORT_MNC=95
-            - PLMN_SUPPORT_TAC=0xa000
+            - PLMN_SUPPORT_MCC=001
+            - PLMN_SUPPORT_MNC=01
+            - PLMN_SUPPORT_TAC=0x0001
             - SST_0=1
-            - SD_0=0xFFFFFF
+            - SD_0=0x1
             - SST_1=1
-            - SD_1=1
-            - SST_2=222
-            - SD_2=123
+            - SD_1=0x2
+            - SST_2=1
+            - SD_2=0x3
             - AMF_INTERFACE_NAME_FOR_NGAP=eth0
             - AMF_INTERFACE_NAME_FOR_N11=eth0
             - SMF_INSTANCE_ID_0=1
@@ -164,6 +164,7 @@ services:
             - MYSQL_USER=root
             - MYSQL_PASS=linux
             - MYSQL_DB=oai_db
+            - OPERATOR_KEY=1006020f0a478bf6b699f15c062e42b3
             - NRF_IPV4_ADDRESS=192.168.70.130
             - NRF_PORT=80
             - EXTERNAL_NRF=no
@@ -184,6 +185,8 @@ services:
             - UDM_PORT=80
             - UDM_API_VERSION=v2
             - UDM_FQDN=oai-udm
+            - INT_ALGO_LIST=["NIA1", "NIA2"]
+            - CIPH_ALGO_LIST=["NEA0", "NEA2"]
@@ -193,7 +196,7 @@ services:
                 ipv4_address: 192.168.70.132
     oai-smf:
         container_name: "oai-smf"
-        image: oai-smf:v1.4.0
+        image: oaisoftwarealliance/oai-smf:develop
         environment:
             - TZ=Europe/Paris
             - INSTANCE=0
@@ -203,8 +206,8 @@ services:
             - SMF_INTERFACE_PORT_FOR_SBI=80
             - SMF_INTERFACE_HTTP2_PORT_FOR_SBI=9090
             - SMF_API_VERSION=v1
-            - DEFAULT_DNS_IPV4_ADDRESS=172.21.3.100
-            - DEFAULT_DNS_SEC_IPV4_ADDRESS=8.8.8.8
+            - DEFAULT_DNS_IPV4_ADDRESS=8.8.8.8
+            - DEFAULT_DNS_SEC_IPV4_ADDRESS=4.4.4.4
             - AMF_IPV4_ADDRESS=192.168.70.132
             - AMF_PORT=80
             - AMF_API_VERSION=v1
@@ -228,25 +231,25 @@ services:
             - UE_MTU=1500
             - DNN_NI0=oai
             - TYPE0=IPv4
-            - DNN_RANGE0=12.1.1.151 - 12.1.1.253
+            - DNN_RANGE0=12.1.1.2 - 12.1.1.253
             - NSSAI_SST0=1
-            - NSSAI_SD0=0xFFFFFF
-            - SESSION_AMBR_UL0=200Mbps
-            - SESSION_AMBR_DL0=400Mbps
-            - DNN_NI1=oai.ipv4
+            - NSSAI_SD0=0x1
+            - SESSION_AMBR_UL0=10000Mbps
+            - SESSION_AMBR_DL0=10000Mbps
+            - DNN_NI1=oai2
             - TYPE1=IPv4
-            - DNN_RANGE1=12.1.1.51 - 12.1.1.150
+            - DNN_RANGE1=12.1.2.2 - 12.1.2.253
             - NSSAI_SST1=1
-            - NSSAI_SD1=1
-            - SESSION_AMBR_UL1=100Mbps
-            - SESSION_AMBR_DL1=200Mbps
-            - DNN_NI2=default
+            - NSSAI_SD1=0x2
+            - SESSION_AMBR_UL1=10000Mbps
+            - SESSION_AMBR_DL1=10000Mbps
+            - DNN_NI2=oai3
             - TYPE2=IPv4
-            - DNN_RANGE2=12.1.1.2 - 12.1.1.50
-            - NSSAI_SST2=222
-            - NSSAI_SD2=123
-            - SESSION_AMBR_UL2=50Mbps
-            - SESSION_AMBR_DL2=100Mbps
+            - DNN_RANGE2=12.1.3.2 - 12.1.3.253
+            - NSSAI_SST2=1
+            - NSSAI_SD2=0x3
+            - SESSION_AMBR_UL2=10000Mbps
+            - SESSION_AMBR_DL2=10000Mbps
             - DNN_NI3=ims
             - TYPE3=IPv4v6
             - DNN_RANGE3=14.1.1.2 - 14.1.1.253
@@ -260,7 +263,7 @@ services:
                 ipv4_address: 192.168.70.133
     oai-spgwu:
         container_name: "oai-spgwu"
-        image: oai-spgwu-tiny:v1.4.0
+        image: oaisoftwarealliance/oai-spgwu-tiny:develop
@@ -271,10 +274,10 @@ services:
             - NETWORK_UE_IP=12.1.1.0/24
             - SPGWC0_IP_ADDRESS=192.168.70.133
             - BYPASS_UL_PFCP_RULES=no
-            - MCC=208
-            - MNC=95
-            - MNC03=095
-            - TAC=40960
+            - MCC=001
+            - MNC=01
+            - MNC03=001
+            - TAC=1
             - GW_ID=1
             - THREAD_S1U_PRIO=80
             - S1U_THREADS=8
@@ -292,14 +295,14 @@ services:
             - NRF_API_VERSION=v1
             - NRF_FQDN=oai-nrf
             - NSSAI_SST_0=1
-            - NSSAI_SD_0=0xFFFFFF
+            - NSSAI_SD_0=0x1
             - DNN_0=oai
             - NSSAI_SST_1=1
-            - NSSAI_SD_1=1
-            - DNN_1=oai.ipv4
-            - NSSAI_SST_2=222
-            - NSSAI_SD_2=123
-            - DNN_2=default
+            - NSSAI_SD_1=0x2
+            - DNN_1=oai2
+            - NSSAI_SST_2=1
+            - NSSAI_SD_2=0x3
+            - DNN_2=oai3
         depends_on:
             - oai-nrf
             - oai-smf
@@ -313,13 +316,13 @@ services:
             public_net:
                 ipv4_address: 192.168.70.134
     oai-ext-dn:
-        image: trf-gen-cn5g:latest
+        image: oaisoftwarealliance/trf-gen-cn5g:latest
         privileged: true
-        init: true
         container_name: oai-ext-dn
         entrypoint: /bin/bash -c \
-              "ip route add 12.1.1.0/24 via 192.168.70.134 dev eth0; ip route; sleep infinity"
-        command: ["/bin/bash", "-c", "trap : SIGTERM SIGINT; sleep infinity & wait"]
+              "ip route add 12.1.1.0/24 via 192.168.70.134 dev eth0; sleep infinity"
+        depends_on:
+            - oai-spgwu
         healthcheck:
             test: /bin/bash -c "ip r | grep 12.1.1"
             interval: 10s
diff --git a/docker-compose/docker-compose-basic-vpp-nrf.yaml b/docker-compose/docker-compose-basic-vpp-nrf.yaml
index fcbd7d2..06ab73d 100644
--- a/docker-compose/docker-compose-basic-vpp-nrf.yaml
+++ b/docker-compose/docker-compose-basic-vpp-nrf.yaml
@@ -22,7 +22,7 @@ services:
                 ipv4_address: 192.168.70.131
     oai-udr:
         container_name: "oai-udr"
-        image: oai-udr:v1.4.0
+        image: oaisoftwarealliance/oai-udr:develop
@@ -51,7 +51,7 @@ services:
                 ipv4_address: 192.168.70.136
     oai-udm:
         container_name: "oai-udm"
-        image: oai-udm:v1.4.0
+        image: oaisoftwarealliance/oai-udm:develop
@@ -77,7 +77,7 @@ services:
                 ipv4_address: 192.168.70.137
     oai-ausf:
         container_name: "oai-ausf"
-        image: oai-ausf:v1.4.0
+        image: oaisoftwarealliance/oai-ausf:develop
@@ -102,7 +102,7 @@ services:
                 ipv4_address: 192.168.70.138
     oai-nrf:
         container_name: "oai-nrf"
-        image: oai-nrf:v1.4.0
+        image: oaisoftwarealliance/oai-nrf:develop
@@ -115,7 +115,7 @@ services:
                 ipv4_address: 192.168.70.130
     oai-amf:
         container_name: "oai-amf"
-        image: oai-amf:v1.4.0
+        image: oaisoftwarealliance/oai-amf:develop
@@ -185,7 +185,7 @@ services:
                 ipv4_address: 192.168.70.132
     oai-smf:
         container_name: "oai-smf"
-        image: oai-smf:v1.4.0
+        image: oaisoftwarealliance/oai-smf:develop
@@ -227,7 +227,7 @@ services:
             public_net:
                 ipv4_address: 192.168.70.133
     vpp-upf:
-        image: oai-upf-vpp:v1.4.0
+        image: rdefosseoai/oai-upf-vpp
         privileged: true
         container_name: "vpp-upf"
         environment:
@@ -270,7 +270,7 @@ services:
             public_net_core:
                 ipv4_address: 192.168.73.134
     oai-ext-dn:
-        image: trf-gen-cn5g:latest
+        image: oaisoftwarealliance/trf-gen-cn5g:latest
         privileged: true
         init: true
         container_name: "oai-ext-dn"
         
```
