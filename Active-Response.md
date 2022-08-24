ACTIVE RESPONSE 

ANALYSISD
        
Test_os_exec

           - Description: Check if 'active response' messages are sent in the correct format depending on the agent version used. For this purpose, 
                          simulated agents with different properties are created, and messages in two formats (string and JSON) are sent to the socket of the 'wazuh-analisysd' daemon, which should process them correctly.

           - Cases:  
                - Case 1: Local AR, new agent (version 4.2)
                - Case 2: Local AR, old agent (version < 4.2)
                - Case 3: Local AR, old and new agent
                - Case 4: Local AR, old and new agent with extra_args
                - Case 5: Local AR, old and new agent with timeout
                - Case 6: All AR, old and new agent

           - Platform: Linux
           - Tier: 0
           - Minimum Version: 4.2.0