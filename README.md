# JavaScript API
[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/)
 
## Deploying ##
Click the "Deploy to Azure" button above.  You can create new resources or reference existing ones (resource group, gateway, service plan, etc.)  **Site Name and Gateway must be unique URL hostnames.**  The deployment script will deploy the following:
* Resource Group (optional)
* Service Plan (if you don't reference exisiting one)
* Gateway (if you don't reference existing one)
* API App (JavaScriptAPI)
* API App Host (this is the site behind the api app that this github code deploys to)
 
## API Documentation ##
The API app has one action - Execute Script - which returns a single "Result" parameter.
 
The action has three input parameters:
 
| Input | Description |
| ----- | ----- |
| Script | JavaScript syntax |
| Context Object *(optional)* | Objects to reference in the script.  Can pass in multiple objects, but base must be a single Object { .. }. 
 
#### Context Object Structure ####
```javascript
{ "object1": { ... }, "object2": "value" }
```
In script could then reference object1 and object2 - both passed in as a JToken.

 
###Trigger###
You can use the JavaScript API as a trigger.  It takes a single input of "expression" and will trigger the logic app (and pass result) whenever the script returns anything but `false`.  You set the frequency in which the script runs.
 
## Example ##
| Step   | Info |
|----|----|
| Action | Execute Script |
| Script | `return message;` |
| Context Object | `{"message": {"Hello": "World"}}` |
| Output | `{"Hello": "World"}` |
 
You can also perform more complex scripts like the following:

####Context Object####
```javascript
{ "tax": 0.06, "orders": [{"order": "order1", "subtotal": 100}] }
```
#### Script ####
```javascript
return orders.map(function(obj){obj.total = obj.subtotal * (1 + tax); return obj;});
```
 
#### Result ####
```javascript
[ {"order": "order1", "subtotal": 100, "total": 106.0 } ]
```
 