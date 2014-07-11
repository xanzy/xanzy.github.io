---
layout: project
project: chef-guard
---

### Prerequisites
There are a few steps you need execute before you can start installing and configuring Chef-Guard. Only the steps marked with a * are mandatory, the other steps are dependend on how you choose to configuration/setup Chef-Guard.

### Chef*
Chef-Guard needs a valid Chef user account to talk to your Chef Server/Organization to [validate changes]({{ side.url }}/projects/chef-guard/introduction/monitoring_auditing.html#validating-changes). So you need to create a 'chef-guard' user with at least read rights to your Chef Server/Organization. Chef-Guard will never make any changes to your Chef environment, but will only request information about certain objects. Make sure you save the private key into a .pem file, as you will need this file while configuring Chef-Guard.

### Github
If you want Chef-Guard to [commit changes]({{ side.url }}/projects/chef-guard/introduction/monitoring_auditing.html#committing-changes) made to your Chef Server/Organization, you will need to create a Github repository into which Chef-Guard can commit the changes.

When using Open Source Chef you will need a single repo with the name 'config'. If instead you use Enterprise Chef, it probably makes more sense to create a dedicated 'chef-guard' Github organization with repos for every Chef Organization that you want Chef-Guard to monitor. In this case the names of the repos should be exactly the same as the 'Short Name' of you Chef Organizations.

After creating the needed repo(s) you will also need to [create an OAuth token](https://help.github.com/articles/creating-an-access-token-for-command-line-use){:target="_blank"} that Chef-Guard can use to authenticate with Github. Save both the OAuth token and the Github organization name where you created the repo(s), as you will need these later on when configuring Chef-Guard.

### Supermarket
When you have a local Supermarket installation and you want Chef-Guard to automatically [publish new cookbooks]({{ side.url }}/projects/chef-guard/introduction/cookbook_validation.html#publishing-the-cookbook), you will also need to create a Supermarket user. Currently this is not something you can do easily using a GUI. It will take some digging into the internals of Supermarket.

Internally we created just one user (called 'chef-guard') that manages all cookbook publications. For now, until Supermarket is more mature and ready for on premises use, this seems a feasible solution. You can manually add users by following this thread <https://github.com/opscode/supermarket/issues/425>.

_NOTE: The key we used to create the user was the public key of the already created Chef user. When using this public key to create the user, you will only need to save a single .pem file for your Chef-Guard configuration._
