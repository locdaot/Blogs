## name: 🐞 Bug | issue_01_encryption/decryption JSON-data in URL (Kong_Gateway_Routing_Traffic)
### Concept:
An URL with format: /v1/[encrypt:JSON-data] </br>

BE: decypted:JSON-data to get all information, check rules, check timestamp valid, generate uuid... </br>
FE: access URL successfully </br>
### Issue: 
Access a valid URL and get err400: `An error occurred during encryption/decryption data process, errorMessage :: java.lang.IllegalArgumentException: Last unit does not have enough valid bits` </br> 

KONG-GATEWAY receiving the client's request >>> matching it to a route >>> apply plugins >>> transforming the path >>> forwarding to the upstream service >>> and returing the response.</br>
### Root cause:
The  following normalization techniques are used for incoming request URIs, duplicate slashes are merged. For example: /foo//bar becomes /foo/bar.
(https://docs.konghq.com/gateway/latest/how-kong-works/routing-traffic/#request-path)</br>
Example:
```
uri:  /api//users 
url: /:port/api//users 
response: { [+]
route: { [+]
service: { [+]
upstream_status: 200
upstream_uri: /api/users 
```
Example of Slash Merging </br>
Incoming Request: http://kong:8000/api//users </br>
Route Configuration:
```
curl -X POST http://localhost:8001/routes \
  --data "paths[]=/api" \
  --data "strip_path=true" \
  --data "service.id=<SERVICE_ID>"
```
Service Configuration:
```
curl -X POST http://localhost:8001/services \
  --data "name=my-service" \
  --data "url=http://upstream:8080"
```
Forwarded Request: http://upstream:8080/users </br>
The // in /api//users is merged into /users after strip_path removes /api.</br>
Even without strip_path, Kong’s normalization might still merge slashes, resulting in http://upstream:8080/api/users.</br>
### How to fix : Scope DevOps
