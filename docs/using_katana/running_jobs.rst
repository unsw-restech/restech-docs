.. _running_jobs:

======================
Running Jobs on Katana
======================

.. contents::
   :depth: 1 
   :local:
   :backlinks: top 

The :ref:`def_head_node` or login node of a cluster is a shared resource for all users and is used for preparing, submitting and managing jobs. Never run any computationally intensive processes on the head node. Jobs are submitted from the head node, but they actually run on one or more of the compute nodes. The procedure by which jobs are allocated to compute nodes and managed during their lifetime is the responsibility of the resource manager and the job scheduler. Different clusters use different tools to manage resources and schedule jobs. For example Katana, like NCI, uses PBS Pro for these purpose.

Jobs are submitted using the qsub command. There are two types of job that qsub will accept: interactive jobs and batch jobs. An interactive job provides a login session on a compute node. This enables you to interact directly with the compute node by issuing any sequence of commands within the login session. Consequently, interactive jobs are useful for experimentation and debugging. In contrast, a batch job is a scripted job that runs from start to finish without any user intervention. The vast majority of jobs on the cluster are batch jobs. This type of job is appropriate for production runs of several hours or days.

When you wish to submit a batch job using qsub you will need to create a job script which specifies the resources that your job requires and calls your program. The general structure of a job script is shown below.

When you are starting a job as the amount of memory that you request goes up the number of places that your job can run goes down. The points at which you have access to fewer nodes are (in order):

    124gb, 180gb, 248gb, 370gb, 750gb and 1000gb.

It is the same thing when considering the number of CPU cores where the numbers are:

     16, 20, 24, 28, 32, 44 and 64 CPU cores.

For walltime the numbers that matter are:

    12, 48, 100 and 200 hours

noting that your jobs have the following restrictions:

    Jobs of up to 12 hours can run anywhere on the cluster.
    Jobs of up to 100 hours can run on your own nodes and the general nodes.
    Jobs of up to 200 hours can only run on your own nodes.

.. _interactive_job:
.. _interactive_session:

Interactive Jobs
================

An interactive job provides the user with a login session on a compute node with the required physical resources for the period of time requested. Interactive jobs are submitted with qsub using the -I flag. If you do not specify your resource requirements you will be given a session with 1 core and 1 Gb of memory that will last for 1 hour.

For example, the following command will provide a login session for 1 hour on a compute node on Katana with access to a single CPU core and 8Gb memory:

::

    z1234567@katana1:~$ qsub -I -l select=1:ncpus=1:mem=8gb,walltime=1:00:00
    qsub: waiting for job 1234.kman.restech.unsw.edu.au to start
    qsub: job 1234.kman.restech.unsw.edu.au ready
 
    z1234567@kc201:~$ 

If you want to know what resources you used when your job completes then you can add -M me@unsw.edu.au to your qsub command and you will receive an email when your job completes and we have:

::
 
   z1234567@katana1:~$ qsub -I -l select=1:ncpus=1:mem=8gb,walltime=1:00:00 -M me@unsw.edu.au
    qsub: waiting for job 1234.kman.restech.unsw.edu.au to start
    qsub: job 1234.kman.restech.unsw.edu.au ready
     
    z1234567@k201:~$ 

The interactive job is constrained by the resources that were requested. So the previous example would be terminated after 1 hour or if a command executed within the session consumed more than 8GB memory. The job can also be terminated by the user with CTRL-D or the logout command.

Interactive jobs can be particularly useful while developing and testing code for a future batch job, or performing an interactive analysis that requires significant compute resources. Never attempt such tasks on the head node -- submit an interactive job instead.


.. _using_keepalive:

Keeping SSH connected whilst you are running an interactive job
---------------------------------------------------------------

With all networks there is a limit to how long a connection between two computers will stay open if no data is travelling between them. This can cause problems when you are connected to Katana to run interactive jobs or even if you step away from your computer. The solution to this problem is to set the SSH keepalive variable to 60 seconds.

.. _using_tmux:

Keeping things running while you disconnect
-------------------------------------------

In order to make sure that your copy will keep running even if you are disconnected you should use the 'screen' command. To start a new screen we use the command screen -S ID so we start a new screen by typing

::

    z1234567@katana1.restech.unsw.edu.au:~$ screen -S zID

and then you can run the commands that you usually do.

