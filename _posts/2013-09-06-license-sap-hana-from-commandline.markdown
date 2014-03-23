---
comments: true
layout: post
title: How to license SAP HANA from the command line
tags:
- SAP
- HANA
---
To license an SAP HANA instance if you're not able to connect via HANA studio, follow this procedure:

1. Start-Up the system

    ```bash
HDB start
    ```
2. Grab the Hardware Key via hdbsql

    ```bash
    export INSTANCE=01
    hdbsql -n localhost:3${INSTANCE}15 -i ${INSTANCE} -u SYSTEM -p password -m <<EOF
    SELECT * FROM M_LICENSE;
    EOF
    
    Welcome to the SAP HANA Database interactive terminal.
    
    Type:  \h for help with commands
    \q to quit
    
     HARDWARE_KEY,SYSTEM_ID,INSTALL_NO,SYSTEM_NO,PRODUCT_NAME,PRODUCT_LIMIT,PRODUCT_USAGE,START_DATE,EXPIRATION_DATE,LAST_SUCCESSFUL_CHECK,PERMANENT,VALID,ENFORCED,LOCKED_DOWN,MEASUREMENT_XML
     "**THIS IS THE HARDWARE KEY**","XXXX","XXXX","XXXX","SAP-HANA",8192,2576,"2013-09-05 00:00:00.000000000","2013-09-05 00:00:00.000000000","2013-09-06 00:00:00.000000000","TRUE","TRUE","FALSE","FALSE","<?xml version=\"1.0\" encoding=\"UTF-8\"?><!DOCTYPE Measurement SYSTEM \"GLasXML1.0.dtd\"><Measurement><Header><Transfer><Version>GXML1.0</Version><Program>SAPHANA1.0</Program><Name>SYSTEM</Name></Transfer><Sender/><Receiver/></Heade
    ```

3. Request a new license key
4. Copy the license key file (in this example XX1_Standard.txt) to the target system and execute the following commands:

    ```bash
    LICENSE=$(cat XX1_Standard.txt)
    INSTANCE=01
    hdbsql -n localhost:3${INSTANCE}15 -i ${INSTANCE} -u SYSTEM -p password -m <<EOF
    SET SYSTEM LICENSE '${LICENSE}';
    EOF
    
    
    0 rows affected (overall time 129.122 msec; server time 115.297 msec)
    Check if the license was successfully applied by repeating step 2
    ```
    
5. Check if the license was successfully applied by repeating step 2
