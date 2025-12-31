# Case-Management-and-Threat-Analysis-Engine-Integration
This project integrates TheHive as a case management tool and Cortex as a threat analysis engine into a SOC Lab

## Objective
To centralize incident management and automate security analysis and response, enabling faster investigation, consistent handling of alerts, and improved operational efficiency across security operations.

### Pre-requisites
- Linux Server (Ubuntu preferrably) for each instances of thehive and cortex (Although its not compulsory to install each on different servers, its best practice to do that)
- 16gb RAM and 100gb storage on each server (8gb ram and less storage for small setups like this)
### My Setup
- Ubuntu server instances on Digital Ocean (Get free compute resources from cloud providers like Digital Ocean when resources for lab setup is an issue)

## Architecture
The SOC architecture consists of TheHive as the centralized incident management and orchestration platform, integrated with Cortex as the automated security analysis engine. 
Analysts interact with TheHive to manage cases and trigger Cortex analyzers, enabling automated enrichment of observables to support analyst investigations.

## Skills Learned
- Integration of TheHive and Cortex for SOC operations
- Understanding of SOAR concepts (analysis automation and orchestration)
- Configuration and management of Cortex analyzers
- Connecting Cortex to TheHive for automated observable analysis
- Incident and case management using TheHive
- Interpreting and correlating enrichment results for threat intelligence
- Troubleshooting and validating connectivity and execution between SOC components

## Steps
Documentation from StrangeBee, creators of TheHive and cortex was used <br> 
Refer: <a href="https://docs.strangebee.com/thehive/installation/installation-guide-linux-standalone-server/"> Install on Linux </a>  or  <a href="https://docs.strangebee.com/thehive/installation/automated-installation-script-linux/"> Quick Install on Linux Systems: One-Command Setup </a> <br> <br>
- One-command install is preferred for a quick setup: *Download and run the Wazuh installation assistant.*
- NB: When using server instances on Digital Ocean, here's what you can do to make the installation process seamless. Create a user and give it sudo permissions. Open the web console of the instance on Digital Ocean as the new user and run the single install command.
<br>

```
wget -q -O /tmp/install_script.sh https://scripts.download.strangebee.com/latest/sh/install_script.sh ; sudo -v ; bash /tmp/install_script.sh
```
<br>

### TheHive setup stack 
- Elasticsearch – Stores and indexes all cases, observables, alerts, and logs; enables fast search and retrieval
- Cassandra – Data storage
- TheHive – The core SOC platform for case management, investigation, and orchestration of alerts and observables.
- For a small lab size, remember to edit the */etc/elasticsearch/jvm.options.d/heap.otions* file with your preferred memory heap size. <br>
find Xms and Xmx in the *heap.options* file and reduce them both to 512m or 1g each
save and exit

Add a 2 to 4gb swap space to prevent out of memory issues. <br>
see code below <br>

```
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

- Run the following commands to ensure that all the components are working

```
sudo systemctl status cassandra
```

<img width="1130" height="385" alt="image" src="https://github.com/user-attachments/assets/bee1efd8-43c0-450b-8570-c0e04afd6f7b" />
<br> <br>

```
sudo systemctl status elasticsearch
```
<img width="569" height="280" alt="{78A501BA-8591-42E3-92A5-F4466BFD5FBF}" src="https://github.com/user-attachments/assets/3474d6f4-9331-4135-9ed9-43d08fce76d9" />
<br> <br>

```
sudo systemctl status thehive
```
<img width="568" height="212" alt="{087F8642-8473-48D7-A9DA-5E375EE9A766}" src="https://github.com/user-attachments/assets/444c8ee1-590a-4939-9f81-841548b9a754" />
<br> <br>

Access thehive instance by typing http://<Your server's IP> in the browser <br> to see an the following interface. 
Default credentials:  Username: *admin*  Password: *secret* 

<img width="960" height="448" alt="{803BEB1A-9A4B-4476-9809-FC470FD92108}" src="https://github.com/user-attachments/assets/9ef19ae5-5816-4b11-9360-eca6d76641cc" />

<br>
<img width="960" height="445" alt="{A6D234C3-D6FA-46D7-ABDB-5B1F00B65826}" src="https://github.com/user-attachments/assets/ad7c8f43-99a2-4d92-b75d-6d4789b02825" />
<br>

After first logon:
- Change default password.&nbsp; Refer: &nbsp; *<a href="https://docs.strangebee.com/thehive/administration/perform-initial-setup-as-admin/"> First Start </a>*
- Create an organization. &nbsp; Refer: &nbsp; *<a href="https://docs.strangebee.com/thehive/administration/organizations/create-an-organization/"> Create Organization </a>* <br>
- Add users. &nbsp; Refer: &nbsp; *<a href="https://docs.strangebee.com/thehive/user-guides/organization/configure-organization/manage-user-accounts/create-a-user-account/"> Create User Account </a>*
<br>







