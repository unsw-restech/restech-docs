.. _storage:


.. contents::
   :depth: 1 
   :local:
   :backlinks: top 

========================
Katana Storage Locations
========================

The storage on Katana is split into several different types, each of which serves a different purpose. 

.. important::
    We have just said *each of which serves a different purpose*. Despite that, there will be overlap. And personal preference. In most cases it will be obvious where to put your information. If it isn't and you need help with your decision making, you can `email <rds@unsw.edu.au>`_ the `Research Data <https://research.unsw.edu.au/research-data-management-unsw>`_ team for advice. They are friendly people.



Cluster Home Drive
==================

Location
--------

::

    /home/z1234567

Also known as
-------------

::

    $HOME

Attributes
----------

    - Backed up
    - 10Gb limit
    - By default only user has access
    - Able to be used everywhere on Katana

Best used for
-------------

Source code and programs

Global Scratch
==============

Location
--------

::

    /srv/scratch/z1234567

Also known as
-------------

::

    /srv/scratch/$USER

Attributes
----------

    - Not available to users in STUDENT groups such as ESTUDENT.
    - Not backed up
    - Generally 16Tb shared between multiple users
    - By default only user has access 
    - Able to be used everywhere on Katana

Best used for
-------------

Data files


Shared Scratch
==============

Only available to groups of users that have requested it - primarily for teams that share data sets or results.

Location
--------

::

    /srv/scratch/name

Attributes
----------

    - Not backed up
    - Spaced based on group requirements
    - All users in the group have access 
    - Able to be used everywhere on Katana

Best used for
-------------

Shared data files and copies of programs


Local Scratch
=============

Found on the node on which your job is running. 

Location
--------

The location is created by the job scheduler as part of initialising the running of the job.

Also known as
-------------

::

    $TMPDIR

Attributes
----------

    - Not backed up
    - Only exists whilst job is running
    - 200Gb shared between node users
    - Storage located on compute node so good for compute

Best used for
-------------

Much fast completion of jobs that require large datasets to be near the CPU, calculations and temp files.

UNSW Research Storage
=====================

Location
--------

::

    /home/z1234567/sharename

Also known as
-------------

::

    $HOME/sharename

Attributes
----------

    - Backed up
    - Only available on Katana head node

Best used for
-------------

Shared and user data files.

UNSW Home Drive
===============

.. warning::
    TODO: doesn't exist from what I can tell


.. _katana_data_mover:

=================
Katana Data Mover
=================

Also known as kdm.

If you have data that you would like to copy to or within the Katana cluster, archive or even compress and decompress you should use the Katana Data Mover - also known as the KDM server - rather than using the head node. This section contains instructions on how to use KDM server.

If you are familiar with using Linux commands to copy or move files then you can do that directly by logging on to :code:`kdm.restech.unsw.edu.au` via :code:`ssh` in the same way that you would log in to Katana and then use the :code:`cp`, :code:`mv` and :code:`rsync` commands that you would normally use under Linux.

If you are not familiar with using the Linux command line for moving or copying files then the easiest way to move files around is to use client software such as FileZilla_. Once you have connected to :code:`kdm.restech.unsw.edu.au` using your zID and zPass you should see a remote view which corresponds to the files sitting on Katana. You can then use the FileZilla interface to move files and folders around.

.. note::
    We require people to "move data" through the data mover. We have hundreds of users, most of whom have data ranging from very large to impossibly large. This is why we have the KDM. If you are transferring a couple of small text files - job scripts for instance - you can copy directly to the Katana. But we would ask you to keep it to a minimum, and nothing bigger than 2-3 MB.

Copying Files To and From a Cluster
===================================

The method of transferring files to and from clusters depends on your local machine. If you are a Linux user then you should use rsync and if you are a Windows user then you should download and install WinSCP_ or FileZilla_

.. _using_filezilla:

Filezilla
---------

Once you have installed Filezilla you can go into the site manager and create a new site in the site manager using the settings below.

.. image:: _static/filezilla.png

You can also use the Quick Connect bar as shown here: 

.. image:: _static/filezillaquick.png


From my computer to Katana Home
-------------------------------

To copy the directory /home/1234567/my-directory from your local computer to Katana scratch

::

    me@localhost:~$ rsync -avh /path/to/my/directory z1234567@kdm.restech.unsw.edu.au:

From my computer to Katana Scratch
----------------------------------

::

    me@localhost:~$ rsync -avh /path/to/my/directory z1234567@kdm.restech.unsw.edu.au:/srv/scratch/z1234567


From Katana to my computer
--------------------------

First, you need to make sure the data is in either your Home directory or your scratch 

If the data is in :code:`/home/z1234567/my-remote-results` and you want it in your home directory:

::

    me@localhost:~$ rsync -avh z1234567@kdm.restech.unsw.edu.au:my-remote-results .

If the data is in :code:`/srv/scratch/my-remote-results` and you want it in your home directory:

::

    me@localhost:~$ rsync -avh z1234567@kdm.restech.unsw.edu.au:/srv/scratch/my-remote-results .

.. warning::
    TODO: old docs have a heading here about rsync which turns into a screen tutorial. Scrapped. Make a tmux tutorial somewhere sensible (software) and point at that.
    Do not need another rsync. If I'm wrong, put it in software

