---
title: "Openshift Service Based Labeling"
date: 2018-02-09T16:32:56+13:00
tags: ["Development", "OpenShift", "Blogging"]
categories: ["OpenShift"]
series: ["Beyond the OpenShift install"]
draft: true
---
# Intro
So through my experiance there are things that I've learned. While many of us focus on the technical element of the products we are using, few focus on the value these tools can bring to others. For to long religious wars have waged over the value of OverlayFS, over LVM and dare I mention it BTRFS and lost sight of the real value we can offer to the end user. 

You'll come to see through out my blog posts that while from a technical background, I tack a design lens to my work. In this post I'll focus on how we can use concepts such as OpenShift labels to develop so cool markertable services. 

# Background
We're given examples of how to use labeling for Node placement, typically this comes down to Infrastructure workloads, however this concept can be easily extended to include the things discussed in this article. 

## Dedicate Build Servers
Their are varying workloads styles and one we've found needs plenty of love and attention are builds. High impact and normally well monitored by develoeprs in our environment builds have been seen as time critical. 

### Problems this solves
* Reduces impact on other applications
* Can easily be scaled to meet large numbers of builds
* Provides single management plane for adminstration

To help manage this we've created dedicated build servers using the following setup. 

### Master Config Changes
### Dedicate Build Pools
We've used Node Labels to create a collection of resource into a build pool.


## Compute Tier (Bronze Silver Gold)
## Dedicated Licensing Pools
## Dedicate Application Pools

