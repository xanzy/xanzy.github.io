---
layout: project
project: chef-guard
---

### Supermarket Section
Each of the different sections will first show the applicable part of the configuration and then explain every config option seperately.

~~~ ini
[supermarket]
  server          = supermarket.company.com
  port            = 443
  sslnoverify     = false
  version         = 11.12.0
  user            = chef-guard
  key             = /opt/chef-guard/chef-guard.pem
~~~

#### server
The FQDN of your **private** Supermarket.

#### port
The port your **private** Supermarket is listening on.

#### sslnoverify
Set to `true` if you are using self-signed certificates for your **private** Supermarket , otherwise you should keep this turned off by setting it to `false`.

#### version
The Chef-Client version you are using. This settings currently does not have any impact, so just use the default here.

#### user
A Supermarket user which has access to your **private** Supermarket.

#### key
The path to a .pem file containing a valid key for the configured Supermarket user.
