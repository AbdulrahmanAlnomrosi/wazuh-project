# wazuh-project
this repo is made to showcase the active response in wazuh how to integrate virustotal(VT) to wazuh 

## prerequisite:
**wazuh server and an agent**

**a virusTotal api key**

to deploy a wazuh server check the [wazuh documentation an ubuntu server ](https://documentation.wazuh.com/current/quickstart.html)

to deploy a wazuh agent check the [wazuh documentation for an ubuntu agent ](https://documentation.wazuh.com/current/installation-guide/wazuh-agent/wazuh-agent-package-linux.html)

## File integrity monitoring (FIM) configuration 

first in the _**agent Virtual machine(Vm)**_ create a folder that we will monitor using the mkdir command 


<img width="1918" height="878" alt="ubuntu agent 1-2026-11-23-22-54-26" src="https://github.com/user-attachments/assets/4f30315a-a240-4705-a5af-614bac83026c" />


now in the _**server Vm**_ enter the wazuh dashboard locate the agents managment and enter groups section and press the enter the **edit group configuration**


<img width="1748" height="732" alt="image" src="https://github.com/user-attachments/assets/7a9b03d9-c907-4931-92a2-b680c2dbbe33" />


 and type the agent configuration which is the following with the name of the directory inside the <directory> command and save


 <img width="1918" height="878" alt="Ubuntu 64-bit (2)-2026-11-23-23-00-05" src="https://github.com/user-attachments/assets/40e69141-95ad-4435-8a1d-ba6e0ecd276a" />


 finally in the **__agent Vm_** restart the wazuh agent 


 <img width="1918" height="878" alt="ubuntu agent 1-2026-11-23-23-02-27" src="https://github.com/user-attachments/assets/f766c9cb-fd77-4b33-8f2d-a5bd8719c741" />


### checking if the FIM work 

to check if the FIM is working go create a simple file in the directory (first you need to start a bash shell as root)


<img width="1918" height="878" alt="ubuntu agent 1-2026-11-23-23-47-46" src="https://github.com/user-attachments/assets/c740102e-c28e-4827-bc56-8d2e6e4cedf9" />


if it works it should show in the FIM: Recent events


<img width="1918" height="878" alt="Ubuntu 64-bit (2)-2026-11-23-23-49-10" src="https://github.com/user-attachments/assets/1eaf28ab-fb52-44f6-923c-d3c050fb0054" />


## integrate VirusTotal 

to integrate Vt first go to the Terminal on the **_Server Vm_** and enter the config 


<img width="1918" height="878" alt="Ubuntu 64-bit (2)-2026-11-23-23-53-23" src="https://github.com/user-attachments/assets/477ca349-b805-4d6d-b7db-39a99045f0a0" />


scroll to the end and add the integration section and put your Vt api key inside the <api_key> </api_key> section


<img width="1918" height="920" alt="Ubuntu 64-bit (2)-2026-11-23-22-27-06" src="https://github.com/user-attachments/assets/de9b391f-844b-4fd2-8ffb-2c3a643a8594" />


finally restart the wazuh manager using the following command 


<img width="1918" height="878" alt="Ubuntu 64-bit (2)-2026-11-23-23-57-53" src="https://github.com/user-attachments/assets/8f1b5ac4-69b0-4d63-8a9a-166158222247" />


to check if the integration works in the **_agent Vm_** download eicar file ( a safe file that check if the antivirus works )


<img width="1918" height="878" alt="ubuntu agent 1-2026-11-23-23-59-59" src="https://github.com/user-attachments/assets/303a5d07-a31b-4c73-b94c-304e05da2932" />


and you will see the Vt response in the **Threat hunting** section of the wazuh


<img width="1918" height="878" alt="Ubuntu 64-bit (2)-2026-11-24-00-03-27" src="https://github.com/user-attachments/assets/030cac2b-6759-40b2-a313-bd1b2c486cd3" />


## deploying the active response  

first in the **_agent Vm_** install **jq**


<img width="1918" height="878" alt="ubuntu agent 1-2026-11-24-00-07-47" src="https://github.com/user-attachments/assets/13214934-b008-40db-82e5-7ebb3cd30d0e" />

then go to the /var/ossec/active-response/bin/ and create a bash script 


<img width="1918" height="878" alt="ubuntu agent 1-2026-11-24-00-11-33" src="https://github.com/user-attachments/assets/ee770fdd-f9a8-4f6d-9151-5ecd474f667b" />


and copy the script shown in the [proof of concept ](https://documentation.wazuh.com/current/proof-of-concept-guide/detect-remove-malware-virustotal.html) on the wazuh site


<img width="1918" height="878" alt="ubuntu agent 1-2026-11-24-00-13-51" src="https://github.com/user-attachments/assets/ace71501-c30b-42c6-83d7-762d82371e0f" />

and then change the file ownership and permission using the following command


<img width="1918" height="878" alt="ubuntu agent 1-2026-11-24-00-21-15" src="https://github.com/user-attachments/assets/24df5db6-f26e-4da5-8381-ef0424d01607" />


in the **_server Vm_** go to the conf text (using the xdg command)  and search for the active response and type the following in the end 


<img width="1918" height="920" alt="Ubuntu 64-bit (2)-2026-11-23-22-29-12" src="https://github.com/user-attachments/assets/08073315-74c3-43b7-911b-ed3f9892db86" />


now restart the wazuh manager in the **_server Vm_** and the wazuh agent in the **_Agent Vm_**

## final testing

now go the **_agent Vm_** and install the eicar file again and see it get removed in realtime and at the same time is shown as deleted in the wazuh threat hunting section 


<img width="1918" height="529" alt="Ubuntu 64-bit (2)-2026-11-24-00-30-42" src="https://github.com/user-attachments/assets/a95b38ce-e177-43c5-8a8f-8447a36c4369" />







