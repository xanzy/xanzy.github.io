---
layout: project
project: chef-guard
project_name: Chef-Guard
---

### Chef Section
Each of the different sections will first show the applicable part of the configuration and then explain every config option seperately.

~~~ ini
[chef]
  type            = enterprise    # Valid options are 'enterprise', 'opensource' and 'goiardi'
  version         = 12
  server          = chef.company.com
  port            = 443
  sslnoverify     = false
  erchefip        = 127.0.0.1
  erchefport      = 8000
  bookshelfkey    = xxx
  bookshelfsecret = xxx
  user            = chef-guard
  key             = /opt/chef-guard/chef-guard.pem
~~~

#### type
The type of Chef Server you are using. When using Chef 12, you should choose `enterprise` here.

#### version
The Chef Server version you are using. Only `11` and `12` are known versions.

#### server
The FQDN of your Chef Server (as used by all your nodes to connect to the Chef Server).

#### port
The port your Chef Server is listening on.

#### sslnoverify
Set to `true` if you are using self-signed certificates for your Chef Server, otherwise you should keep this turned off by setting it to `false`.

#### erchefip
The IP address your ErChef API (the actual Chef Server so to speak) currently is configured to bind to. Unless you specifically altered the default, this should be `127.0.0.1`.

#### erchefport
The port your ErChef API (the actual Chef Server so to speak) is listening on. Unless you specifically altered the default, this should be `8000`.

#### bookshelfkey
The bookshelf access_key_id. Could be found in either `/etc/opscode/private-chef-secrets.json` (for Enterpise Chef 11) or `/etc/chef-server/chef-server-secrets.json` (for Open Source Chef 11 and Chef 12).

#### bookshelfsecret
The bookshelf secret_access_key. Could be found in either `/etc/opscode/private-chef-secrets.json` (for Enterpise Chef 11) or `/etc/chef-server/chef-server-secrets.json` (for Open Source Chef 11 and Chef 12).

#### user
A Chef user which has access to your Chef Server (for Open Source Chef 11) or is invited to all configured Chef Organizations (for Enterprise Chef 11 or Chef 12).

#### key
The path to a .pem file containing a valid key for the configured Chef user.
