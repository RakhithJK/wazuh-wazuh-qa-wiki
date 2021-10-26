## How to use `qa-docs` tool

After installing `qa-docs`(you can follow the [installation guide](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-installation-guide)  steps), you can check the `qa-docs` help.

- `-h, --help`: Print the `qa-docs` help message.
- `-s, --sanity-check`: Perform a sanity check using the data previously parsed. Tests coverage, errors, and tests count.
- `-v, --version`: Print the `qa-docs` version.
- `-d, --debug`: Run in debug mode.
- `-I, --tests-path`: Specify the path of the tests to be parsed. Set it when you want to parse or run a sanity check.
- `-t, --tests`: Parse the test(s) you specify as an argument.
- `--types`: Parse the tests from type(s) that it is passed as an argument. Some types examples are `integration`, `system`, etc.
- `--modules`: Parse the tests from module(s) that it is passed as an argument. Some module examples are `test_active_response`, `test_fim`, etc.
- `-o`: Select the output directory. `python -m site --user-site` as default, where the `setup.py` installs the framework.
- `-e`: Check if the given test(s) exist(s).
- `-i`: Index the data previously parsed.
- `-l`: Launch `search-ui`with the data previously indexed.
- `-il`: Index the data previously parsed and launch `search-ui` 

## Parameter restrictions

- `-t` cannot be launched with `--types` and `--modules`, `-e`, `--exist`, `-i`, `-l` and `-il` options.
- `-t`, `--types`, `--modules`, `-t`, `-e` and `-s` need the `-I` option.
- `-e` cannot be launched with `--types` and `--modules`, `-i`, `-l` and `-il` options.
- `-o` cannot be launched with `--types` or `--modules`, `-e`, `--exist`, `-i`, `-l` and `-il` options.
- `-s` only allows a run with the `-I` option.

## Examples:


<details>
<summary>Generate the documentation in <code>YAML</code> and <code>JSON</code> formats in the framework installation directory</summary>

```bash
qa-docs -I /path/to/tests/
```

</details>


<details>
<summary>Parse <code>integration</code> tests</summary>

```bash
qa-docs -I /path/to/tests/ --types integration
```

</details>

<details>
<summary>Parse <code>test_active_response</code> and <code>test_agentd</code> tests</summary>

```bash
qa-docs -I /path/to/tests/ --types integration --modules test_active_response test_agentd
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
<summary>Parse integration tests modules, index the data and launch <code>search-ui</code> to visualize it via web browser</summary>

```bash
qa-docs -I /path/to/tests/ --types integration --modules test_active_response test_agentd -il index_name
```

</details>

<details>
<summary>Perform a sanity check after a previous parse run</summary>

```bash
qa-docs -I /path/to/tests/ -s
```

</details>

<details>
<summary>Parse a list of tests</summary>

```bash
qa-docs -I /path/to/tests/ -t test_cache test_general_settings_enabled
```

</details>

<details>
<summary>Parse a list of tests and give a custom output directory</summary>

```bash
qa-docs -I /path/to/tests/ -t test_cache test_general_settings_enabled -o /tmp
```

</details>

<details>
<summary>Check if a list of tests exists</summary>

```bash
qa-docs -I /path/to/tests/ -e test_cache test_general_settings_enabled
```

</details>

More details and examples can be found in the `qa-docs` [README.MD](https://github.com/wazuh/wazuh-qa/tree/1864-qa-docs-fixes/deps/wazuh_testing/wazuh_testing/qa_docs).

## Docker deployment

By running a script you can parse the types/modules tests that you want, just need a branch as input. Optionally, you can pass the types or types and modules that you want to parse using the tests allocated in that branch.

<details>
<summary>Parse all test types and launch search-ui to visualize the documentation.</summary>

```bash
./deploy_qa_docs.sh 1796-migrate-doc-schema-2
```

</details>


<details>
<summary>Parse integration tests and launch search-ui to visualize the documentation.</summary>

```bash
./deploy_qa_docs.sh 1796-migrate-doc-schema-2 integration
```

</details>


<details>
<summary>Parse some integration modules and launch search-ui to visualize the documentation.</summary>

```bash
./deploy_qa_docs.sh 1796-migrate-doc-schema-2 integration test_active_response test_agentd
```

</details>