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
  - If they're large, use Globus:
    - [Globus Connect Personal](https://www.globus.org/globus-connect-personal) is available on the lab workstation at `/home/shared/globus/globusconnectpersonal-3.2.0/`
    - Set up your Globus Connect Personal account on the workstation by running `/home/shared/globus/globusconnectpersonal-3.2.0/globusconnectpersonal -setup`
      - Open the generated link in a browser and follow the instructions. Recommendation: use the label `bonner-lab-workstation` for the workstation when prompted.
      - Enter the generated authentication code at the terminal when prompted. You will need to re-enter the label you used in the previous step.
      - Append any directories you want to allow MARCC to read/write to to the file `~/.globusonline/lta/config-paths` in the format `<path>,0,1`. The `0` is the share bit (no sharing allowed). The `1` is the read-write bit that allows MARCC to read/write to the directory.
    - Run a Globus Connect Personal instance on the workstation by running `/home/shared/globus/globusconnectpersonal-3.2.0/globusconnectpersonal -start&`
    - Log in to the [Globus web app](https://app.globus.org/) using your JHU credentials
    - On the Globus File Manager, you can access MARCC's Data Transfer Node(s) by searching for entering the Collection name `Rockfish.dtn01.user.data`. You can find the workstation at the label you entered during setup (`bonner-lab-workstation`).
    - You can now schedule transfers and other filesystem operations.
    - WARNING: If you transfer large files, the workstation IO will become very slow for everyone using the system.
    - WARNING: Only one instance of Globus Connect Personal can run on the workstation at a time. If anyone else is running a job, you might need to ask them to stop. Not sure about this. If anyone figures it out, please update this page!
- If you have a few small files, you can scp/rsync via the login node

### Requesting an interactive shell

`interact -h` provides instructions on starting an interactive shell.

## Scheduling jobs via SLURM

TODO

### Sample SLURM template

```bash
#!/bin/bash -l
#SBATCH --job-name=foo
#SBATCH --time=1:0:0
#SBATCH --partition=a100
#SBATCH --nodes=1
#SBATCH --cpus-per-task=24
#SBATCH --mem-per-cpu=4G
#SBATCH --gres=gpu:1
#SBATCH --account=mbonner5_gpu

ml anaconda  # loads the anaconda module
conda activate <path_to_env>
python bar.py
```

## Archive

- Follow [this link](https://www.evernote.com/shard/s573/sh/117a7ca1-4af1-e68a-026f-b6512cafee55/7949a4dcde4d0a1318b914460c8e575a) to access our guide to getting started with MARCC
