# MARCC

When you [request an account](https://www.marcc.jhu.edu/request-access/request-an-account/) on MARCC, please note that Mick's JHED is mbonner5

Follow [this link](https://www.evernote.com/shard/s573/sh/117a7ca1-4af1-e68a-026f-b6512cafee55/7949a4dcde4d0a1318b914460c8e575a) to access our guide to getting started with MARCC

## Sample SLURM template

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
conda activate foo
python bar.py
```

Please contribute if you have additional information!
