### How to use `qa-docs` tool

After you have installed `qa-docs`(you can follow the [installation guide](https://github.com/wazuh/wazuh-qa/wiki/QADOCS-tool-installation-guide)  steps), you can check the `qa-docs` help.

- `-h, --help`: Print the `qa-docs` help message.
- `-s, --sanity-check`: Perform a sanity check using the data previously parsed.
- `-v, --version`: Print the `qa-docs` version.
- `-d, --debug`: Run in debug mode.
- `-I, --tests-path`: Specify the path of the tests to be parsed. Set it when you want to parse or run a sanity check.
- `-t, --tests`: Parse the test(s) you specify as an argument.
- `--types`: Parse the tests from type(s) that you pass as an argument.
- `--modules`: Parse the tests from module(s) that you pass as an argument.
- `-o`: Select the output directory. `python -m site --user-site` as default.
- `-e`: Check if the test(s) exist(s).
- `-i`: Index the data previously parsed.
- `-l`: Launch `search-ui`with the data previously indexed.
- `il`: Index the data previously parsed and launch `search-ui` 

### Parameter restrictions

- `-t` and `-e` cannot be launched with `--types` and `--modules` parameters. They store the parsed data into /output and `-t` save it where the user specifies with `-o`.
- `-t`, `--types, `--modules`, `-t`, `-e` and `-s` need the `-I` parameter.
- `-o` cannot be launched with `--types` or `--modules`.
- `-t` and `e` cannot be launched with `-i`, `-l` and `-il`.
- `-s` only allows a run with `-I` parameter.

### Examples:

To generate the documentation in `YAML` and `JSON` formats, just run using the tests to parse:
```
$ qa-docs -I /path/to/tests/
```

You can parse by test types and modules:
```
$ qa-docs -I /path/to/tests/ --types integration
```

```
$ qa-docs -I /path/to/tests/ --types integration --modules test_active_response test_agentd
```

Then, you can index them:
```
$ qa-docs -I /path/to/tests/ -i my_index
```

And run SearchUI so you can visualize the doc via web browser:
```
$ qa-docs -I /path/to/tests/ -l my_index
```

Or both:
```
$ qa-docs -I /path/to/tests/ -il my_index
```

To perform a sanity check after a previous parse:
```
$ qa-docs -I /path/to/tests/ -s
```

Also, a list of tests can be parsed:
```
$ qa-docs -I ../../tests/ -t test_cache test_general_settings_enabled
``` 

Or check if they exist:
```
$ qa-docs -I ../../tests/ -e test_cache test_general_settings_enabled
``` 

More details and examples can be found in the `qa-docs` [README.MD](https://github.com/wazuh/wazuh-qa/tree/1864-qa-docs-fixes/deps/wazuh_testing/wazuh_testing/qa_docs).

### Docker deployment

By running a script you can parse the types/modules tests that you want, just need a branch as input. Optionally, you can pass the types or types and modules that you want to parse using the tests allocated in that branch.

```
$ ./deploy_qa_docs.sh 1796-migrate-doc-schema-2
```

```
$ ./deploy_qa_docs.sh 1796-migrate-doc-schema-2 integration
```

```
$ ./deploy_qa_docs.sh 1796-migrate-doc-schema-2 integration test_active_response test_agentd
```