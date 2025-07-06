## name: 🐞 Bug | issue_02_overide_params_in_URL
### Concept:
An URL with format: `/pos/BqaCjo8ShMHkiYqNltuQ%3D%3D?utm_source=ABC&user_id=ONSWkY%2Bz8xzLP9ykH4fA%3D%3D&url_success=abcapp%3A%2F%2%2embedded%3Fresult%3Dsuccess&url_fail=homezuiapp%3A%2F%2Fapp%2Fembedded%3Fresult%3Dfail` </br>


BE: decoding string after pos/ to get all information </br>
FE: access URL successfully </br>
### Issue: 
**Actual**: ?utm_source=zzz </br>
**Expect**: ?utm_source=ABC


### Root cause:
When the users send urls through 3rd party (as Z). App Z automatically add utm_source param to url and overide this.  </br> 
Failed to decrypt URL to get information.
