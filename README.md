# SwitchBoard
SwitchBoard is a corporate PBX phone system built on Asterisk for telephony and Grails for the management interface.

By leveraging the Asterisk Real-time capabilities introduced in Asterisk 12 and the flexible data-sourcing made available by Grails, SwitchBoard can fulfill a company's phone system needs from a user-friendly web interface.

## Primary features
- Call queues
- Multiple extension locations available for each registered Agent, including Mobile lines
- Direct lines to specific Agents, regardless of their current location
- Call conferencing and parking
- Call reports with audio recordings
- Run-time configuration changes that can be changed from within the interface

## Switchboard and Vagrant
For the sake of easy testing and instance creation, this project has utilized [Vagrant](http://www.vagrantup.com) for easy creation of the necessary virtual machines to run a Switchboard instance.

Each instance is comprised of three VMs working in concert; an Asterisk VM, a Tomcat VM to run the Switchboard interface Grails application and a PostgreSQL instance which stores both Grails domain objects and the Real-time data maintained by Asterisk. These three VMs have been organized into their own Github projects, with the source files for the interface Grails application available in the `interface-source` directory.

## Creating a Switchboard instance using Vagrant
The first step in spinning up a Switchboard instance is to clone this repo:

```git clone https://github.com/MarkProminic/PhoneSystem```

Next you will need to install Vagrant on your machine by following the instructions [here](https://www.vagrantup.com/docs/installation/).

Before starting any of the Vagrant VMs for Switchboard you will want to ensure that the following IP addresses are available on your local network and add them to your hosts file for easy access:
```
172.16.1.90     vagrant.postgres
172.16.1.91     vagrant.asterisk
172.16.1.93     vagrant.interface
```
If these are not available and cannot be made available on your network, check the ['Changing Vagrant VM IPs'](#changing-vagrant-vm-ips) section below.

Following that, the only remaining step is starting each of the three VMs of the Switchboard instance; first the PostgreSQL VM, followed by the Asterisk VM and lastly the Tomcat instance running the interface Grails application.

This can be done by entering the following commands in a terminal:
```
cd PhoneSystem
vagrant up
```
This will run the Vagrantfile in `VagrantPostgres` to start and provision the VM using VirtualBox. Vagrant may prompt you to specify which network device to bridge to if your machine has more than one, just choose the one that is used for public network access.

Repeat this proceess for the `VagrantAsterisk` and then the `VagrantPhoneInterface` directories, following any instructions given by Vagrant. Lastly, navigate to (vagrant.interface:8080) to view the SwitchBoard interface. Default login credentials for a placeholder admin user can be found in `default_admin_cred.txt` in the `VagrantPhoneInterface` directory.

## Changing Vagrant VM IPs
The IPs of the three Vagrant VMs are configured statically by the Vagrantfile used to provision them. In order to change these IP adresses, you will need to change the configuration in the Hosts.yml in the root of this repo, for each VM(aka Host) and change your hosts file to reflect these changes. For example, in order to change the IP of the VagrantAsterisk VM to `172.16.0.0`, you need only change the following line:
```
    gateway: "172.16.66.1"
    identifier: sb-01.int.dc-01.m4kr.net
    ip: "172.16.66.3"
    mac: 52540fc9cf91
    name: sb-01.mydomain.net
    netmask: "255.255.0.0"
```
 to include the desired address instead:
 
 ```
    gateway: "172.16.66.1"
    identifier: sb-01.int.dc-01.m4kr.net
    ip: "172.16.0.0"
    mac: 52540fc9cf91
    name: sb-01.mydomain.net
    netmask: "255.255.0.0"
 ```
 
 All internal references to the VagrantAsterisk VM are done via it's hostname `vagrant.asterisk`, meaning that no code change is required in order for the other two VMs to be able to locate the VagrantAsterisk VM. There is a `hosts` file that becomes each VM's `/etc/hosts` and this is set as template using the values in Hosts.yml, so the following would comprise a correct `hosts` file for the above change without any modification on your part:
 
 ```
172.16.1.90 vagrant.postgres
172.16.0.0  vagrant.asterisk
172.16.1.93 vagrant.interface
 ```
 
 ## Questions?
 Any further questions regarding Switchboard can be sent via email to the lead developer Luis Alcantara at luis@prominic.net and Automation Engineer Mark Gilbert at Mark.Gilbert@Prominic.net

## Built With
* [Vagrant](https://www.vagrantup.com/) - Portable Development Environment Suite.
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) - Hypervisor.
* [Ansible](https://www.ansible.com/) - Virtual Manchine Automation Management.
* [vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest) - A Vagrant plugin to keep your VirtualBox Guest Additions up to date.
* [vagrant-reload](https://github.com/aidanns/vagrant-reload) - A Vagrant plugin that allows you to reload a Vagrant plugin as a provisioning step.
* [vagrant-disksize](https://github.com/sprotheroe/vagrant-disksize) - A Vagrant plugin to resize disks in VirtualBox.


## Contributing

Please read [CONTRIBUTING.md](https://www.prominic.net) for details on our code of conduct, and the process for submitting pull requests to us.

## Authors

* **Mark Gilbert** - *Automation* - [Makr91](https://github.com/Makr91)
* **Luis Alcantara** -- *Project Lead / Grails Development* -  [luisalcantara](https://github.com/luisalcantara)

See also the list of [contributors](https://github.com/MarkProminic/Switchboard/graphs/contributors) who participated in this project.

## License

This project is licensed under the SSPL v3 License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
#   V a g r a n t - M o n g o D B  
 