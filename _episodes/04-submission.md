---
title: "ARCHER2 scheduler: Slurm"
teaching: 30
exercises: 20
questions:
- "How do I write job submission scripts?"
- "How do I control jobs?"
- "How do I find out what resources are available?"
objectives:
- "Understand the use of the basic Slurm commands."
- "Know what components make up and ARCHER2 scheduler."
- "Know where to look for further help on the scheduler."
keypoints:
- "ARCHER2 uses the Slurm scheduler."
- "`srun` is used to launch parallel executables in batch job submission scripts."
- "There are a number of different partitions (queues) available."
---

ARCHER2 uses the Slurm job submission system, or *scheduler*, to manage resources and how they are made
available to users. The main commands you will use with Slurm on ARCHER2 are:

* `sinfo`: Query the current state of nodes
* `sbatch`: Submit non-interactive (batch) jobs to the scheduler
* `squeue`: List jobs in the queue
* `scancel`: Cancel a job
* `salloc`: Submit interactive jobs to the scheduler
* `srun`: Used within a batch job script or interactive job session to start a parallel program

Full documentation on Slurm on ARCHER2 can be found in the [Running Jobs on ARCHER2](https://docs.archer2.ac.uk/user-guide/scheduler/) section of the ARCHER2 documentations.

## Finding out what resources are available: `sinfo`

The `sinfo` command shows the current state of the compute nodes known to the scheduler:

```
auser@ln01:~> sinfo
```
{: .language-bash}
```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
standard     up 1-00:00:00      6 drain$ nid[001693,002059,002546,003583,005123,005343]
standard     up 1-00:00:00     18 drain* nid[001395,001442,002195,002447,003082,003257,003395,003598,004148,004213,004231,004336-004337,004999,005481,005731,005799,006210]
standard     up 1-00:00:00      8  drain nid[001638,002315,002689,005507,005930,006127,006190,006759]
standard     up 1-00:00:00     95   resv nid[001256-001257,001259-001262,001296-001383,002760]
standard     up 1-00:00:00   5067  alloc nid[001000-001255,001258,001263-001295,001384-001394,001396-001416,001419-001441,001443-001574,001579-001582,001593-001637,001639-001692,001694-001799,001812,001817-002051,002053-002058,002060-002121,002123,002126-002194,002196-002200,002202-002229,002234-002239,002251-002256,002259-002279,002300-002303,002311,002334,002355,002368-002369,002391-002400,002408-002409,002412,002414-002415,002418-002421,002423-002429,002431,002433-002434,002437-002446,002448-002502,002507-002514,002525-002534,002536-002545,002547-002688,002690-002759,002761-002950,002952-002958,002982,002991-002995,003002-003003,003015-003036,003038,003041-003042,003048-003081,003083-003256,003258-003362,003364-003369,003371-003394,003396-003431,003461-003463,003465-003467,003478-003500,003521-003525,003527-003532,003539,003542,003544-003552,003555-003582,003584-003597,003599-003943,003953,003956,003959,003965-003971,003977-003978,003980,003984,004002-004003,004005,004010,004012-004013,004015-004017,004021,004025-004026,004028,004038-004057,004060,004063-004068,004072-004147,004149-004212,004214-004230,004232-004335,004338-004583,004620-004623,004628-004629,004632-004633,004636-004638,004647,004653,004672,004679-004680,004695,004701-004702,004705-004715,004718-004744,004747-004748,004758-004761,004763,004765-004807,004811-004818,004821-004835,004837-004974,004983-004998,005000-005122,005124-005342,005344-005348,005351-005398,005404,005408-005480,005482-005506,005508-005526,005528-005558,005560,005565,005569-005576,005578-005587,005589-005591,005593-005595,005598,005604-005606,005608-005635,005638-005659,005661-005689,005691-005695,005697-005705,005707-005711,005717-005729,005735-005747,005749-005758,005784,005807-005808,005810-005814,005824,005828,005845-005848,005850-005861,005864-005929,005931-006037,006054-006126,006128-006189,006191-006209,006211-006387,006389-006390,006392-006756,006758,006760-006859]
standard     up 1-00:00:00    666   idle nid[001417-001418,001575-001578,001583-001592,001800-001811,001813-001816,002052,002122,002124-002125,002201,002230-002233,002240-002250,002257-002258,002280-002299,002304-002310,002312-002314,002316-002333,002335-002354,002356-002367,002370-002390,002401-002407,002410-002411,002413,002416-002417,002422,002430,002432,002435-002436,002503-002506,002515-002524,002535,002951,002959-002981,002983-002990,002996-003001,003004-003014,003037,003039-003040,003043-003047,003363,003370,003432-003460,003464,003468-003477,003501-003520,003526,003533-003538,003540-003541,003543,003553-003554,003944-003952,003954-003955,003957-003958,003960-003964,003972-003976,003979,003981-003983,003985-004001,004004,004006-004009,004011,004014,004018-004020,004022-004024,004027,004029-004037,004058-004059,004061-004062,004069-004071,004584-004619,004624-004627,004630-004631,004634-004635,004639-004646,004648-004652,004654-004671,004673-004678,004681-004694,004696-004700,004703-004704,004716-004717,004745-004746,004749-004757,004762,004764,004808-004810,004819-004820,004836,004975-004982,005349-005350,005399-005403,005405-005407,005527,005559,005561-005564,005566-005568,005577,005588,005592,005596-005597,005599-005603,005607,005636-005637,005660,005690,005696,005706,005712-005716,005730,005732-005734,005748,005759-005783,005785-005798,005800-005806,005809,005815-005823,005825-005827,005829-005844,005849,005862-005863,006038-006053,006388,006391,006757]
highmem      up 1-00:00:00      1   resv nid002760
highmem      up 1-00:00:00    516  alloc nid[002756-002759,002761-002911,002916-002950,002952-002958,002982,002991-002995,003002-003003,003015-003036,003038,003041-003042,006376-006387,006389-006390,006392-006411,006416-006667]
highmem      up 1-00:00:00     59   idle nid[002951,002959-002981,002983-002990,002996-003001,003004-003014,003037,003039-003040,003043-003047,006388,006391]
serial       up 1-00:00:00      2   idle dvn[01-02]
```
{: .output}

There is a row for each node state and partition combination. The default output shows the following columns:

* `PARTITION` - The system partition
* `AVAIL` - The status of the partition - `up` in normal operation
* `TIMELIMIT` - Maximum runtime as `days-hours:minutes:seconds`: on ARCHER2, these are set using *QoS*
  (Quality of Service) rather than on partitions
* `NODES` - The number of nodes in the partition/state combination
* `STATE` - The state of the listed nodes (more information below)
* `NODELIST` - A list of the nodes in the partition/state combination

The nodes can be in many different states, the most common you will see are:

* `idle` - Nodes that are not currently allocated to jobs
* `alloc` - Nodes currently allocated to jobs
* `draining` - Nodes draining and will not run further jobs until released by the systems team
* `down` - Node unavailable
* `fail` - Node is in fail state and not available for jobs
* `reserved` - Node is in an advanced reservation and is not generally available
* `maint` - Node is in a maintenance reservation and is not generally available

If you prefer to see the state of individual nodes, you can use the `sinfo -N -l` command.

> ## Lots to look at!
> Warning! The `sinfo -N -l` command will produce a lot of output as there are 5860 individual 
> nodes on the current ARCHER2 system! (It will be even worse on the full system when there will be
> over 5,860 nodes.) 
{: .callout}

```
auser@ln01:~> sinfo -N -l
```
{: .language-bash}
```
Mon Nov 29 19:02:37 2021
NODELIST   NODES PARTITION       STATE CPUS    S:C:T MEMORY TMP_DISK WEIGHT AVAIL_FE REASON              
dvn01          1    serial        idle 256    2:64:2 515450        0      1 DVN,AMD_ none                
dvn02          1    serial        idle 256    2:64:2 515450        0      1 DVN,AMD_ none                
nid001000      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001001      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001002      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001003      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001004      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001005      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001006      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001007      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001008      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001009      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001010      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001011      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001012      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001013      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001014      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                
nid001015      1  standard   allocated 256    2:64:2 227328        0      1 COMPUTE, none                                   
...lots of output trimmed...

```
{: .output}

> ## Explore a compute node
> Let’s look at the resources available on the compute nodes where your jobs will actually run. Try running this
> command to see the name, CPUs and memory available on the worker nodes (the instructors will give you the ID of
> the compute node to use):
> ```
> [auser@ln01:~> sinfo -n nid001000 -o "%n %c %m"
> ```
> {: .language-bash}
> This should display the resources available for a standard node. Are they what you expect given what we learnt
> earlier about the configuration of a compute node?
> > ## Solution
> > The output should show:
> > ```
> > HOSTNAMES CPUS MEMORY
> > nid001000 256 227328
> > ```
> > You may be surprised that the output shows 256 CPUS per compute node when the earlier description stated
> > that there were 128 cores per node. This is because each physical core can support two hardware threads
> > (often referred to as hyperthreads). Most jobs will only make use of one of these threads as using the
> > other thread can reduce performance.
> {: .solution}
{: .challenge}

## Using batch job submission scripts

### Header section: `#SBATCH`

As for most other scheduler systems, job submission scripts in Slurm consist of a header section with the
shell specification and options to the submission command (`sbatch` in this case) followed by the body of
the script that actually runs the commands you want. In the header section, options to `sbatch` should 
be prepended with `#SBATCH`.

Here is a simple example script that runs the `xthi` program (which shows process and thread placement) across
two nodes.

```
#!/bin/bash
#SBATCH --job-name=my_mpi_job
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=128
#SBATCH --cpus-per-task=1
#SBATCH --time=0:10:0
#SBATCH --account=ta001
#SBATCH --partition=standard
#SBATCH --qos=standard

# Now load the "xthi" package
module load xthi

export OMP_NUM_THREADS=1

# Load modules, etc.
# srun to launch the executable
srun --hint=nomultithread --distribution=block:block xthi
```
{: .language-bash}

The options shown here are:

* `--job-name=my_mpi_job` - Set the name for the job that will be displayed in Slurm output
* `--nodes=2` - Select two nodes for this job
* `--ntasks-per-node=128` - Set 128 parallel processes per node (usually corresponds to MPI ranks)
* `--cpus-per-task=1` - Number of cores to allocate per parallel process 
* `--time=0:10:0` - Set 10 minutes maximum walltime for this job
* `--account=ta001` - Charge the job to the `t01` budget

We will discuss the `srun` command further below.

### Submitting jobs using `sbatch`

You use the `sbatch` command to submit job submission scripts to the scheduler. For example, if the
above script was saved in a file called `test_job.slurm`, you would submit it with:

```
auser@ln01:~> sbatch test_job.slurm
```
{: .language-bash}
```
Submitted batch job 23996
```
{: .output}

Slurm reports back with the job ID for the job you have submitted

### Checking progress of your job with `squeue`

You use the `squeue` command to show the current state of the queues on ARCHER2. Without any options, it
will show all jobs in the queue:

```
auser@ln01:~> squeue
```
{: .language-bash}
```

JOBID  PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON) 
451729  standard 243d9382        a PD       0:00     64 (AssocMaxCpuMinutesPerJobLimit) 
451750  standard Neg_3.51        b PD       0:00     35 (QOSMaxNodePerUserLimit) 
451752  standard    PosL1        b PD       0:00     35 (QOSMaxNodePerUserLimit) 
451754  standard   Pos_R1        b PD       0:00     35 (QOSMaxNodePerUserLimit) 
451757  standard    PosL2        b PD       0:00     35 (QOSMaxNodePerUserLimit) 
451759  standard    PosL4        b PD       0:00     35 (QOSMaxNodePerUserLimit) 
461382  standard       NO        c PD       0:00      2 (AssocMaxCpuMinutesPerJobLimit) 
461387  standard       ON        c PD       0:00      4 (AssocMaxCpuMinutesPerJobLimit) 
461392  standard       ON        c PD       0:00      2 (AssocMaxCpuMinutesPerJobLimit) 
404533  standard  xml_gen        d PD       0:00      1 (DependencyNeverSatisfied) 
404600  standard  xml_gen        d PD       0:00      1 (DependencyNeverSatisfied) 
406679  standard  xml_gen        d PD       0:00      1 (DependencyNeverSatisfied) 
406696  standard  xml_gen        d PD       0:00      1 (DependencyNeverSatisfied) 
462580_[29-55]  standard   int1.9       e PD       0:00     24 (QOSMaxNodePerUserLimit) 
464006  standard lammps_t        f PD       0:00      4 (QOSMaxJobsPerUserLimit) 
464007  standard lammps_t        f PD       0:00      4 (QOSMaxJobsPerUserLimit) 
463164+1  standard    KrJ50      g PD       0:00     56 (Resources) 
463164+0  standard    KrJ50      g PD       0:00     21 (Resources) 
461842  standard u-cg647.        h PD       0:00     82 (Priority) 
462637+0  standard eO1_medu      i PD       0:00      8 (Resources) 
462637+1  standard eO1_medu      i PD       0:00      1 (Resources) 
462086  standard nemo_tes        j PD       0:00      8 (Priority) 
...
```
{: .output}

You can show just your jobs with `squeue -u $USER`.

### Cancelling jobs with `scancel`

You can use the `scancel` command to cancel jobs that are queued or running. When used on running jobs
it stops them immediately.

### Running parallel applications using `srun`

Once past the header section your script consists of standard shell commands required to run your
job. These can be simple or complex depending on how you run your jobs but even the simplest job
script usually contains commands to:

* Load the required software modules
* Set appropriate environment variables (you should always set `OMP_NUM_THREADS`, even if you are
  not using OpenMP you should set this to `1`)

After this you will usually launch your parallel program using the `srun` command. At its simplest,
`srun` only needs 2 arguments to specify the correct binding of processes to cores (it will use the
values supplied to `sbatch` to work out how many parallel processes to launch). In the example above,
our `srun` command simply looks like:

```
srun --hint=nomultithread --distribution=block:block xthi
```
{: .language-bash}

## STDOUT/STDERR from jobs

STDOUT and STDERR from jobs are, by default, written to a file called `slurm-<jobid>.out` in the
working directory for the job (unless the job script changes this, this will be the directory
where you submitted the job). So for a job with ID `12345` STDOUT and STDERR would be in
`slurm-12345.out`.

If you run into issues with your jobs, the Service Desk will often ask you to send your job
submission script and the contents of this file to help debug the issue.

If you need to change the location of STDOUT and STDERR you can use the `--output=<filename>`
and the `--error=<filename>` options to `sbatch` to split the streams and output to the named
locations.

<!-- Exercise to check output of test job -->

> ## Underpopulation of nodes
> You may often want to *underpopulate* nodes on ARCHER2 to access more memory or more memory 
> bandwidth per task. Can you state the `sbatch` options you would use to run `xthi`:
> 
> 1. On 4 nodes with 64 tasks per node?
> 2. On 8 nodes with 2 tasks per node, 1 task per socket?
> 3. On 4 nodes with 32 tasks per node, ensuring an even distribution across the 8 NUMA regions
> on the node?
> 
> Once you have your answers run them in job scripts and check that the binding of tasks to 
> nodes and cores output by `xthi` is what you expect.
> 
> > ## Solution
> > 1. `--nodes=4 --ntasks-per-node=64`
> > 2. `--nodes=8 --ntasks-per-node=2 --ntasks-per-socket=1`
> > 3. `--nodes=4 --ntasks-per-node=32 --ntasks-per-socket=16 --cpus-per-task=4`
> {: .solution}
{: .challenge}



### Hybrid MPI and OpenMP jobs

When running hybrid MPI (with the individual tasks also known as ranks or processes) and OpenMP
(with multiple threads) jobs you need to leave free cores between the parallel tasks launched
using `srun` for the multiple OpenMP threads that will be associated with each MPI task.

As we saw above, you can use the options to `sbatch` to control how many parallel tasks are
placed on each compute node and can use the `--cpus-per-task` option to set the stride 
between parallel tasks to the right value to accommodate the OpenMP threads - the value
for `--cpus-per-task` should usually be the same as that for `OMP_NUM_THREADS`. To ensure
you get the correct thread pinning, you also need to specify an additional OpenMP environment
variable. Specifically:

   - Set the `OMP_PLACES` environment variable to `cores` with `export OMP_PLACES=cores` in 
     your job submission script

As an example, consider the job script below that runs across 2 nodes with 8 MPI tasks
per node and 16 OpenMP threads per MPI task (so all 256 cores across both nodes are used,
128 cores per node).

```
#!/bin/bash
#SBATCH --job-name=my_hybrid_job
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=16
#SBATCH --time=0:10:0
#SBATCH --account=t01
#SBATCH --partition=standard
#SBATCH --qos=standard

# Now load the "xthi" package
module load xthi

export OMP_NUM_THREADS=16
export OMP_PLACES=cores

# Load modules, etc.
# srun to launch the executable
srun --hint=nomultithread --distribution=block:block xthi
```
{: .language-bash}

Each ARCHER2 compute node is made up of 8 NUMA (*Non Uniform Memory Access*) regions (4 per socket) 
with 16 cores in each region. Programs where the threads of a task span multiple NUMA regions
are likely to be *much less* efficient so we recommend using thread counts that fit well into the
ARCHER2 compute node layout. Effectively, this means one of the following options for hybrid jobs
on nodes where all cores are used:

* 8 MPI tasks per node and 16 OpenMP threads per task: equivalent to 1 MPI task per NUMA region
* 16 MPI tasks per node and 8 OpenMP threads per task: equivalent to 2 MPI tasks per NUMA region
* 32 MPI tasks per node and 4 OpenMP threads per task: equivalent to 4 MPI tasks per NUMA region
* 64 MPI tasks per node and 2 OpenMP threads per task: equivalent to 8 MPI tasks per NUMA region 



## Other useful information

In this section we briefly introduce other scheduler topics that may be useful to users. We
provide links to more information on these areas for people who may want to explore these 
areas more. 

### Interactive jobs: direct `srun` 

Similar to the batch jobs covered above, users can also run interactive jobs using the `srun`
command directly. `srun` used in this way takes the same arguments as `sbatch` but, obviously, these are 
specified on the command line rather than in a job submission script. As for `srun` within 
a batch job, you should also provide the name of the executable you want to run.

For example, to execute `xthi` across all cores on two nodes (1 MPI task per core and no
OpenMP threading) within an interactive job you would issue the following commands:

```
auser@ln01:~> srun --partition=standard --qos=standard --nodes=2 --ntasks-per-node=128 --cpus-per-task=1 --time=0:10:0 --account=ta001 xthi
```
{: .language-bash}
```
Node    0, hostname nid001030
Node    1, hostname nid001031
Node    0, rank    0, thread   0, (affinity = 0,128)
Node    0, rank    1, thread   0, (affinity = 16,144)
Node    0, rank    2, thread   0, (affinity = 32,160)
Node    0, rank    3, thread   0, (affinity = 48,176)
Node    0, rank    4, thread   0, (affinity = 64,192)
Node    0, rank    5, thread   0, (affinity = 80,208)
Node    0, rank    6, thread   0, (affinity = 96,224)
Node    0, rank    7, thread   0, (affinity = 112,240)
Node    0, rank    8, thread   0, (affinity = 1,129)
Node    0, rank    9, thread   0, (affinity = 17,145)
Node    0, rank   10, thread   0, (affinity = 33,161)
Node    0, rank   11, thread   0, (affinity = 49,177)
Node    0, rank   12, thread   0, (affinity = 65,193)
Node    0, rank   13, thread   0, (affinity = 81,209)
Node    0, rank   14, thread   0, (affinity = 97,225)
...long output trimmed...
```
{: .output}


<!-- Need to add information on the solid state storage and Slurm once it is in place

### Using the ARCHER2 solid state storage

-->


{% include links.md %}


