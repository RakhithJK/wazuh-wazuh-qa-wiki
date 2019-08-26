The format for the automatic test template files consists of two blocks, configuration block and the tests block.

## Configuration block
This block contains the configuration necessary to perform the checks described in the tests block.
This configuration will be define by the tags `[config-block:append|replace] ... [end]`.

The configuration block uses one of the following options:

- `append` : The configuration will be appended to the `ossec.conf` file. 
- `replace` : The configuration replaces the block defined in the `ossec.conf` file.

## Tests block
In this block will be defined all the tests to be performed using the configuration added in the config-block.
All lines will be placed inside the tags `[tests] ... [end]`.
The test lines have the following format:
```
test name; event; alerts fields
```

- Tests name: is the name for each test. It will be showed in the result output.
- Event: is the event necessary to trigger an event. The valid event triggers are the following:

    | Trigger | Syntax | Example |
    | --- | --- | --- |
    | **create_file** | `create_file=file_path` | `create_file=/tmp/file.txt` |
    | **delete_file** | `delete_file=file_path` | `delete_file=/tmp/file.txt` |
    | **create_folder** | `create_folder=folder_path` | `create_folder=/tmp/my_folder` |
    | **delete_folder** | `delete_folder=folder_path` | `delete_folder=/tmp/my_folder` |
    | **modify_content** | `modify_content=file_path` | `modify_content=/tmp/file.txt` |
    | **modify_owner** | `modify_owner=file_path%user_id` | `modify_owner=/tmp/file.txt%1001` |
    | **modify_group** | `modify_group=file_path%group_id` | `modify_owner=/tmp/file.txt%90` |
    | **modify_perm** | `modify_perm=file_path%perms` | `modify_perm=/tmp/file.txt%644` |
    | **execute_command** | `execute_command=command` | `execute_command=service auditd stop` |

- Alert fields: is the list of fields to be checked from the generated alert. The test will pass only if **ALL** the conditions in the list are passed.
    ```
    syscheck.path=/root/test/file1.txt, syscheck.event=added
    ```
    It's possible to negate a condition using the character `!`:
    ```
    syscheck.path=/root/test/file1.txt, !syscheck.event=added
    ```

### Test file example:
```
Test file for Syscheck
========================

[config-block]
  <syscheck>
    <directories check_all="yes" realtime="yes" >/root/test</directories>
  </syscheck>
[end]


[tests]
File creation; create_file:/root/test/file1.txt; syscheck.path=/root/test/file1.txt, syscheck.event=added
File perm modification; modify_perm:/root/test/file1.txt % 600; syscheck.path=/root/test/file1.txt, syscheck.event=modified
File deletion; delete_file:/root/test/file1.txt; syscheck.path=/root/test/file1.txt, syscheck.event=deleted
[end]
```


