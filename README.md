<p align="center">
  <h3 align="center">Runtime Fabric Automation using Ansible</h3>
</p>

<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#about-the-proof-of-concept">About The Proof of Concept</a></li>
    <li><a href="#built-with">Built With</a></li>
    <li><a href="#architecture">Architecture</a></li>
    <li><a href="#setup">Setup</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The RTF Platform Automation

This POC aims to implement Automation for Runtime Fabric.
To address this objective, the POC will implement:

* Create RTF Appliance
* Setup Ansible in RTF Appliance
* Updating RTF using Ansible
* Updating OS using Ansible

## Built With
* [Ansible](https://www.ansible.com/)

## Architecture

<img src="/images/architecture.png" alt="Ansible Controller and RTF Architecture" height="800" width="600" />


## Setup

* [API Platform Automation - Create RTF Appliance]

1. Create Runtime Fabric in Anypoint Platform
2. Retrieve the Verification Code
3. Retrieve / Encrypt the Mule License Key
4. Create Runtime Fabric in Anypoint Platform
5. Setup Terraform in a workstation. This will work even from personal workstation.
6. Set Azure Credentials in rtfinstall.bat
7. Create a Key Pair in the AWS/ Azure console, download the key pair to the terraform directory
8. Execute terraform init, then terraform apply
9. Check the created infra in the cloud console
10. Go to /opt/anypoint/runtimefabric and execute sudo ./init.sh. This will execute 17 Steps for Runtime Fabric installation.

* API Plaform Automation - Setup Ansible in RTF Appliance


| Step | Command |
| ------ | ------ |
| 1. Setup Ansible connection. Follow guide here :| https://blog.cloudthat.com/how-to-install-ansible-on-rhel-8/ |
| 2. Install Python 3 | sudo yum install python3 -y |
| 3. install python pip | sudo yum -y install python3-pip |
| 4. in RHEL, Ansible can't using root. Create ansible user "ansiblecontrol" | sudo useradd ansiblecontrol |
|  | sudo -s |
|  | sudo passwd ansiblecontrol |
| 5. create managedhost users in workers  | sudo useradd managedhost |
| 6. add ansiblecontrol to sudoers | echo "ansiblecontrol ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers |
| 7. add managedhost to sudoers | echo "managedhost ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers |
| 8. change managedhost passwords | sudo passwd managedhost |
| 8. enable passwordtext to access managedhost | sudo vi /etc/ssh/sshd_config |
| | PubkeyAuthentication yes |
| | PasswordAuthentication yes |
| | PasswordAuthentication yes |
| 9. reload sshd service | sudo service sshd reload |
| 10. test logging in to worker vm using root, ansiblecontrol user | ssh managedhost@<ip> |
| 11. generate keys for ansiblecontrol | ssh managedhost@<ip> |
| 12. transfer keys of ansiblecontrol user to managedhost | ssh-copy-id managedhost@<ip>> |
| 13. revert passwordtext to access managedhost | sudo vi /etc/ssh/sshd_config |
| | PasswordAuthentication no |
| 14. reload sshd service | sudo service sshd reload |
| 15. test logging in to worker vm using root, ansiblecontrol user, this time without password | ssh managedhost@<ip> |
| 16. install ansible, using ansiblecontrol and managedhost users | su -ansiblecontrol |
| | su -managedhost |
| | pip3 install ansible -user |
| 17. test ansible installation | ansible --version |
| 18. install git in ansiblecontrol | sudo yum install git |
| 19. clone rtf ansible project | sudo git checkout https://github.com/mobius-next/rtf-ansible.git |
| 20. update the IP addresses in the env inventory files |  |
| 21. ping the 2 managedhost vm | ansible all -m ping |

* API Platform Automation - Updating RTF using Ansible

| Step | Command |
| ------ | ------ |
| 1. Run the rtf-upgrade.yml playbook using needed inventory and by default "rhel" role/task | ansible-playbook -i env/dev rtf-upgrade.yml |
| 2. add flags for dry run, and verbose logs | --check -v |


* API Platform Automation - Updating OS using Ansible

| Step | Command |
| ------ | ------ |
| 1. Run the os-upgrade.yml playbook using needed inventory and by default "rhel" role/task | ansible-playbook -i env/dev os-upgrade.yml |
| 2. add flags for dry run, and verbose logs | --check -v |
| 3. start at specific task, refer to  roles/rhel/task/main.yml for tasks | --start-at-task="<taskname>" |
| 4. maintain uniform apps installed by using "install packages" task.  | --start-at-task="install packages" |

<!-- CONTRIBUTING -->
## Roadmap

1. Install in a Ansible Controller
2. Configure connection to RTF Controllers
3. Configuration Management of VM's - To maintain similar configuration for a certain criteria (purpose, environment). 


<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request


<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE` for more information.



<!-- CONTACT -->
## Contact

Mobius NEXT - contact@mobiusnext.com

Project Link: (https://github.com/mobius-next/rtf-ansible)

