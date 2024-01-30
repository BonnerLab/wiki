# Lab workstations

We currently have two active workstations:

- `lab-1` (IP address `10.160.191.207`, hostname `cogsci-mb-gpu1.acasmart.jh.edu`) -- a powerful machine with a 2 TB SSD, 128 GB RAM, and an RTX 4090 Ti (24 GB VRAM)
- `lab-old` (IP address `10.160.192.70`, hostname `cogsci-mb-lab3`) -- a weaker computer with a 1 TB SSD, a 5 TB HDD, 128 GB RAM, and an RTX 2080 (8 GB VRAM)

This document describes how to set up and configure an account on either machine. You are strongly recommended to use the new machine `lab-1`.

1. SSH into the computer with `ssh <jhed>@<IP address>`. In the case of `lab-1`, the `<jhed>` should be replaced with just the JHED (e.g. `rgautha1`). In the case of `lab-old`, replace it with the entire email address (e.g. `rgautha1@jh.edu`). You will be prompted to enter a password, which should be your JHU SSO password (i.e. the password for your Hopkins email account).
2. Create the file `~/.config/pip/pip.conf` and add the following content to the file:

```
[global]
cache-dir=/data/shared/cache/pip
```

3. Add the following line to `~/.bashrc`:

`export TORCH_HOME=/data/shared/cache/torch`

4. Activate the Conda package manager by running `source /data/shared/miniconda3/bin/activate` followed by `conda init`. Restart your shell.
5. Add the following lines to `~/config/conda/.condarc`:

```
pkgs_dirs:
    - /data/shared/miniconda3/pkgs
envs_dirs:
    - /data/shared/miniconda3/envs
```
