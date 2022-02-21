It's a Draft

## EPIC
    - General Description 
    - Issues linked with description
    - Labels

## GENERAL ISSUE
    - Description 
    - Requeriments
    - Testing Required (Gherkin/text)  / Fix to applied / List affected tests.
    - DoD
    - Labels

<details>
  <summary>BUGS</summary>

1. Title:
     
     ```bash
       The title must be short and descriptive
       Schema: [Module][description]

       Example: FIM: Real-time: a folder that was deleted and restored/created is not monitored
     ```

2. Description: 
     ```bash
       The description must be precise indicating what was detected, you can also add information
       on the cause of the failure if you have information about it.

       Example:
       If a folder is monitored in real-time mode, and this folder was deleted, when is created again there is no monitoring again.
       It generates a risk for the user, who believes that he is being monitored.
     ```

3. Details of environment:
     ```bash
       In this section considered add all the information related to the environment
       
       Example:
       Wazuh version     Installation type                  Branch                      Platform
            4.4            Manager/Agent    dev-10771-agent-groups-files-to-wazuh-db	    Centos8
     ```

4. Local_internal_options:
     ```bash
       Examples:
       
       - syscheck.debug=2
       - agent.debug=2
       - monitord.rotate_log=0

       Note: If local internal options are not required please add: Local internal options are not required
     ```
5. Step to reproduce: 

   ```bash
       This section must specify step by step what has been done in order to reproduce the bug. In as much detail as possible.

       Example:
       1) Add folder in ossec.conf with realtime="yes" : 
            <directories realtime="yes">/test1</directories>

       2) Create a folder to monitor: 
            mkdir /test1

       3) Restart agent: 
            /var/ossec/bin/wazuh-control restart

       4) Verify on ossec.log that the directory set for real time monitoring

       5) Delete the folder 
            rm -d /test1

       6) Verify on ossec.log that the folder was deleted

       7) Create the folder again. 
            mkdir /test1

     ```

6) Current Results:
     ```bash
       Example: The folder is not added back to monitoring.
     ```

7) Expected Results:
     ```bash
       Example: The folder should be added to the monitoring
     ```

8) Evidence: 
     ```bash
       This section is for attaching all the evidence, for example, images and videos. Also is for attach file of logs.
     ```
9) Label to add:
     ```bash
       - Bug
       - core/{module}
       - qa/report
       - reporter/{subteam}

     ```

</details>

## PR
    - Description (generic)
    - Labels
    - Creator: 
         - OS, version, branch, type env (agent/manager)
         - Changes applied. Execution of module complete for each OS supported.(3 executions local - 3 executions on Jenkins)
         - Rules to applied.
    - Reviewer: To approved, attach 3 executions on Jenkins or 1 executions local and Jenkins. 
    - Note: The results are in a table with test path, type (manager/agent), OS, Type Executions (Jenkins/local), Status with link, date, executed by, Note (if it's necessary)
***
