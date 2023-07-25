```patch
diff --git a/docker-compose/docker-compose-basic-nrf.yaml b/docker-compose/docker-compose-basic-nrf.yaml
index a72e476..fcd9ba7 100644
--- a/docker-compose/docker-compose-basic-nrf.yaml
+++ b/docker-compose/docker-compose-basic-nrf.yaml
@@ -22,7 +22,7 @@ services:
                 ipv4_address: 192.168.70.131
     oai-udr:
         container_name: "oai-udr"
-        image: oaisoftwarealliance/oai-udr:v1.5.0
+        image: oaisoftwarealliance/oai-udr:develop
         environment:
             - TZ=Europe/Paris
             - UDR_NAME=OAI_UDR
@@ -31,6 +31,7 @@ services:
             - MYSQL_USER=test
             - MYSQL_PASS=test
             - MYSQL_DB=oai_db
+            - OPERATOR_KEY=1006020f0a478bf6b699f15c062e42b3
             - WAIT_MYSQL=120
             - USE_FQDN_DNS=yes
             - REGISTER_NRF=yes
@@ -44,7 +45,7 @@ services:
                 ipv4_address: 192.168.70.136
     oai-udm:
         container_name: "oai-udm"
-        image: oaisoftwarealliance/oai-udm:v1.5.0
+        image: oaisoftwarealliance/oai-udm:develop
         environment:
             - TZ=Europe/Paris
             - UDM_NAME=OAI_UDM
@@ -62,7 +63,7 @@ services:
                 ipv4_address: 192.168.70.137
     oai-ausf:
         container_name: "oai-ausf"
-        image: oaisoftwarealliance/oai-ausf:v1.5.0
+        image: oaisoftwarealliance/oai-ausf:develop
         environment:
             - TZ=Europe/Paris
             - AUSF_NAME=OAI_AUSF
@@ -80,7 +81,7 @@ services:
                 ipv4_address: 192.168.70.138
     oai-nrf:
         container_name: "oai-nrf"
-        image: oaisoftwarealliance/oai-nrf:v1.5.0
+        image: oaisoftwarealliance/oai-nrf:develop
         environment:
             - TZ=Europe/Paris
             - NRF_INTERFACE_NAME_FOR_SBI=eth0
@@ -89,34 +90,34 @@ services:
                 ipv4_address: 192.168.70.130
     oai-amf:
         container_name: "oai-amf"
-        image: oaisoftwarealliance/oai-amf:v1.5.0
+        image: oaisoftwarealliance/oai-amf:develop
         environment:
             - TZ=Europe/paris
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
             # Slice 0 (1, 0xFFFFFF)
             - SST_0=1
             # At least one slice SHALL be defined.
             # All the others are optional
             # Slice 1 (1, 1)
             - SST_1=1
-            - SD_1=1
+            - SD_1=0x2
             # Slice 2 (222, 123)
-            - SST_2=222
-            - SD_2=123
+            - SST_2=1
+            - SD_2=0x3
             - AMF_INTERFACE_NAME_FOR_NGAP=eth0
             - AMF_INTERFACE_NAME_FOR_N11=eth0
             # One single SMF instance
@@ -145,13 +146,13 @@ services:
                 ipv4_address: 192.168.70.132
     oai-smf:
         container_name: "oai-smf"
-        image: oaisoftwarealliance/oai-smf:v1.5.0
+        image: oaisoftwarealliance/oai-smf:develop
         environment:
             - TZ=Europe/Paris
             - SMF_INTERFACE_NAME_FOR_N4=eth0
             - SMF_INTERFACE_NAME_FOR_SBI=eth0
-            - DEFAULT_DNS_IPV4_ADDRESS=172.21.3.100
-            - DEFAULT_DNS_SEC_IPV4_ADDRESS=8.8.8.8
+            - DEFAULT_DNS_IPV4_ADDRESS=8.8.8.8
+            - DEFAULT_DNS_SEC_IPV4_ADDRESS=4.4.4.4
             - AMF_IPV4_ADDRESS=192.168.70.132
             - AMF_FQDN=oai-amf
             - UDM_IPV4_ADDRESS=192.168.70.137
@@ -169,26 +170,26 @@ services:
             # Slice 0 (1, 0xFFFFFF)
             - DNN_NI0=oai
             - TYPE0=IPv4
-            - DNN_RANGE0=12.1.1.151 - 12.1.1.253
+            - DNN_RANGE0=12.1.1.2 - 12.1.1.253
             - NSSAI_SST0=1
-            - SESSION_AMBR_UL0=200Mbps
-            - SESSION_AMBR_DL0=400Mbps
+            - SESSION_AMBR_UL0=10000Mbps
+            - SESSION_AMBR_DL0=10000Mbps
             # Slice 1 (1, 1)
             - DNN_NI1=oai.ipv4
             - TYPE1=IPv4
-            - DNN_RANGE1=12.1.1.51 - 12.1.1.150
+            - DNN_RANGE1=12.1.2.2 - 12.1.2.253
             - NSSAI_SST1=1
             - NSSAI_SD1=1
-            - SESSION_AMBR_UL1=100Mbps
-            - SESSION_AMBR_DL1=200Mbps
+            - SESSION_AMBR_UL1=10000Mbps
+            - SESSION_AMBR_DL1=10000Mbps
             # Slice 2 (222, 123)
             - DNN_NI2=default
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
             # Slice 3 for ims
             - DNN_NI3=ims
             - TYPE3=IPv4v6
@@ -203,7 +204,7 @@ services:
                 ipv4_address: 192.168.70.133
     oai-spgwu:
         container_name: "oai-spgwu"
-        image: oaisoftwarealliance/oai-spgwu-tiny:v1.5.0
+        image: oaisoftwarealliance/oai-spgwu-tiny:develop
         environment:
             - TZ=Europe/Paris
             - SGW_INTERFACE_NAME_FOR_S1U_S12_S4_UP=eth0
@@ -222,12 +223,12 @@ services:
             - DNN_0=oai
             # Slice 1 (1, 1)
             - NSSAI_SST_1=1
-            - NSSAI_SD_1=1
-            - DNN_1=oai.ipv4
+            - NSSAI_SD_1=0x2
+            - DNN_1=oai2
             # Slice 2 (222, 123)
-            - NSSAI_SST_2=222
-            - NSSAI_SD_2=123
-            - DNN_2=default
+            - NSSAI_SST_2=1
+            - NSSAI_SD_2=0x3
+            - DNN_2=oai3
         depends_on:
             - oai-nrf
             - oai-smf
@@ -246,8 +247,9 @@ services:
         container_name: oai-ext-dn
         image: oaisoftwarealliance/trf-gen-cn5g:latest
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
index 2c960aa..77c3d71 100644
--- a/docker-compose/docker-compose-basic-vpp-nrf.yaml
+++ b/docker-compose/docker-compose-basic-vpp-nrf.yaml
@@ -22,7 +22,7 @@ services:
                 ipv4_address: 192.168.70.131
     oai-udr:
         container_name: "oai-udr"
-        image: oaisoftwarealliance/oai-udr:v1.5.0
+        image: oaisoftwarealliance/oai-udr:develop
         environment:
             - TZ=Europe/Paris
             - UDR_NAME=OAI_UDR
@@ -44,7 +44,7 @@ services:
                 ipv4_address: 192.168.70.136
     oai-udm:
         container_name: "oai-udm"
-        image: oaisoftwarealliance/oai-udm:v1.5.0
+        image: oaisoftwarealliance/oai-udm:develop
         environment:
             - TZ=Europe/Paris
             - UDM_NAME=OAI_UDM
@@ -62,7 +62,7 @@ services:
                 ipv4_address: 192.168.70.137
     oai-ausf:
         container_name: "oai-ausf"
-        image: oaisoftwarealliance/oai-ausf:v1.5.0
+        image: oaisoftwarealliance/oai-ausf:develop
         environment:
             - TZ=Europe/Paris
             - AUSF_NAME=OAI_AUSF
@@ -80,7 +80,7 @@ services:
                 ipv4_address: 192.168.70.138
     oai-nrf:
         container_name: "oai-nrf"
-        image: oaisoftwarealliance/oai-nrf:v1.5.0
+        image: oaisoftwarealliance/oai-nrf:develop
         environment:
             - TZ=Europe/Paris
             - NRF_INTERFACE_NAME_FOR_SBI=eth0
@@ -89,7 +89,7 @@ services:
                 ipv4_address: 192.168.70.130
     oai-amf:
         container_name: "oai-amf"
-        image: oaisoftwarealliance/oai-amf:v1.5.0
+        image: oaisoftwarealliance/oai-amf:develop
         environment:
             - TZ=Europe/paris
             - MCC=208
@@ -142,7 +142,7 @@ services:
                 ipv4_address: 192.168.70.132
     oai-smf:
         container_name: "oai-smf"
-        image: oaisoftwarealliance/oai-smf:v1.5.0
+        image: oaisoftwarealliance/oai-smf:develop
         environment:
             - TZ=Europe/Paris
             - SMF_INTERFACE_NAME_FOR_N4=eth0
@@ -200,7 +200,7 @@ services:
     vpp-upf:
         privileged: true
         container_name: "vpp-upf"
-        image: oaisoftwarealliance/oai-upf-vpp:v1.5.0
+        image: oaisoftwarealliance/oai-upf-vpp:develop
         environment:
             - IF_1_IP=192.168.70.201
             - IF_1_TYPE=N4
```