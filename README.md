* only download with unarchived if not exists
* molecule tests
  * check if oc login command works via 'crc console --credentials'
  * check if webconsole is accessible and a login can be done with selenium
  * 

# CodeReadyContainers
This repo can be used to setup a code ready containers platform (openshift 4 equivalent) on your local development machine.
The installation was tested with molecule on Ubuntu, Debian and Fedora. See the [molecule.yml](ansible/roles/download_install_crc/molecule/default/molecule.yml) for more the details.

# Requirements
To be able to run code ready containers, you need to:
    * [setup a redhat account](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjGw8bPx9L3AhUNCewKHT11D7EQFnoECAYQAQ&url=https%3A%2F%2Fwww.redhat.com%2Fwapps%2Fugc%2Fregister.html&usg=AOvVaw0XN5agOwobjJWWJmiitUP7) to access the documentation and create a mandatory pullsecret file
    * have 4 physical CPU cores (6 when cluster monitoring is enabled)
    * 9GB of RAM (15GB when cluster monitoring is enabled)
    * 35Gb of free storage space
    * One of the following operating systems:
        - RHEL/CentOS 7.5 or later, including 8.x
        - One of the latest two stable Fedora releases
        - Ubuntu 18.04 LTS or later
        - Debian 10 or later
# Usage
After you have created a redhat account, download the pullsecret file and place it in 'ansible/pullsecret.json'.

Afterwards:
    * Navigate to the ansible dir via <code>cd ansible</code>
    * Run the setup script for the virtual python environment via <code>./venv-setup.sh</code>
    * Activate the virtual python environment via <code>source .venv/bin/activate</code>
    * Run the playbook to start the crc installtion via <code>ansible-playbook playbook.yaml</code>

When the installation is done, refresh your shell with <code>su $(whoami)</code> so the group changes can take effect. You can then get your credentials via <code>crc console --credentials</code> and login to the openshift webconsole at 'https://console-openshift-console.apps-crc.testing'.

# Update
The installation always uses the latest release of crc. If you want to update a previously installed crc platform to the current latest release, run the 'deinstall.yaml' playbook and then the 'playbook.yaml' for the installation again.

By the time of writing, there is no way to update crc in place, so we have to deinstall and install it again.

# Deinstall
To deinstall crc, run the [deinstall playbook](ansible/deinstall.yaml). This will cleanup all installed packages, files, caches etc. that was created during the installation.

# Known Issues
* If the installation fails, it can get stuck and prevent new installations to run succesfully. See https://github.com/code-ready/crc/issues/1027
