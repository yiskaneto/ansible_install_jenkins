# Ansible_Install_Jenkins

Ansible helper runbook to install Jenkins from [ansible-role-jenkins](https://github.com/geerlingguy/ansible-role-jenkins/tree/master)

## First time Installation

1. Install the collections and roles from requirements.yaml:

    ```bash
    ansible-galaxy install -r requirements.yaml
    ```

1. Open the main.yaml file and adjust the parameters values as needed.

    >Note: you can set the `jenkins_admin_username` and`jenkins_admin_password` variable values by using an Ansible vault file, see section [Updating existing installation](#updating-existing-installation) for a quick way to do so, the overall idea is to NOT pass these values in plain text.

1. Run the playbook on the target node:

    ```bash
    time ansible-playbook main.yaml -i <node>, -b -K

    ## if run locally
    time ansible-playbook main.yaml -i "$(hostname -i)", -b -K --connection=local
    ```

For a clean installation on the target node we may need to remove some files to avoid package conflicts (only do this if you are definitely not using them), the commands below explain how to do it on RHEL distribution systems but adjust it for Ubuntu:

```bash
sudo dnf remove java-11-openjdk java jenkins java-1.8.0-openjdk-headless -y ; sudo dnf clean all ; sudo dnf clean packages ; sudo rm -rf /var/lib/jenkins /opt/jenkins-cli.jar
```

In case the default version of java needs to be replaced you can run:

```bash
update-alternatives --config java
```

## Updating existing installation

You can update an existing installation by providing the `jenkins_admin_username` and `jenkins_admin_password` variables into a vault file:

1. Create a new Ansible vault:

    ```bash
    ansible-vault create <path to>/jenkins_credentials.yaml
    ```

1. Enter a new password and confirm it.
1. Add the `jenkins_admin_username` and `jenkins_admin_password` parameters with their corresponding values, example:

    ```bash
    jenkins_admin_username: "<username>"
    jenkins_admin_password: "<admin user password"
    ```

1. Save and quit.
1. Now we can run the playbook:

    ```bash
    time ansible-playbook main.yaml -e @<path to>/jenkins_credentials.yaml --ask-vault-pass -i <node>, -b -K

    ## If run locally:
    time ansible-playbook main.yaml -e @<path to>/jenkins_credentials.yaml --ask-vault-pass -i "$(hostname -i)", -b -K --connection=local
    ```
