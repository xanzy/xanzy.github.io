---
layout: project
project: chef-guard
project_name: Chef-Guard
---

### Prerequisites
There are a few steps you need to take/execute before you can start installing and configuring Chef-Guard. Only the steps marked with a '*' are mandatory, the other steps are dependent on how you choose to configure/setup Chef-Guard.

### *Chef
Chef-Guard needs a valid Chef user account to talk to your Chef Server/Organization to [validate changes]({{ side.url }}/projects/chef-guard/introduction/monitoring_auditing.html#validating-changes). So you need to create a 'chef-guard' user with at least read rights to your Chef Server/Organization. Chef-Guard will never make any changes to your Chef environment, but will only request information about certain objects. Make sure you save the private key as a .pem file, as you will need this file while configuring Chef-Guard.

### *Chef-Client version >= 11.14.0
Because of a bug ([CHEF-5261](https://tickets.opscode.com/browse/CHEF-5261){:target="_blank"}) in older clients, it is **strongly advised** to use at least version 11.14.0 in your environment. Older clients will work for the most part, but you will see some random Chef run failures. Also using certain knife commands (e.g. `knife cookbook list`) from a machine with an older client, will fail.

### Berkshelf version >= 3.2.0
When you want to use Chef-Guard for more than only monitoring and you are using Berkshelf to manage your cookbook dependencies, you will need to use at least version 3.2.0. This version contains a [change](https://github.com/berkshelf/berkshelf/commit/8313c9139c92dbb5e3dfa888426e1afb2cb0ea6c){:target="_blank"} that will make sure the cookbooks are uploaded in their respective dependency order. Which in turn is needed when having dependencies on specific versions that are uploaded during one and the same `berks upload` command. Older versions will upload the cookbooks in alphabetical order which could break the Chef-Guard dependency checks.

Note that you will have to add enable the alternative ordering by adding the following line to your knife.rb:

~~~ ruby
knife[:chef_guard] = true
~~~

### GitHub/GitLab
If you want Chef-Guard to [commit changes]({{ side.url }}/projects/chef-guard/introduction/monitoring_auditing.html#committing-changes) made to your Chef Server/Organization, you will need to create a GitHub repository or GitLab project into which Chef-Guard can commit the changes.

When using Open Source Chef 11 you will need a single repo/project with the name 'config'. If instead you use Enterprise Chef 11 or Chef 12, it makes more sense to create a dedicated 'chef-guard' GitHub organization or GitLab group with repos/projects for every Chef Organization that you want Chef-Guard to monitor. In this case the names of the repos/projects should be exactly the same as the 'Short Name' of your Chef Organizations.

If you are using GitHub you will need to [create an OAuth token](https://help.github.com/articles/creating-an-access-token-for-command-line-use){:target="_blank"} that Chef-Guard can use to authenticate with GitHub.

If you are using GitLab you will need to [retrieve a private token](https://www.safaribooksonline.com/library/view/gitlab-cookbook/9781783986842/ch06s05.html){:target="_blank"} that Chef-Guard can use to authenticate with GitLab.

### Supermarket
When you have a local Supermarket installation and you want Chef-Guard to automatically [publish new cookbooks]({{ side.url }}/projects/chef-guard/introduction/cookbook_validation.html#publishing-the-cookbook), you will also need to create a Supermarket user.

Internally we created just one user (called 'chef-guard') that manages all cookbook publications which seems a feasible solution. You can manually add users by following this (**outdated**) thread <https://github.com/opscode/supermarket/issues/425> or by the following the more recent documentation: <https://github.com/chef/supermarket/blob/master/docs/CONFIGURING.md>

_NOTE: The key we used to create the user was the public key of the already created Chef user. When using this public key to create the user, you will only need to save a single .pem file for your Chef-Guard configuration._