At any time you can detach the screen by typing Control a then Control d and log out. When you log back in you can check your progress by typing

::

    z1234567@katana1.restech.unsw.edu.au:~$ screen -R

When you are finished with the screen you can close it in the same manner that you would use to log out.

Note: If you want to use the screen command you will need to keep track of which of the two login nodes, katana1.restech.unsw.edu.au or katana2.restech.unsw.edu.au you are connected to and return to the same login node the next time.

.. warning::
    TODO: replace this with TMUX

.. _graphical_applications:

Graphical Applications
----------------------

It is even possible to launch graphical applications from within interactive jobs, but this requires an X server on your local machine. On Linux or Mac establish an SSH session to the Katana head node with X11 forwarding enabled. For example:

:: 

    desktop:~$ ssh -X z1234567@katana.restech.unsw.edu.au

Graphical output can then be relayed from the head node to your desktop. In addition, if an interactive job is submitted with the -X flag then its graphical output will be relayed back to the desktop via the head node. For example, assuming an SSH connection to the head node with X11 forwarding enabled, the following commands will launch the MATLAB GUI, running on a compute node, but displayed on your own machine:

::

    z1234567@katana:~$ qsub -I -X
    qsub: waiting for job 1236.kman.restech.unsw.edu.au to start
    qsub: job 1236.kman.restech.unsw.edu.au ready
     
    z1234567@k201:~$ matlab

Note that X11 forwarding requires a good network connection to Katana. So this technique is only practical from machines on-campus.

If you have a Mac or Windows computer that does not have X11 installed then you will need to download and install the X2Go client. Set the host to be 'katana.restech.unsw.edu.au', the session type to 'MATE' and leave the SSH connection on port 22. If you wish to return to a X2Go session you will need to reconnect to the same server, katana1.restech.unsw.edu.au or katana2.restech.unsw.edu.au depending on the system that you connected to last time.

.. _simple_batch_jobs:

Simple Batch Jobs
=================

A batch job is a script that runs autonomously on a compute node. The script must contain the necessary sequence of commands to complete a task independently of any input from the user. This section contains information about how to create and submit a batch job on Katana.

Getting Started
---------------

The following script simply executes a pre-compiled program in the user's home directory:

::
    
    #!/bin/bash
 
    cd $HOME
 
    ./myprogram

This job can be submitted to the cluster with the qsub command. Assuming the filename of the script is myjob.pbsthen the following command will submit the job with the default resource requirements (1 CPU core for 1 hour and 1Gb of memory):

::

    z1234567@katana:~$ qsub myjob.pbs
    1237.kman.restech.unsw.edu.au

As with interactive jobs, the -l (lowercase L) flag can be used to specify resource requirements for the job:

::

    z1234567@katana:~$ qsub -l select=1:ncpus=1:mem=4gb,walltime=12:00:00 myjob.pbs
    1238.kman.restech.unsw.edu.au

Job Scripts
-----------



Job scripts offer a much more convenient method for invoking any of the options that can be passed to qsub on the command-line. In a shell script, a line starting with # is a comment and will be ignored by the shell interpreter. However, in a job script, a line starting with #PBS can be used to pass options to the qsub command.

Here is an overview of the different parts of a job script which we will examine further below.

For example, the previous job script could be rewritten as:

:: 

    #!/bin/bash
 
    #PBS -l select=1:ncpus=1:mem=4gb
    #PBS -l walltime=12:00:00
     
    cd $HOME
     
    ./myprogram


Then the script can be submitted with much less typing on the command-line:

::

    z1234567@katana:~$ qsub myjob.pbs
    1239.kman.restech.unsw.edu.au

Unlike submission of an interactive job, which results in a login session ready to accept commands, the submission of a batch job appears to simply return the ID of the new job. However, this is confirmation that the job was submitted successfully. The job is now in the hands of the job scheduler and the resource manager. Commands for checking the status of the job can be found in the Job Monitoring section.

Notifications
-------------

If you wish to be notified by email when the job finishes then use the -M flag to specify the email address and the -mflag to declare which events cause a notification.

::

    #PBS -M fred.bloggs@unsw.edu.au
    #PBS -m ae

This example will send an email if the job aborts (-m a) due to an error or ends (-m e) naturally. If required, users can also be notified when the job begins (-m b). The email sent when the job ends includes a summary of all the resources used while the job was running. This information is very useful for refining the resource requirements for future jobs.

