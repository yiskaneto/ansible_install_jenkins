# Ansible_Install_Jenkins

Ansible helper runbook to install Jenkins from well known roles

## Installation order

1. Install the requirements.yaml:

    ```bash
    ansible-galaxy collection install -r requirements.yaml
    ansible-galaxy collection role -r requirements.yaml
    ```

1. Run the playbook on the target node:

    ```bash
    time ansible-playbook main.yaml -i <node>, -b -K
    ```

    >Note: you can set the password by using the `jenkins_admin_password` variable but pass the value using a vault file, don't pass your password in plain text.

Note for a clean intallation on the target node we must remove some files:

```bash
sudo dnf remove java-11-openjdk java jenkins java-1.8.0-openjdk-headless -y ; sudo dnf clean all ; sudo dnf clean packages ; sudo rm -rf /var/lib/jenkins /opt/jenkins-cli.jar
```

In case the default version of java needs to be replaced you can run:

```bash
update-alternatives --config java
```
