---
layout: project
project: chef-guard
project_name: Chef-Guard
---

### Github Section
Each of the different sections will first show the applicable part of the configuration and then explain every config option seperately.

~~~ ini
[github "chef-guard"]
  serverurl       =       # Empty means that it will use github.com
  sslnoverify     = false
  token           = xxx

[github "org1"]
  serverurl       = https://github.company.com/api/v3
  sslnoverify     = true
  token           = xxx
~~~

As you can see you can have multiple github configurations. So every Github organization you use in your Chef-Guard config, should have it's own named section in order for Chef-Guard to be able to find and connect to that specific Github organization.

#### serverurl
The Github API URL to your Github Enterpise appliance. Leave blank if you are using [github.com](https://github.com){:target="_blank"}.

#### sslnoverify
Set to `true` if you are using self-signed certificates for your Github Enterpise appliance, otherwise you should keep this turned off by setting it to `false`.

#### token
A valid OAuth token that can be used to connect to this Github organization.
