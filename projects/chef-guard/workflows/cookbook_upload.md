---
layout: project
project: chef-guard
project_name: Chef-Guard
---

### Cookbook Upload Workflow
The introduction section about [Cookbook Validation]({{ side.url }}/projects/chef-guard/introduction/cookbook_validation.html) already describes part of the workflow that takes place when you upload a cookbook with Chef-Guard enabled. In an effort to put things in perspective and to better explain the impact of the configuration choices on the workflow, we tried to visualize all the steps in a single flowchart. Hopefully this will make understanding and troubleshooting the workflow a lot easier.

_NOTE: The blue boxes all contain configurable features!_

![Cookbook Upload Flowchart]({{ side.url }}/images/cookbook_upload_flowchart.png)

### Cookbook Delete Workflow
Because deleting a cookbook follows a completely different and much smaller workflow, it makes sense to have an separate flowchart describing the 'Cookbook Delete' workflow.

_NOTE: The blue boxes all contain configurable features!_

![Cookbook Delete Flowchart]({{ side.url }}/images/cookbook_delete_flowchart.png)
