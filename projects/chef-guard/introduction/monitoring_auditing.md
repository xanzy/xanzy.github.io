---
layout: project
project: chef-guard
---

### Monitoring/Auditing
Currently one of the most important features that Chef Server is missing in order to satify auditors, is a good monitoring and auditing subsystem. But auditors are not the only ones that benefit from a good monitoring and auditing subsystem. Also engineers working with Chef every day can greatly benefit from the fact that they can see who changed what at what time. So in order to fill this gap, Chef-Guard is able to monitor all changes to the following objects:

- Nodes
- Roles
- Environments
- Data Bags
- Cookbooks

As Chef-Guard is a fully server-side solution it doesn't matter how you change the configuration, Chef-Guard will always monitor these objects for you! Of course monitoring a change to an object in itself doesn't solve anything, so below are a few configurable features that determine what Chef-Guard will do when an object is changed. Make sure you also have a look at the [Configuration Changes Workflow]({{ site.url }}/projects/chef-guard/workflows/configuration_changes.html) which gives a graphical overview of the complete workflow including the impact of the configurable options discussed here.

### Validating Changes
You can configure Chef-Guard to validate changes as they are being processed by Chef-Guard. Of course it's impossible for Chef-Guard to determine if the runlist of a node or an attribute value in a role is either good or bad, so it will by no means try to validate those kind of values. The only thing Chef-Guard will validate are any cookbook constrains you defined in the environment, role or cookbook you are changing. The following snippets are examples of such constraints validated by Chef-Guard:

~~~ json
# environment example
{
  "name": "Production",
  "description": "Our production environment for platform X",
  "cookbook_versions": {
    "apache2": "= 1.10.2",
    "java": "= 1.17.6",
    "yum": "= 2.4.4"
  },
  "json_class": "Chef::Environment",
  ...
}
~~~

~~~ json
# role example
{
  "name": "Server Role X",
  "description": "A custom role for server type X",
  "json_class": "Chef::Role",
  ...
  "run_list": [
    "recipe[apache2@1.10.2]",
    "recipe[java@1.17.6]",
    "recipe[yum@2.4.4]"
  ],
  ...
}
~~~

So whenever Chef-Guard finds a constraint for a specific version of a cookbook, it will validate if that specific version is present **and is frozen** in your Chef Server/Organization. The reasoning behind this validation stategy is the assumption that if you depend on a specific version of a cookbook, you would want to be sure that this cookbook never changes. And as long as a cookbook isn't frozen, it can be overwritten as often as you want.

Now I can almost hear you think: "But what if I use `knife ... --force`?" Well in that case Chef-Guard will let you know that is no longer allowed as with Chef-Guard the behavior of `--force` is [changed]({{ side.url }}/projects/chef-guard/introduction/overview.html#changed-behavior-for-knife----force).

### Committing Changes
In order to satify any auditors and give engineers the possibility to search back in time to see who changed what at what time, it will be necessary to save all the changes in such a way that it can be read back at any given point in time. To achive this, Chef-Guard uses a Github repo into which it commits all changes. And as all calls to Chef are authenticated calls, Chef-Guard uses the authenticated user as the author for the commit.

Using a Github repo for monitoring and auditing has an additional advantage, being that you also have a way to go back to a previous configuration when someone makes a mistake. If a role is updated with wrong values or even if the role is completely deleted, you can use the configuration saved in Github (which is all in json format) to restore the role to it's previous state.

Since all your configuration info will be in this Github repo, it's probably **not be a good idea** to use a public repo on [github.com](https://github.com){:target="_blank"} for this! So we **strongly** advise to use either a private repo or, if you have one, a repo on your Github Enterprise appliance!

### Mailing Changes
Being able to look into a Github repo and see all configuration changes is great when you need to find the details of a specific change, but it's much less usefull as a way to keep track of all the day-to-day changes taking place in your Chef Server/Organization. So you can configure Chef-Guard to send a mail for every processed change, containing a diff of the commit done in your Github repo (see examples below). Enabling this feature provides you with a very easy way to keep an eye on things as they happen.

![commit_example1]({{ site.url }}/images/commit_example1.png)
![commit_example2]({{ site.url }}/images/commit_example2.png)
