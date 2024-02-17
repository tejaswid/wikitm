---
layout: page
title: "Building A Single Node GPU Cluster using SLURM"
categories: [SLURM]
---

### Contents
1. [Installing Munge](#munge)
2. 


### Tested on
These instructions were tested on **Ubuntu 18.04**.

### Installing Munge
Open a terminal and install munge using apt-get.
```bash
sudo apt-get install munge libmunge-dev
```

Check that the istalling is correct by checking that the following folders have the corresponsing permissions using the ```ls -ld``` command.

Folder | permissions
----|----
ls -ld /etc/munge/		|	0700
ls -ld /var/lib/munge/	|	0711
ls -ld /var/log/munge/	|	0700
ls -ld /var/run/munge/	|	0755

