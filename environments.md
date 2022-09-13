# Environments

This repository contains utilites and instructions for creating reproducible environments.

## Conda

- If you are on the lab server (`cogsci-mb-lab3.win.ad.jhu.edu` at IP 10.160.192.70), ensure that your `conda` is initialized by running `/home/shared/miniconda3/bin/conda init` and restarting your shell
- If you are on MARCC, load the `anaconda` module using `ml anaconda`

Here's a sample `environment.yml` file that can be used to create environments using `conda env create -f environment.yml`:

```YAML
name: <your-environment-name>
dependencies:
  - python=3.9  # change your Python version here
  - pip
  - pip:  # preferably, use `pip` to install packages
    - numpy
variables:  # please use these environment variables so we can have a shared torch and dataset cache
  TORCH_HOME: "/data/shared/torch"
  BONNER_BRAINIO_CACHE: "/data/shared/brainio"
  BONNER_DATASETS_CACHE: "/data/shared/datasets"
```

### Tips

- Avoid installing any packages in the `base` environment
- Use an `environment.yml` file to create reproducible environments using `conda env create -f environment.yml`
- `pip` install any additional packages you require, preferably using a `requirements.txt` file and `pip install -r requirements.txt`
- [Installing packages from a `git` repository](https://pip.pypa.io/en/stable/topics/vcs-support/#git)
  - Run `pip install git+https://git.example.com/<project>.git@<git ref>`
  - `git ref` can be a branch name, a commit hash, or a tag name
- Installing packages in editable mode
  - When working on a package, it is useful to have a development environment that changes with the source code in real-time
  - Run `pip install -e "git+https://<project_url.git>#egg=<project_name>" --src <source_dir>`
  - `<source_dir>` is the location of the source code you are working on
- Faster installation of `conda` packages
  - [`mamba`](https://github.com/mamba-org/mamba) is a drop-in replacement for the `conda` package manager that has much faster dependency resolution
  - `mamba` has been installed in both `base` `conda` environments on the lab server (`conda install mamba -n base -c conda-forge`)
  - To use it, run `mamba init` and replace `conda` with `mamba` in (pretty much) all commands
  - Prefer [`pip`](https://pip.pypa.io/en/stable/) over [`conda`](https://docs.conda.io/projects/conda/en/latest/index.html) though the `conda` documentation [recommends the converse](https://www.anaconda.com/blog/understanding-conda-and-pip)

## Access to the Bonner Lab repositories

- Request access to the [Bonner-Lab GitHub organization](https://github.com/BonnerLab) using the web interface and ask someone in the lab to approve the request
- Create a [personal access token](https://github.com/settings/tokens)
- In `~/.config/git/credentials`, add the line `https://<github-username>:<personal-access-token>@github.com`
- TODO In `~/.config/git/config`, add the lines

## SSH public key authentication to the storage server

- Our shared datasets are stored in [BrainIO format](https://github.com/BonnerLab/brainio) on the lab storage server (`cogsci-ml.win.ad.jhu.edu:/export/data2/shared/brainio/bonner-datasets` at IP 10.99.95.227)
- When using the BrainScore libraries, the datasets are downloaded to your local cache from the storage server using [`rsync`](https://download.samba.org/pub/rsync/rsync.1)
- This dataset-fetching backend assumes your local machine running the code has SSH access to the storage server using public key authentication
- Generate an SSH key pair on your local machine using `ssh-keygen -t ed25519`
  - Since you want automated access to the server, don't enter a passphrase when prompted; leave it blank
  - **Security concern**: Keep your private key safe! If compromised, you should delete the corresponding public key from `cogsci-ml.win.ad.jhu.edu:~/.ssh/authorized_keys` immediately.
- Copy your public key to the storage server using `ssh-copy-id -i ~/.ssh/id_ed25519.pub <your_JHED>@10.99.95.227`
