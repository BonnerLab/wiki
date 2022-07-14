# Environments

This repository contains utilites and instructions for creating reproducible environments.

## Conda

### Shared `conda` installation

- If you are on the lab server (`cogsci-mb-lab3.win.ad.jhu.edu` at IP 10.160.192.70), ensure that your `conda` is initialized by running `/home/shared/miniconda3/bin/conda init` and restarting your shell
- If you are on MARCC, load the `anaconda` module using `ml anaconda`

### SSH public key authentication to the storage server

- Our shared datasets are stored in [BrainIO format](https://github.com/BonnerLab/brainio) on the lab storage server (`cogsci-ml.win.ad.jhu.edu:/export/data2/shared/brainio_bonner` at IP 10.99.95.227)
- When using the BrainScore libraries, the datasets are downloaded to your local cache from the storage server using [`rsync`](https://download.samba.org/pub/rsync/rsync.1)
- Currently, this dataset-fetching backend assumes your local machine running the code has SSH access to the storage server using public key authentication
- Generate an SSH key pair on your local machine using `ssh-keygen -t ed25519`
  - Since you want automated access to the server, don't enter a passphrase when prompted; leave it blank
  - **Security concern**: Keep your private key safe! If compromised, you should delete the corresponding public key from `cogsci-ml.win.ad.jhu.edu:~/.ssh/authorized_keys` immediately.
- Copy your public key to the storage server using `ssh-copy-id -i ~/.ssh/id_ed25519.pub <your_JHED>@10.99.95.227`
- TODO: add support for interactive usage in `NetworkStorageFetcher`

### `bonner-lab` base environment

#### Conda environment file (lab server)

```YAML
name: bonner-lab
dependencies:
  - python=3.7
  - pip
  - pip:
    - git+https://github.com/BonnerLab/bonner-lab
    - git+https://github.com/BonnerLab/brain-score
    - git+https://github.com/BonnerLab/brainio
    - git+https://github.com/BonnerLab/result-caching
    - git+https://github.com/BonnerLab/model-tools
variables:
  RESULTCACHING_HOME: "/data/shared/.cache/brainscore/result-caching"
  MT_HOME: "/data/shared/.cache/brainscore/model-tools"
  CM_HOME: "/data/shared/.cache/brainscore/candidate-models"
  BRAINIO_HOME: "/data/shared/.cache/brainscore/brainio"
  BRAINIO_LOCATION_TYPE: "network-storage"
  BRAINIO_LOCATION: "cogsci-ml.win.ad.jhu.edu:/export/data2/shared/brainio_bonner"
  BRAINIO_CATALOG_NAME: "brainio_bonner"
  BRAINIO_DOWNLOAD_CACHE: "/data/shared/datasets"
  TORCH_HOME: "/data/shared/.cache/torch"
```

This environment is a minimal base environment; please avoid installing other packages here. The following packages are installed by default:

- [`bonner-lab`](https://github.com/BonnerLab/bonner-lab)
- [`model-tools`](https://github.com/BonnerLab/model-tools)
- [`brain-score`](https://github.com/BonnerLab/brain-score)
- [`brainio`](https://github.com/BonnerLab/brainio)
- [`result-caching`](https://github.com/BonnerLab/result-caching)

Additionally, the following environment variables are set:

| Environment variable     | Lab server                                                     | MARCC                                                          |
| ------------------------ | -------------------------------------------------------------- | -------------------------------------------------------------- |
| `RESULTCACHING_HOME`     | `/data/shared/.cache/brainscore/result-caching`                | `$HOME/work/shared/.cache/brainscore/result-caching`           |
| `MT_HOME`                | `/data/shared/.cache/brainscore/model-tools`                   | `$HOME/work/shared/.cache/brainscore/model-tools`              |
| `CM_HOME`                | `/data/shared/.cache/brainscore/candidate-models`              | `$HOME/work/shared/.cache/brainscore/candidate-models`         |
| `BRAINIO_HOME`           | `/data/shared/.cache/brainscore/brainio`                       | `$HOME/work/shared/.cache/brainscore/brainio`                  |
| `BRAINIO_LOCATION_TYPE`  | `network-storage`                                              | `network-storage`                                              |
| `BRAINIO_LOCATION`       | `cogsci-ml.win.ad.jhu.edu:/export/data2/shared/brainio_bonner` | `cogsci-ml.win.ad.jhu.edu:/export/data2/shared/brainio_bonner` |
| `BRAINIO_CATALOG_NAME`   | `brainio_bonner`                                               | `brainio_bonner`                                               |
| `BRAINIO_DOWNLOAD_CACHE` | `/data/shared/datasets`                                        | `$HOME/work/shared/.cache/datasets`                            |
| `TORCH_HOME`             | `/data/shared/.cache/torch`                                    | `$HOME/work/shared/.cache/torch`                               |

### Customizing your environment

Run `conda env create -f environment.yml`, where `environment.yml` contains the packages you want installed. A template file is shown above for the `bonner-lab` environment.

Note: for some reason, 

### Tips

- Avoid installing any packages in the `base` environment or in the `bonner-lab` base environment
- `pip` install any additional packages you require, preferably using a `requirements.txt` file as `pip install -r requirements.txt`
- [Installing packages from a `git` repository](https://pip.pypa.io/en/stable/topics/vcs-support/#git)
  - Run `pip install git+https://git.example.com/<project>.git@<git ref>`
  - `git ref` can be a branch name, a commit hash, or a tag name
- Installing packages from private repositories (e.g. [`bonner-lab`](https://github.com/BonnerLab/bonner-lab))
  - Request access to the [Bonner-Lab GitHub organization](https://github.com/BonnerLab)
  - Create a [personal access token](https://github.com/settings/tokens)
  - Export the environment variable `GITHUB_TOKEN` by adding it to your shell's `.rc' file (e.g. .bashrc`) as `GITHUB_TOKEN=<token>`
  - Run `pip install "git+https://${GITHUB_TOKEN}@github.com/<repository.git>`
- Installing packages in editable mode
  - When working on a package, it is useful to have a development environment that changes with the source code in real-time
  - Run `pip install -e "git+https://<project_url.git>#egg=<project_name>" --src <source_dir>`
  - `<source_dir>` is the location of the source code you are working on
- Faster installation of `conda` packages
  - [`mamba`](https://github.com/mamba-org/mamba) is a drop-in replacement for the `conda` package manager that has much faster dependency resolution
  - `mamba` has been installed in both `base` `conda` environments on the lab server (`conda install mamba -n base -c conda-forge`)
  - To use it, run `mamba init` and replace `conda` with `mamba` in (pretty much) all commands
  - Prefer [`pip`](https://pip.pypa.io/en/stable/) over [`conda`](https://docs.conda.io/projects/conda/en/latest/index.html) though the `conda` documentation [recommends the converse](https://www.anaconda.com/blog/understanding-conda-and-pip)
`