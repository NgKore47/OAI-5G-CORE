# OAI-5G-CORE

## Prerequisite:

- Suitable Docker-compose version for deploying `docker-compose-basic-nrf.yaml` file:
```bash
# Remove the existing Docker Compose binary (if any):
sudo rm /usr/local/bin/docker-compose

# Download the latest stable release of Docker Compose:
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Apply execute permissions:
sudo chmod +x /usr/local/bin/docker-compose

# Verify the installation:
docker-compose --version
```

- Clone the repository:

```bash
git clone https://github.com/NgKore47/OAI-5G-CORE.git
```

- For PLMN 00101, do the below changes first:

	**In OAI-5G-CORE, we have to configure the following files:**
	- `vim /home/ubuntu/OAI-5G-CORE/docker-compose/docker-compose-basic-nrf.yaml`
		- changing the version of images from `v1.5.0 --> develop`
		- changing MCC, MNC, SD, SST, TAC and default DNS:
		```bash
			MCC=001
			MNC=01
			SERVED_GUAMI_MCC_0=001
			SERVED_GUAMI_MNC_0=01
			PLMN_SUPPORT_MCC=001
			PLMN_SUPPORT_MNC=01
			PLMN_SUPPORT_TAC=0x0001
			SD_1=0x2
			SST_2=1
			SD_2=0x3
			NSSAI_SST2=1
			NSSAI_SD2=0x3
			NSSAI_SD_1=0x2
			DNN_1=oai2
			DEFAULT_DNS_IPV4_ADDRESS=8.8.8.8
			DEFAULT_DNS_SEC_IPV4_ADDRESS=4.4.4.4
			DNN_RANGE0=12.1.1.2 - 12.1.1.253
			SESSION_AMBR_UL0=10000Mbps
			SESSION_AMBR_DL0=10000Mbps
			DNN_RANGE1=12.1.2.2 - 12.1.2.253
			SESSION_AMBR_UL1=10000Mbps
			SESSION_AMBR_DL1=10000Mbps
			DNN_RANGE2=12.1.3.2 - 12.1.3.253
			SESSION_AMBR_UL2=10000Mbps
			SESSION_AMBR_DL2=10000Mbps
			NSSAI_SST_2=1
			NSSAI_SD_2=0x3
			DNN_2=oai3

		```

	- `vim /home/ubuntu/OAI-5G-CORE/docker-compose/docker-compose-basic-vpp-nrf.yaml`
		- changing the version of images from `v1.5.0 --> develop`


> **NOTE**: Here is the list of all the changes that needs to be done on fresh OAI-5G-CORE </br>
Check out [here](./docs/git-diff.md)

## Nework Architecture
![oai](./docs/OAI-5g-Core.png)

## Deployment:
```bash
cd ~/OAI-5G-CORE/docker-compose/
docker-compose -f docker-compose-basic-nrf.yaml up -d
```

## Output:

![Alt text](./docs/images/oai_deploy_output.png)


