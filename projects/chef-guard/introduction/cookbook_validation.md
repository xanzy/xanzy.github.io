---
layout: project
project: chef-guard
---

### Some background info...
Before explaining al the configurable options of this rather big feature in more detail, let's first spend a few minutes on the thoughts and reasoning behind the feature. This will set the stage and will probably answer most questions about the choices we made when designing it.

One of the biggest risks we have seen when running Chef in critical production environments, is that there is no way to tell what cookbook code is actually being uploaded to your Chef Server. Yes every cookbook has a version, but unfortunately there is no way to tell if the person who is uploading the cookbook didn't make any changes that are not yet committed back to the orginal source cookbook repo (i.e. that have been reviewed and made auditable) prior to uploading the altered cookbook.

And of course I know and understand that we are all perfect programmers who write lot's of beautiful tests and run at least Test Kitchen, Foodcritic and Rubocop before even thinking about uploading a cookbook to our sacred Chef Server... Uuh... **NOT!!** I'm just kiding here of course! Of course there are cookbook developers out there that do exactly this, but there are probably even more DevOps/Ops engineers (with less programming experience) who don't follow this process as closely as one would like.

I can probably write a book about all the different problems and frustrations you will run into as soon as someone in your organisation starts doing cookbook uploads with untested or uncommitted code, but I guess that most of you have experienced one or more of these examples yourselves already...

So this feature is all about making sure that whatever is uploaded into your Chef Server is something that is shared, committed and tested so that it's auditable and everybody in your organization knows where to find the source of the cookbooks used in your Chef environment. Make sure you also have a look at the [Cookbook Upload Workflow]({{ site.url }}/projects/chef-guard/workflows/cookbook_upload.html) that gives a graphical overview of the complete workflow containing this feature, including the impact of the configurable options.

### Cookbook Validation
The cookbook validation feature is responsable for actually validating that the cookbook is tested and that the cookbook code is either committed into a known cookbook repo or is an existing artifact, before passing the cookbook on to your Chef Server/Organization. Some of the steps described below are configurable options that can impact the way the validation feature works. For more details about these options check the [Cookbook Upload Workflow]({{ site.url }}/projects/chef-guard/workflows/cookbook_upload.html) or the Chef-Guard [configuration]({{ site.url }}/projects/chef-guard/configuration/github_section.html) section.

#### Comparing the cookbook
In order to validate that the cookbook code is committed or is an existing artifact, the uploaded cookbook is compared against either one. For this Chef-Guard needs to find a matching cookbook/version to do the comparison against. This is done by first contacting a [Berkshelf API](https://github.com/berkshelf/berkshelf-api){:target="_blank"} server to check if the cookbook/version combination being uploaded already exists. If the Berkshelf API server returns a match, Chef-Guard uses the matching artifact for the comparison.

When the Berkshelf API server doesn't return a match, Chef-Guard will search for the cookbook repo in all configured Github organizations. When a matching repo is found, Chef-Guard will also try to find a matching tag. If it finds one, the tagget version will be used for the comparison. If it doesn't find one the cookbook will be compared against the master branch of the matching repo.

If no matching artifact if found using the Berkshelf API server and no matching repo is found in any of the configured Github organizations, Chef-Guard will return with a `412 Precondition Failed` error and some additional details about the error. In that case the cookbook will **not** be uploaded!

Assuming that the source cookbook can be found, Chef-Guard will then compare the source cookbook against the cookbook being uploaded. If there if a diff between the cookbooks Chef-Guard will again return a `412 Precondition Failed` and will **not** upload the cookbook. Only when the cookbooks are completely in sync, Chef-Guard will proceed with the validation.

When the comparison succeeds and the matching cookbook was found in an untagged Github repo, Chef-Guard will now tag this repo with the used version. This will make sure that the cookbook/version being uploaded into your Chef Server/Organization can always be retrieved/found in your Github repo.

#### Publishing the cookbook
If you don't have a local [Chef Supermarket](https://github.com/opscode/supermarket){:target="_blank"} installation, this is currently probably the best you can do in in order to manage the version of your private cookbooks. But if you have gone the extra mile and also have a local Supermarket installation, your are really in for a treat!

In that case Chef-Guard can be configured to upload the cookbook directly to your local Supermarket. By doing so you now have a clean artifact of the cookbook that is being frozen for future use. If you also have a local Berkshelf API server this cookbook will now also be indexed so next time Chef-Guards needs to search for the cookbook/version combination it can find it much faster and much more reliable.

_NOTE: Of course another big advantage of having your cookbook directly uploaded to the local Supermarket, is that all your colleagues who are using the local Supermarket to search for existing private cookbooks, can now instantly find the new cookbook!_

#### Testing the cookbook
The only sensible way for Chef-Guard to make sure that a cookbook is tested, is by running the tests by itself. So that is exactly what Chef-Guard is doing when a cookbook that is uploaded is being frozen. Currently you can [configure]({{ site.url }}/projects/chef-guard/configuration/tests_section.html) Chef-Guard to execute the following tests:

- Foodcritic
- Rubocop

If one of the checks fails Chef-Guard will return a `412 Precondition Failed`. The additional error details will describe the failed test, giving direct feedback to the colleague uploading the cookbook. This will not only help to improve your organizations cookbooks, but it will also help your colleagues to become better cookbook developers at the same time.

Whether or not the cookbook will be uploaded after a failed test depends on how Chef-Guard is configured and which flags are passed during the upload. As described in the [Overview]({{ side.url }}/projects/chef-guard/introduction/overview.html) section the knife option `--force` is [changed]({{ side.url }}/projects/chef-guard/introduction/overview.html#changed-behavior-for-knife----force) and is now used in conjunction with these tests. When the [effective mode]({{ side.url }}/projects/chef-guard/configuration/default_section.html) for your Chef Server/Organization is set to 'permissive' **and** the `--force` option is passed, Chef-Guard will upload the cookbook despite of the failed tests. If however the effective mode is set to 'enforced', the cookbook will **not** be uploaded when a test has failed!