Job Output
----------

The standard output and error streams of a batch job are redirected by the resource manager to files on the compute node where the job is running. Only when the job finishes are the output and error files transferred to the head node. By default these files will be called JOB_NAME.oJOB_ID and JOB_NAME.eJOB_ID, and they will appear in the directory that was the current working directory when the job was submitted.

You can also specify the name of the output files by using the -o and -eflags. For example the following code combines the output and error information in the file /home/z1234567/results/Output_Report once the job completes

::

    #PBS -j oe
    #PBS -o /home/z1234567/results/Output_Report

and the following commands will save standard output and standard error to 2 separate files.

::

    #PBS -e /home/z1234567/results/Error_Report
    #PBS -o /home/z1234567/results/Output_Report

If required, the output and error streams can be redirected to a single file instead of two separate files. The qsuboption -j oe will combine both streams into the standard output file.

If you want the output and error files available as soon as your job starts then you can add the qsuboption -k oed and then watch the files as they change using the tail -f command while the job is still running.

Job Directories
---------------

When a job starts, its current working directory is defined by the variable $PBS_O_INITDIR. By default the resource manager will assign the user's home directory to $PBS_O_INITDIR. So unless all your scripts and executables are stored in your home directory (not recommended!) it is very important that each job sets its current working directory appropriately. This can be achieved by changing directory at the beginning of the job script:

::

    #!/bin/bash
     
    #PBS -l nodes=1:ppn=1:mem=1gb
    #PBS -l walltime=1:00:00
    #PBS -j oe
     
    cd $HOME/projects/hardsums
     
    ./myprogram

However, if that job script was reused elsewhere then it must be updated because the working directory is hard-wired into the script. An alternative approach is to use another variable provided by the resource manager: $PBS_O_WORKDIR. By default $PBS_O_WORKDIR will be assigned the current working directory of the qsub command that launched the job. In most cases the directory from where you submit the job is exactly where you would like the job to start running. Consequently, the following script provides a more convenient and reusable method of giving the job an appropriate working directory:

::

    #!/bin/bash
     
    #PBS -l select=1:ncpus=1:mem=1gb
    #PBS -l walltime=1:00:00
    #PBS -j oe
     
    cd $TMPDIR
     
    $PBS_O_WORKDIR/myprogram



Array Jobs
==========

One common use of computational clusters is for parametric sweeps. This involves running many instances of the same application but each with different input data. Manually creating and managing large numbers of such jobs would be quite tedious. However, Torque supports the concept of array jobs which greatly simplifies the process.

An array job is a single job script that spawns many almost identical sub-jobs. The only difference between the sub-jobs is an environment variable PBS_ARRAY_INDEX whose value uniquely identifies an individual sub-job. A regular job becomes an array job when it uses the -J flag to express the required range of values for PBS_ARRAY_INDEX. For example, the following script will spawn 100 sub-jobs. Each sub-job will require one cpu core, 1GB memory and 1 hour run-time, and it will execute the same application. However, a different input file will be passed to the application within each sub-job. The first sub-job will read input data from a file called 1.dat, the second sub-job will read input data from a file called 2.dat and so on.

::

    #!/bin/bash
     
    #PBS -l select=1:ncpus=1:mem=1gb
    #PBS -l walltime=1:00:00
    #PBS -j oe
    #PBS -J 1-100
     
    cd ${PBS_O_WORKDIR}
     
    ./myprogram ${PBS_ARRAY_INDEX}.dat


If you have 2 independent parameters that you want to cycle through then there is a number of different ways to do it. The simplest way is to create an array job and then use the BASH command line to submit multiple array jobs. For example if you have data files red_1, ..., red_12, green_1, ..., green_12, blue_1, ..., blue_12, yellow_1, ..., yellow_12

::

    for MY_VAR in red green blue yellow; do export $MY_VAR; qsub array.pbs; done;

where the following file is called array.pbs. To make the variable MY_VAR usable within the job script we have added the line

::

    #PBS -v MY_VAR

to the start of the job script below.

::

    #!/bin/bash
     
    #PBS -N ARRAY4 - $MY_VAR
    #PBS -l select=1:ncpus=1:mem=1gb
    #PBS -l walltime=1:00:00
    #PBS -j oe
    #PBS -v MY_VAR
     
    #PBS -J 1-12
     
    cd $HOME
     
    ./my_prog ${MY_VAR}_${PBS_ARRAY_INDEX}

