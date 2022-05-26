* molecule tests
  * check if oc login command works with credentials from 'crc console --credentials'
  * check if webconsole is accessible and a login can be done with selenium
  * deploy a statefulset and check the rollout status

# CodeReadyContainers
This repo can be used to setup a code ready containers platform (openshift 4 equivalent) on Ubuntu/Debian.
The installation was tested with molecule on Ubuntu 20.04 LTS. See the [molecule.yml](ansible/roles/download_install_crc/molecule/default/molecule.yml) for more the details.

# Requirements
To be able to setup code ready containers with this repo, you need to:
    * [setup a free redhat account](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjGw8bPx9L3AhUNCewKHT11D7EQFnoECAYQAQ&url=https%3A%2F%2Fwww.redhat.com%2Fwapps%2Fugc%2Fregister.html&usg=AOvVaw0XN5agOwobjJWWJmiitUP7) to access the documentation and create a mandatory pullsecret file
    * have 4 physical CPU cores (6 when cluster monitoring is enabled)
    * 9GB of RAM (15GB when cluster monitoring is enabled)
    * 35GB of free storage space
    * One of the following operating systems:
        - Ubuntu 18.04 LTS or later
        - Debian 10 or later
# Usage
After you have created a redhat account, download the pullsecret file and place it in 'ansible/pullsecret.json'.

Afterwards:
    * Navigate to the ansible dir with <code>cd ansible</code>
    * Run the setup script for the virtual python environment with <code>./venv-setup.sh</code>
    * Activate the virtual python environment with <code>source .venv/bin/activate</code>
    * Run the playbook to start the crc installtion with <code>ansible-playbook playbook.yaml</code>

When the installation is done, refresh your shell with <code>su $(whoami)</code>. You can get your credentials via <code>crc console --credentials</code> and login to the openshift webconsole at 'https://console-openshift-console.apps-crc.testing'.

# Update
The installation uses the release version of crc specified in the [default vars file](ansible/roles/download_install_crc/defaults/main.yml). If you want to update an installed crc platform to a newer release, follow these steps in order:

1. run the 'deinstall.yaml' playbook from the ansible dir with <code>ansible-playbook deinstall.yaml</code>
2. configure the version to install in the [default vars file](ansible/roles/download_install_crc/defaults/main.yml)
3. install the new crc release with the 'playbook.yaml' from the ansible dir with <code>ansible-playbook playbook.yaml</code> again

By the time of writing, there is no way to update crc in place, so we have to deinstall/configure/install it again.

# Deinstall
To deinstall crc, run the [deinstall playbook](ansible/deinstall.yaml). This will cleanup all installed packages, files, caches etc. that were created during the installation.

# Known Issues
* If the installation fails, it can get stuck and prevent new installations to run succesfully. See https://github.com/code-ready/crc/issues/1027
