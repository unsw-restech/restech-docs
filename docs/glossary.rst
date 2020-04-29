.. _glossary:

========
Glossary
========

.. _def_active_job:

Active Job 
----------

Active jobs are jobs that have been assigned to a compute node and are currently running. 
These can be seen by running :code:`qstat` and looking for an A in the second last column. See :ref:`more_info_from_pbs`

.. _def_array_job:

Array Job
---------

If you want to run the same job multiple times with slight differences (filenames, data source, etc), then you can create an array job which will submit multiple jobs for you from the one job script. 

.. _def_batch_job:

Batch Job
---------

A batch job is a job on a cluster that runs without any further input once it has been submitted. Almost all jobs on the cluster are batch jobs. All jobs are either batch jobs or :ref:`def_interactive_jobs`.

.. _def_blade:

Blade 
-----

Some of the compute nodes Katana are called blade servers which allow a higher density of servers in the same space. Each blade consists of multiple CPUs with 6 or more cores.


.. _def_cluster:

Cluster
-------

A computer cluster is a set of connected computers that work together so that, in many respects, they can be viewed as a single system. Using a cluster is referred to as High Performance Computing or HPC. Most will have a :ref:`def_mangement_plane` and several :ref:`def_compute_nodes`.

.. _def_compute_nodes:

Compute Nodes
-------------

The compute nodes are where the compute jobs run. Users submit jobs from the :ref:`def_login_node` and the :ref:`def_job_scheduler` on the :ref:`def_head_node` will assign the job to one or more compute nodes.

.. _def_cpu_code:

CPU Core
--------

Each node in the cluster has one or more CPUs each of which has 6 or more cores. Each core is able to run one job at a time so a node with 12 cores could have 12 jobs running in parallel.

.. _def_data_transfer_node:

Data Transfer Node
------------------

The Data Transfer Node, also known as the :ref:`katana_data_mover` (KDM), is a server that is used for transferring files to, from, and within the cluster. Due to the nature of moving data around, it uses a significant amount of memory and network bandwidth. This server is used to take that load off the :ref:`def_login_node`.

.. _def_environment_variables:

Environment Variables 
---------------------

Environment variables are variables that are set in Linux to tell applications where to find programs and set program options. They will start with a $ symbol. For example, all users can reference :code:`$TMPDIR` in their :ref:`def_job_script` in order to use :ref:`def_local_scratch`

.. _def_global_scratch:

Global Scratch 
--------------

Global scratch is a large data store for data that isn't backed up. It differs from local scratch in that it is available from every node including the :ref:`def_head_node`. If you have data files or working directories this is where you should put them.

.. _def_head_node:

Head Node
---------

The head node of the :ref:`def_cluster` is the computer that manages job and resource management. This is where the :ref:`def_job_scheduler` and :ref:`def_resource_manager` run. It is kept separate from the :ref:`def_login_node` so that production doesn't stop if someone accidentally breaks the :ref:`def_login_node`.

.. _def_held_jobs:

Held Jobs
---------

Held jobs are jobs that cannot currently run. They are put into that state by either the server or the system administrator. Jobs stay held until released by a systems administrator, at which point they become :ref:`def_queued_jobs`. 
These can be seen by running :code:`qstat` and looking for an H in the second last column. See :ref:`more_info_from_pbs`

.. _def_interactive_jobs:

Interactive Jobs 
----------------

An interactive job is a way of testing your program and data on a cluster without negatively impacting the :ref:`def_login_node`. Once a request has been submitted and accepted for an interactive job, the user will no longer be on the relatively small login nodes, and will have access to the resources requested on the :ref:`def_compute_nodes`. In other words, your terminal session will move from a small (virtual) computer you share with many people to a large computer you share with very few people. All jobs are either :ref:`def_batch_jobs` or interactive jobs.  

.. _def_job_scheduler:

Job Scheduler
-------------

The job scheduler monitors the jobs currenty running on the cluster and assigns :ref:`def_queued_jobs` to :ref:`def_compute_nodes` based on recent cluster useage, job resource requirements and nodes available to the research group of the submitter. In summary the job scheduler determines when and where a job should run. The job scheduler that we use is called PBSPro.

.. _def_job_script:

Job Script
----------

A job script is a file containing all of the information needed to run a :ref:`def_batch_job` including the resource requirements and the actual commands to run the job.

.. _def_local_scratch:

Local Scratch 
-------------

Local scratch refers to the storage available internally on each compute node. Of all the different scratch directories this storage has the best performance however you will need to move your data into local scratch as part of your job script. You can use local scratch with the :ref:`def_environment_variable` :code:`$TMPDIR`

.. _def_login_node:

Login Node
----------

The login nodes of the cluster is the computer that you log in to when you connect to the cluster. This node is used to compile software and submit jobs.

.. _def_module:

Module
------

The module command is a means of providing access to different versions of software without risking version conflicts across multiple users.

.. _def_management_plane:

Management Plane
----------------

The Management Plane is the set of servers that sit above or adjacent to the :ref:`def_compute_node`. These servers are used to manage the system, manage the storage, or manage the network. User's have access to the :ref:`def_login_node` and :ref:`def_data_transfer_node`. Other servers include the :ref:`def_head_node`. 

.. _def_mpi:

MPI
---

Message Passing Infrastructure (MPI) is a technology for running :ref:`def_compute_jobs` on more than :ref:`def_compute_node`. Designed for situations where parts of the job can run on independent nodes with the results being transferred to other nodes for the next part of the job to be run.

.. _def_network_drive:

Network Drive 
-------------

A network drive is a drive that is independant from the cluster. 

.. _def_queued_jobs:

Queued Jobs 
-----------

Queued jobs are eligible to run but are waiting for a :ref:`def_compute_node` that matches their requirements to become available. Which idle job will be assigned to a compute node next depends on the :ref:`def_job_scheduler`.
These can be seen by running :code:`qstat` and looking for a Q in the second last column. See :ref:`more_info_from_pbs`

.. _def_resource_manager:

Resource Manager 
----------------

A resource manager works with the :ref:`def_job_scheduler` to manage running jobs on a cluster. Amongst other tasks it receives and parses job submissions, starts jobs on :ref:`def_compute_nodes`, monitors jobs, kills jobs, and manages how many :ref:`def_cpu_cores` are available on each :ref:`def_compute_node`

.. _def_scratch_space:

Scratch Space 
-------------

Scratch space is a non backed up storage area where users can store transient data. It should not be used for job code as it is not backed up.
