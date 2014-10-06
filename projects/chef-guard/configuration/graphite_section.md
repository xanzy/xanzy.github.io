---
layout: project
project: chef-guard
project_name: Chef-Guard
---

### Graphite Section
Each of the different sections will first show the applicable part of the configuration and then explain every config option seperately.

~~~ ini
[graphite]
  server          = 127.0.0.1
  port            = 2003
~~~

#### server
The IP addres of FQDN of your Graphite server. Leave empty if you do not want to collect metrics about cookbook uploads.

#### port
The port your Graphite server is listening on.