Note: If you use an array job to start more than one copy of a program then, depending on the application, you may run into problems as multiple nearly identical jobs start all at once. If this occurs you can simply add a random wait in your script by adding the following line in your script immediately before the line where the application is launched.

::

    sleep $((RANDOM % 240))

There are some more examples of array jobs including how to group your computations in an array job on the examples page.

.. warning::
    TODO: old documentation had examples here. Move all examples to github


Chaining Batch Jobs
===================

If your data processing can be split into multiple steps then rather than creating one large batch job you may want to split it up into a number of smaller jobs. Some of the reasons that you may wish to do this are:

    - Your large job runs for over 200 hours.
    - Your job has multiple steps which use different amounts of resources at each step

.. warning::
    TODO: old documentation had examples here. Move all examples to github

How to get more information from PBS
====================================

The scheduler combines multiple variables such as where the jobs can run, how many recent jobs they have run, the proportion of the system that the, the time spent waiting, resources required, etc. to figure out what job can / should run next.

If you use the following commands then you can look at the scheduler including your jobs, the current status of the nodes that your job will run on and expanded detail of the jobs currently running on those nodes. This will give you some idea of when your job might start

    **qstat** – Show all jobs on the system.
    **qstat -u $USER** – List my jobs.
    **qstat -s -u $USER** – List my jobs with current status including the reason that they haven’t started yet.
    **qstat -f JOBID* – Get details on job JOBID
    **pbsnodes** – Get information about free memory and CPU cores on all nodes. Also the JOBID of all jobs currently running on the nodes. FIgure out which nodes your group has access to
    **pbsnodes k121** - Get information about node k121.

Managing your queued jobs
=========================

After qsub the most useful job commands on Katana are qstat to get information about jobs and qdel to remove jobs from the queue. This section provides some information on those commands.

The following table gives a few of the most common commands that you may want to use.

qstat
-----

List all jobs currently queued. In the below example we can see lots of information. Most is self explanitory but the Status column ("S") shows
jobs that are in the Queue (Q), Running (R), on Hold (H) and have an Array job that has at least one subjob running (B). The array jobs have square brackets after their JOBID.

::

    [z1234567@katana2 ~]$ qstat
    Job id            Name             User              Time Use S Queue
    ----------------  ---------------- ----------------  -------- - -----
    245821.kman       s-m20-i20-200h   z1234567                 0 Q medicine200     
    276087[].kman     nl16             z1234567                 0 B simi12          
    276672[].kman     postvgpu         z1234567                 0 B simigpu48       
    279260.kman       2020-04-06.BUSC  z1234567          178:10:2 R babs200         
    280163.kman       Magcomp25A2      z1234567          1370:47: R mech700         
    290128[].kman     spod_d3t2        z1234567                 0 B qmchda100       
    305762.kman       nNGC64111_31349  z1234567                 0 H physics12       
    305798.kman       runSSTsp.sh      z1234567          00:03:40 R maths200        
    305799.kman       STDIN            z1234567                 0 Q babs12   


    -u  zID 	List only my jobs
    -S 	Expand list with a single status line for each job
    -f  JOBID 	Provide detailed information for job JOBID
    -T JOBID 	Get an estimate of when the job will start
    -x 	Include completed jobs in list

qdel
----

 	JOBID 	Remove job JOBID from the queue if it has not started and kill job if job has started.

qalter
------
    
    JOBID 	Can be adjust job options once job has been submitted. Users can only lower resource requests. If you need to increase resources, contact a systems administrator.

qselect
-------

 	-u zID 	Give me a list of my jobs. See below for some examples

To kill a single batch job is easy but it is slightly more complicated to remove an array job. In that situation you use “qdel 12345\[\]”.

If you want to remove all of your (non-array) jobs then you can use qselect and qdel together and type “qdel `qselect -u $USER`”. In the same way you can use "qstat -f `qselect -u $USER`" to get detailed information on all of your jobs.


Job Monitoring: Once your job starts
====================================

As well as providing you information about the general status of the cluster this information can be used to help build up a profile of your job as you can easily look at what resources were used.

If you want to see the status of the jobs that you have submitted then you can use the qstat command. For example

::

    qstat -u $USER

will show you the status of all of your jobs. If you type


qstat -f JOBID

