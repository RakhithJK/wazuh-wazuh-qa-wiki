QA team defined a standard for writing each type of issue.


* [New test developments](https://github.com/wazuh/wazuh-qa/wiki/Template:-New-test-developments)
* [Bug Report](https://github.com/wazuh/wazuh-qa/wiki/Template:-Bug-report) 


<details>
  <summary>EPIC</summary>

1. `Title`: The title must be short and descriptive.

2. `Description`: It indicates the general development to do. Each issue involved will have specific information and details.

   2.1. `PRs Involved`: List of PRs that are working on the issues contained by this Epic. It helps to follow the details.

4. `Related Issues`: In this section add the related issue numbers. Also, do not forget to Mark the Linked Issues in the sidebar and Connected Issues.

5. `Label to add`: feature/{module}, test/{type of test}, team/qa, subteam/{subteam}.

6. `Example`: [Wazuh-DB: Migration of agent-group files](https://github.com/wazuh/wazuh-qa/issues/2504)

</details>

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
