---
layout: project
project: chef-guard
project_name: Chef-Guard
---

### Default Section
Each of the different sections will first show the applicable part of the configuration and then explain every config option seperately.

~~~ ini
[default]
  listen          = 127.0.0.2
  logfile         = /var/log/chef-guard.log
  tempdir         = /var/tmp/chef-guard
  mode            = silent      # Valid options are 'silent', 'permissive' and 'enforced'
  maildomain      = company.com
  mailserver      = smtp.company.com
  mailport        = 25
  mailrecipient   = chef-changes@company.com
  validatechanges = false
  commitchanges   = false
  mailchanges     = true
  searchgithub    = true
  publishcookbook = true
  blacklist       =      # This can be multiple regexes divided by a ','
  gitorganization = chef-guard
  gitcookbookorgs = org1, org2 # When using multiple orgs (divided by a ','), the order here determines the lookup order!
  includefcs      =      # This should be the full path to a custom .rb file containing your custom checks
  excludefcs      =      # This can be multiple FC's divided by a ','
~~~

#### listen
The IP address the Chef-Guard server will bind to. As Chef-Guard should always run on your Chef Server, the default of `127.0.0.2` makes the most sense and doesn't require any additional configuration.

#### logfile
The logfile Chef-Guard will use to write all log events to.

#### tempdir
In order to run the tests (e.g. foodcritic, rubocop) the cookbook files need to be on disk. The tempdir is used to stage the files of cookbooks that are being tested.

#### mode
This determines the mode Chef-Guard operates in. In silent mode it will only do monitoring and auditing. When changing to permissive mode, Chef-Guard will also do validations and tests but in this mode you can bypass failed tests by using `--force` while uploading a cookbook. When you set Chef-Guard to operate in enforced mode there is no more escaping or bypassing possible. All validations and tests must be green in order for a change or cookbook upload to succeed. Please see (among other sections) the [Cookbook Upload]({{ site.url }}/projects/chef-guard/workflows/cookbook_upload.html) section for more details.

#### maildomain
This domain is used to set the sending mail domain. Settings this to a correct value may help prevent mail from Chef-Guard being threated as spam. In addition the domain is also used to convert Chef usernames to mail adresses (by simply prepending the maildomain to the username).

#### mailserver
The server Chef-Guard will use to send mail.

#### mailport
The port Chef-Guard will connect to when connecting to the mailserver.

#### mailrecipient
A mail(list) address used to send all [configuration changes]({{ site.url }}/projects/chef-guard/introduction/monitoring_auditing.html#mailing-changes) to.

#### validatechanges
A configuration option to enable or disable the [validation of changes]({{ site.url }}/projects/chef-guard/workflows/configuration_changes.html).

#### commitchanges
A configuration option to enable or disable [commiting changes]({{ site.url }}/projects/chef-guard/introduction/monitoring_auditing.html#committing-changes) to Github.

#### mailchanges
A configuration option to enable or disable [sending changes]({{ site.url }}/projects/chef-guard/introduction/monitoring_auditing.html#mailing-changes) by mail.

#### searchgithub
A configuration option to enable or disable [searching Github]({{ site.url }}/projects/chef-guard/introduction/cookbook_validation.html#comparing-the-cookbook) for source cookbooks.

#### publishcookbook
A Configuration option to enable or disable the [automatic uploading]({{ site.url }}/projects/chef-guard/introduction/cookbook_validation.html#publishing-the-cookbook) of cookbook to a private Supermarket.

#### blacklist
A comma separated list of regexes. Cookbooks matching one of the regexes will **not** be uploaded to the Supermarket, even when `publishcookbooks = true`.

#### gitorganization
The Github organization Chef-Guard uses to [commit all config]({{ site.url }}/projects/chef-guard/introduction/monitoring_auditing.html#committing-changes) info into. When using Open Source Chef a repo called `config` needs to be created in this organization. When using Enterpise Chef, you need to create a repo for every Chef Organzation you have configured to be monitored using Chef-Guard.

#### gitcookbookorgs
A comma separated list of Github organizations that Chef-Guard will use to [search for source cookbooks]({{ site.url }}/projects/chef-guard/introduction/cookbook_validation.html#comparing-the-cookbook).

#### includefcs
A (full) path to a ruby file containing custom foodcritic tests. When a file with custom checks is supplied, these will be executed in addition to the default foodcritic tests.

#### excludefcs
A comma separated list of FC's you would like to exclude (e.g. FC001, FC0031)