to get details on how your job is going including elapsed time, CPU time and max memory usage. To find out more about available options type man qstat to see the full list of what you can see with the qstat command.

qstat

command. Typing it will show you the the jobs that are active (i.e. currently running on a compute node), idle (i.e. waiting for the required resources to become available on a node that can be used by the job) and blocked (i.e. currently being blocked from running due to the number of cores or amount of memory available to the individual or research group already being used). For more information have a look at the job scheduling and queues page.

The pbsnodes command allows you to list all the nodes of the cluster along with what jobs are running on those nodes, node memory usage, node load and if a node is able to accept more jobs. 


Monitoring Jobs Manually (Useful for Array Jobs)
------------------------------------------------

There are times when a different approach is desired or required. For example:

    Some job management commands won't work properly if you are running an array job.
    If can be useful to monitor exactly what resources your job is using at a specific time.
    You can see exactly what you job is doing.

The answer to these situations is to log into the compute node running your job and look at things there.

The steps in the process are:

    List your current jobs using the qstat -u $USER command.
    Show what node(s) the running job is using via the qstat -f JOBID command. The node(s) will have a name that looks like kXYZ where X, Y and Z are numbers.
    Log on to the compute node using ssh.
    Use the tail command to look at the output or other Linux commands.

See Exactly What is Going On
----------------------------

Once you have logged on you can also use command

::

    top

or even

::

    htop

to see what your job is currently doing. In the example below z1234567 has 16 python based jobs running which are all running at full capacity and aren't spending time waiting for other things to happen.


When your job completes
=======================

When your compute job finishes it is a good time to examine how it went and if there are changes that you should make before submitting your next job(s). In particular here are some suggestions.

# Compare the resource requirements in your job script versus what you actually used to see if you should adjust the resource requirements to bring them closer to what you actually need.

# Look at your job and see if it can be split into multiple jobs that take less than 12 hours (and greater than 1 hour)

Becuase jobs which request a WALLTIME of less that 12 hours can run on any Katana node but jobs over 12 hours can only run on nodes purchased by your school or research group or the Faculty of Science splitting jobs up into chunks that are under 12 hours will mean that your job will run sooner and faster.

# Did you run your job as a batch job?

If your job was an interactive job then you should look at running it as a batch job instead. Running jobs in batch mode allows you to submit jobs that don't require any further interaction and you can easily submit more than one job at a time.

# Can you make your job into an array job?

If you want to run many instances of the same application with different input data each time then manually creating and managing large numbers of such jobs is tedious. Instead of doing this you can create an array job which greatly simplifies the process.

# Do you have results that should be transferred to the UNSW Data Archive?

If you have results that should be transferred to the UNSW Data Archive (www.dataarchive.unsw.edu.au) which is the primary research storage facility provided by UNSW then you can use KDM, the Katana Data Mover to copy files to the LTRDS.

.. warning::
    TODO: old docs had a list of examples here - move them all to github

.. _katana_compute_faq:

FAQ
===

Katana is a blade based cluster which is available for use by members of groups who have bought in to it. The extensive information under HPC Basics are of this site combined with the Katana specific information is a good starting point for making use of Katana. The answers to some commonly asked questions about Katana is included below. 

Does Katana run a 32 bit or a 64 bit operating system?
------------------------------------------------------

Katana runs a 64 bit version of the Centos distribution of Linux.

How much memory is available per core and/or per node on Katana?
----------------------------------------------------------------

The amount of memory available varies across the cluster. To determine how much memory each node has available use the 'pbsnodes' command.

How much memory can I use on the head node on Katana for compiling software?
----------------------------------------------------------------------------

The head node has a total of 24GB of memory. Each individual user is limited to 6GB and should only be used to compile software.

Why isn't my job making it onto a node on Katana even though it says that some nodes are free?
----------------------------------------------------------------------------------------------

There are three main reasons for you to see this behavior. The first of them is specific to Katana and the other two apply to any cluster.

Firstly, the compute nodes in Katana belong to various schools and research groups across UNSW . Any job with an expected run-time longer than 12 hours can only run on a compute node that is somehow associated with the owner of the job. For example, if you are in the CCRC you are entitled to run 12+ hour jobs on the General nodes and the nodes jointly purchased by CCRC. However, you cannot run 12+ hour jobs on the nodes purchased by Astrobiology, Statistics, TARS, CEPAR or Physics. So you may see idle nodes, but you may not be entitled to run a 12+ hour job on them.

