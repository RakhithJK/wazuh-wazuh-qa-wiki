## How to use `qa-docs` tool

After installing `qa-docs`(you can follow the [installation guide](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-installation-guide)  steps), you can check the `qa-docs` help.

- `-h, --help`: Print the `qa-docs` help message.
- `--sanity-check`: Perform a sanity check using the data previously parsed. Tests coverage, errors, and tests count.
- `-v, --version`: Print the `qa-docs` version.
- `--no-logging`: Disable `qa-docs` logging.
- `-d, --debug`: Run in debug mode.
- `-p, --tests-path`: Specify the path of the tests to be parsed. Set it when you want to parse or run a sanity check.
- `-t, --types`: Parse the tests from type(s) that it is passed as an argument.
- `-c, --components`: Parse the tests from type(s) that it is passed as an argument.
- `-s, --suites`: Parse the tests from type(s) that it is passed as an argument.
- `-m, --modules`: Parse the tests from module(s) that it is passed as an argument.
- `-o`: Select the output directory where the documentation will be generated.
- `--format`: Select the generated files format. Set as JSON by default.
- `-e, --exist`: Check if the given test(s) exist(s).
- `--check-documentation`: Check if the modules(s) passed with `-m` is(are) documented following the current `qa-docs` schema.
- `--validate-parameters`:  Validate the parameters passed to the qa-docs tool.
- `-i`: Index the data previously parsed.
- `-l`: Launch `search-ui`with the data previously indexed.
- `-il`: Index the data previously parsed and launch `search-ui` 
- `--docker-run`: Run a docker container that runs `qa-docs` with the same arguments that it receives. This allows you to run `qa-docs` as you would do in a local environment.
- `--qa-branch`: Specifies the branch that allocates the tests input when `--docker-run` is passed.

## Parameter restrictions

- `--types`, `--components`, `--suites`, `--modules`, `--exist` and `--sanity-check` need the `-p` option.
- `-e` cannot be launched with `--types` and `--modules`, `-i`, `-l` and `-il` options.
- `--check-documentation` only allows runs with the `-m` option.
- `--qa-branch` only allows runs with the `--docker-run` option.

## Examples:


<details>
<summary>Parse all the tests within the path.</summary>

```bash
qa-docs --tests-path /tmp/wazuh-qa/tests
```

</details>


<details>
<summary>Parse <code>integration</code> tests</summary>

```bash
qa-docs --tests-path /tmp/wazuh-qa/tests --types integration
```

</details>

<details>
<summary>Parse <code>test_active_response</code> and <code>test_agentd</code> components</summary>

```bash
qa-docs --tests-path /tmp/wazuh-qa/tests --types integration --components test_active_response test_agentd
```

</details>

<details>
<summary>Parse <code>config</code> and <code>rbac</code> suites from <code>api</code> component</summary>

```bash
qa-docs --tests-path /path-to-tests/ --types integration --components test_api --suites test_config test_rbac
```

</details>

<details>
<summary>Parse some modules from <code>logtest</code> <code>configuration</code> suite</summary>

```bash
qa-docs --tests-path /path-to-tests/ --types integration --components test_logtest --suites test_configuration --modules test_configuration_file test_get_configuration_sock
```

</details>

<details>
<summary>Check if some modules exist</summary>

```bash
qa-docs --tests-path /path-to-tests/ --types integration --components test_logtest --suites test_configuration --exist test_configuration_file test_get_configuration_sock
```

</details>

<details>
<summary>Parse some modules and select the custom output directory</summary>

```bash
qa-docs --tests-path /path-to-tests/ --types integration --components test_logtest --suites test_configuration --modules test_configuration_file test_get_configuration_sock -o /tmp/logtest_cfg
```

</details>

<details>
<summary>Check if some <code>logtest</code> modules has documentation blocks</summary>

```bash
qa-docs -p /path-to-tests/ --types integration --components test_logtest --suites test_configuration --modules test_configuration_file test_get_configuration_sock --check-documentation
```

</details>

<details>
<summary>Create an index with the data previously parsed</summary>

```bash
qa-docs -i index_name
```

</details>

<details>
<summary>Launch <code>search-ui</code> the data previously indexed via web browser</summary>

```bash
qa-docs -l index_name
```

</details>

<details>
<summary>Index the data previously parsed and launch <code>search-ui</code></summary>

```bash
qa-docs -il index_name
```

</details>

<details>
<summary>Parse integration tests components, index the data and launch <code>search-ui</code> to visualize it via web browser</summary>

```bash
qa-docs --tests-path /tmp/wazuh-qa/tests --types integration --components test_active_response test_agentd -il index_name
```

</details>

<details>
<summary>Parse integration tests components, index the data and launch <code>search-ui</code> to visualize it via web browser(using YAML format)</summary>

```bash
qa-docs --tests-path /tmp/wazuh-qa/tests --types integration --components test_active_response test_agentd -il index_name --format yaml
```

</details>

<details>
<summary>Perform a sanity check after a previous parse run</summary>

```bash
qa-docs --tests-path /tmp/wazuh-qa/tests --sanity-check
```

</details>

<details>
<summary>Parse <code>test_active_response</code> tests and generate the documentation in <code>/tmp/qa_docs</code> using Docker</summary>

```bash
qa-docs --docker-run --qa-branch 1796-migrate-doc-active-response --types integration --components test_active_response
```

</details>

</details>

<details>
<summary>Parse <code>test_fim</code> tests and generate the documentation in custom output path using Docker</summary>

```bash
qa-docs --docker-run --qa-branch 1796-migrate-doc-active-response --types integration --components test_fim -o /custom/path
```

</details>

<details>
<summary>Parse <code>test_active_response</code> tests and launch search-ui to visualize the documentation using Docker</summary>

```bash
qa-docs --docker-run --qa-branch 1796-migrate-doc-active-response --types integration --components test_active_response -il qa-index
```

</details>

More details and examples can be found in the `qa-docs` [README.MD](https://github.com/wazuh/wazuh-qa/blob/master/deps/wazuh_testing/wazuh_testing/qa_docs/README.md).
