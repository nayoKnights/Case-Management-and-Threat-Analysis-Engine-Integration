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
The architecture consists of TheHive as the centralized incident management and orchestration platform, integrated with Cortex as the automated security analysis engine. 
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
Refer: <a href="https://docs.strangebee.com/thehive/installation/installation-guide-linux-standalone-server/"> Install on Linux </a> &nbsp; or &nbsp; <a href="https://docs.strangebee.com/thehive/installation/automated-installation-script-linux/"> Quick Install on Linux Systems: One-Command Setup </a> <br> <br>
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

Access thehive instance by typing http://<Your server's IP> in the browser <br> to see the interface below. 
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
Organization and users created 
<br> <br>
<img width="1504" height="688" alt="image" src="https://github.com/user-attachments/assets/b6081c74-cd67-4299-ba7a-2039242b361b" /> <br>

Request a community license here: &nbsp; *<a href="https://docs.strangebee.com/thehive/installation/licenses/request-a-community-license/"> Request a community license </a>*

Sample case created in TheHive <br><br>
<img width="956" height="443" alt="{06484EEB-0A1D-4095-B87C-8F550994282D}" src="https://github.com/user-attachments/assets/4f1ce33d-5252-4fbe-ab17-78796b4e5fca" />


### Cortex Setup
Prerequisites
- Docker
- Java 11
  
Refer to the docs here: *<a href= "https://docs.strangebee.com/cortex/installation-and-configuration/step-by-step-guide/"> Step by step guide </a>* &nbsp; or &nbsp; *<a href= "https://docs.strangebee.com/thehive/installation/automated-installation-script-linux/"> One Command Install</a>* 
<br>

What to do on first start: *<a href= "https://docs.strangebee.com/cortex/user-guides/first-start/"> First start<a/>*

### Cortex Screenshot Demos
<br>

<img width="960" height="344" alt="{76B60F3A-B8A1-4149-A284-2C87C27E4F3D}" src="https://github.com/user-attachments/assets/7dd657fa-2587-456b-873d-016d7a5123eb" />

<br>

<img width="959" height="360" alt="{A7D3C9F0-A20F-427F-9824-62520010067B}" src="https://github.com/user-attachments/assets/09609bd5-1629-47ed-9ca6-f1f3a2911cc2" />
<br>

<img width="960" height="380" alt="{5C310FFA-0202-435A-A227-A094F2B1B009}" src="https://github.com/user-attachments/assets/62a9a4ec-6a13-4c9a-a8e4-8514cce86c96" />

<br>
Use this docs to configure analyzers and responders in Cortex: *<a href= "https://docs.strangebee.com/cortex/installation-and-configuration/analyzers-responders/"> Configure Analyzers and Responders<a/>*

## Cortex Integration into TheHive

- Create a user in Cortex and generate an API key for that user.

- Open the file /etc/thehive/application.conf on the TheHive server.
- Check whether the Cortex configuration block already exists in the file. Add the block only if it is not already present.
  <br> Configuration block below
```
cortex {
servers = [
    *{*
      \*name = "cortex"\*

      \*url = "http://CORTEX\\\_IP:9001"\*

      \*auth {\*

        \*type = "bearer"\*

        \*key  = "CORTEX\\\_API\\\_KEY"\*

      \*}\*

      \*refreshDelay = 1 minute\*

    \*}\*

]
}
```
<br>

- Insert the generated Cortex API key into the configuration block.
- Set the Cortex server IP address in the block
- Connect the Cortex server in TheHive. Refer to docs here: *<a href= "https://docs.strangebee.com/thehive/administration/cortex/add-a-cortex-server/"> Add a Cortex server<a/>*

  <br>
Cortex added to Thehive successfully
<img width="960" height="442" alt="{77A85E47-9EA4-45CF-A8AA-6735B098E02C}" src="https://github.com/user-attachments/assets/73c1c5f6-d238-49c8-837a-c4b37f975ce1" />

Running Cortex Analyzers in TheHive Demo
<br>

<img width="951" height="417" alt="{C4374E14-23CD-4311-A625-543E9723B783}" src="https://github.com/user-attachments/assets/2446c4ce-04d0-44f6-9fd8-4165e9f43e3a" /> <br>

<img width="960" height="447" alt="{A3A16D58-177C-46A7-AAC5-1B9D4EB4FDF6}" src="https://github.com/user-attachments/assets/a1475c58-16a2-41ef-aa47-5d5240c34071" />

  







