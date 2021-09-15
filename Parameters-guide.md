The aim of this article is to show the necessary parameters to be able to launch all the integration tests of the wazuh-qa repository.

<table>
 <tr>
  <td><strong>CAPABILITIES</strong></td>
  <td><strong>TARGET</strong></td>
  <td><strong>LOCAL INTERNAL OPTIONS</strong></td>
  <td><strong>Pytest ARGS</strong></td>
 </tr>
 <tr>
  <td rowspan="2">Active Response</td>
  <td> Manager - Linux</td>
  <td>monitord.rotate_log=0</td>
  <td>&nbsp;</td>
 </tr>
 <tr>
 <td> Agent - (Linux, Windows)</td>
  <td>monitord.rotate_log=0</td>
  <td>&nbsp;</td>
 </tr>

  <tr>
  <td>Agentd</td>
  <td> Agent - (Linux, Windows)</td>
  <td>agent.debug=2<br/>execd.debug=2<br/>monitord.rotate_log=0</td>
  <td>&nbsp;</td>
 </tr>

 <tr>
  <td>Analysid</td>
  <td> Manager - Linux</td>
  <td>analysisd.debug=2<br/>monitord.rotate_log=0<br/>monitord.rotate_log=0</td>
  <td>&nbsp;</td>
 </tr>

 <tr>
  <td>API</td>
  <td> Manager - Linux</td>
  <td>monitord.rotate_log=0</td>
  <td>&nbsp;</td>
 </tr>

 <tr>
  <td>Authd</td>
  <td>Manager - Linux</td>
  <td></td>
  <td>&nbsp;</td>
 </tr>

 <tr>
  <td rowspan="2">FIM</td>
  <td>Manager - Linux</td>
  <td>syscheck.debug=2<br/>analysisd.debug=2<br/>monitord.rotate_log=0</td>
  <td> --tier 0 --tier 1 --tier 2 --fim_mode="realtime" --fim_mode="whodata"</td>
 </tr>
 <tr>
  <td>Agent - (Linux, Windows, MacOS, Solaris) </td>
  <td>syscheck.debug=2<br/>agent.debug=2<br/>monitord.rotate_log=0</td>
  <td>--tier 0 --tier 1 --tier 2 --fim_mode="realtime" --fim_mode="whodata"</td>
 </tr>

 <tr>
  <td>GCloud</td>
  <td>Manager - Linux</td>
  <td>analysisd.debug=2<br/>wazuh_modules.debug=2<br/>monitord.rotate_log=0</td>
  <td>--gcp-project-id wazuh-dev-258815 --gcp-subscription-name test_gcloud_blog --gcp-credentials-file /home/vagrant/gcp_credentials_file.json</td>
 </tr>
 <tr>
  <td>Logtest</td>
  <td>Manager - Linux</td>
  <td>analysisd.debug=2</td>
  <td>&nbsp;</td>
 </tr>

 <tr>
  <td>Remoted</td>
  <td>Manager - Linux</td>
  <td>remoted.debug=2<br/>wazuh_database.interval=1<br/>wazuh_db.commit_time=2<br/>wazuh_db.commit_time_max=3<br/>monitord.rotate_log=0</td>
  <td>&nbsp;</td>
 </tr>

 <tr>
  <td>Rids</td>
  <td>Manager - Linux</td>
  <td></td>
  <td>&nbsp;</td>
 </tr>
 <tr>
  <td>Rootcheck</td>
  <td>Manager - Linux</td>
  <td></td>
  <td>&nbsp;</td>
 </tr>
 <tr>
  <td>Vulnerability Detector</td>
  <td>Manager - Linux</td>
  <td>wazuh_modules.debug=2<br/>monitord.rotate_log=0</td>
  <td>&nbsp;</td>
 </tr>
 <tr>
  <td>WazuhDB</td>
  <td>Manager - Linux</td>
  <td>wazuh_modules.debug=2<br/>monitord.rotate_log=0</td>
  <td>--wpk_version=v4.x.x</td>
 </tr>

 <tr>
  <td rowspan="2">WPK</td>
  <td>(Manager, Agent) - Linux</td>
  <td>wazuh_modules.debug=2</td>
  <td>&nbsp;</td>
 </tr>
  <td>Agent - Windows</td>
  <td>windows.debug=2</td>
  <td>--wpk_version=v4.x.x</td>
 </tr>

</table>

## Header 2