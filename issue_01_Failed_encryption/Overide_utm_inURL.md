
### Concept:
An URL with format: /v1/[encrypt:JSON-data]?utm_source=ABC&utm_medium=medium_name&utm_campaign=campaign_name </br>

BE: decypted:JSON-data to get all information, check rules, check timestamp valid, generate uuid... </br>
FE: access URL successfully </br>
### Issue: 
Actual: ?utm_source=zzz
Expect: ?utm_source=ABC


### Root cause:
When the users send urls through 3rd party (as Z). App Z automatically add utm_source param to url and overide this.  </br> 
Failed to decrypt URL to get information.
