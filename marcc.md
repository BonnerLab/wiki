# MARCC/Rockfish Cluster

MARCC/Rockfish is the computing cluster used by Hopkins, with access to with powerful CPUs/GPUs and large-memory nodes. Since the lab workstation is currently heavily in-demand, we should aim to gradually shift most of our compute-heavy workload to the cluster and use the workstation for quick analyses.

This document is intended to be a guide to using MARCC/Rockfish, though you should be sure to consult the official documentation for the latest information:

- [User guide](https://www.arch.jhu.edu/access/user-guide/)
- [readthedocs](https://marcc.readthedocs.io/index.html)
- [MARCC website](https://www.marcc.jhu.edu/)

## Guide

### Requesting an account

- [Request a user account](https://coldfront.rockfish.jhu.edu/user/register/) on the [ColdFront Portal](https://coldfront.rockfish.jhu.edu/) using your JHED (e.g. `mbonner5`)
- Once your account is created, inform Mick to add you to his project/allocation through the [ColdFront Portal](https://coldfront.rockfish.jhu.edu/)

### Structure of MARCC

MARCC has three types of [nodes](https://marcc.readthedocs.io/HPC_Terminology.html):

- login nodes (`login**.rockfish.jhu.edu`)
- compute nodes (`c***.rockfish.jhu.edu`)
- data transfer nodes (`rfdtn*.rockfish.jhu.edu`)

The login nodes are meant for logging in and doing simple, non-compute-heavy tasks. Here's a [list of things you're allowed to do on the login nodes](https://www.arch.jhu.edu/access/user-accounts/rockfish-citizen/). Be warned that any excessive use of these nodes will likely lead to your account being suspended since it impacts others' use of the cluster.

The compute nodes are the powerful computers with many CPU cores, GPUs, and high RAM. These are typically of three types: normal, GPU-equipped, and large-memory.

The data transfer nodes are used for transferring data to/from MARCC to networked machines. The lab workstation and storage server can both connect to MARCC.

### [Logging in](https://marcc.readthedocs.io/Connecting_to_Rockfish.html)

If you run `ssh <username>@login.rockfish.jhu.edu`, you will get access to a login node. Your username is probably your JHED. You can use the `ssh -X` option to enable X11 forwarding (i.e. a GUI).

### [Storage allocations](https://marcc.readthedocs.io/Allocation.html)

In your home directory, you will find symlinks to two other directories, `scratch4-mbonner5` (actual path: `/scratch4/mbonner5`) and `data-mbonner5` (actual path: `/data/mbonner5`).

- Your `home` directory (50 GB, NVMe SSD, weekly offsite backup), should be used for commonly used applications.
- The `scratch` directory (10 TB, shared, no backup) should be used for IO during your compute-heavy workloads.
- The `data` directory (10 TB, shared, no backup) should be used for long-term storage of large files.

Be sure to back up any important files to the Bonner Lab storage server!

### Data transfer

- Perform any large data transfer only using the data transfer node(s)
  - If your files are relatively small (a few GB), use scp/rsync
  - If they're large, use Globus (instructions to follow)
- If you have a few small files, you can scp/rsync via the login node

### Requesting an interactive shell

`interact -h` provides instructions on starting an interactive shell.

## Scheduling jobs via SLURM

TODO

### Sample SLURM template

TODO

```bash
#!/bin/bash
#SBATCH --job-name=foo
#SBATCH --partition=gpup100
#SBATCH --time=12:0:0
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=24
#SBATCH --gres=gpu:2
#SBATCH --mail-type=end
#SBATCH --mail-user=foo@jhu.edu
#SBATCH --workdir=/home-net/home-4/foo@jhu.edu/scratch/

ml anaconda  # loads the anaconda module
conda activate <path_to_env>
python bar.py
```

## Archive

- Follow [this link](https://www.evernote.com/shard/s573/sh/117a7ca1-4af1-e68a-026f-b6512cafee55/7949a4dcde4d0a1318b914460c8e575a) to access our guide to getting started with MARCC
