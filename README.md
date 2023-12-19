## Introduction
Script for assist [ApiFox](https://apifox.com/) in calling [OKX private API](https://www.okx.com/docs-v5/en/#overview-rest-authentication-making-requests).
## Steps
- Add the following Enviornment varilables on [ApiFox](https://apifox.com/). Create them from [OKX](https://www.okx.com/account/my-api)
  *  OKX_API_KEY
  *  OKX_API_SECRET
  *  OKX_PASSPHRASE
- Click "AddPreProcessor" -> "Custom Script" on the folder or API in ApiFox.
- Add the following code.
## Code
```javascript
var cryptoJs = require("crypto-js");

var passphrase = pm.environment.get("OKX_PASSPHRASE");
var apiKey = pm.environment.get("OKX_API_KEY");
var apiSecret = pm.environment.get("OKX_API_SECRET");

var time = new Date().toISOString()
var method = pm.request.method
var url = pm.request.url.getPathWithQuery()
var body = pm.request.body
console.log(time + method + url + body)
var sign = cryptoJs.enc.Base64.stringify(cryptoJs.HmacSHA256(time + method + url + body, apiSecret))
var headers = pm.request.headers
headers.add(
    {
        key: "OK-ACCESS-SIGN",
        value: sign,
    }
);
headers.add(
    {
        key: "OK-ACCESS-TIMESTAMP",
        value: time,
    }
);
headers.add(
    {
        key: "OK-ACCESS-KEY",
        value: apiKey,
    }
);
headers.add(
    {
        key: "OK-ACCESS-PASSPHRASE",
        value: passphrase,
    }
);

```
