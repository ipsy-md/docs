## What is Slurm?

A computational cluster is made up of multiple resources which can be exploited to run a big number of tasks at the same time. In order to achieve this goal, you need to use a software called job scheduler, in our case is [Slurm](https://slurm.schedmd.com/quickstart.html), an open source cluster management and job scheduler system.

The job scheduler allows users to specify computational tasks, known as **jobs**, that are going to be automatically distributed across the available and most suitable resources (see image below). A job can be any computational task as long as it respects specific [restrictions], for example it could be a specific section of your preprocessing analysis, or the whole analysis of a single subject.

[restrictions]: #job-restrictions-and-defaults

<figure markdown="span">
  ![Slurm](../images/slurm.png){ width="600" }
  <figcaption></figcaption>
</figure>

## How to think about jobs

If you are using Cecile, you most likely need to run multiple similar jobs at the same time, the key to achieve this goal is to code your analysis with the objective of making it parallelizable:

- Create scripts that process only one dataset (e.g. one participant) at a time. In this way one job is going to correspond to one task, for example the preprocessing of a single subject dataset.
- Chunk your analysis in steps. In this way, especially if you are new to slurm, you have more control on your analysis, your outcomes and it reduces the duration of your jobs.

## How to run a job in Slurm

There are different types of jobs in slurm (e.g. single jobs, interactive jobs etc.), but before talking about the specific jobs there are a few things you need to take care of:

1. Make sure that the code you want to use for your jobs works properly. Especially at the beginning it might difficult to identify the reason why a job is failing, thus by making sure that your scripts run as intended you can restrict your error space.
2. Make sure your scripts (e.g. in bash) are executable.
   
    ??? note "Making a file executable"

        Add the so called **shebang** on top of your bash script, it will tell the system to use the bash interpreter to run the code:
        
        ```bash
        #!/bin/bash
        ```
        Once you have done that, run the following command to make your script exacutable:
        
        ```bash
        chmod +x <script.sh>
        ``` 
        For python use the following shebang in the first line of your script:
        
        ```python
        #!/usr/bin/env python3
        ```
        And then make it exacutable from the command line:
        
        ```bash
        chmod +x <script.py>
        ```

3. Create a folder called `slurm-logs` (or you can give it another meaningful name), in the same directory as the scripts are or in the `scratch` of your project. Slurm will use it to dump the logs reporting error files and printed outputs. 

4. Activate the [software stack] environment or load the modules that you need for your code.  
   This needs to be done inside the script used to run your slurm jobs (see in the script examples).
   [software stack]: ../software/##how-to-use-the-stacks 

In order to specify a job in Slurm, you need to make a few decisions and provide a few essential information to the system beforehand. 

!!! note "Ask yourself the following questions before setting up your job"

    1. How long does your job need to run?</b>  
    2. How many CPUs does your job need?</b>  
    3. How much memory does your job need?</b>  
    4. Does your job require any special hardware feature?

The following parameters are not all mandatory, but we **strongly recommend** to define all of them. If you do not define them properly your job might not run as expected, might stop or even fail and you might involuntarily take resources away from other users.

Here it is how you turn the previous questions in parameters for slurm script.

```bash title="Setting Slurm parameters" linenums="1"
#!/bin/sh 

#SBATCH --job-name=my_job
#SBATCH --mail-user=<name.lastname>@ovgu.de  
#SBATCH --mail-type=BEGIN,END,FAIL            
#SBATCH --cpus-per-task=1                    
#SBATCH --nodes=1                             
#SBATCH --mem-per-cpu=1g                     
#SBATCH --time=01:00:00                       
#SBATCH --output=slurm-logs/%x-%A-%a.out 
#SBATCH --error=slurm-logs/%x-%A-%a.err  

```

!!! note "# is not a comment in SLURM"
    Keep in mind that for your slurm script the # before `SBATCH` is not interpreted as a shell comment

- `#SBATCH --job-name=my_job`: the name you provide for your job
- `#SBATCH --mail-user=<name.lastname>@ovgu.de`: your email address, in case you want to receive a job feedback by email.
- `#SBATCH --mail-type=BEGIN,END,FAIL`: get an email at the beginning of a job, end of a job and when a job fails, respectively.
- `#SBATCH --cpus-per-task=1`: number of CPUs you request for a task
- `#SBATCH --nodes=1`: number of requested nodes, keep it to 1, it is fine for our type of analyses.
- `#SBATCH --mem-per-cpu=1g`: amount of memory you request per cpu. 
- `#SBATCH --time=01:00:00`: maximum duration you assign to a job (D-HH:MM:SS, for example `--time=00:01:00` is a one minute job).
- `#SBATCH --output=slurm-logs/%x-%A-%a.out`: here you specify where job printed output should be saved, specifically in the folder `slurm-logs`. 
- `#SBATCH --error=slurm-logs/%x-%A-%a.err`: here you specify where job related errors should be saved, again in the folder `slurm-logs`.

Instead of `--mem-per-cpu` you could also use `--mem`, the latter specifies the amount of RAM you request in a node.

!!! Warning "Be aware of memory and time"
    - If your job exceeds the requested memory, your job will be aborted.
    - If your job exceeds the requested time, your job will be aborted.


!!! note "Job ID"
    The job ID is the identification number relative to a job that can be used to acquire information about your job, or to abort your job. It might look something like this `217379_5`, where the prefix number before the underscore indicates the general job session, while the number after the underscore indicates the specific number of a job. You might have multiple jobs sharing the session number.   

## Types of Slurm jobs

=== "Single job"

    It allowes to run only a single job.</b>  

    **Use case:** It is ideal when you want to test a specific analysis and test it on one subject, or when you need to run just one analysis like a group statistical analysis.

    The following is a prototypical slurm script. You can use the name and the extension you prefer, we recommend to use `<filename>.slurm`

    ```bash title="Slurm single job"
    #!/bin/sh
 
    #SBATCH --job-name=my_job
    #SBATCH --cpus-per-task=1
    #SBATCH --nodes=1
    #SBATCH --mem-per-cpu=1g
    #SBATCH --time=01:00:00
    #SBATCH --output=slurm-logs/%x-%A-%a.out
    #SBATCH --error=slurm-logs/%x-%A-%a.err

    ./my_script.sh 
    ```
    
    To run a job you use the following command:

    ```bash
    sbatch <filename>.slurm
    ```

    You can now run `squeue` to see the status of your job, let's understand the `squeue` output:

    **A running job**
    ```
                JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            217379_0       all fmriprep    user   R       1:10      1 compute05
    ```

    **A pending job**
    ```
                JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            217379_0       all fmriprep    user  PD       1:10      1 compute05
    ```

    **An idled job**
    ```
                JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            217379_5       all bad_job     user   IDLE    0:00      1 compute05
    ```

    Let's understand the `squeue` output:

    - `JOBID`: Job indentification number
    - `PARTITION`: Name of the partition your jobs are currently using 
    - `NAME`: Name you gave to your job
    - `USER`: Your username
    - `ST`: Status of your job
    - `R`: A running job
    - `PD`: A pending job, it is waiting for resources to be freed
    - `IDLE`: An idled job, something might be wrong with your scripts or your in the scirpt for submitting jobs check the error logs
    - `TIME`: Time since the job started running
    - `NODES`: Number of nodes
    - `NODELIST` : Node name


    #### Single job with Matlab

    ```bash title="Single job Matlab"
    #!/bin/bash
    #SBATCH --job-name=matlab        # job name, you give it a name you like
    #SBATCH --nodes=1                # number of nodes
    #SBATCH --ntasks=1               # number of tasks
    #SBATCH --cpus-per-task=1        # cpu per task
    #SBATCH --mem-per-cpu=1G         # memory per cpu
    #SBATCH --time=00:01:00          # max amount of time (D:HH:MM:SS)
    #SBATCH --output=slurm-logs/%x-%A-%a.out   # printed output
    #SBATCH --error=slurm-logs/%x-%A-%a.err     # errors

    ## load the software stack
    . /software/current/env.sh
    ## you need to load matlab even if you have loaded already all the modules
    ## from the software stack 
    module load matlab

    subject=01

    ## run matlab: you pass just the name of the script without .m in my case matlab_script
    matlab -singleCompThread -nodisplay -nosplash -r "matlab_script(${subject})"
    ```


    #### Single job with python

    ```bash title="Single job Python"
    #!/bin/bash
    #SBATCH --job-name=python        # job name, you give it a name you like
    #SBATCH --nodes=1                # number of nodes
    #SBATCH --ntasks=1               # number of tasks
    #SBATCH --cpus-per-task=1        # cpu per task
    #SBATCH --mem-per-cpu=1G         # memory per cpu
    #SBATCH --time=00:01:00          # max amount of time (D:HH:MM:SS)
    #SBATCH --output=slurm-logs/%x-%A-%a.out   # printed output
    #SBATCH --error=slurm-logs/%x-%A-%a.err     # errors

    # load the stack and the module you need
    . /software/current/env.sh
    module load python

    subject=01

    python -u python_script ${subject}
    ```

    In case you made your python script executable you can remove the line `module load python` and call the script as follows: `./python_script ${subject}` without prepending `python -u`

=== "Array job"

    An array job allows you to run multiple jobs in parallel.</b> 

    **Use case:** It is ideal when you need to run similar tasks multiple times. For example, the same analysis for many subjects, or you need to test different parameters for the same analysis.


    In an array job the crucial parameter is `#SBATCH --array` which specifies the number of jobs we would like to run simultaneously. You define the array of jobs indicating the range of the jobs, e.g. `1-5` (see highlightened line in the script below)

    !!! note "How to define the `--array` parameter"

        - Define a range: ```#SBATCH --array 0-5```  from 0 to 5 for 6 total jobs.</b>  
        - Define specific numbers: ```#SBATCH --array 1, 4, 6, 10```  (e.g. they might be specific subject IDs) for 4 total jobs.</b>  
        - Define a range with specific intervals: ```#SBATCH --array 1-20:2```</b>  
        - Define a range and specify the number of parallel jobs: ```#SBATCH -- array 1-400%100``` only 100 jobs are run simultaneously.</b>  


    ```bash linenums="1" hl_lines="10"
    #!/bin/sh

    #SBATCH --job-name=my_job
    #SBATCH --cpus-per-task=1
    #SBATCH --nodes=1
    #SBATCH --mem-per-cpu=1g
    #SBATCH --time=01:00:00
    #SBATCH --output=slurm_logs/output-%A-%a.out
    #SBATCH --error=slurm_logs/error-%A-%a.err
    #SBATCH --array 1-5
    ```

    #### Example of an array job

    Assume you want to run a specific analysis on 5 subjects in parallel and you want to use a script called `my_analysis.sh` that requires as unique parameter the subject ID. In order to set up a job array you can follow the next steps:

    - You need to provide to slurm the number of jobs by setting up `#SBATCH --array` as we have seen above. In this case our range is `0-4`, in total 5 jobs as the number of your subjects (remember that the shell interpreter counts starting from 0).
    - Now you need to pass the subject ID parameter to the script. To do that, you can take advantage of the slurm variable `SLURM_ARRAY_TASK_ID` which keeps track of the jobs count. This means that for the first job of the array `SLURM_ARRAY_TASK_ID` will take the value 0, for the second job it will tale the value 1 and so on until the fifth job, for which it will take the value 4. `SLURM_ARRAY_TASK_ID` can be used as an index to extract the subject ID from the variable `subjects`. </b>  
    - For the sake of practicality, we can assign `SLURM_ARRAY_TASK_ID` to the variable `idx`, which can be used to extract for each job the subject ID, `${subjects[idx]}`, and then it can be passed to the script `my_analysis.sh`. 
  
    In other words, for the first job `SLURM_ARRAY_TASK_ID` is going to be equal to 0 (because your array starts at 0) and so will be `idx`. Hence, the expression `${subjects[idx]}` will extract from the `subjects` array the first item, namely `01`, which is the ID of your first subject. For the second job, `SLURM_ARRAY_TASK_ID` will be equal to 1 and consequently `${subjects[idx]}` will be equal to `02` and so on until the last job.

    ```bash title="Array job" 
    #!/bin/sh

    #SBATCH --job-name=my_job
    #SBATCH --cpus-per-task=1
    #SBATCH --nodes=1
    #SBATCH --mem-per-cpu=1g
    #SBATCH --time=01:00:00
    #SBATCH --output=slurm_logs/output-%A-%a.out
    #SBATCH --error=slurm_logs/error-%A-%a.err
    #SBATCH --array 0-4  ## 5 jobs

    idx=$((SLURM_ARRAY_TASK_ID))

    subjects=(01 02 03 04 05)

    ./my_analysis.sh ${subjects[idx]}


    ```

    Now that your array job script is ready you can run it by using the `sbatch` command as follows:

    ```bash
    sbatch my_array_job.slurm
    ```

    Check whether your jobs are actually running via the command `squeue` or alternatively with `sinfo`:

    ```bash
    squeue
    ```

    You should see a similar output:

    ```
                JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            217379_[6]     all     my_job   user  PD       0:00      1 (Resources)
            217379_0       all     my_job   user   R       1:10      1 compute05
            217379_1       all     my_job   user   R       1:10      1 compute05
            217379_2       all     my_job   user   R       1:10      1 compute05
            217379_3       all     my_job   user   R       1:10      1 compute05
            217379_4       all     my_job   user   R       1:10      1 compute05
            217379_5       all     my_job   user   IDLE    0:00      1 compute05


    ```

    Let's understand the `squeue` output:

    - `JOBID`: Job indentification number
    - `PARTITION`: Name of the partition your jobs are currently using 
    - `NAME`: Name you gave to your job
    - `USER`: Your username
    - `ST`: Status of your job
    - `R`: A running job
    - `PD`: A pending job, it is waiting for resources to be freed
    - `IDLE`: An idled job, something might be wrong with your scripts or your in the scirpt for submitting jobs check the error logs
    - `TIME`: Time since the job started running
    - `NODES`: Number of nodes
    - `NODELIST` : Node name



    #### Job array with Matlab

    ```bash title="Array job Matlab"
    #!/bin/bash

    #SBATCH --job-name=matlab        # job name, you give it a name you like
    #SBATCH --nodes=1                # number of nodes
    #SBATCH --ntasks=1               # number of tasks
    #SBATCH --cpus-per-task=1        # cpu per task
    #SBATCH --mem-per-cpu=1G         # memory per cpu
    #SBATCH --time=00:01:00          # max amount of time (D:HH:MM:SS)
    #SBATCH --output=slurm_logs/output-%A-%a.out   # printed output
    #SBATCH --error=slurm_logs/error-%A-%a.err     # errors
    #SBATCH --array 0-4

    # load the stack and the module you need
    . /software/current/env.sh
    module load matlab

    idx=$((SLURM_ARRAY_TASK_ID))

    subjects=(01 02 03 04 05)

    ## run matlab: you pass just the name of the script without .m in my case matlab_script
    matlab -singleCompThread -nodisplay -nosplash -r "matlab_script(${subjects[idx]})"
    ```

    #### Job array with python

    ```bash title="Array job Python"
    #!/bin/bash
    #SBATCH --job-name=python        # job name, you give it a name you like
    #SBATCH --nodes=1                # number of nodes
    #SBATCH --ntasks=1               # number of tasks
    #SBATCH --cpus-per-task=1        # cpu per task
    #SBATCH --mem-per-cpu=1G         # memory per cpu
    #SBATCH --time=00:01:00          # max amount of time (D:HH:MM:SS)
    #SBATCH --output=slurm_logs/output-%A-%a.out   # printed output
    #SBATCH --error=slurm_logs/error-%A-%a.err     # errors
    #SBATCH --array 0-4

    # load the stack and the module you need
    . /software/current/env.sh
    module load python3

    idx=$((SLURM_ARRAY_TASK_ID))

    subjects=(01 02 03 04 05)

    python -u python_script ${subjects[idx]}

    ```

    In case you made your python code executable you can remove the line `module load python` and call the script as follows: `./python_script ${subjects[idx]}` without prepending `python -u`


    #### Job array with nested parameters

    A typical analysis might require to analyze different subjects with different sessions or different parameters. The following scripts exmplifies a similar case in which each subject needs to be run for three different sessions.

    ```bash
    #!/bin/bash

    #SBATCH --job-name=multi_param
    #SBATCH --output=logs/%x-%A-%a.out
    #SBATCH --error=logs/%x-%A-%a.err
    #SBATCH --array 0-11

    SUBJECTS=(01 02 03 04)
    SESSIONS=("ses-01", "ses-02", "ses-03")

    SUBJECT_IDX=$((SLURM_ARRAY_TASK_ID % 4))
    SESSION_IDX=$((SLURM_ARRAY_TASK_ID % 3))

    ./my_analysis.sh --sub "sub-${SUBJECTS[SUBJECT_IDX]}" -ses "${SESSIONS[SESSION_IDX]}"
    ```

    In order to set up a similar script it is necessary to follow a few simple steps:

    1. Set the array range according to the total number of jobs you need to run. In this example we have 4 subjects and 3 different sessions per subject, thus we can set the `--array` range as `0-11`, which amounts to 12 jobs.
    2. Set up one array that defines the subject IDs `SUBJECTS` and another array that defines the session names `SESSIONS`.
    3. To assign each session to each subject we can leverage on the modulus operator (take a look at [modular arithmetic](https://en.wikipedia.org/wiki/Modular_arithmetic)) to provide the correct index for all the combinations. See in the example below how the modulus allows you to obtain all the index combinations.

        ```bash
        Job index  Subject index   Session index      Subject ID      Session ID
                   (job index % 4) (job index % 3)
        0               0               0               sub-01          ses-01  
        1               1               1               sub-02          ses-02
        2               2               2               sub-03          ses-03
        3               3               0               sub-04          ses-01
        4               0               1               sub-01          ses-02
        5               1               2               sub-02          ses-03 
        6               2               0               sub-03          ses-01
        7               3               1               sub-04          ses-01
        8               0               2               sub-01          ses-02
        9               1               0               sub-02          ses-01
        10              2               1               sub-03          ses-02
        11              3               2               sub-04          ses-03

        ```

    4. Then extract subject IDs and sessions by using the related indices.

=== "Interactive job"

    This approach allows you to work interactively inside a given node.</b>  

    **Use case:** It is ideal if you need to do some testing, or you just need to run some analyses interactively. Using an interactive job you avoid to work on the head node and take precious resources that are shared among all users.

    You can launch an interactive job in the following way:

    ```bash
    srun --mem=1G --time=01:00:00 --pty bash
    ```

    - `--mem`: memory requested (it can be specified in K|G)
    - `--time`: time requested for the interactive job
    - `--pty`: `bash -i` allows you use the terminal

    !!! Warning "Memory and time defaults"
        In case you do not specify time and memory requirements, by default the maximum amount of time allowed will be **24 hours** and the maximum amount of **memory** will be **4G**. If your software requires more time and/or more memory you must specify it.

    If you need to use a software with a GUI within an interactive session you should start the job using the following command:

    ```bash
    srun --pty --x11 bash
    ```

    !!! Warning "Closing a session" 
        If you close your session on the cluster the interactive job will be ended as well


## How to define the amount of resources and time for your jobs

The amount of resources necessary for a job must be decided on a case by case basis, there is no precise rule for choosing them given that too many variables can influence resources and time required. We will give you a few general principles which should work well anough in many instances. 

Please do not underestimate this step, your choices influence your own jobs and other users.

**One job to rule them all:**
As a general rule, when your are setting up new slurm jobs, test that everything works as intended using **only one job**. It is superfluous to say that this approach saves time for you and time and resources for other users, and importantly it reduces the amount of potential errors that might occur.

**If you are clueless about your job resource requirements, you could adopt the following approaches:**

- **Trial and error:** Set up one job using a very liberal estimate for the resources (e.g. `--mem-per-cpu`, `--time`). Once the job is over, you can use one of the following two commands to see the actual resources used and implement them in your future jobs:

    ```bash
    # job-id is the id of your job
    seff <job-id>
    ```
    If you run `seff` when your job is still ongoing, it will give you an unreliable estimate. 


    ```bash
    sacct -j <job-id> --format=jobid,jobname,reqmem,maxrss,elapsed,timelimit,state
    ```

- **Hunt for information:** In case you are using a standard tool (e.g. library, toolbox) you might find some indications about minimal resource requirements in the documentation, alternatively you could ask a more expert member of your group or other users (on Mattermost) who might have used a similar tool or pipeline. Both these approaches could be good starting estimates for your jobs.

- **Run your computation with an interactive job:** Set up an interactive job being rather liberal in your choice of resources. Run your computation (e.g. analysis, simulation) and check the resource requirements using the command `htop`. 

## Job restrictions and defaults

As for data storage, also jobs need some restrictions in order to guarantee a fair usage for everybody.

- **Time requested:** One job can have a **maximum duration of 24 hours**. This time duration could be extended by the cluster administrator upon reasonable request. Keep in mind that slurm will stop your job as soon as it exceeds the maximum time duration requested.
  
- **Maximum number of array jobs:** The maximum number of array jobs you can set are **10000**

## Slurm basic commands

```bash
sinfo # shows you current information about the system
```

```bash
squeue # shows the current jobs status

squeue -U <username> # it provides job status for a given user
```

```bash
sbatch <job_script># launch a job or an array job 
```

```bash
scancel <job_ID> # it cancels jobs

scancel -U <username> # all jobs belonging to that user will be cancelled
```

```bash
scontrol show partition compute # allows you to see partitions present and limitations 
```