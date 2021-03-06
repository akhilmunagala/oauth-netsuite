oauth-netsuite
==========

OAuth-Netsuite is a fork of OAuth 1.0a specifically for use with NetSuite's SuiteScript 2.0.

It includes the crypto.js dependency necessary to make it function.

## Quick Start

1. Copy ``oauth.js`` and ``cryptojs.js`` to your SuiteScript Filecabinet location.
2. Create a ``secret.js`` file in there as well. (*DO NOT COMMIT THIS TO YOUR REPOSITORY. ADD IT TO YOUR .gitignore*)

Here is an example of a ``secret.js`` file:
```js

define([], function() {
    return {
        consumer: {
            public: '01234567890abcdef01234567890abcdef01234567890abcdef01234567890ab',
            secret: '01234567890abcdef01234567890abcdef01234567890abcdef01234567890ab'
        },
        token: {
            public: '01234567890abcdef01234567890abcdef01234567890abcdef01234567890ab',
            secret: '01234567890abcdef01234567890abcdef01234567890abcdef01234567890ab'
        },
        realm: '1234567'
    }
});

```

Profit! Here is a debugger example using the library

```js
require(['N/https', '/SuiteScripts/oauth', '/SuiteScripts/secret'], function(https, oauth, secret) {

    var url = 'https://rest.sandbox.netsuite.com/app/site/hosting/restlet.nl?script=123&deploy=1';
    var method = 'GET';
    var headers = oauth.getHeaders({
        url: url,
        method: method,
        tokenKey: secret.token.public,
        tokenSecret: secret.token.secret
    });

    headers['Content-Type'] = 'application/json';

    var restResponse = https.get({
        url: url,
        headers: headers
    });
    log.debug('response', JSON.stringify(restResponse));
    log.debug('headers', headers);

});
```

And here is a PUT example:
```js
require(['N/https', '/SuiteScripts/oauth', '/SuiteScripts/secret'], function(https, oauth, secret) {

    var url = 'https://rest.sandbox.netsuite.com/app/site/hosting/restlet.nl?script=123&deploy=1';
    var method = 'PUT';
    var headers = oauth.getHeaders({
        url: url,
        method: method,
        tokenKey: secret.token.public,
        tokenSecret: secret.token.secret
    });

    var body = {
        datum1: 'datum1',
        datum2: 'foobar'
    }

    headers['Content-Type'] = 'application/json';

    var restResponse = https.put({
        url: url,
        headers: headers,
        body: JSON.stringify(body)
    });
    log.debug('response', JSON.stringify(restResponse));
    log.debug('headers', headers);

});
```
*NOTE*: You do not need to add the Body to the OAuth constructor.

By default, the oauth library comes with a wrapper function ``getHeaders`` using CryptoJS's SHA256 encryption.
``getHeaders`` will extract your Query params automatically. Be sure to add any PUT/POST params to ``options.data`` though.


The OAuth-Netsuite library also exports the OAuth object (that has Realm functionality) and the CryptoJS hash functions available.

```js
oauth = {
    getHeaders: getHeaders
    OAuth: OAuth
    sha1: hash_function_sha1,
    sha256: hash_function_sha256,
}
```

See the OAuth 1.0a or CryptoJS documentation for more information about those methods.

Hopefully more documentation and type annotations coming soon!