Secondly, the idle nodes may not have sufficient resources for your job. For example, there may not be sufficient cpu cores or memory available on a single compute node.

Thirdly, there may be distributed memory jobs ahead of your job in the queue which have reservations on the idle nodes, and they are just waiting for all of their requested resources to become available. In this case, your job can only use the reserved nodes if your job can finish before the nodes are required by the distributed memory job.

How many jobs can I submit at the one time?
-------------------------------------------

Technically you can submit as many jobs as you wish as the scheduling system takes into account the purchaser of the available nodes, the current load on the system, the requirements of your jobs and your usage of the cluster to determine which jobs get assigned to a node as space becomes available. In short, if you have submitted a large number of jobs you should expect that someone could come along afterwards and submit jobs that start to run ahead of some of your queued jobs.

Whilst there is not a technical limit to the number of jobs you can submit, submitting more that 2,000 jobs at the one time can place an unacceptable load on the job scheduler and your jobs may be deleted without warning.

How many cores of Katana can I use at once over all of my jobs?
---------------------------------------------------------------

The Job Scheduling and Queues page has information about the maximum number of cores that you can use at the one time.

What is the maximum number of CPUs I can use in parallel on Katana?
-------------------------------------------------------------------

If you are regularly wanting to run large parallel jobs on Katana you should consider speaking to the Faculty of Science HPC team so that they are aware of your jobs. They may be able to provide you additional assistance on resource usage for parallel jobs.

Why does my SSH connection to Katana periodically dsconnect?
------------------------------------------------------------

With all networks there is a limit to how long a connection between two computers will stay open if no data is travelling between them. More information about how to have the connection remain open is available on the cluster access page.

I used the module command but it still can't find the application that I am trying to use.
------------------------------------------------------------------------------------------

If you want your job to access an application via the module command you should include it in your job script. An easy way to check is to submit an interactive job (using qsub -I) and then run your commands and see what happens.

I put my files in my home drive (H-drive) but I can't seem to get my job to run.

The likely answer is that your files need to be in your cluster home drive and not your H-drive as your H-drive is only available on the head node and not the compute nodes. Have a look at the storage page for a discussion about the different storage locations and the copying files page for information about copying files to your cluster home drive.

Can I change the job script after it has been submitted?
--------------------------------------------------------

Yes you increase the resource values for queued jobs, but even then you are constrained by the limits of the particular queue that you are submitting to. Once it has been assigned to a node the intricacies of the scheduling policy means that it becomes impossible for anyone including the administrator to make any further changes

Where does Standard Output (STDOUT) go when a job is run?
---------------------------------------------------------

By default Standard Output is redirected to storage on the node and then transferred when the job is completed. If you are generating data you should redirect STDOUT to a different location. The best location depends on the characteristics of your job but in general all STDOUT should be redirected to local scratch.

How do I figure out what the resource requirements of my job are?
-----------------------------------------------------------------

The best way to determine what the resource requirements of your job is to run it for the first time whilst being generous with the resource requirements and then refine the requirements based on what the job actually used. If you put the following information in your job script you will receive an email when the job finishes which will include a summary of the resources used.

:: 

    #PBS -M email@unsw.edu.au 
    #PBS -m ae

How many cores should I request?
--------------------------------

Look at the email that you get when the job finishes.

Can I cause problems to other users if I request too many resources or make a mistake with my job script?
---------------------------------------------------------------------------------------------------------

No.

Will a job script from another cluster work on cluster X?
---------------------------------------------------------

It depends. Some aspects are fairly common across different clusters (e.g. walltime) others are not (e.g. select is on Tensor but not on Katana). You should look at the cluster specific information to see what queuing system is being used on that cluster and what commands you will need to change.

How can I see exactly what resources (I/O, CPU, memory and scratch) my job is currently using?
----------------------------------------------------------------------------------------------

If you run

:: 

    qstat -nru $USER

then you can see a list of your running jobs and where they are running. You can then use ssh to log on to the individual nodes and run top or dtop to see the load on the node including memory usage for each of the processes on the node. For more detailed information on the resources that your job is using, visit the page on job profiling.

What is the difference between virtual memory (VMEM or VSZ) and physical memory (MEM or RSZ)?
---------------------------------------------------------------------------------------------

