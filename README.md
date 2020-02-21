# [SmartThings](https://www.smartthings.com/) REST API

## Setup

### Installation

1. Login at https://graph.api.smartthings.com/
2. Navigate: My SmartApps > New SmartApp > From Code (tab)
3. Paste contents of rest-api.groovy
4. Click: Create, Publish > For Me
6. Navigate: App Settings > OAuth
7. Click: Enable OAuth in Smart App
8. Note the Client ID (i.e. `abc123`) and Client Secret (i.e. `def321`)

### Get Access Token via OAuth 

1. Open this URL in a browser: https://graph.api.smartthings.com/oauth/authorize\?response_type\=code\&client_id\=abc123\&scope\=app\&redirect_uri\=http://localhost
2. Login and Click "Authorize".
3. You will be redirected to http://localhost; note the code query value (i.e. aD4kF5 in http://localhost/?code=aD4kF5)
4. Run `curl -v -H "Content-Type: application/x-www-form-urlencoded" -X POST --data 'grant_type=authorization_code&code=aD4kF5&client_id=abc123&client_secret=def321&redirect_uri=http%3A%2F%2Flocalhost' https://graph.api.smartthings.com/oauth/token`
5. Note access_token in response (i.e. `xyz123`).  **This will be the access token used to authenticate to the REST API.**
6. Run `curl -v -H "Authorization: Bearer xyz123" https://graph.api.smartthings.com/api/smartapps/endpoints`
7. Note the `uri` value (i.e. https://graph1.smartthings.com/api/smartapps/installations/123987). **This will be the Endpoint URL used to access the REST API.**

## Usage

_(Assumming Access Token: `xyz123` and Endpoint URL: `https://graph1.smartthings.com/api/smartapps/installations/123987`)_


#### List Devices
```
curl -v -H "Authorization: Bearer xyz123" \
  https://graph1.smartthings.com/api/smartapps/installations/123987/devices
```

#### Get Device Attribute (i.e. on/off for a switch)
```
curl -v -H "Authorization: Bearer xyz123" \
  https://graph1.smartthings.com/api/smartapps/installations/123987/device/123/attribute/switch
```

#### Turn Device On
```
curl -H "Authorization: Bearer xyz123" -X POST \
  https://graph1.smartthings.com/api/smartapps/installations/123987/device/123/command/on
```

#### Turn Device Off
```
curl -H "Authorization: Bearer xyz123" -X POST \
  https://graph1.smartthings.com/api/smartapps/installations/123987/device/123/command/off
```

#### Adjust Brightness to 50%
```
curl -H "Authorization: Bearer xyz123" -X POST \
  https://graph1.smartthings.com/api/smartapps/installations/123987/device/123/command/setLevel?arg=50
```
