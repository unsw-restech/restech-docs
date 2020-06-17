.. _storage:

########################
Katana Storage Locations
########################

The storage on Katana is split into several different types, each of which serves a different purpose. 

.. important::
    We have just said *each of which serves a different purpose*. Despite that, there will be overlap. And personal preference. In most cases it will be obvious where to put your information. If it isn't and you need help with your decision making, you can `email <rds@unsw.edu.au>`_ the `Research Data <https://research.unsw.edu.au/research-data-management-unsw>`_ team for advice. They are friendly people.


******************
Cluster Home Drive
******************

Location
========

.. code-block:: bash

    /home/z1234567

Also known as
=============

.. code-block:: bash

    $HOME

Attributes
==========

- Backed up
- 10Gb limit
- By default only user has access
- Able to be used everywhere on Katana

Best used for
=============

Source code and programs

**************
Global Scratch
**************

Location
========

.. code-block:: bash

    /srv/scratch/z1234567

Also known as
=============

.. code-block:: bash

    /srv/scratch/$USER

Attributes
==========

- Not available to users in STUDENT groups such as ESTUDENT.
- Not backed up
- Generally 16Tb shared between multiple users
- By default only user has access 
- Able to be used everywhere on Katana

Best used for
=============

Data files


**************
Shared Scratch
**************

Only available to groups of users that have requested it - primarily for teams that share data sets or results.

Location
========

.. code-block:: bash

    /srv/scratch/name

Attributes
==========

- Not backed up
- Spaced based on group requirements
- All users in the group have access 
- Able to be used everywhere on Katana

Best used for
=============

Shared data files and copies of programs


*************
Local Scratch
*************

Found on the node on which your job is running. 

Location
========

The location is created by the job scheduler as part of initialising the running of the job.

Also known as
=============

.. code-block:: bash

    $TMPDIR

Attributes
==========

- Not backed up
- Only exists whilst job is running
- 200Gb shared between node users
- Storage located on compute node so good for compute

Best used for
=============

Much fast completion of jobs that require large datasets to be near the CPU, calculations and temp files.

*********************
UNSW Research Storage
*********************

Location
========

.. code-block:: bash

    /home/z1234567/sharename

Also known as
=============

.. code-block:: bash

    $HOME/sharename

Attributes
==========

- Backed up
- Only available on Katana head node

Best used for
=============

Shared and user data files.

#################
Katana Data Mover
#################

Also known as kdm.

If you have data that you would like to copy to or within the Katana cluster, archive or even compress and decompress you should use the Katana Data Mover - also known as the KDM server - rather than using the head node. This section contains instructions on how to use KDM server.

If you are familiar with using Linux commands to copy or move files then you can do that directly by logging on to :code:`kdm.restech.unsw.edu.au` via :code:`ssh` in the same way that you would log in to Katana and then use the :code:`cp`, :code:`mv` and :code:`rsync` commands that you would normally use under Linux.

If you are not familiar with using the Linux command line for moving or copying files then the easiest way to move files around is to use client software such as FileZilla_. Once you have connected to :code:`kdm.restech.unsw.edu.au` using your zID and zPass you should see a remote view which corresponds to the files sitting on Katana. You can then use the FileZilla interface to move files and folders around.

.. note::
    We require people to "move data" through the data mover. We have hundreds of users, most of whom have data ranging from very large to impossibly large. This is why we have the KDM. If you are transferring a couple of small text files - job scripts for instance - you can copy directly to the Katana. But we would ask you to keep it to a minimum, and nothing bigger than 2-3 MB.

***********************************
Copying Files To and From a Cluster
***********************************

The method of transferring files to and from clusters depends on your local machine. If you are a Linux user then you should use rsync and if you are a Windows user then you should download and install WinSCP_ or FileZilla_

.. _using_filezilla:

Filezilla
=========

Once you have installed Filezilla you can go into the site manager and create a new site in the site manager using the settings below.

.. image:: _static/filezilla.png

You can also use the Quick Connect bar as shown here: 

.. image:: _static/filezillaquick.png


From my computer to Katana Home
===============================

To copy the directory /home/1234567/my-directory from your local computer to Katana scratch

.. code-block:: bash

    me@localhost:~$ rsync -avh /path/to/my/directory z1234567@kdm.restech.unsw.edu.au:

From my computer to Katana Scratch
==================================

.. code-block:: bash

    me@localhost:~$ rsync -avh /path/to/my/directory z1234567@kdm.restech.unsw.edu.au:/srv/scratch/z1234567


From Katana to my computer
==========================

First, you need to make sure the data is in either your Home directory or your scratch 

If the data is in :code:`/home/z1234567/my-remote-results` and you want it in your home directory:

.. code-block:: bash

    me@localhost:~$ rsync -avh z1234567@kdm.restech.unsw.edu.au:my-remote-results .

If the data is in :code:`/srv/scratch/my-remote-results` and you want it in your home directory:

.. code-block:: bash

    me@localhost:~$ rsync -avh z1234567@kdm.restech.unsw.edu.au:/srv/scratch/my-remote-results .

.. note::
    :ref:`TMUX` is available if your data is large and the rsync might take a long time.


:ref:`TMUX`

################################
How to use the UNSW Data Archive
################################

The UNSW Data Archive is the primary research storage facility provided by UNSW. The Data Archive gives UNSW researchers a free, safe and secure storage service to store and access research data well beyond the life of the project that collected that data.

To help researchers make use of this system the Katana Data Mover has a script that you can use to copy files from Katana into a project on the Data Archive system.

.. note::
    To use this script you must have access to the UNSW Data Archive which requires setting up a `Research Data Management Plan <https://research.unsw.edu.au/research-data-management-unsw>`_.

.. note::
    You cannot use the data archive via Filezilla or WinSCP - you will need to use the command line.

To see what versions of the Data Archive script are available log on to :code:`kdm.science.unsw.edu.au` and type

.. code-block:: bash

    module avail unswdataarchive

Use the help command for usage

.. code-block:: bash

    module help unswdataarchive/2020-03-19

.. warning::
    This advice has poor results. The help file is too long for most screen sizes and there's no pagination in modules version < 4. Last line should include a location that the researcher can read directly (using less)

*************
Initial Setup
*************

To use the Data Archive you need to set up a configuration file. Here's how to create the generic config in the directory you are in:

::

    [z1234567@kdm ~]$ module add unswdataarchive/2020-03-19
    [z1234567@kdm ~]$ get-config-file


Before you use the script for the first time you will need to generate a token for uploading data to the archive. To generate a token send an email to the `IT Service Centre <ITServiceCentre@unsw.edu.au>`_ asking for a Data Archive token to be generated. 

Then edit the configuration file :code:`config.cfg` and to change the line that looks like :code:`token=`

If you haven't generated a token you can also upload content using your zID and zPass by adding the following line to the file :code:`config.cfg` and you will be asked for your zPass when you start the upload.

.. code-block:: bash

    user=z1234567

************************
Starting a data transfer
************************

To get data **into** the archive, we use :code:`upload.sh`

.. code-block:: bash

    upload.sh /path/to/your/local/directory /UNSW_RDS/D0000000/your/collection/name


To get data **from** the archive, we use :code:`download.sh`

.. code-block:: bash

    download.sh /UNSW_RDS/D0000000/your/collection/name /path/to/your/local/directory

.. _Filezilla: https://filezilla-project.org/
.. _WinSCP: https://winscp.net/eng/download.php
