---
layout: page
title: Configuring Your Paths
permalink: /activities/pathconfiguration/
---

The following lines will need to be added to your `.bash_profile` to have access to all of the necessary software for the workshop:

```sh
export JAVA_HOME=/mnt/shared/scratch/gtiley/apps/libraries/jdk-21.0.1
export PATH=/mnt/shared/training/conda/bin:/mnt/shared/scratch/gtiley/apps/network-workshop-apps/bin:/mnt/shared/scratch/gtiley/apps/network-workshop-apps/scripts:$JAVA_HOME/bin:$PATH
export LD_LIBRARY_PATH=/mnt/shared/scratch/gtiley/apps/network-workshop-apps/lib:$LD_LIBRARY_PATH
```

After you log off of gruffalo and log back in, you should be ready. Test a few programs to check that everything went well with

```
bpp -h
mdl -h
julia --version
```

Remember when running something on the crop diversity cluster to grab an interactive node. 
```bash
srun --cpus-per-task=4 --mem-per-cpu=2G --partition=debug --pty bash
```