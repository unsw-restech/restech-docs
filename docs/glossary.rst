.. _glossary:

========
Glossary
========

.. _def_cluster:

Cluster
-------

A High Performance Computer composed of multiple computers where jobs are farmed out from the login or head node.

.. _def_head_node:

Head Node
---------

The head node of the cluster is the computer that you log in to when you connect to the cluster. This node is used to compile software and submit jobs.

.. _def_storage_node:

Storage Node 
-------------

To reduce the load on the head node cluster home directories and global scratch are attached to a storage node which is then accessible from anywhere in the cluster. This means that when compute jobs are run they can talk directly to the storage node without consuming any resources on the head node.

.. _def_data_transfer_node:

Data Transfer Node
------------------

The Data Transfer Node also known as the Katana Data Mover (KDM) is a server that is used for transferring files to, from and within the cluster. By using KDM, the network traffic is offloaded from the Katana Head Node.

.. _def_compute_node:

Compute Node
------------

The compute nodes are where the compute jobs run. Jobs are submitted from the head node and assigned to compute nodes by the job scheduler.

.. _def_blade:

Blade 
-----

Some of the compute nodes Katana are called blade servers which allow a higher density of servers in the same space. Each blade consists of multiple CPUs with 6 or more cores.

.. _def_cpu_code:

CPU Core
--------

Each node in the cluster has one or more CPUs each of which has 6 or more cores. Each core is able to run one job at a time so a node with 12 cores could have 12 jobs running in parallel.

.. _def_mpi:

MPI
---

MPI (Message Passing Infrastructure) is a technology for running compute jobs on more than node. Designed for situations where parts of the job can run on independent nodes with the results being transferred to other nodes for the next part of the job to be run.

.. _def_module:

Module
------

The module command is a means of providing access to different versions of software without risking version conflicts.

.. _def_job_script:

Job Script
----------

A job script is a file containing all of the information needed to run a job including the resource requirements and the actual commands to run the job.

.. _def_job_scheduler:

Job Scheduler
-------------

The job scheduler monitors the jobs currenty running on the cluster and assigns waiting jobs to nodes based on recent cluster useage, job resource requirements and nodes available to the research group of the submitter. In summary the job scheduler determines when and where a job should run. The job scheduler that we use is called Maui.

.. _def_resource_manager:

Resource Manager 
----------------

A resource manager does everything to do with running job on a cluster apart from scheduling them. Amongst other tasks it receives and parses job submissions, starts jobs on compute nodes, monitors jobs, kills jobs, transfers files, etc. The resource manager that we use is called Torque and it is based on an older resource manager called PBS.

.. _def_scratch_space:

Scratch Space 
-------------

Scratch space is a non backed up storage area where users can store transient data. It should not be used for job code as it is not backed up.

.. _def_local_scratch:

Local Scratch 
-------------

Local scratch refers to the storage available internally on each compute node. Of all the different scratch directories this storage has the best performance however you will need to move your data into local scratch as part of your job script. You can use local scratch with the environment variable %TMPDIR%.

.. _def_global_scratch:

Global Scratch 
--------------

Global scratch differs from local scratch in that it is available from every node including the head node. If you have data files or working directories this is where you should put them.

.. _def_network_drive:

Network Drive 
-------------

A network drive is a drive that is independant from the cluster. In our case the UNSW "H-Drive", the CCRC drives and some other shared drives are available by running the "network" command. Jobs should NEVER be run directly of the H-Drive for performance and reliability reasons.

.. _def_interactive_job:

Interactive Job 
---------------

An interactive job is a way of testing your program and data on a cluster. You request a terminal on one of the nodes and then load and run files from the command line. Once you have your job working well interactively you should turn it into a batch job.

.. _def_batch_job:

Batch Job
---------

A batch job is a job on a cluster that runs without any further input once it has been submitted. Almost all jobs on the cluster are batch jobs.

.. _def_array_job:

Array Job
---------

If you want to run the same job multiple times with only a handful of variables (filename, etc.) changing then you can create an array job which will submit multiple jobs for you from the one job script.

.. _def_environment_variables:

Environment Variables 
---------------------

Environment variables are variables that are set in Linux to tell applications where to find programs and set program options.

.. _def_active_job:

Active Job 
----------

When you look at the job list using showq active jobs are jobs that have been assigned to a compute node and are currently running.

.. _def_idle_job:

Idle Job 
--------

When you look at the job list using showq idle jobs are eligible to run but are waiting for a compute node that matches their requirements to become available. Which idle job will be assigned to a compute node next depends on the scheduler.

.. _def_blocked_job:

Blocked Job 
-----------

When you look at the job list using showq blocked jobs are jobs that cannot currently run due to a policy limitation on the system such as a restriction on the number of cores that can be used by the same person. Jobs stay blocked until the limit is no longer exceeded at which point the job will be reclassified as an idle job and will then wait for the scheduler to assign it to a compute node.