================================
How to use the UNSW Data Archive
================================

The UNSW Data Archive is the primary research storage facility provided by UNSW. The Data Archive gives UNSW researchers a free, safe and secure storage service to store and access research data well beyond the life of the project that collected that data.

To help researchers make use of this system the Katana Data Mover has a script that you can use to copy files from Katana into a project on the Data Archive system.

.. note::
    To use this script you must have access to the UNSW Data Archive which requires setting up a `Research Data Management Plan <https://research.unsw.edu.au/research-data-management-unsw>`_.

.. note::
    You cannot use the data archive via Filezilla or WinSCP - you will need to use the command line.

To see what versions of the Data Archive script are available log on to :code:`kdm.science.unsw.edu.au` and type

::

    module avail unswdataarchive

Use the help command for usage

::

    module help unswdataarchive/2020-03-19

.. warning::
    This advice has poor results. The help file is too long for most screen sizes and there's no pagination in modules version < 4. Last line should include a location that the researcher can read directly (using less)

Initial Setup
=============

To use the Data Archive you need to set up a configuration file. Here's how to create the generic config in the directory you are in:

::

    [z1234567@kdm ~]$ module add unswdataarchive/2020-03-19
    [z1234567@kdm ~]$ get-config-file


Before you use the script for the first time you will need to generate a token for uploading data to the archive. To generate a token send an email to the `IT Service Centre <ITServiceCentre@unsw.edu.au>`_ asking for a Data Archive token to be generated. 

Then edit the configuration file :code:`config.cfg` and to change the line that looks like :code:`token=`

If you haven't generated a token you can also upload content using your zID and zPass by adding the following line to the file :code:`config.cfg` and you will be asked for your zPass when you start the upload.

::

    user=z1234567

Starting a data transfer
========================

To get data **into** the archive, we use :code:`upload.sh`

::

    upload.sh /path/to/your/local/directory /UNSW_RDS/D0000000/your/collection/name


To get data **from** the archive, we use :code:`download.sh`

::

    download.sh /UNSW_RDS/D0000000/your/collection/name /path/to/your/local/directory

.. _storage_faq:

===========
Storage FAQ
===========

What storage is available to me?
================================

Katana provides three different storage areas, cluster home drives, local scratch and global scratch. The storage page has additional information on the differences and advantages of each of the different types of storage. You may also want to consider storing your code using a version control seryive like GitHub. This means that you will be able to keep every version of your code and revert to an earlier version if you require.

Which storage is fastest?
=========================

In order of performance the best storage to use is local scratch, global scratch and cluster home drive.

Is any of the cluster based storage backed up?
==============================================

The only cluster based storage that gets backed up is the cluster home drives. All other storage including local and global scratch is not backed up.

How do I actually use local scratch?
====================================

The easiest way of making use of local scratch is to use scripts to copy files to the node at the start of your job and from the node when your job finishes. You should also use local scratch for your working directory and temporary files.

Why am I having trouble creating a symbolic link?
=================================================

Not all filesystems support symbolic links. The most common examples are some Windows network shares. On Katana this includes Windows network shares such as hdrive. The target of the symbolic link can be within such a filesystem, but the link itself must be on a filesystem that supports symbolic links, e.g. the rest of your home directory or your scratch directory. 

What is the Disk Usage message that I get when I log on to a cluster?
=====================================================================

When you log on to Katana a command is run to display how much space you currently have available in the different file systems.

How do I get access to my UNSW Home drive when I log on to a cluster?
=====================================================================

When you log on to kdm.restech.unsw.edu.edu you can run the network command to mount your UNSW Home drive.

What storage is available on compute nodes?
===========================================

As well as local scratch, global scratch and your cluster home drives are accessible on the compute nodes of the clusters.

What is the best way to transfer a large amount of data onto a cluster?
=======================================================================

Use :code:`rsync` to copy data to the KDM server. More information is above.

Is there any way of connecting my own file storage to one of the clusters?
==========================================================================

Whilst it is not possible to connect individual drives to any of the clusters, some units and research groups have purchased large capacity storage units which are co-located with the clusters. This storage is then available on the cluster nodes. For more information please contact the Research Technology Service Team by placing a request with the `IT Service Centre <ITServiceCentre@unsw.edu.au>`_.

Can I specify how much file storage I want on local scratch?
============================================================

If you want to specify the minimum amount of space on the drive before your job will be assigned to a node then you can use the file option in your job script. Unfortunately setting up more complicated file requirements is currently problematic.

Can I run a program directly from scratch or my home drive after logging in to the cluster rather submitting a job?
===================================================================================================================

As the file server does not have any computational resources you would be running the job from the head node on the cluster. If you need to enter information when running your job then you should start an interactive job.

Where does Standard Output (STDOUT) go when a job is run?
=========================================================

By default Standard Output is redirected to storage on the node and then transferred when the job is completed. If you are generating data you should redirect :code:`STDOUT` to a different location. The best location depends on the characteristics of your job but in general all :code:`STDOUT` should be redirected to local scratch.



.. _Filezilla: https://filezilla-project.org/
.. _WinSCP: https://winscp.net/eng/download.php
