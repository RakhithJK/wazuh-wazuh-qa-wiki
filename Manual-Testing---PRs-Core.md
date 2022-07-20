The QA team is responsible for performing the different manual tests to improve visibility and add more detail to each test. It is necessary to do: 

# Consideration: Before starting

   - [ ] Verify that PR related is approved and it contains all check green.
   - [ ] Sync with core responsable that this test will start to test. 

# Consideration: Our coverage

1. If there are a new change added in the PR while you are testing: You need to test it again.
2. When do we perform upgrade tests?
3. When do we need to test on more than one supported OS?
4. When should we test on different versions of the same OS?


# Consideration: How to show our evidence

- [ ] Before adding comments:

   - Keep the issue updated.
   - Several related issues were found when running a start/stop/restart. For that reason, we now need to attach evidence that the state of the daemons is as expected after running this command.
   - If the tests require using an agent with the administrator, we must leave evidence that they are connected.
   - Keep indentation in line.
   - Enumerate the steps.
   - Paste output as text not pictures.


- [ ] The first comment related to testing should be:
	## Review data

	| Tester | PR commit               | 
	|--------|-------------------------|
	| @user  |  <commit_short_hash>    | 

	### Testing environment

	| OS | OS version | Deployment                                    | Image/AMI | Notes |
	|----|------------|-----------------------------------------------|-----------|-------|
	|    |            | `<LOCAL, AWS> \| <Vagrant, Docker, EC2, ECS>` |           |       |


	### Tested packages

	| `wazuh-manager` | `wazuh-agent` | 
	|-----------------|---------------|
	|                 |               |

	### Status

	- [ ] In progress
	- [ ] Pending to review
	- [ ] Team leader approved
	- [ ] Manager approved

- [ ] Then the following comments must contain:

	## Case Name - (References Status)

	This name is the same that are in the description section of the Manual Testing issue. For each case name you need to add this section to separate cases.

	### Steps enumerated with evidence.

	   1. Step Y
	   2. Step X
	   ...


# Note: References Status

|Color|Status |
|:--:|:--|
|🟢|All tests passed successfully|
|🟡|All tests passed but there are some warnings|
|🔴|Some tests have failures or errors|
|🔵|Test execution in progress|
|⚫|To Do|
|🟠|Jenkins provision fails|
|:purple_circle:| All skipped |