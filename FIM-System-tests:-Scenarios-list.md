## Scenarios list [#493](https://github.com/wazuh/wazuh-qa/issues/493)
### Index

- [0201 - Default syscheck configuration: Linux/Windows](#201)
- [0202 - Real time monitoring - add: Linux/Windows](#202)
- [0203 - Whodata Linux/Windows](#203)
- [0204 - Whodata Linux - no audit installed](#204)
- [0205 - Use of restrict option: Linux/Windows](#205)
- [0206 - Use of tags: Linux/Windows](#206)
- [0207 - Use of  report changes: Linux/Windows](#207)
- [0208 - Use of ignore files: Linux/Windows](#208)
- [0209 - Recursion level](#209)
- [0210 - Scheduled scan](#210)
- [0211 - Custom configuration](#211)
- [0212 - Check overlap of scheduled syscheck scan and realtime scan](#212)


### <a name="201"></a>201 - Default syscheck configuration: Linux/Windows
#### Purpose
To ensure that the same number of syscheck alerts are produced in the same environment, so no false positives are reported. Default syscheck configuration
#### Configuration - Linux
- `<frequency>10</frequency>`
- `<directories>/opt/fim_testing</directories>`
#### Configuration - Windows
- `<frequency>10</frequency>`
- `<directories recursion_level="320">C:\fim_testing</directories>`
#### Input values
- 
#### Expected results
A static number of syscheck alerts correctly indexed in the `alerts.json` file and the Elasticsearch indices.
___
<br>



### <a name="202"></a>202 - Real time monitoring - add: Linux/Windows
**related issue**: [#531](https://github.com/wazuh/wazuh-qa/issues/531)
#### Purpose
To ensure the 'realtime' feature takes effect in time.
#### Configuration - Linux
- `<frequency>1000000</frequency>`
- `<directories realtime="yes" check_all="yes" recursion_level="4">/opt/fim_testing</directories>`
#### Configuration - Windows
- `<frequency>1000000</frequency>`
- `<directories recursion_level="320" check_all="yes" realtime="yes">C:\fim_testing</directories>`
#### Input values
- Generate 1,100, 1000 and 10.000 files at the same time.
#### Expected results
The number of alerts must ONLY match in Elasticsearch to the number of added files. Also the timestamp should be close in time to the timestamp of the alert generation.


___
<br>



### <a name="203"></a>203 - Whodata Linux/Windows
**related issue**: [#528](https://github.com/wazuh/wazuh-qa/issues/528)
#### Purpose
To ensure the 'whodata' feature takes effect in time.
#### Configuration - Linux
- ` <frequency>43200</frequency>`
- `<directories recursion_level="320" check_all="yes" whodata="yes">/opt/fim_testing</directories>`
#### Configuration - Windows
- `<frequency>43200</frequency>`
- `<directories recursion_level="320" check_all="yes" whodata="yes">C:\fim_testing</directories>`
#### Input values
- Generate 1,100, 1000 and 10.000 files at the same time.
#### Expected results
The number of alerts must ONLY match in Elasticsearch to the number of added files. Also the timestamp should be close in time to the timestamp of the alert generation, and the fields of the alerts must be matching to Whodata fields.
___
<br>

### <a name="204"></a>204 - Whodata Linux - no audit installed
**related issue**: [#520](https://github.com/wazuh/wazuh-qa/issues/520
#### Purpose
To ensure the missing dependency is properly handled.
#### Configuration - Linux
- ` <frequency>43200</frequency>`
- `<directories recursion_level="320" check_all="yes" whodata="yes">/opt/fim_testing</directories>`
#### Configuration - Windows - N/A
#### Input values
- Generate 1,100, 1000 and 10.000 files at the same time.
#### Expected results
An Error log must be provoked in the alerts.log, and realtime mode should be switched to.
___
<br>

### <a name="205"></a>205 - Use of restrict option: Linux/Windows
**related issue**: [#526](https://github.com/wazuh/wazuh-qa/issues/526)
#### Purpose
To ensure that only restricted files are effectively monitored.
#### Configuration - Linux
- `<frequency>10</frequency>`
- `<directories recursion_level="320" check_all="yes" restrict="fimtest">/opt/fim_testing</directories>`
#### Configuration - Windows
- `<frequency>10</frequency>`
- `<directories recursion_level="320" check_all="yes" restrict="fimtest">C:\fim_testing</directories>`
#### Input values
Create an 'ignoredfile.txt' file within `/opt/fim_testing while` creating other files.
#### Expected results
There should only be one alert related to the restricted file.
___
<br>

### <a name="206"></a>206 - Use of tags: Linux/Windows
**related issue**: [#524](https://github.com/wazuh/wazuh-qa/issues/524)
#### Purpose
Ensure that alerts are generated with the specified tags
#### Configuration - Linux
- `<frequency>10</frequency>`
- `<directories tags="test_tag">/opt/fim_testing</directories>`
#### Configuration - Windows
- `<frequency>10</frequency>`
- `<directories recursion_level="320" tags="test_tag">C:\fim_testing</directories>`
#### Input values
Create a test file.
#### Expected results
There should be one alert with the specified tag within its body.
___
<br>


### <a name="207"></a>207 - Use of  report changes: Linux/Windows
**related issue**: [#523](https://github.com/wazuh/wazuh-qa/issues/523)
#### Purpose
To be sure that the content of the change is in the alert.
#### Configuration - Linux
- `<frequency>10</frequency>`
- `<directories recursion_level="320" check_all="yes" realtime="yes" report_changes="yes">/opt/fim_testing</directories>`
#### Configuration - Windows
- `<frequency>10</frequency>`
- `<directories recursion_level="320" check_all="yes" realtime="yes" report_changes="yes">C:\fim_testing</directories>`
#### Input values
Modify a monitored file.
#### Expected results
There should be one alert with the text change within the body.
___
<br>


### <a name="208"></a>208 - Use of ignore files: Linux/Windows
**related issue**: [#538](https://github.com/wazuh/wazuh-qa/issues/538)
#### Purpose
To be sure that specified files are ignored and not monitored.
#### Configuration - Linux
- `<frequency>10</frequency>`
- `<directories>/opt/fim_testing</directories>`
#### Configuration - Windows
- `<frequency>10</frequency>`
- `<directories recursion_level="320">C:\fim_testing</directories>`
#### Input values
Create a file that satisfies the ignored type regex.
#### Expected results
There should not be any alert related to this action.
___
<br>

### <a name="209"></a>209 - Recursion level
**related issue**: [#540](https://github.com/wazuh/wazuh-qa/issues/540)
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

### <a name="210"></a>210 - Scheduled scan
**related issue**: [#553](https://github.com/wazuh/wazuh-qa/issues/553)
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

### <a name="211"></a>211 - Custom configuration
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

### <a name="212"></a>212 - Check overlap of scheduled syscheck scan and realtime scan
**related issue**: [#556](https://github.com/wazuh/wazuh-qa/issues/556)
#### Purpose
Determine how does the periodic fim scan affect to realtime and if the overlap produces failures in alert generation or big delays.
#### Configuration - Linux
- `<frequency>600</frequency>`
- `<directories realtime="yes" check_all="yes" recursion_level="4">/opt/fim_testing</directories>`
#### Configuration - Windows
- `<frequency>600</frequency>`
- `<directories recursion_level="320" check_all="yes" realtime="yes">C:\fim_testing</directories>`
#### Input values
- Generate 1,100, 1000 and 10.000 files at the same time.
#### Expected results
