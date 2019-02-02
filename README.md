# Simple Lynis Deployment

A simple Lynis deployment Playbook using Ansible. It can be run on a single node or multiple nodes.

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

What things you need to install the software and how to install them

```
- Ansible 2.7.6+ (but will probably run on lower versions)
- Ubuntu 18.04.1 LTS
- Lynis Enterprise or trial Serial-Key (community edition.. soon(tm))
```

## Installing

A step by step series of examples that tell you how to get a development env running

Clone this repository:

```
git clone https://github.com/Christo-Stoyanov/lynis-ansible
```

Switch to 'lynis-ansible' directory:

```
cd lynis-ansible
```

### Setup your hosts 
You need to change the hosts file to meet your env requirements. If you dont, it will fail!

The inventory file 'hosts' defines the nodes in which lynis should be configured.

        [myhosts]
        www.server.example.com ansible_user=ubuntu ansible_python_interpreter=/usr/bin/python3

'ansible_user' set the ssh user (on the target) for ansible, use ubuntu for a new default installation of Ubuntu. 
'ansible_python_interpreter' it tells ansible where python3 bin.

```
vi ./hosts
```

### Setup lynis-ansible config
 
Review var/config file and add your lynis enterprise or trial key under lynis_license_key:

```
vi ./group_vars/all
```

The default settings are:

```
# Enterprise License key. Go to cisofy.com for a trial 
lynis_license_key: none

# APT/Install settings
lynis_reset_repo: true

# LYNIS run options
lynis_options: "--upload"

# CRONJOB Settings
lynis_cronjob: true
lynis_cronjob_dir:  "/opt/lynis/cronjobs"
lynis_cronjob_file: "lynis_cronjob"
lynis_cronjob_options: "--cronjob --upload"
lynis_cronjob_running: daily
lynis_cronjob_log_dir:  "/var/log/lynis/" 

# MISC Settings, change if you want but i am sure. u will delete the internet!
lynis_main_package_name: lynis
lynis_plugin_package_name: lynis-plugins
lynis_log_dir: "/var/log/lynis"
lynis_bin_location: "/usr/sbin"
```

### Cronjob settings

Default lynis-ansible will install a cronjob. If you don't want it, change the setting in ./group_vars/all to lynis_cronjob: false

```
`lynis_cronjob`  			[default: `true`]  Set to `false` if you dont want a cronjob
`lynis_cronjob_running`  	[default: `daily`] Available options:  reboot, yearly, annually, monthly, weekly, daily, hourly
```


### Running / Deployment

Lynis-ansible can be deployed using the following command:

```
ansible-playbook -i hosts lynis.yml
```

Once done, you can check the results [on cisofy.com](http://cisofy.com) or your local installation of lynis.


## Help / Trouble?

Report any issues please! If you run in trouble, set 'lynis_reset_repo' in ./group_vars/all

```
vi ./group_vars/all
#add/change ->  lynis_reset_repo: true
```

## Contributing

Please read [CONTRIBUTING.md](https://github.com/Christo-Stoyanov/lynis-ansible/contribute) for details on our code of conduct, and the process for submitting pull requests to us.

## Authors

* **Christo Stoyanov** - *Initial work* - [CS](https://github.com/Christo-Stoyanov/)

See also the list of [contributors](https://github.com/Christo-Stoyanov/lynis-ansible/contributors) who participated in this project.

## License

This project is licensed under the GPL License - see the [LICENSE.md](LICENSE.md) file for details

