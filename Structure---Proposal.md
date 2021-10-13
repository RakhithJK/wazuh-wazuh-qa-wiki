`TENEMOS`

- deps
    - wazuh_testing
         - wazuh_testing 
            - data 
               - archivos sueltos
            - qa-ctl 
            - scripts
            - tools
            - archivos sueltos
         - setup.py 
- docs
- tests
    - integration
    - legacy
    - system

---------------------------------------------------------------------------------------------

`PODRIAMOS`

- deps
    - schemas
        - alerts
            - analysis_alert_windows.json
            - analysis_alert.json
            - state_integrity_analysis_schema.json   `Cambiaria el nombre a` analysis_state_integrity.json
        - events
            - gcp_event.json
            - mitre_event.json
            - syscheck_event_windows.json
            - syscheck_event.json
            - syscollector.py                           `Cambiaria el nombre - Nose si esta bien ubicado`     
            - winevt.py                                 `Cambiaria el nombre - Nose si esta bien ubicado`    
    - custom-config
        - agent.conf
    - simulator
        - keepalives.txt
        - rootcheck.txt
    - qa-ctl
    - setup.py 
- docs
- tests
    - integration
    - legacy
    - system