# Frequently Asked Questions

If you have a very common problem it might be covered in this FAQ section, if you are unlucky and you cannot find any help on this page, please contact us at `cecile-admins-l at ovgu.de`.

## Cecile access

- **I can't access the cluster from outside the university.**

    If you are outside the university network you can access Cecile only though [eduVPN](https://www.urz.ovgu.de/en/vpn-path-204,616.html).

## Data

- **I would like to put my data on Cecile, what should I do?**

    Data on Cecile are organized as `projects`. A project can be requested to the admin after filling in a questionnaire. Please, read the relevant [information](../cecile/data.md/#how-to-create-a-project-on-cecile).

- **I am trying to transfer data from Cecile to my local computer via `rsync`, but it fails with the following error: `rsync: mkdir "/home/my-local-computer/Documents" failed: No such file or directory (2) rsync error: error in file IO (code 11) at main.c(663) [Receiver=3.1.3]`.**

    When transferring data from the cluster to your local computer via `rsync`, you have to run the `rsync` from your local computer and not from cluster. Please, read the relevant [information](../cecile/data.md/#how-to-transfer-data-fromto-cecile)

- **I am unable to produce new files on my `home` or `groups` or `projects`**

    In Cecile directories such as `home`, `groups` and `projects` have quotas, which means that there is a fix limit to the amount of data that can be added to those directories. Please,refer to the relevant [information](../cecile/data.md/#data-storage-quotas).

## Slurm

### Failing jobs

- **My jobs failed with no apparent error, what could the cause be?**

    You might not have created a folder for saving your log files (errors and outputs), try to create the log folder. Please, refer to the relevant [information](../cecile/slurm.md/#how-to-run-a-job-in-slurm)

- **My job failed with the following error: `error: *** JOB 217698 ON compute01 CANCELLED AT 2024-09-26T23:22:59, DUE TO TIME LIMIT ***`**

    Your job failed because you specified a lower duration than the one needed by your computations. Try increasing the duration of your job with the parameter `#SBATCH --time=01:00:00 #D-HH:MM:SS`. Please, refer to the relevant [information](../cecile/slurm.md/#how-to-run-a-job-in-slurm)

- **My job failed withe the following error: `error: Detected 1 oom-kill event(s) in StepId=217699.batch cgroup. Some of your processes may have been killed by the cgroup out-of-memory, handler.`.**

    Your job might have failed because the memory you have requested for your job is not enough. You might want to investigate your failed job via `seff` or `sacct` to understand the actual amount of RAM that your job requires, and increase your RAM accordingly by changing the parameter `#SBATCH --mem-per-cpu`. Depending on your job, you might also need to change your CPU request. Please refer to the relevant [information](../cecile/slurm.md/#how-to-define-the-amount-of-resources-and-time-for-your-jobs)  

- **My job failed with the following error: `line 13: ./my_script.py: Permission denied`.**

    Your job might have failed because your script is not executable, you can either make your script [executable](../cluster/cecile/slurm.md/#how-to-run-a-job-in-slurm), or you could explicitly call the language you need to run your script (e.g. `python my_script.py` or `bash my_script.sh`).

- **Although I load the software stack my job failed because the software I needed was not found. What can be the reason for it?**

    Did you load the software stuck from your slurm script? If this is not the case try to load the software stack directly in the slurm script.

### Slurm common commands

- **How do I check whether my job is running?**
        
    Use the command [`squeue`](../cecile/slurm.md/#slurm-basic-commands)

- **How can I see information about my job?**

    You can use either the command `seff` or `sacct`, please, refer to the relevant [information](../cecile/slurm.md/#how-to-define-the-amount-of-resources-and-time-for-your-jobs)

## Software stack

- **I cannot find a software in Cecile**

    Software in Cecile are available in a software stack that needs to be loaded before getting access to a specific software. Please, refer to the relevant [information](../cecile/software.md)

- **I cannot find the software I need in the software stack.**

    1. After loading the software stack, you can use the command `module avail` to print the list of available software.
    2. Some software in the software stack have slightly different names, for example python packages are called `py-<package name>` (e.g. `py-pandas`) or R packages are all called `r-<library name>`.
    3. If you cannot still find the software among the available software that means that the software is not yet included in the software stack. Please contact cecile-admins-l at ovgu.de to request the software.
