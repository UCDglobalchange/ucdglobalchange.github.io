# Submitting Jobs on FARM

FARM uses the **Slurm** scheduler. You describe the resources you need (CPUs, memory, time) and
Slurm runs your job on a compute node when they become available.

## Choosing a partition

Partitions are queues with different priorities. In our lab:

| Partition | When to use it | Preemptible? |
|-----------|----------------|--------------|
| **`low`** | **Default for most jobs.** Analysis, short runs, large arrays of short tasks, anything you can restart. | Yes |
| **`high`** | Jobs that need **high priority** and must not be interrupted: long model runs, MPI jobs, deadline-critical work. Uses the lab's purchased CPUs. | No |
| **`bml`** | Jobs that need a **very large amount of memory** (analysis of very large datasets). | Yes |

**Preemptible** means a higher-priority job can stop yours at any time, and Slurm requeues it to
restart **from the beginning**. On `low` and `bml`, keep jobs short or save progress (checkpoints)
so a restart costs little.

```{note}
The lab no longer has access to the `bmh` partition, so use `bml` for big-memory jobs. Older
scripts that use `high2`, `med2`, or `bmh` need updating.
```

Always include the lab's Slurm account, `--account=moniergrp`. Check exactly what you have access to with:

```bash
sacctmgr -s list user $USER format=account%20,partition%20
```

## A basic batch job

Save this as `myjob.sh` (a template is also in the
[lab FARM repo](https://github.com/UCDglobalchange/FARM)):

```bash
#!/bin/bash -l
#SBATCH --job-name=myjob
#SBATCH --account=moniergrp          # the lab's Slurm account
#SBATCH --partition=low              # default; high = high priority; bml = very large memory
#SBATCH --time=0-04:00:00            # D-HH:MM:SS wall-clock limit (always set this)
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4            # threads for one program
#SBATCH --mem=16G                    # always give a unit; a bare number means MB
#SBATCH --output=logs/myjob-%j.out   # %j = job ID, keeps log files unique
#SBATCH --error=logs/myjob-%j.err
#SBATCH --mail-type=END,FAIL         # optional: email when the job ends or fails
#SBATCH --mail-user=<you>@ucdavis.edu

module load conda
conda activate /quobyte/emoniergrp/<you>/envs/<your-env>

cd "$SLURM_SUBMIT_DIR"
python analysis.py --threads "$SLURM_CPUS_PER_TASK"
```

Submit and monitor:

```bash
mkdir -p logs
sbatch myjob.sh                       # submit
squeue -u $USER                       # your queued/running jobs
scancel <jobid>                       # cancel a job
sacct -j <jobid> --format=JobID,State,Elapsed,MaxRSS,ReqMem   # after it finishes
```

## Key ideas

- **Always set `--account=moniergrp`, `--partition`, `--time`, and memory.** No partition has a default time.
- **Threads vs. tasks**: one multithreaded program uses `--ntasks=1 --cpus-per-task=N`; MPI
  programs (like most climate models) use `--ntasks=N` and are launched with `srun`.
- **Memory is enforced.** If your job uses more than requested it is killed with `OUT_OF_MEMORY`.
  Check `MaxRSS` in `sacct` after a test run and adjust.
- **Pass the thread count** to your program with `$SLURM_CPUS_PER_TASK` instead of hard-coding it.
- Don't use `--exclusive` unless the PI says so; it can block lab mates.

## Interactive sessions

For debugging or exploration, get a shell on a compute node:

```bash
srun --account=moniergrp --partition=high --time=2:00:00 --ntasks=1 --cpus-per-task=4 --mem=8G --pty /bin/bash -l
```

To look inside a job that is already running:

```bash
srun --jobid=<jobid> --overlap --pty /bin/bash -l
```

You cannot `ssh` directly into compute nodes. Run long interactive sessions inside `tmux` so they
survive a dropped connection.

## Many similar jobs: job arrays

```bash
#SBATCH --account=moniergrp
#SBATCH --partition=low                  # short array tasks suit the low partition
#SBATCH --array=1-100%20                 # 100 tasks, at most 20 at a time
#SBATCH --output=logs/run-%A_%a.out      # %A = array ID, %a = task index
python run_member.py --member "$SLURM_ARRAY_TASK_ID"
```

Each array task must write its own output file: on `/quobyte`, two tasks writing to the same file
can hang nodes and get accounts locked.

## Using local scratch for heavy I/O

Software that reads or writes many files is much faster from the compute node's local disk:

```bash
cd "$TMPDIR"                                         # per-job local scratch
cp /quobyte/emoniergrp/<you>/input.nc .
python process.py input.nc output.nc
cp output.nc /quobyte/emoniergrp/<you>/results/output-${SLURM_JOB_ID}.nc   # copy back before exit
```

Local scratch is deleted when the job ends.

## Handy aliases

Add these to `~/.bashrc` on FARM:

```bash
alias sq='squeue -u $USER'              # my jobs
alias sjob='scontrol show -d job'       # details: sjob <jobid>
```

## When a job fails

1. Read the `.err` and `.out` logs.
2. `sacct -j <jobid> --format=JobID,State,ExitCode,Elapsed,MaxRSS,ReqMem` shows whether it ran out
   of time (`TIMEOUT`) or memory (`OUT_OF_MEMORY`).
3. Still stuck? Ask in the lab chat, then email `farm-hpc@ucdavis.edu` with your username, the
   cluster, the job ID, the exact command, and the error.

More: <https://docs.hpc.ucdavis.edu/farm/>
