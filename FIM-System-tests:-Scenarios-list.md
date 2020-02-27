## Scenarios list
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
#### Configuration
- queue_size: 15000
- events_per_second: 500
#### Input values
- 
#### Expected results
"A static number of syscheck alerts indexed in Elasticsearch."
___
<br>



### <a name="202"></a>202 - Real time monitoring - add: Linux/Windows
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



### <a name="203"></a>203 - Whodata Linux/Windows
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

### <a name="204"></a>204 - Whodata Linux - no audit installed
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

### <a name="205"></a>205 - Use of restrict option: Linux/Windows
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

### <a name="206"></a>206 - Use of tags: Linux/Windows
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


### <a name="207"></a>207 - Use of  report changes: Linux/Windows
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


### <a name="208"></a>208 - Use of ignore files: Linux/Windows
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

### <a name="209"></a>209 - Recursion level
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

#### Purpose
Determine how does the periodic fim scan affect to realtime and if the overlap produces failures in alert generation or big delays.
#### Configuration
- Monitor `/opt/fim_testing` with 4 recursion level.
- `frequency="10"`, `check_all="yes"`
#### Input values
- Generate 1,100, 1000 and 10.000 files at the same time.
#### Expected results
