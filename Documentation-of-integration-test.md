# INTEGRATORD 

       
## Test_integratord_change_json_inode

* `Description`: Check that if when reading the `alerts.json` file, the inode for the file changes, `integratord` will reload the file and continue reading from it.
* `Test phases`:
  * Insert an alert alerts.json file.
  * Replace the alerts.json file while it being read.
  * Check integratord detects the file's inode has changed.
  * Wait for integratord to start reading from the file again.
  * Insert an alert
  * Check virustotal response is added in ossec.log

      
* `Cases`:  
  * **Case 1:** Replace alerts.json, wait for expected response.

* `Platform`: Linux
* `Tier`: 1
* `Minimum Version`: 4.3.5
* `Path`: `tests/integration/test_integratord/test_integratord_change_inode_alert.py`




## Test_integratord_read_valid_alerts

* `Description`: Check that when a given alert is inserted into `alerts.json`, integratord works as expected. In case
    of a valid alert, a virustotal integration alert is expected in the `alerts.json` file.
* `Test phases`:
  * Insert an alert alerts.json file.
  * Check virustotal response is added in ossec.json

      
* `Cases`:  
  * **Case 1:** Insert alert, read expected response.

* `Platform`: Linux
* `Tier`: 1
* `Minimum Version`: 4.3.5
* `Path`: `tests/integration/test_integratord/test_integratord_read_json_alerts.py`


## test_integratord_read_invalid_alerts

* `Description`: Check that when a given alert is inserted into ' alerts.json', integratord works as expected. If the alert is invalid/    broken, or overly long a message will appear in the ossec.log file.
* `Test phases`:
  * Insert an alert alerts.json file.
  * Check that the expected response message is given for an invalid alert.
      
* `Cases`:  
  * **Case 1:** Insert invalid alert, wait for `Invalid alert` response.
  * **Case 2:** Insert overlong alert, wait for `Overlong alert` response.

* `Platform`: Linux
* `Tier`: 1
* `Minimum Version`: 4.3.5
* `Path`: `tests/integration/test_integratord/test_integratord_read_json_alerts.py`


## test_integratord_read_json_file_deleted

* `Description`: Check that if while integratord is reading from the `alerts.json` file, it is deleted, the expected error message is displayed, and if the file is created again and alerts are inserted, integratord continues
* `Test phases`:
  * Remove alerts.json file.
  * Wait for integratord to detect the file was removed.
  * Create new alerts.json file.
  * Wait for the new file to be detected.
  * Insert an alert
  * Check virustotal response is added in ossec.log

      
* `Cases`:  
  * **Case 1:** Delete and recreate alerts.json, wait for expected responses.

* `Platform`: Linux
* `Tier`: 1
* `Minimum Version`: 4.3.5
* `Path`: `tests/integration/test_integratord/test_integratord_read_json_file_deleted.py`

