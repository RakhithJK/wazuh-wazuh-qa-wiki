## Scenarios list
### Index



- [0201 - Default syscheck configuration: Linux/Windows](https://github.com/wazuh/wazuh-qa/wiki/FIM-System-tests%3A-Scenarios-list/_0201---default-syscheck-configuration-linuxwindows)
- [0202 - Real time monitoring - add: Linux/Windows](https://github.com/wazuh/wazuh-qa/wiki/FIM-System-tests%3A-Scenarios-list/_0202---real-time-monitoring---add-linuxwindows)
- [0203 - Whodata Linux/Windows](https://github.com/wazuh/wazuh-qa/wiki/FIM-System-tests%3A-Scenarios-list/_0203---whodata-linuxwindows)
- [0204 - Whodata Linux - no audit installed](https://github.com/wazuh/wazuh-qa/wiki/FIM-System-tests%3A-Scenarios-list/_0204---whodata-linux---no-audit-installed)
- [0205 - Use of restrict option: Linux/Windows](https://github.com/wazuh/wazuh-qa/wiki/FIM-System-tests%3A-Scenarios-list/_0205---use-of-restrict-option-linuxwindows)
- [0206 - Use of tags: Linux/Windows](https://github.com/wazuh/wazuh-qa/wiki/FIM-System-tests%3A-Scenarios-list/_0206---use-of-tags-linuxwindows)
- [0207 - Use of  report changes: Linux/Windows](https://github.com/wazuh/wazuh-qa/wiki/FIM-System-tests%3A-Scenarios-list/_0207---use-of--report-changes-linuxwindows)
- [0208 - Use of ignore files: Linux/Windows](https://github.com/wazuh/wazuh-qa/wiki/FIM-System-tests%3A-Scenarios-list/_0208---use-of-ignore-files-linuxwindows)
- [0209 - Recursion level](https://github.com/wazuh/wazuh-qa/wiki/FIM-System-tests%3A-Scenarios-list/_0209---recursion-level)
- [0210 - Scheduled scan](https://github.com/wazuh/wazuh-qa/wiki/FIM-System-tests%3A-Scenarios-list/_0210---scheduled-scan)
- [0211 - Custom configuration](https://github.com/wazuh/wazuh-qa/wiki/FIM-System-tests%3A-Scenarios-list/_0211---custom-configuration)
- [0212 - Check overlap of scheduled syscheck scan and realtime scan](https://github.com/wazuh/wazuh-qa/wiki/FIM-System-tests%3A-Scenarios-list/_0212---check-overlap-of-scheduled-syscheck-scan-and-realtime-scan)




### 0201 - Default syscheck configuration: Linux/Windows
#### Purpose
To ensure that the same number of syscheck alerts are produced in the same environment, so no false positives are reported. Default syscheck configuration
#### Configuration
- queue_size: 15000
- events_per_second: 500
#### Input values
- 
#### Expected results
"A static number of syscheck alerts indexed in Elasticsearch."
___
<br>




### 0202 - Real time monitoring - add: Linux/Windows
#### Purpose
To ensure the 'realtime' feature takes effect in time.
#### Configuration
-  Monitor `/opt/fim_testing` with 320 recursion level.
- `whodata="yes"`, `check_all="yes"`
#### Input values
- Generate 1,100, 1000 and 10.000 files at the same time.
#### Expected results
The number of alerts must ONLY match in Elasticsearch to the number of added files. Also the timestamp should be close in time to the timestamp of the alert generation.
___
<br>




### 0203 - Whodata Linux/Windows
#### Purpose
To ensure the 'whodata' feature takes effect in time.
#### Configuration
-  Monitor `/opt/fim_testing` with 320 recursion level.
- `whodata="yes"`, `check_all="yes"`
#### Input values
- Generate 1,100, 1000 and 10.000 files at the same time.
#### Expected results
The number of alerts must ONLY match in Elasticsearch to the number of added files. Also the timestamp should be close in time to the timestamp of the alert generation, and the fields of the alerts must be matching to Whodata fields.
___
<br>


### 0204 - Whodata Linux - no audit installed
#### Purpose
To ensure the missing dependency is properly handled.
#### Configuration
-  Monitor `/opt/fim_testing` with 320 recursion level.
- whodata='yes', check_all="yes"
#### Input values
- Generate 1,100, 1000 and 10.000 files at the same time.
#### Expected results
An Error log must be provoked in the alerts.log, and realtime mode should be switched to.
___
<br>


### 0205 - Use of restrict option: Linux/Windows
#### Purpose
To ensure that only restricted files are effectively monitored.
#### Configuration
- Monitor `/opt/fim_testing` with 320 recursion level.
- `frequency="10"`check_all="yes"
- restrict="^ignoredfile.txt$"
#### Input values
Create an 'ignoredfile.txt' file within `/opt/fim_testing while` creating other files.
#### Expected results
There should only be one alert related to the restricted file.
___
<br>

### 0206 - Use of tags: Linux/Windows
#### Purpose
Ensure that alerts are generated with the specified tags
#### Configuration
-  Monitor `/opt/fim_testing` with 320 recursion level.
- `frequency="10"`  check_all="yes"
3. tags="tagtest"
#### Input values
Create a test file.
#### Expected results
There should be one alert with the specified tag within its body.
___
<br>


### 0207 - Use of  report changes: Linux/Windows
#### Purpose
To be sure that the content of the change is in the alert.
#### Configuration
-  Monitor `/opt/fim_testing` with 320 recursion level.
- `frequency="10"`  check_all="yes"
3. report_changes="yes"
#### Input values
Modify a monitored file.
#### Expected results
There should be one alert with the text change within the body.
___
<br>



### 0208 - Use of ignore files: Linux/Windows
#### Purpose
To be sure that specified files are ignored and not monitored.
#### Configuration
- `<ignore type="sregex">.mp3$|.avi$|.mpg$</ignore>`
`frequency="10"`
#### Input values
Create a file that satisfies the ignored type regex.
#### Expected results
There should not be any alert related to this action.
___
<br>


### 0209 - Recursion level.
#### Purpose
Check the behavior of the recursion_level option.
#### Configuration
- Monitor `/opt/fim_testing` with 4 recursion level.
- `frequency="10"`, check_all="yes"
#### Input values
"Create files at level 4. Also create a 
level 5 folder with some files."
#### Expected results
___
<br>


### 0210 - Scheduled scan
#### Purpose
"Check that the scans are effectively launched when reached
the specified time."
#### Configuration
- Monitor `/opt/fim_testing` with 320 recursion level.
- `frequency="10"` , check_all="yes"
#### Input values
- Generate 1,100, 1000 and 10.000 files at the same time.
#### Expected results
"After the specified time, the scan must launch
and the alerts must be triggered. "
___
<br>


### 0211 - Custom configuration
#### Purpose
Check the filters applied and the custom configuration works properly
#### Configuration
- Monitor different <directories> folder
#### Input values
- Generate 1,100, 1000 and 10.000 files at the same time.
#### Expected results
After the specified time, the scan must launch and the alerts must be triggered.
___
<br>


### 0212 - "Check overlap of scheduled syscheck scan and realtime scan
#### Purpose
"Determine how does the periodic fim scan affect to realtime and if the
 overlap produces failures in alert generation or big delays"
#### Configuration
- Monitor `/opt/fim_testing` with 4 recursion level.
- `frequency="10"` ', check_all="yes"
#### Input values
- Generate 1,100, 1000 and 10.000 files at the same time.
#### Expected results
