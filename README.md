# CodeReadyContainers
This repo can be used to setup a code ready containers (openshift 4 equivalent) installation on your local development machine. See https://access.redhat.com/documentation/en-us/red_hat_openshift_local/2.2/html/getting_started_guide/introducing_gsg for more information.

The installation was tested with molecule on Ubuntu, Debian, Fedora and RHEL. See the molecule.yml for more the details.

# Cheatsheet
crc console
crc console --credentials

# Requirements
To be able to run code ready containers, you need to [setup a redhat account](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjGw8bPx9L3AhUNCewKHT11D7EQFnoECAYQAQ&url=https%3A%2F%2Fwww.redhat.com%2Fwapps%2Fugc%2Fregister.html&usg=AOvVaw0XN5agOwobjJWWJmiitUP7) to access the documentation and create a mandatory pullsecret file. 

check if your system meets the [system and hardware requirements](https://access.redhat.com/documentation/en-us/red_hat_openshift_local/2.2/html/getting_started_guide/installation_gsg#minimum-system-requirements_gsg).
    system requirements
    redhat account
        pullsecret path var and link to docs

# Usage
relogin after playbook run to enable groups
enable_cluster_monitoring (RAM + cpu requirements)

# Update

# Uninstall
crc cleanup && crc delete
rm -rf ~/.crc && sudo rm /usr/local/bin/oc && sudo rm /usr/local/bin/crc-*

# Known Issues
https://github.com/code-ready/crc/issues/1027 && rm -rf ~/.crc
