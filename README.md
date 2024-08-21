[comment]: # "Auto-generated SOAR connector documentation"
# Symantec ATP

Publisher: Splunk  
Connector Version: 1.0.14  
Product Vendor: Symantec  
Product Name: Symantec ATP  
Product Version Supported (regex): ".\*"  
Minimum Product Version: 4.0.1068  

This app integrates with a Symantec ATP (Advanced Threat Protection) device to implement ingestion, investigative and containment actions

### Configuration Variables
The below configuration variables are required for this Connector to operate.  These variables are specified when configuring a Symantec ATP asset in SOAR.

VARIABLE | REQUIRED | TYPE | DESCRIPTION
-------- | -------- | ---- | -----------
**server** |  required  | string | URL
**verify_server_cert** |  required  | boolean | Verify server certificate
**client_id** |  required  | string | OAuth client ID
**client_secret** |  required  | password | OAuth client secret key
**first_scheduled_ingestion_span** |  optional  | numeric | Limit last n days for first scheduled polling
**first_scheduled_ingestion_limit** |  optional  | numeric | Limit last n incidents for first scheduled polling
**poll_now_ingestion_span** |  optional  | numeric | Limit last n days for 'Poll Now'

### Supported Actions  
[test connectivity](#action-test-connectivity) - Validate credentials provided for connectivity  
[on poll](#action-on-poll) - Ingest incidents  
[delete file](#action-delete-file) - Delete a file from an endpoint  
[quarantine device](#action-quarantine-device) - Quarantine an endpoint  
[unquarantine device](#action-unquarantine-device) - Unquarantine an endpoint  
[check status](#action-check-status) - Check the status of an action  
[hunt file](#action-hunt-file) - Retrieve information about a file  

## action: 'test connectivity'
Validate credentials provided for connectivity

Type: **test**  
Read only: **True**

#### Action Parameters
No parameters are required for this action

#### Action Output
No Output  

## action: 'on poll'
Ingest incidents

Type: **ingest**  
Read only: **True**

For the first poll, ingest all the atp incidents during past number of days set in <b>first_scheduled_ingestion_span</b>, not more than the limit set in <b>first_scheduled_ingestion_limit</b>. Subsequent polls ingest new or updated incidents. The incidents are sorted in descending order based on the priority and updated time.<table><tbody><tr class='plain'><th>IOC</th><th>Artifact Name</th><th>CEF Field</th></tr><tr><td>Address IPv4</td><td>IP Artifact</td><td>externalAddress, internalAddress, deviceAddress, destinationAddress, sourcefileAddress, dataSourceIPAddress</td></tr><tr><td>File</td><td>File Artifact</td><td>fileHashMd5, fileHashSha256, fileName</td></tr><tr><td>Host</td><td>Domain Artifact</td><td>sourceDnsDomain</td></tr><tr><td>URL</td><td>URL Artifact</td><td>dataSourceURL, intrusionURL</td></tr><tr><td>Endpoint</td><td>Endpoint Artifact</td><td>deviceUid</td></tr><tr><td>Email</td><td>Email Artifact</td><td>sourceEmailAddress, sourceAddress, receiverAddress</td></tbody></table>

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**container_count** |  optional  | Maximum number of containers to ingest | numeric | 
**container_id** |  optional  | Comma (',') separated container IDs | string | 
**start_time** |  optional  | Start of time range, in epoch time (milliseconds) | numeric | 
**artifact_count** |  optional  | Parameter ignored in this app | numeric | 
**end_time** |  optional  | End of time range, in epoch time (milliseconds) | numeric | 

#### Action Output
No Output  

## action: 'delete file'
Delete a file from an endpoint

Type: **contain**  
Read only: **False**

This action supports deleting multiple files on the endpoint. If any of the hash does not belong to the device_uid, the action would fail.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**hash** |  required  | Comma separated (',') SHA256 of the files | string |  `sha256` 
**device_uid** |  required  | Device UID | string |  `symantecatp target endpoint` 

#### Action Output
DATA PATH | TYPE | CONTAINS | EXAMPLE VALUES
--------- | ---- | -------- | --------------
action_result.data.\*.command_id | string |  `symantecatp command id`  |  
action_result.status | string |  |  
action_result.message | string |  |  
action_result.parameter.hash | string |  `sha256`  |  
action_result.parameter.device_uid | string |  `symantecatp target endpoint`  |  
action_result.summary.command_id | string |  `symantecatp command id`  |  
summary.total_objects | numeric |  |  
summary.total_objects_successful | numeric |  |    

## action: 'quarantine device'
Quarantine an endpoint

Type: **contain**  
Read only: **False**

This action may take time to isolate the endpoint. To check status of action, execute <b>get status</b> using the resulting <b>command_id</b>.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**targets** |  required  | Comma (',') separated targets to isolate | string |  `symantecatp target endpoint` 

#### Action Output
DATA PATH | TYPE | CONTAINS | EXAMPLE VALUES
--------- | ---- | -------- | --------------
action_result.data.\*.command_id | string |  `symantecatp command id`  |  
action_result.status | string |  |  
action_result.message | string |  |  
action_result.parameter.targets | string |  `symantecatp target endpoint`  |  
action_result.summary.command_id | string |  `symantecatp command id`  |  
action_result.message | string |  |  
summary.total_objects | numeric |  |  
summary.total_objects_successful | numeric |  |    

## action: 'unquarantine device'
Unquarantine an endpoint

Type: **correct**  
Read only: **False**

This action may take time to rejoin the endpoint. To check status of action, execute <b>get status</b> using the resulting <b>command_id</b>.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**targets** |  required  | Comma (',') separated targets to rejoin | string |  `symantecatp target endpoint` 

#### Action Output
DATA PATH | TYPE | CONTAINS | EXAMPLE VALUES
--------- | ---- | -------- | --------------
action_result.data.\*.command_id | string |  `symantecatp command id`  |  
action_result.status | string |  |  
action_result.message | string |  |  
action_result.parameter.targets | string |  `symantecatp target endpoint`  |  
action_result.summary.command_id | string |  `symantecatp command id`  |  
summary.total_objects | numeric |  |  
summary.total_objects_successful | numeric |  |    

## action: 'check status'
Check the status of an action

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**command_id** |  required  | Command ID of action | string |  `symantecatp command id` 

#### Action Output
DATA PATH | TYPE | CONTAINS | EXAMPLE VALUES
--------- | ---- | -------- | --------------
action_result.data.\*.action | string |  |  
action_result.data.\*.status.\*.state | numeric |  |  
action_result.data.\*.status.\*.target | string |  `symantecatp target endpoint`  |  
action_result.data.\*.status.\*.message | string |  |  
action_result.data.\*.status.\*.error_code | numeric |  |  
action_result.data.\*.command_id | string |  `symantecatp command id`  |  
action_result.status | string |  |  
action_result.message | string |  |  
action_result.parameter.command_id | string |  `symantecatp command id`  |  
action_result.summary.target_status | string |  |  
summary.total_objects | numeric |  |  
summary.total_objects_successful | numeric |  |    

## action: 'hunt file'
Retrieve information about a file

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**hash** |  required  | Hash of file (MD5 or SHA256) | string |  `md5`  `sha256`  `hash` 

#### Action Output
DATA PATH | TYPE | CONTAINS | EXAMPLE VALUES
--------- | ---- | -------- | --------------
action_result.status | string |  |   success 
action_result.parameter.hash | string |  `md5`  `sha256`  `hash`  |   f618862c3754c54581b0db78cb46c788a27104422cf42c7981849d18a96f9d64 
action_result.data.\*.file_list.\*.file_age | numeric |  |   2 
action_result.data.\*.file_list.\*.file_health | numeric |  |   3 
action_result.data.\*.file_list.\*.file_instances.\*.name | string |  |   d2.exe 
action_result.data.\*.file_list.\*.md5 | string |  `md5`  |   1d5731cbee22dbad79ae45ea378ffef9 
action_result.data.\*.file_list.\*.mime_type | string |  |   application/x-dosexec 
action_result.data.\*.file_list.\*.prevalence_band | numeric |  |   2 
action_result.data.\*.file_list.\*.reputation_band | numeric |  |   6 
action_result.data.\*.file_list.\*.sha2 | string |  `sha256`  |   f618862c3754c54581b0db78cb46c788a27104422cf42c7981849d18a96f9d64 
action_result.data.\*.file_list.\*.targeted_attack | boolean |  |   True  False 
action_result.data.\*.file_list.\*.threat_name | string |  |   W32.Golroted 
action_result.data.\*.total | numeric |  |   1 
action_result.summary.files_found | numeric |  |   1 
action_result.message | string |  |   Files found: 1 
summary.total_objects | numeric |  |   1 
summary.total_objects_successful | numeric |  |   1 