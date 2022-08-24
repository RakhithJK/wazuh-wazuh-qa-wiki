# OPTION 1

<details><summary>Test_integratord_change_json_inode</summary>

<p>

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
* `Minimum Version`: 4.3.7
* `Path`: `tests/integration/test_integratord/test_integratord_change_inode_alert.py`


</p>

</details>

# OPTION 2

<details><summary>Test_integratord_change_json_inode</summary>

<p>

  <table>
    <tr>
      <th>Description</th>
      <td>Check that if when reading the `alerts.json` file, the inode for the file changes, `integratord` will reload the file and continue reading from it.</td>
    </tr>
    <tr>
      <th>Test phases</th>
      <td><ul> <li>Insert an alert alerts.json file.</li>
               <li>Replace the alerts.json file while it being read.</li>
               <li>Check integratord detects the file's inode has changed.</li>
               <li>Wait for integratord to start reading from the file again.</li>
               <li>Insert an alert.</li>
               <li>Check virustotal response is added in ossec.log</li>
      </ul></td>
    </tr>
    <tr>
      <th>Cases</th>
      <td><ul><li>Case 1: Replace alerts.json, wait for expected response.</li></ul></td>
    </tr>
    <tr>
      <th>Platform</th>
      <td>Linux</td>
    </tr>
    <tr>
      <th>Tier</th>
      <td>1</td>
    </tr>  
    <tr>
      <th>Minimum Version</th>
      <td>4.3.7</td>
    </tr> 
    <tr>
      <th>Path</th>
      <td><a href="https://github.com/wazuh/wazuh-qa/blob/4.3/tests/integration/test_integratord/test_integratord_change_inode_alert.py">test_integratord_change_inode_alert.py</a></td>
    </tr> 
  </table> 
</p>

</details>


# OPTION 3


# Test_integratord_change_json_inode

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
  * `Minimum Version`: 4.3.7
  * `Path`: `tests/integration/test_integratord/test_integratord_change_inode_alert.py`