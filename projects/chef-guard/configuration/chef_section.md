---
layout: project
project: chef-guard
---

### Chef Section
Each of the different sections will first show the applicable part of the configuration and then explain every config option seperately.

~~~ ini
[chef]
  enterprisechef  = true
  server          = chef.company.com
  port            = 443
  sslnoverify     = false
  erchefip        = 127.0.0.1
  erchefport      = 8000
  s3key           = xxx
  s3secret        = xxx
  version         = 11.12.0
  user            = chef-guard
  key             = /opt/chef-guard/chef-guard.pem
~~~

#### enterprisechef
Set to `true` if using Enterprise Chef, otherwise this needs to be `false`.

#### server
The FQDN of your Chef Server (as used by all your nodes to connect to the Chef Server).

#### port
The port your Chef Server is listening on.

#### sslnoverify
Set to `true` if you are using self-signed certificates for your Chef Server, otherwise you should keep this turned off by setting it to `false`.

#### erchefip
The IP address your ErChef API (the actuall Chef Server so to speak) currently is configured to bind to. Unless you specifically altered the default, this should be `127.0.0.1`.

#### erchefport
The port your ErChef API (the actuall Chef Server so to speak) is listening on. Unless you specifically altered the default, this should be `8000`.

#### s3key
The bookshelf access_key_id. Could be found in either `/etc/chef-server/chef-server-secrets.json` (for Open Source Chef) or `/etc/opscode/private-chef-secrets.json` (for Enterpise Chef).

#### s3secret
The bookshelf secret_access_key. Could be found in either `/etc/chef-server/chef-server-secrets.json` (for Open Source Chef) or `/etc/opscode/private-chef-secrets.json` (for Enterpise Chef).

#### version
The Chef-Client version you are using. This settings currently does not have any impact, so just use the default here.

#### user
A Chef user which has access to your Chef Server (for Open Source Chef) or is invited to all configured Chef Orangizations (for Enterpise Chef).

#### key
The path to a .pem file containing a valid key for the configured Chef user.
