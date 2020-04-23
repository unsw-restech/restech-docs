.. _software:

.. contents::
   :depth: 1 
   :local:
   :backlinks: top 

=================
Operating Systems
=================

Katana nodes are running either RedHat 7.7 (management plane) or CentOS 7.7 (compute nodes). 

Research software is installed in modules so that they can be loaded or unloaded as necessary. This way we can offer and run multiple versions of each package at the same time.

The sections below provide information about some of the specific software that is installed on Katana.

===================
Environment Modules
===================

Environment Modules offer a simple means of customising your environment to access the required versions of installed software and this section provides information on how they are used on Katana.

When we use modules, we are changing our "environment", hence the name. 


How do I discover what software is available?
---------------------------------------------

::
 
    [z1234567@katana ~]$ module avail 
 
    -------------------------- /share/apps/modules/intel ---------------------------
    intel/11.1.080(default)  intel/12.1.7.367  intel/13.0.1.117  intel/13.1.0.146
 
    --------------------------- /share/apps/modules/pgi ----------------------------
    pgi/13.7
 
    -------------------------- /share/apps/modules/matlab --------------------------
    matlab/2007b          matlab/2010b          matlab/2012a(default)
    matlab/2008b          matlab/2011a          matlab/2012b
    matlab/2009b          matlab/2011b          matlab/2013a


What if the software that I want is not on the list?
----------------------------------------------------

If you require software installed on the cluster, email the `IT Service Centre <ITServiceCentre@unsw.edu.au>`_ detailing the software that you would like installed and that you would like to have it installed on Katana. Please include links and desired version numbers.

How do I add a particular version of software to my environment?
----------------------------------------------------------------

:: 
    
    [z1234567@katana1 ~]$ module add matlab/2018b

or

:: 
    
    [z1234567@katana1 ~]$ module load matlab/2018b


How do I remove a particular version of software from my environment?
---------------------------------------------------------------------

::

    [z1234567@katana1 ~]$ module rm matlab/2018b

or
    
::
    
    [z1234567@katana1 ~]$ module unload matlab/2018b


How do I remove all modules from my environment?
------------------------------------------------

::

    [z1234567@katana1 ~]$ module purge

Which versions of software am I currently using?
------------------------------------------------

::

    [z1234567@katana1 ~]$ module list
    Currently Loaded Modulefiles:
     1) intel/18.0.1.163   2) matlab/2018b

How do I find out more about a particular piece of software?
------------------------------------------------------------

You can find out more about a piece of software by using the module help command. For example:

::

    [z1234567@katana1 ~]# module help mrbayes
     
    ----------- Module Specific Help for 'mrbayes/3.2.2' --------------
     
    MrBayes 3.2.2 is installed in /apps/mrbayes/3.2.2
     
    This module was complied against beagle/2.1.2 and openmpi/1.6.4 with MPI support.
     
    More information about the commands made available by this module is available
    at http://mrbayes.sourceforge.net

How do I switch between particular versions of software?
--------------------------------------------------------

::

    [z1234567@katana1 ~]$ module switch matlab/2018b matlab/2017b

How can I find out what paths and other environment variables a module uses?
----------------------------------------------------------------------------

::

    [z1234567@katana1 ~]$ module show mothur/1.42.3
    -------------------------------------------------------------------
    /apps/modules/bio/mothur/1.42.3:

    module-whatis     Mothur 1.42.3 
    conflict     mothur 
    setenv         MOTHUR_ROOT /apps/mothur/1.42.3 
    prepend-path     PATH /apps/mothur/1.42.3/bin 
    setenv         LAST_MODULE_TYPE bio 
    setenv         LAST_MODULE_NAME mothur/1.42.3 
    setenv         LAST_MODULE_VERSION 1.42.3 
    -------------------------------------------------------------------


Why does the cluster forget my choice of modules?
-------------------------------------------------

Environment modules only affect the particular session in which they are loaded. Loading a module in one SSH session will not affect any other SSH session or even any jobs submitted from that session. Modules must be loaded in every session where they will be used.


How can I invoke my module commands automatically?
--------------------------------------------------

The best way of doing this is to add your Module commands to your job scripts. This approach is useful for preserving the required environment for each job. For example:a

