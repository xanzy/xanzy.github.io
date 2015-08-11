---
layout: project
project: chef-guard
project_name: Chef-Guard
---

### Git Section
Each of the different sections will first show the applicable part of the configuration and then explain every config option separately.

~~~ ini
[git "chef-guard"]
  type            = github   # Valid options are 'github' and 'gitlab'
  serverurl       =          # Empty means that it will use github.com
  sslnoverify     = false
  token           = xxx

[git "org1"]
  type            = gitlab   # Valid options are 'github' and 'gitlab'
  serverurl       =          # Empty means that it will use gitlab.com
  sslnoverify     = false
  token           = xxx
~~~

As you can see you can have multiple GitHub and GitLab configurations. So every GitHub organization or GitLab group you use in your Chef-Guard config, should have it's own named section in order for Chef-Guard to be able to find and connect to that specific GitHub organization or GitLab group.

#### type
The type of git service to use.

#### serverurl
The API URL to your GitHub Enterprise appliance or on-premises GitLab server. Leave blank if you are using [github.com](https://github.com){:target="_blank"} or [gitlab.com](https://gitlab.com){:target="_blank"}

#### sslnoverify
Set to `true` if you are using self-signed certificates for your GitHub Enterprise appliance or on-premises GitLab server, otherwise you should keep this turned off by setting it to `false`.

#### token
A valid OAuth token or private token that can be used to connect to this GitHub organization or GitLab group.
