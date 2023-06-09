# CodeReadyContainers
This repo can be used to setup a local code ready containers (CRC) installation (openshift 4 equivalent) on Ubuntu/Debian.

## Requirements
To install CRC with this repo, you need to:
* [Setup a free redhat account](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjGw8bPx9L3AhUNCewKHT11D7EQFnoECAYQAQ&url=https%3A%2F%2Fwww.redhat.com%2Fwapps%2Fugc%2Fregister.html&usg=AOvVaw0XN5agOwobjJWWJmiitUP7)
* [Download the mandatory pullsecrets file](https://console.redhat.com/openshift/install/pull-secret) and save it in "ansible/pullsecret.json"
* Have 4 cpu cores (6 when cluster monitoring is enabled)
* 9GB of memory (15GB when cluster monitoring is enabled)
* 35GB of free storage space
* One of the following operating systems:
    - Ubuntu 18.04 LTS or later
    - Debian 10 or later

The cluster monitoring is disabled by default, because it increases the cpu and memory consumption by ~50%.
To enable it, set `crc.cluster_monitoring: true` in [vars.yaml](vars.yaml).

## Install
When you've created a redhat account and downloaded the pullsecret file, run:
  * `python3 -m pip install -r requirements.txt`
  * `ansible-playbook install.yaml` and provide your sudo password when prompted
  * After the installation has finished, refresh your groups with `newgrp libvirt`
  * Login to the openshift webconsole at https://console-openshift-console.apps-crc.testing
    with `developer/developer` or `kubeadmin/kubeadmin` credentials

The CRC installation will not be started automatically after you have rebooted or shutdown your system. See the "Cheatsheet" section below for common commands.

### Known Issues
If the installation fails, it can get stuck and prevent new installations to run succesfully. If you have trouble running the playbook consecutively after an installation failed, see https://github.com/code-ready/crc/issues/1027 for more information on how to resolve this.

## Deinstall
To deinstall CRC, run `ansible-playbook deinstall.yaml` and provide your sudo password when prompted.

## Update
If you want to update an existing CRC installation to a new release, set the new version in
the [vars.yaml file](vars.yaml) and run `ansible-playbook update.yaml`. You can find the available
versions at https://developers.redhat.com/content-gateway/rest/mirror/pub/openshift-v4/clients/crc/.

**WARNING:** By the time of writing, there is no way to update an existing CRC installation in place, so we have to deinstall/install the current CRC installation. This will delete openshift cluster including *all data and deployments in it*.

## Cheatsheet
| Command     | Description                                        |
| ----------- | -------------------------------------------------- |
| crc status  | Display the status of the current CRC installation |
| crc start   | Start a stopped CRC installation                   |
| crc stop    | Stop a started CRC installation                    |
| crc console | Open the OpenShift webconsole in your browser      |
