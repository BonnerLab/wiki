# Lab workstations

We currently have two active workstations:

- `lab-1` (IP address `10.160.191.207`, hostname `cogsci-mb-gpu1.acasmart.jh.edu`) -- a powerful machine with a 2 TB SSD + 4 TB SSD, 128 GB RAM, and an RTX 4090 Ti (24 GB VRAM)
- `lab-old` (IP address `10.160.192.70`, hostname `cogsci-mb-lab3`) -- a weaker computer with a 1 TB SSD + 5 TB HDD, 128 GB RAM, and an RTX 2080 (8 GB VRAM)

This document describes how to set up and configure an account on either machine. You are strongly recommended to use the new machine `lab-1`.

## Connect to the workstation

1. SSH into the computer with `ssh <jhed>@<IP address>`. In the case of `lab-1`, the `<jhed>` should be replaced with just the JHED (e.g. `rgautha1`). In the case of `lab-old`, replace it with the entire email address (e.g. `rgautha1@jh.edu`). You will be prompted to enter a password, which should be your JHU SSO password (i.e. the password for your Hopkins email account).

## Set default shared directories

2. Create the file `~/.config/pip/pip.conf` and add the following content to the file:

```
[global]
cache-dir=/data/shared/cache/pip
```

3. Add the following line to `~/.bashrc`:

`export TORCH_HOME=/data/shared/cache/torch`

## Set up Conda

4. Activate the Conda package manager by running `source /data/shared/miniconda3/bin/activate` followed by `conda init`. Restart your shell.
5. Add the following lines to `~/.config/conda/condarc`:

```
pkgs_dirs:
    - /data/shared/miniconda3/pkgs
envs_dirs:
    - /data/shared/miniconda3/envs
```
## Your directories

- You have a home directory at `/home/<user>` on the 2 TB SSD.
- The 4 TB SSD is mounted at `/data`.
- You have access to the directories under `/data/shared`.
- You may want to create directories under `/data` for your personal use (e.g. `/data/rgautha1`). Keep in mind that everyone can read/write files in `/data` by default, including any directories you create there. If this is a problem, let me know and I can try to adjust the permissions (though everything is working now and I'd rather not break anything!)
- Please don't pollute the `/data` and `/data/shared` namespaces (i.e. don't create random files/directories in there without confirming with me).

## sudo access

I have `sudo` access on these workstations, so if you need anything administered (e.g. software install, fixing permissions issues), you might want to let me know. Otherwise you can contact KSAS IT/Brance as usual.
