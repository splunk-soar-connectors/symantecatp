[comment]: # "Auto-generated SOAR connector documentation"
# Symantec ATP

Publisher: Splunk  
Connector Version: 1\.0\.12  
Product Vendor: Symantec  
Product Name: Symantec ATP  
Product Version Supported (regex): "\.\*"  
Minimum Product Version: 4\.0\.1068  

This app integrates with a Symantec ATP \(Advanced Threat Protection\) device to implement ingestion, investigative and containment actions

### Configuration Variables
The below configuration variables are required for this Connector to operate.  These variables are specified when configuring a Symantec ATP asset in SOAR.

VARIABLE | REQUIRED | TYPE | DESCRIPTION
-------- | -------- | ---- | -----------
**server** |  required  | string | URL
**verify\_server\_cert** |  required  | boolean | Verify server certificate
**client\_id** |  required  | string | OAuth client ID
**client\_secret** |  required  | password | OAuth client secret key
**first\_scheduled\_ingestion\_span** |  optional  | numeric | Limit last n days for first scheduled polling
**first\_scheduled\_ingestion\_limit** |  optional  | numeric | Limit last n incidents for first scheduled polling
**poll\_now\_ingestion\_span** |  optional  | numeric | Limit last n days for 'Poll Now'

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

For the first poll, ingest all the atp incidents during past number of days set in <b>first\_scheduled\_ingestion\_span</b>, not more than the limit set in <b>first\_scheduled\_ingestion\_limit</b>\. Subsequent polls ingest new or updated incidents\. The incidents are sorted in descending order based on the priority and updated time\.<table><tbody><tr class='plain'><th>IOC</th><th>Artifact Name</th><th>CEF Field</th></tr><tr><td>Address IPv4</td><td>IP Artifact</td><td>externalAddress, internalAddress, deviceAddress, destinationAddress, sourcefileAddress, dataSourceIPAddress</td></tr><tr><td>File</td><td>File Artifact</td><td>fileHashMd5, fileHashSha256, fileName</td></tr><tr><td>Host</td><td>Domain Artifact</td><td>sourceDnsDomain</td></tr><tr><td>URL</td><td>URL Artifact</td><td>dataSourceURL, intrusionURL</td></tr><tr><td>Endpoint</td><td>Endpoint Artifact</td><td>deviceUid</td></tr><tr><td>Email</td><td>Email Artifact</td><td>sourceEmailAddress, sourceAddress, receiverAddress</td></tbody></table>

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**container\_count** |  optional  | Maximum number of containers to ingest | numeric | 
**container\_id** |  optional  | Comma \(','\) separated container IDs | string | 
**start\_time** |  optional  | Start of time range, in epoch time \(milliseconds\) | numeric | 
**artifact\_count** |  optional  | Parameter ignored in this app | numeric | 
**end\_time** |  optional  | End of time range, in epoch time \(milliseconds\) | numeric | 

#### Action Output
No Output  

## action: 'delete file'
Delete a file from an endpoint

Type: **contain**  
Read only: **False**

This action supports deleting multiple files on the endpoint\. If any of the hash does not belong to the device\_uid, the action would fail\.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**hash** |  required  | Comma separated \(','\) SHA256 of the files | string |  `sha256` 
**device\_uid** |  required  | Device UID | string |  `symantecatp target endpoint` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.data\.\*\.command\_id | string |  `symantecatp command id` 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.parameter\.hash | string |  `sha256` 
action\_result\.parameter\.device\_uid | string |  `symantecatp target endpoint` 
action\_result\.summary\.command\_id | string |  `symantecatp command id` 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'quarantine device'
Quarantine an endpoint

Type: **contain**  
Read only: **False**

This action may take time to isolate the endpoint\. To check status of action, execute <b>get status</b> using the resulting <b>command\_id</b>\.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**targets** |  required  | Comma \(','\) separated targets to isolate | string |  `symantecatp target endpoint` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.data\.\*\.command\_id | string |  `symantecatp command id` 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.parameter\.targets | string |  `symantecatp target endpoint` 
action\_result\.summary\.command\_id | string |  `symantecatp command id` 
action\_result\.message | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'unquarantine device'
Unquarantine an endpoint

Type: **correct**  
Read only: **False**

This action may take time to rejoin the endpoint\. To check status of action, execute <b>get status</b> using the resulting <b>command\_id</b>\.

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**targets** |  required  | Comma \(','\) separated targets to rejoin | string |  `symantecatp target endpoint` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.data\.\*\.command\_id | string |  `symantecatp command id` 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.parameter\.targets | string |  `symantecatp target endpoint` 
action\_result\.summary\.command\_id | string |  `symantecatp command id` 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'check status'
Check the status of an action

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**command\_id** |  required  | Command ID of action | string |  `symantecatp command id` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.data\.\*\.action | string | 
action\_result\.data\.\*\.status\.\*\.state | numeric | 
action\_result\.data\.\*\.status\.\*\.target | string |  `symantecatp target endpoint` 
action\_result\.data\.\*\.status\.\*\.message | string | 
action\_result\.data\.\*\.status\.\*\.error\_code | numeric | 
action\_result\.data\.\*\.command\_id | string |  `symantecatp command id` 
action\_result\.status | string | 
action\_result\.message | string | 
action\_result\.parameter\.command\_id | string |  `symantecatp command id` 
action\_result\.summary\.target\_status | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric |   

## action: 'hunt file'
Retrieve information about a file

Type: **investigate**  
Read only: **True**

#### Action Parameters
PARAMETER | REQUIRED | DESCRIPTION | TYPE | CONTAINS
--------- | -------- | ----------- | ---- | --------
**hash** |  required  | Hash of file \(MD5 or SHA256\) | string |  `md5`  `sha256`  `hash` 

#### Action Output
DATA PATH | TYPE | CONTAINS
--------- | ---- | --------
action\_result\.status | string | 
action\_result\.parameter\.hash | string |  `md5`  `sha256`  `hash` 
action\_result\.data\.\*\.file\_list\.\*\.file\_age | numeric | 
action\_result\.data\.\*\.file\_list\.\*\.file\_health | numeric | 
action\_result\.data\.\*\.file\_list\.\*\.file\_instances\.\*\.name | string | 
action\_result\.data\.\*\.file\_list\.\*\.md5 | string |  `md5` 
action\_result\.data\.\*\.file\_list\.\*\.mime\_type | string | 
action\_result\.data\.\*\.file\_list\.\*\.prevalence\_band | numeric | 
action\_result\.data\.\*\.file\_list\.\*\.reputation\_band | numeric | 
action\_result\.data\.\*\.file\_list\.\*\.sha2 | string |  `sha256` 
action\_result\.data\.\*\.file\_list\.\*\.targeted\_attack | boolean | 
action\_result\.data\.\*\.file\_list\.\*\.threat\_name | string | 
action\_result\.data\.\*\.total | numeric | 
action\_result\.summary\.files\_found | numeric | 
action\_result\.message | string | 
summary\.total\_objects | numeric | 
summary\.total\_objects\_successful | numeric | 