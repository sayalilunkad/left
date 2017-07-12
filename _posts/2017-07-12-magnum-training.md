---
layout: post
title: Magnum Training
comments: true
---

It has been a while since my last post here and thought it is time I write new one!
It has almost been two years since I started working with SUSE (yayy:)) and it has been great so
far.
I have been working on various different things since I started at SUSE which include integrating
new features into our SUSE OpenStack Cloud product, going through product releases(we recently
released SOC7), fixing bugs in the product and of course OpenStack upstream. Previously I worked on
parts of horizon, neutron and barbican and currently my focus is on magnum. This post is mainly
about a training I conducted along with one of my colleague, [Johannes Grassler](https://github.com/jgrassler/)
on Magnum.

Magnum Training
--------

This was an intensive, day long magnum training in which we covered what magnum is about, detailed
architecture, and everything that happens in the background when you create a magnum cluster. Then
we had a very interactive debugging session where we provided some scripts which we used to sabotage
the magnum cluster and then debug the several failures. This method seemed to work quite well with
the participants as they could see the failures and fix them simultaneously. It was a bit time
consuming because magnum in general takes times to create a cluster especially if you have limited
resources but we provided transcripts for everything the participants could go back at any point and
continue with the debugging.

You can download the material used for the training [here](https://w3.nue.suse.com/~jgrassler/SOC7-Training-Magnum.tar.bz2).
The training is split into two parts the intro and handson. The intro explains magnum in detail along
with the architecture and the handson is more like the troubleshooting guide. The tarball contains all
the slides, scripts used for creating and fixing the sabotage and various other small tasks and also
the transcripts.

PS: Some parts of this training are specific to SUSE OpenStack Cloud but most of it should
be usable on any OpenStack Cloud or at least easily modifiable.

Go ahead and download the material and let me know if you found it helpful :). Most of the
feedback that we got from the participants at the training was very positive so I hope it helps
others too!
