---
layout: project
project: chef-guard
---

### Customer Section
Each of the different sections will first show the applicable part of the configuration and then explain every config option seperately.

~~~ ini
[customer "demo1"]
  commitchanges   = true
  mailchanges     = false
  mode            = permissive
  blacklist       = ^some.*[^abd]ookbook$, ^another(?:some)?cook.*$

[customer "demo2"]
  mode            = enforced
  gitcookbookorgs = demo2 # If a customer org is used in conjunction with default org(s), The default orgs are searched first!
~~~

**NOTE: This sections is only needed/used when you are using Enterprise Chef!**

The Customer Section is a special section, that is only meant for setups that use Enterprise Chef. The Customer Section does not have any 'own' settings, but you can override any specific setting configured in the [Default Section]({{ site.url }}/projects/chef-guard/configuration/default_section.html). This enables you to have a completely different configuration for each individual Chef Organization. So they can for example have different operating modes or different maildomains. Simple specify the setting you want to override for a specific Chef Organization and assign it a custom value.

**There are however 2 exceptions!** Both the `blacklist` and the `gitcookbookorgs` settings are complementary to the values assigned for these settings in the [Default Section]({{ site.url }}/projects/chef-guard/configuration/default_section.html). Meaning that they will **both** be used! Of all other settings only the custom value (in configured) will be used and the default one ignore.
