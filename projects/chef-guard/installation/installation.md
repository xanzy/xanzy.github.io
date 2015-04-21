---
layout: project
project: chef-guard
project_name: Chef-Guard
---

### Chef Server Components
Before explaining the steps needed to install and configure Chef-Guard, let's have a quick look at how the different Chef components work together:

![Server Components without Chef-Guard]({{ site.url }}/images/server_components_without_cg.png)

As you can clearly see on this picture all Chef components are build around the Chef Server which functions as a central hub. More information and details about all Chef components shown in this picture can be found [here](http://docs.opscode.com/server_components.html){:target="_blank"}.

Now in order for Chef-Guard to be able to monitor and validate **all** calls to your Chef server, it needs to take over the initial place of the Chef Server and become the central hub itself. So the Chef Server will be moved right behind Chef-Guard and will receive all calls leaving Chef-Guard. From there the original flow will be followed, so the updated picture then looks like this:

![Server Components with Chef-Guard]({{ site.url }}/images/server_components_with_cg.png)

### Installation using a cookbook
Of course installing with a cookbook has a lot of advantages, so using the cookbook is absolutely the preferred way of installing and managing your Chef-Guard installation. If you choose this route, you just need to [get the cookbook](https://supermarket.getchef.com/cookbooks/chef-guard){:target="_blank"}, configure the attributes according to your needs and place the cookbook on the runlist of your Chef Server(s). See the README of the cookbook and the different [Configuration]({{ site.url }}/projects/chef-guard/configuration/default_section.html) sections for details about the conguration options, which map one-to-one to the cookbook attributes.

### Manual installation
If you really cannot use the cookbook, then you can install Chef-Guard by following just a few easy steps:

1.  Create (and cd into) a Chef-Guard directory
2.  Download the latest release from Github: [https://github.com/xanzy/chef-guard/releases](https://github.com/xanzy/chef-guard/releases){:target="_blank"}
3.  Extract the tar file
4.  Copy chef-guard.conf.example to chef-guard.conf
5.  Customize the config so it fits your environment
6.  Copy chef-guard.conf.upstart to /etc/init/chef-guard.conf
7.  If needed adjust the paths in the upstart script
8.  Be sure to create the needed .pem files!

~~~ bash
Example of the needed commands:

mkdir -p /opt/chef-guard
cd /opt/chef-guard
wget https://github.com/xanzy/chef-guard/releases/download/v0.2.2/chef-guard-v0.2.2-linux-x64.tar.gz
tar zxof chef-guard-v0.2.2-linux-x64.tar.gz
cp chef-guard.conf.example chef-guard.conf
vi chef-guard.conf
cp chef-guard.conf.upstart /etc/init/chef-guard.conf
vi /etc/init/chef-guard.conf
vi chef.pem
vi supermarket.pem
~~~

### Enable the Chef-Guard component within the Chef Server config
**This step is needed in all cases. So regardless of your installation method, please follow this step!**

The last action you need to take before you can actually start using Chef-Guard, is to reconfigure the Chef components so all calls will be redirected to Chef-Guard instead of to Chef Server. This could maybe sound a little daunting, but it's actually pretty easy.

#### Enterprise Chef
There is a file called `/etc/opscode/private-chef.rb` for Enterprise Chef which can contain all kinds of configuration tweaks needed by your Chef Servers. Open the file and add the following configuration to the file:

~~~ ini
lb['upstream'] = {
 "opscode-erchef"=>["127.0.0.2"],
 "opscode-account"=>["127.0.0.1"],
 "opscode-webui"=>["127.0.0.1"],
 "oc_bifrost"=>["127.0.0.1"],
 "opscode-solr"=>["127.0.0.1"],
 "bookshelf"=>["127.0.0.1"]
}
~~~

The IP addresses used here may be different from your actual setup. Make sure you fill in the correct addresses and set the 'opscode-erchef' address to the IP address you configured Chef-Guard to listen on.

#### Open Source Chef
Open Source Chef has a similar file called `/etc/chef-server/chef-server.rb` to make customizations to your installation, but when testing this on the latest Open Source Chef, it did not work as expected. We are currently looking into that, but for the mean time you should edit line 193 the file `/opt/chef-server/embedded/cookbooks/chef-server/attributes/default.rb` to look like this:

~~~ ini
193: default['chef_server']['lb']['upstream']['erchef'] = [ "127.0.0.2" ]
~~~

#### Private Chef 12
In case you're using private installation of Chef 12 you need to edit `/opt/opscode/embedded/cookbooks/private-chef/attributes/default.rb`:

~~~ ini
266: default['private_chef']['lb']['upstream']['opscode-erchef'] = [ "127.0.0.2" ]
~~~

After your done editing the configuration, just run `chef-server-ctl reconfigure` when using Open Source Chef, or `private-chef-ctl reconfigure` when using Enterprise Chef. That's all... Your done! All calls to your Chef Server are now first routed to Chef-Guard instead.

_NOTE:  When using Enterpise Chef version < 11.2.2, you need to manually fix a bug in the private-chef cookbook (the internal cookbook used by Chef to manage it's own setup) before running the reconfigure command. Please change line 323 of `/opt/opscode/embedded/cookbooks/private-chef/libraries/private_chef.rb` from `PrivateChef["lb"]["upstream"] = Mash.new` to `PrivateChef["lb"]["upstream"] ||= Mash.new`. After that you should be able to succesfully execute the reconfigure command._

### Starting Chef-Guard
If all steps are followed correctly, starting Chef-Guard is as easy as typing `sudo start chef-guard` :)
