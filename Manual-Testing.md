The QA team is responsible for performing the different manual tests to improve visibility and add more detail to each test. It is necessary to: 

<details>
  <summary> Comment 1 </summary>

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

</details>

<details>
  <summary> Comment 2 </summary>

## Case Name - (status color)

This name is the same that are in the description section of the Manual Testing issue. For each case name you need to add this section to separate cases.

### Steps enumerated with evidence (we prefer not to use pictures attached).

   1. Step Y
   2. Step X
   ...

</details>

<details>
  <summary> Notes</summary>
  <details>
     <summary> References Status</summary>

|Color|Status |
|:--:|:--|
|🟢|All tests passed successfully|
|🟡|All tests passed but there are some warnings|
|🔴|Some tests have failures or errors|
|🔵|Test execution in progress|
|⚫|To Do|
|🟠|Jenkins provision fails|
|:purple_circle:| All skipped |

</details>

  <details>
  <summary> Considerations</summary>

   - Previous to start verify that PR related is approved and it contains all check green.
   - Sync with core responsable that this test will start to test. 
   - If there are a new change added when you test the Manual execution you need to test it again, 
   - Keep indentation in line 
   - Enumerate the steps
   - Paste output as text not pictures.
  
  </details>
</details>