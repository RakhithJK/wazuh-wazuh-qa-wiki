It's a Draft

---

<details>
  <summary>EPIC</summary>

1. `Title`: The title must be short and descriptive.

2. `Description`: It indicates the general development to do. Each issue involved will have specific information and details.

   2.1. `PRs Involved`: List of PRs that are working on the issues contained by this Epic. It helps to follow the details.

4. `Related Issues`: In this section add the related issue numbers. Also, do not forget to Mark the Linked Issues in the sidebar and Connected Issues.

5. `Label to add`: feature/{module}, test/{type of test}, team/qa, subteam/{subteam}.

6. `Example`: [Wazuh-DB: Migration of agent-group files](https://github.com/wazuh/wazuh-qa/issues/2504)

</details>

---

## GENERAL ISSUE
    - Description 
    - Requeriments
    - Testing Required (Gherkin/text)  / Fix to applied / List affected tests.
    - DoD
    - Labels
    - Example: [IT FIM: Check 'max_fd_win_rt' behaviour ](https://github.com/wazuh/wazuh-qa/issues/2585)
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
	<td style="width: 175px;">-</td>
	<td style="width: 103px;">-</td>
	<td style="width: 97px;">-</td>
	<td style="width: 97px;">-</td>
	</tr>
	</tbody>		 
    </table>

4. `Local_internal_options`: Set local_internal_options.conf. If local internal options are not required please add: **Local internal options** are not required
5. `Step to reproduce`: This section must specify step by step what has been done in order to reproduce the bug. In as much detail as possible.
6. `Current Results`: Add what is the current result after executed "steps to reproduce" 
7. `Expected Results`: Add what is the expected after executed "steps to reproduce" 
8. `Evidence`: This section is for attaching all the evidence, for example, images and videos. Also is for attach file of logs.
9. `Label to add`: Bug, core/{module}, qa/report, qa/report/{subteam} or qa/reporter/{subteam}/tdd
10. `Example`: [FIM: Real-time a folder that was deleted and restored/created is not monitored](https://github.com/wazuh/wazuh/issues/12350)

</details>

---

<details>
  <summary>PR</summary>

1. `Title`: The title must be short and descriptive.
   > **Format:** [Type of test]:[Module]-[Issue] Brief Description.
2. `Description`: It should be precise indicating what was detected, changes applied' or cases cover. 
3. `Related Issues`: In this section add the related issue numbers. Also, do not forget to Mark the Linked Issues in the sidebar and Connected Issues.
4. `Details of environment`: In this section considered add a table with all the information related to the environment.
    <table>
        <tbody>
	<tr>
	<td style="width: 175px;">Wazuh version</td>
	<td style="width: 79px;">Installation type</td>
	<td style="width: 97px;">Branch </td>
	<td style="width: 97px;">Platform</td>
	</tr>
	</tbody>		 
    </table>

5. `Local_internal_options`: Set local_internal_options.conf. If local internal options are not required please add: **Local internal options** are not required
6. `Test Executions:` Attach all executions with results.
    * `Creator`: Three local executions and three Jenkins executions of the complete test module, on each supported system for the module.
    * `Reviewer`: To approve, the reviewer has to add three Jenkins executions, **or** a local and a Jenkins execution of the complete test module, on each supported system for the module.
8. `Rules`: List of rules applied to development
   > - [x] Proven that tests **pass** when they have to pass.
   > - [x] Proven that tests **fail** when they have to fail.
   > - [x] Python codebase satisfies PEP-8 style style guide. `pycodestyle --max-line-length=120 --show-source --show-pep8 file.py`.
   > - [x] Python codebase is documented following the Google Style for Python docstrings.
   > - [x] The test is documented in wazuh-qa/docs.
9. `Label to add`: feature/{module}, test/{type of test}, team/qa, subteam/{subteam}.
10. `Example`: [IT - WDB - 2532- Test set_agent_groups WDB command](https://github.com/wazuh/wazuh-qa/pull/2602)

</details>

---