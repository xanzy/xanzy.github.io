---
layout: project
project: chef-guard
---

### Chef Server Components
Before explaining the steps needed to install and configure Chef-Guard, let's have a quick look at how the different Chef components work together :

![Server Components without Chef-Guard]({{ site.url }}/images/server_components_without_cg.png)

As you can clearly see on this picture all Chef components are build around the Chef Server which functions as a hub  More information and details about all Chef components shown in this picture <http://docs.opscode.com/server_components.html>

![Server Components with Chef-Guard]({{ site.url }}/images/server_components_with_cg.png)

## _Work in progress, more comming soon..._

Need to make sure you alter the NGINX setting `client_max_body_size 10M`
