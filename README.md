# CodeReadyContainers
This repo can be used to setup a local code ready containers (CRC) installation (openshift 4 equivalent) on Ubuntu/Debian.

## Requirements
To install CRC with this repo, you need to:
* [Setup a free redhat account](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&cad=rja&uact=8&ved=2ahUKEwjGw8bPx9L3AhUNCewKHT11D7EQFnoECAYQAQ&url=https%3A%2F%2Fwww.redhat.com%2Fwapps%2Fugc%2Fregister.html&usg=AOvVaw0XN5agOwobjJWWJmiitUP7) to download a mandatory pullsecret file
* Have 4 cpu cores (6 when cluster monitoring is enabled)
* 9GB of memory (15GB when cluster monitoring is enabled)
* 35GB of free storage space
* One of the following operating systems:
    - Ubuntu 18.04 LTS or later
    - Debian 10 or later

Note that the cluster monitoring is disabled by default, because it increases the cpu and memory requirements so much. To enable it, set <code>enable_cluster_monitoring: true</code> in the [default vars file](ansible/roles/download_install_crc/defaults/main.yml).

## Install
When you've created a redhat account, download the pullsecret file and move it to "ansible/pullsecret.json".

Then:
  * <code>cd ansible && ./venv-setup.sh && source .venv/bin/activate</code>
  * Start the installation with <code>ansible-playbook playbook.yaml</code> and provide your sudo password when prompted
  * After the installation has finished, refresh your shell with <code>su $(whoami)</code>
  * Login to the openshift webconsole at https://console-openshift-console.apps-crc.testing with <code>developer/developer</code> or <code>kubeadmin/kubeadmin</code>

### Known Issues
If the installation fails, it can get stuck and prevent new installations to run succesfully.

See https://github.com/code-ready/crc/issues/1027 on how to resolve this.

## Deinstall
To deinstall CRC, run
* <code>cd ansible && ./venv-setup.sh && source .venv/bin/activate</code>
* <code>ansible-playbook deinstall.yaml</code> and provide your sudo password when prompted

This will cleanup all packages, files, caches etc. that were created during the CRC installation. 

## Update
If you want to update an existing CRC installation to a new release, follow the listed steps in order. Keep in mind that only versions > "2.4.0" are tested and "latest" is not a valid version specifier. Only specific versions like "2.4.1" or "2.6.0" are valid for the installation.

1. Deinstall the current CRC installation as described in "Deinstall" above
2. Find the release you want to install at https://developers.redhat.com/content-gateway/rest/mirror/pub/openshift-v4/clients/crc/
3. Configure the version to install in the [default vars file](ansible/roles/download_install_crc/defaults/main.yml)
4. Install the new CRC release as described in "Install" above

**WARNING:** By the time of writing, there is no way to update an existing CRC installation in place, so we have to deinstall/install the current CRC installation. This results in deleting the openshift cluster including *all data and deployments in it*.

## Cheatsheet
| Command      | Description                                        |
|--------------|----------------------------------------------------|
| crc status   | Display the status of the current CRC installation |
| crc start    | Start a stopped CRC installation                   |
| crc stop     | Stop a started CRC installation                    |
| crc console  | Open the OpenShift webconsole in your browser      |
