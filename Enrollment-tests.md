- Test registration (Jenkins): Pipeline that installs a manager and agent, based on launching playbooks that execute agent-auth commands for a number of use cases. The test fails if the rc of the agent auth command is different from 0. The tested use cases can be seen here https://github.com/wazuh/wazuh-jenkins/blob/master/jenkins-files/tests/test_registration.groovy#L59-L140

- Test integration (Automated) --> 
   - test_enrollment: 
      + test_agent_auth_enrollment.py: Test enrollment behavior using agent-auth binary with different parameters.
        - The command is built with parameters to launch with agent-auth
        - agent-auth command is launched
        - If the command parameters are correct, the message sent to 1515 is checked for the expected content and format.
        - If the configuration is incorrect, the corresponding error message is checked in the log.
    
      + test_agentd_enrollment.py: Tests enrollment behavior based on the values of its configuration.
        - ossec.conf configuration is modified
        - Agent is restarted
        - If the configuration is correct, the message sent to 1515 is checked for expected content and format.
        - If the configuration is incorrect, the corresponding error message is checked in the log.
      + (4.4) test_agentd_server_address_configuration.py: Tests that the agent has connection with the IP indicated by both IPV4 and IPV6.
         - Modify ossec.conf to set the server_address of the agent
         - Restart the agent
         - If the IP address is correct, log message is checked.
         - If the IP address is invalid, log message is checked.
- System test (Not automated):
    - test_cluster:
      + test_agent_enrollment: Tests that the enrollment works correctly in an agent pointing to a worker.
        - Add `wazuh-worker1` in the server_address of the agent.
        - Restart master, worker and agent
        - Check the enrollemnt logs in master, worker and agent
        - Check the client.keys of worker and agent
        - Check that the agent is `Active`.
    - (4.4) test_agent_auth:
      + test_agent_auth.py: Tests that the enrollment using the agent-auth binary works correctly
        - Launch agent-auth command with different IPv4, IPv6 and DNS `wazuh-manager`.
        - Logs are checked in the manager and in the agent
        - Compare the client.keys of manager and agent
        - Check that the agent is `Active`.
    
    - (4.4) test_enrollment:
      + test_enrollment.py: Tests that the enrollment works correctly with different server_address in configuration
        - Change agent configuration by adding different IPv4, IPv6 and DNS wazuh-manager
        - Restart manager and agent
        - Logs are checked in manager and agent
        - Compare client.keys of manager and agent
        - Checks that the agent is `Active`.