::

    #!/bin/bash
 
    #PBS -l nodes=1:ppn=1
    #PBS -l vmem=4gb
    #PBS -j oe
     
    module purge
    module add intel/18.0.1.163
     
    cd ${PBS_O_WORKDIR}
     
    ./myprog


.. _notable_softwares:

Perl, Python and R all have their own library/module systems - CPAN_, PyPI_ and CRAN_. If a library or module you want from one of these sources isn't installed in the module, please email us at `IT Service Desk <ITServiceCentre@unsw.edu.au?subject=Katana Software Install>`_

===========
Biosciences
===========

Bioconductor, BioPerl, BioPython, Blast+, Mothur are all installed.

.. _intel_compilers_and_libraries:

======================================
Intel Compilers and Software Libraries
======================================

Research Technology Services has a licence for Intel Compiler Collection which can be accessed by loading a module and contains 3 groups of software, namely compilers, libraries and a debugger. This software has been optimised by Intel to take advantage of the specific capabilities of the different intel CPUs installed in the Intel based clusters.

- Compilers
    - Intel C Compiler (icc)
    - Intel C++ Compiler (icpc)
    - Intel Fortran Compiler (ifort)
- Libraries
    - Intel Math Kernel Library (MKL)
    - Intel Threading Building Blocks (TBB)
    - Intel Integrated Performance Primitives (IPP)
- Debugger
    - Intel Debugger (idbc)

.. _java:

====
Java
====

Java is installed as part of the Operating System but we would strongly recommend against using that version - we cannot guarantee scientific reproducibility with that version. Please use the java modules. 

Each Java module sets 

::
    
    _JAVA_TOOL_OPTIONS -Xmx1g

This sets the heap memory to 1GB. If you need more, set the environment variable :code:`_JAVA_OPTIONS` which overrides :code:`_JAVA_TOOL_OPTIONS`

::

    export _JAVA_OPTIONS -Xmx5g

.. _perl:

====
Perl
====

The default version of Perl on Katana is 5.16.3 which is provided by CentOS 7 and can be found at :code:`/usr/bin/perl`.

This is an older version of Perl. We have Perl 5.28.0 installed as a module. 

It is common for perl scripts to begin with 

::

    #!/usr/bin/perl

If you are using the Perl module, you will need to change the first line to 

::

    #!/usr/bin/env perl

.. _sas:

===
SAS
===

The 64-bit version of SAS is available as a module.

By default SAS will store temporary files in :code:`/tmp` which can easily fill up leaving the node offline. In order to avoid this we have set the default to :code:`$TMPDIR` to save temporary files in :code:`/var/tmp` on the Katana head node and local scratch on compute nodes. If you wish to save temporary files to a different location you can do that by using the :code:`-work` flag with your SAS command or adding this line to your :code:`sasv9.cfg` file:

::

    -work /my/directory

=====
Stata
=====

Stata is availt as a module. 

When using Stata in a pbs batch script, the syntax is

::

    stata -b do StataClusterWorkshop.do

If you wish to load or install additional Stata modules or commands you should use findit command on your local computer to find the command that you are looking for. Then create a directory called :code:`myadofiles` in your home directory and copy the .ado (and possibly the .hlp) file into that directory. Now that the command is there it just remains to tell Stata to look in that directory which can be done by using the following Stata command.

::

    sysdir set PERSONAL $HOME/myadofiles


.. _tmux:

====
TMUX
====

tmux_ is available on Katana. It can be used to keep a session alive despite the terminal losing connectivity.

When you login to Katana using the terminal, it is a "live" session - if you close the terminal, the session will also close. If you shut your laptop or turn off the network, you will also kill the session.

This is fine, except when you have a long running program - say you are downloading a large data set but you need to leave campus.

In these situations, you can use tmux to create an interruptible session. tmux has other powerful features - multiple sessions similtaneously that can be easily switched between being what makes it a popular package.

To start tmux, type :code:`tmux` at the terminal. A new session will start and there will be a green information band at the bottom of the screen. 

Anything you start in this session will keep running even if you are disconnected from that session for what ever reason.

If you do get detached, you can re-attach by logging into the same server and using the command :code:`tmux a`

tmux is similar to another program called :code:`screen` which is also available. 


.. _CPAN: https://www.cpan.org/
.. _PyPI: https://pypi.org/
.. _CRAN: https://cran.r-project.org/
.. _tmux: https://github.com/tmux/tmux/wiki
