The aim of this article is to show the necessary parameters to be able to launch all the integration tests of the wazuh-qa repository.

<table>
 <tr>
  <td><strong>CAPABILITIES</strong></td>
  <td><strong>TARGET</strong></td>
  <td><strong>LOCAL INTERNAL OPTIONS</strong></td>
  <td><strong>Pytest ARGS - Local </strong></td>
  <td><strong>Notes </strong></td>
 </tr>
 <tr>
  <td rowspan="2">Active Response</td>
  <td> Manager</td>
  <td>monitord.rotate_log=0</td>
  <td>&nbsp;</td>
  <td></td>
 </tr>
 <tr>
 <td> Agent (Linux, Windows)</td>
  <td>monitord.rotate_log=0</td>
  <td>&nbsp;</td>
  <td></td>
 </tr>

  <tr>
  <td>Agentd</td>
  <td> Agent (Linux, Windows)</td>
  <td>agent.debug=2<br/>execd.debug=2<br/>monitord.rotate_log=0</td>
  <td>&nbsp;</td>
  <td></td>
 </tr>

 <tr>
  <td>Analysid</td>
  <td> Manager </td>
  <td>analysisd.debug=2<br/>monitord.rotate_log=0<br/>monitord.rotate_log=0</td>
  <td>&nbsp;</td>
  <td></td>
 </tr>

 <tr>
  <td>API</td>
  <td> Manager </td>
  <td>monitord.rotate_log=0</td>
  <td>&nbsp;</td>
  <td></td>
 </tr>

 <tr>
  <td>Authd</td>
  <td>Manager </td>
  <td></td>
  <td>&nbsp;</td>
  <td></td>
 </tr>

 <tr>
  <td rowspan="3">FIM</td>
  <td>Manager </td>
  <td>syscheck.debug=2<br/>analysisd.debug=2<br/>monitord.rotate_log=0</td>
  <td> --tier 0 --tier 1 --tier 2 --fim_mode="realtime" --fim_mode="whodata" --fim_mode="scheduled"</td>
  <td></td>
 </tr>
 <tr>
  <td>Agent (Linux, MacOS, Solaris) </td>
  <td>syscheck.debug=2<br/>agent.debug=2<br/>monitord.rotate_log=0</td>
  <td>--tier 0 --tier 1 --tier 2 --fim_mode="realtime" --fim_mode="whodata" --fim_mode="scheduled"</td>
  <td></td>
 </tr>
 <tr>
  <td>Agent (Windows) </td>
  <td>syscheck.debug=2<br/>agent.debug=2<br/>monitord.rotate_log=0<br/>windows.debug=2</td>
  <td>--tier 0 --tier 1 --tier 2 --fim_mode="realtime" --fim_mode="whodata" --fim_mode="scheduled"</td>
  <td></td>
 </tr>
 <tr>
  <td>GCloud</td>
  <td>Manager </td>
  <td>analysisd.debug=2<br/>wazuh_modules.debug=2<br/>monitord.rotate_log=0</td>
  <td></td>
  <td><ul><li> It requires creating on "test_gcloud/data/" a file with name "configuration.yaml" that will contain the credentials.</li> 
           <li>This execution cannot be launched simultaneously. It is necessary to launch one execution first, and when you finish launching the next one since they share a license, and that usually causes failures in the executions. </li> 
 </ul></td></tr>
 <tr>
  <td>Github</td>
  <td> Manager - Agent (Linux)</td>
  <td></td>
  <td>&nbsp;</td>
  <td></td>
 </tr>
 <tr>
  <td>Logtest</td>
  <td>Manager </td>
  <td>analysisd.debug=2</td>
  <td>&nbsp;</td>
  <td></td>
 </tr>
 <tr>
  <td>Office365</td>
  <td> Manager - Agent (Linux)</td>
  <td></td>
  <td>&nbsp;</td>
  <td></td>
 </tr>
 <tr>
  <td>Remoted</td>
  <td>Manager </td>
  <td>remoted.debug=2<br/>wazuh_database.interval=1<br/>wazuh_db.commit_time=2<br/>wazuh_db.commit_time_max=3<br/>monitord.rotate_log=0</td>
  <td>&nbsp;</td>
  <td></td>
 </tr>

 <tr>
  <td>Rids</td>
  <td>Manager</td>
  <td></td>
  <td>&nbsp;</td>
  <td></td>
 </tr>
 <tr>
  <td>Rootcheck</td>
  <td>Manager</td>
  <td></td>
  <td>&nbsp;</td>
  <td></td>
 </tr>
 <tr>
  <td>Vulnerability Detector</td>
  <td>Manager </td>
  <td>wazuh_modules.debug=2<br/>monitord.rotate_log=0</td>
  <td>&nbsp;</td>
  <td></td>
 </tr>
 <tr>
  <td>WazuhDB</td>
  <td>Manager </td>
  <td>wazuh_modules.debug=2<br/>monitord.rotate_log=0</td>
  <td></td>
  <td></td>
 </tr>

 <tr>
  <td rowspan="2">WPK</td>
  <td>Manager -  Agent (Linux) </td>
  <td>wazuh_modules.debug=2</td>
  <td>--wpk_version=v4.x.x --wpk_package_path='packages-dev.wazuh.com/folder_name/wpk/'</td>
  <td></td>
 </tr>
  <td>Agent (Windows)</td>
  <td>windows.debug=2</td>
  <td>--wpk_version=v4.x.x  --wpk_package_path='packages-dev.wazuh.com/folder_name/wpk/'</td>
  <td></td>
 </tr>

</table>