Physical memory is the memory storage that is located on the physical memory sticks in the server. Swap is the memory storage that is located on the disk. Virtual memory is the entire addressable memory space combining both physical and swap memory.

Why is VMEM so large?
----------------------

With a recent update to glibc (which is used by almost every piece of software on the system) the way that virtual memory is allocated has changed. For performance reasons (to reduce the time spent waiting for memory allocation locks) virtual memory is now set aside for each thread. This means, for example, that a 400mb job with 16 threads may require 1024mb of virtual memory equating to 64mb per thread.

Depending on your job you may want to either increase your VMEM request or revert to something close to the previous behaviour depending on which provides your specific job better performance using:

::

    export MALLOC_ARENA_MAX=1

How do I choose which version of software I use?
------------------------------------------------

To select a specific version of a piece of software you can use the module command. This allow you to choose between different installed versions of software.

How do I request the installation or upgrade of a piece of software ?
---------------------------------------------------------------------

If you wish to have a new piece of software installed or software that is already installed upgraded please send an email to the UNSW IT Servicedesk (servicedesk@unsw.edu.au) from your UNSW email account with details of what software change you require and the cluster that you would like it changed on.

Why is my job stuck in the queue whilst other jobs run?
-------------------------------------------------------

The queues are not set up to be first-in-first-out. In fact all of the queued jobs sit in one big pool of jobs that are ready to run. The scheduler assigns priorities to jobs in the pool and the job with the highest priority is the next one to run. The length of time spent waiting in the pool is just one of several factors that are used to determine priority.

For example, people who have used the cluster heavily over the last two weeks receive a negative contribution to their jobs' priority, whereas a light user will receive a positive contribution. You can see this in action with the diagnose -p and diagnose -f commands.

You mentioned waiting time as a factor, what else affects the job priority?
---------------------------------------------------------------------------

The following three factors combine to generate the job priority.

    # How many resources (cpu and memory) have you and your group consumed in the last 14 days? Your personal consumption is weighted more highly than your group's consumption. Heavy recent usage contributes a negative priority. Light recent usage contributes a positive priority.
    # How many resources does the job require? Always a positive contribution to priority, but increases linearly with the amount of cpu and memory requested, i.e. we like big jobs.
    # How long has the job been waiting in the queue? Always a positive contribution to priority, but increases linearly with the amount of time your job has been waiting in the queue. Note that throttling policies will prevent some jobs from being considered for scheduling, in which case their clock does not start ticking until that throttling constraint is lifted.

What happens if my job uses more memory than I requested?
---------------------------------------------------------

If your job uses more memory than you requested then the carefully balanced node assignments decided by the job scheduler cease to be valid and it may cause issues on the active node. For example the extra memory you use becomes unavailable to another job that thinks that it is there to use so active memory will be swapped to the local disk and the node will slow to a crawl. To avoid this any job that uses more memory than requested will be terminated by the scheduler.

What happens if my job is still running when it reaches the end of the time that I have requested?
--------------------------------------------------------------------------------------------------

In order to ensure that everyone has equitable access to computational resources and to aid the efficient scheduling of jobs the cluster assigns to your job an amount of time matching your request. When that time is exhausted your job is automatically terminated.

200 hours is not long enough! What can I do?
--------------------------------------------

If you find that your jobs take longer than the maximum WALL time then there are several different options to change your code so that it fits inside the parameters.

    - Can your job be split into several independent jobs?
    
    - Can you export the results to a file which can then be used as input for the next time the job is run?

You may want to also look to see if there is anything that you can do to make your code run better like making better use of local scratch if your code is I/O intensive.

Do sub-jobs within an array job run in parallel, or do they queue up serially?
------------------------------------------------------------------------------

Submitting an array job with 100 sub-jobs is equivalent to submitting 100 individual jobs. So if sufficient resources are available then all 100 sub-jobs could run in parallel. Otherwise some sub-jobs will run and other sub-jobs must wait in the queue for resources to become available.

The '%' option in the array request offers the ability to self impose a limit on the number of concurrently running sub-jobs. Also, if you need to impose an order on when the jobs are run then the 'depend' attribute can help.

In a pbs file does the VMEM requested refer to each node or the total memory on all nodes being used (if I am using more than 1 node?
-------------------------------------------------------------------------------------------------------------------------------------

VMEM refers to the amount of memory per node.

