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

---

<details>
  <summary>BUGS</summary>

1. `Title`: The title must be short and descriptive.
2. `Description`: The description must be precise indicating what was detected, you can also add information
       on the cause of the failure if you have information about it.
3. `Details of environment`: In this section considered add all the information related to the environment.
    <table>
        <tbody>
	<tr>
	<td style="width: 175px;">Wazuh version</td>
	<td style="width: 79px;">Installation type</td>
	<td style="width: 97px;">Branch </td>
	<td style="width: 97px;">Platform</td>
	</tr>
	<tr>
	<td style="width: 175px;">4.4</td>
	<td style="width: 103px;">Manager/Agent</td>
	<td style="width: 97px;">dev-10771-agent-groups-files-to-wazuh-db</td>
	<td style="width: 97px;">Centos8</td>
	</tr>
	</tbody>		 
    </table>

4. `Local_internal_options`: Set local_internal_options.conf
5. `Step to reproduce`: This section must specify step by step what has been done in order to reproduce the bug. In as much detail as possible.
6. `Current Results`.
7. `Expected Results`.
8. `Evidence`: This section is for attaching all the evidence, for example, images and videos. Also is for attach file of logs.
9. `Label to add`: Bug, core/{module}, qa/report, reporter/{subteam}.
10. `Example`: [Real-time: Doesn't monitor a folder deleted and created again.](https://github.com/wazuh/wazuh/issues/12350)

</details>

---

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
