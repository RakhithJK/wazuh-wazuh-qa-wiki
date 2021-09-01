## Development
- Integration tests: Encapsulated tests that test the minimum possible functionality.
- Wazuh is assumed off, only the necessary demons are turned on and turned off at the end of the test.
- The test should leave the machine as it was found.
- The test must be 100% self-configurable, no user intervention is expected to have to run it with a PASS.
- Do not use simulators of any kind, basic mockers, if more is needed is system test and is implemented in another way
- Prioritize the use of common repository tools.
- An issue in Wazuh has directly an issue in QA, whether or not tests have to be implemented.
- We use Test-driven development (TDD) as the SLDC for QA tests + Core implementations.

## Contribution
- Every issue must have: The corresponding labels, the assigned weight, the target release, and sprint.
- We must always have a FULL GREEN. Any PR of ours that avoids a full green should not be merge.
- Epics are not scored, Zenhub auto-calculates.
- Labels go to issues, but never to PRs (except draft/hold).
- The QA issue in Core development should keep a list of candidate tests.
- Link the PR with "Connect to issue" to the issue.
- Both reviewer and developer test and run the test. And paste the tests in the PR.
- No brackets in issue titles. Natural language describing the task.
- Style guide for writing commits: https://github.com/emilio1625/guia-estilo-git#commits.
- Good formatted markdown, use Collapse/Details when is necessary.
- PR and issues will use Snippets no screenshots.
- Only tests that obtain a full green will be merged. Where colors codes are the following:
   - :green_circle: : All pass, no error, no fails, no warnings. 
   - :yellow_circle: : All pass, some warning. 
   - :red_circle: : Any fail or error.
- If changes to the wazuh_testing library are required, they must be made in a separate Issue/PR. 

## Style
### " and '

The use of ' and " is the following:
```
f"{var}string"
'raw string'
"this is my raw 'string' with single quotation marks"
```

## Documentation
### Docstring
- Doctrings will follow google docstring style guide: https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html
