The QA team is responsible for performing the different manual tests to improve visibility and add more detail to each test. It is necessary to do: 

# Consideration: Before starting

   - [ ] Verify that PR related is approved and it contains all check green.
   - [ ] Sync with core responsable that this test will start to test. 

<details>
  <summary>Tests: Our coverage</summary>

1. When do we perform upgrade tests?
2. 
3. 

   - [ ] If there are a new change added when you test the Manual execution you need to test it again.
En que nos tenemos que fijar para saber si un upgrade tiene sentido o si no lo tiene?
Sumar el cambio de que a partir de ahora todos los restart tienen que tener la comprobación si los demonios están levantados
En que quedo que se debe agregar que están conectados manager y agente? esto en un momento se hablo recuerdo
De que nos valemos para determinar si probamos en mas de un OS?
De que nos valemos para determinar si probamos en distintas versiones de un mismo OS?

</details>


# Consideration: How to show our evidence

   - [ ] Keep indentation in line.
   - [ ] Enumerate the steps.
   - [ ] Paste output as text not pictures.
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

		## Case Name - (status color)

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
