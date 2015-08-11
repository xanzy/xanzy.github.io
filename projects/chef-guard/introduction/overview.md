---
layout: project
project: chef-guard
project_name: Chef-Guard
redirect_from:
  - /projects/chef-guard/
  - /projects/chef-guard/introduction/
---

### What is Chef-Guard?
Chef-Guard is a feature rich [Chef](http://www.getchef.com){:target="_blank"} add-on that protects your Chef server from untested and uncommitted (i.e. potentially dangerous) cookbooks by running several validations and checks during the cookbook upload process. In addition Chef-Guard will also monitor, audit, save and email (including a diff with the actual change) all configuration changes and is even capable of validating certain changes before passing them through to Chef.

So installing Chef-Guard, which is **completely open source**, onto your Chef server(s) will give you a highly configurable component that enables you to configure and enforce a common [workflow]({{ site.url }}/projects/chef-guard/workflows) for all your colleagues working with Chef.

### How does Chef-Guard work?
You can think of Chef-Guard as an extremely smart reverse proxy server written in [Go](https://golang.org/){:target="_blank"} and located/installed right in between Nginx and the Chef Server (see the [Installation]({{ site.url }}/projects/chef-guard/installation/installation.html) section for more details). This means that Chef-Guard runs completely server-side and does not require **any** client-side changes! This gives you the freedom to use whatever tools you like (e.g. knife, berks, the webui) to work with your Chef server and Chef-Guard will make sure all these tools follow the same workflow.

### Changed behavior for `knife ... --force`
Chef-Guard doesn't change the behavior of your Chef Server, it only adds additional functionality and workflows. Except for one case, being the knife option `--force`. As the reasoning is that a frozen cookbook should remain frozen at all times (why else would you freeze it in the first place?!), with Chef-Guard enabled it is no longer possible to use the `--force` option to overwrite a frozen cookbook. But this doesn't mean the `--force` option has no function anymore! We changed the behavior of this particular knife option from _'a way to overwrite a frozen cookbook'_, to _'a way to [bypass failed tests]({{ side.url }}/projects/chef-guard/introduction/cookbook_validation.html#testing-the-cookbook)'_

### Does Chef-Guard work with all Chef editions and versions?
Yes, Chef-Guard works with Chef 11 (both Open Source Chef and Enterprise Chef) and Chef 12. Since Chef-Guard is build as a completely stateless, server-side component, you can even use Chef-Guard when you have a HA Chef setup.

For the more adventurous Chef users among us, we also added support for [Goiardi](http://goiardi.gl/){:target="_blank"} (an implementation of the Chef server written in Go) as of version 0.6.0!

### Contact
For any comments, questions, sugestions, discussions, improvements or anything else you want to say or share related to Chef-Guard, please find us on IRC: #chef-guard of file an [issue](https://github.com/xanzy/chef-guard/issues){:target="_blank"} on Github.
