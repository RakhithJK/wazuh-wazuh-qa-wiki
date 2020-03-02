## Local environment creation

 The `devel` branch on `wazuh-automation` repository contains the Vagrantfile and dependencies used on the current local testing.

Using the  `sudo vagrant up` command on `wazuh-automation/vagrant/fim_tests` path should be enough to correctly create the ready to use environment.

## Local environment provisioning

 The `fim-system-dev-environment` branch on `wazuh-qa` repository holds the related ansible required files:
```
#Hosts file
wazuh-qa/tests/system/provisioning/dev_hosts
#Playbook
wazuh-qa/tests/system/provisioning/launch_environment.yml

#Launch command
sudo ansible-playbook launch_test_scenario.yml -i dev_hosts

```


## Local environment scenario deploy

After creating and provisioning the environment, scenarios can be launched over it. `feature-493-create-test-scenarios` on `wazuh-qa` repository holds the different scenarios. Each scenario has its own launch playbook: `launch_test_scenario.yml`