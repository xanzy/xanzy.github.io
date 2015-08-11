---
layout: project
project: chef-guard
project_name: Chef-Guard
---

### Community Section
Each of the different sections will first show the applicable part of the configuration and then explain every config option separately.

~~~ ini
[community]
  supermarket     = https://supermarket.chef.io
  forks           = org1     # When using multiple orgs (divided by a ','), the order here determines the lookup order!
~~~

#### supermarket
The address of the community Supermarket. At the time of writing this is <https://supermarket.getchef.com>.

#### forks
A GitHub organizations or GitLap projects where Chef-Guard will search for forked community cookbooks. \\
_NOTE: This options need more/better explaning!_
