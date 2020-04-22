.. _software:

========
Software
========

.. contents::
   :depth: 1 
   :local:
   :backlinks: top 

.. toctree::
   :maxdepth: 1
   :hidden:
   :glob:

   intel_compilers_and_libraries
   java
   perl
   python
   python-virtualenvs
   python-jupyter-notebooks
   r   


Katana nodes are running either RedHat 7.7 (management plane) or CentOS 7.7 (compute nodes). 

Research software is installed in modules so that they can be loaded or unloaded as necessary. This way we can offer and run multiple versions of each package at the same time.

If you require software installed on the cluster then the best way to do so is to email the IT Service Centre (ITServiceCentre@unsw.edu.au) detailing the software that you would like installed and mention that you would like it to be installed on Katana.

The sections below provide information about some of the specific software that is installed on Katana.

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


What can I do if the software that I want to use is not on the list?
--------------------------------------------------------------------

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

Notable Software List
=====================

Perl, Python and R all have their own library/module systems - CPAN_, PyPI_ and CRAN_. If a library or module you want from one of these sources isn't installed in the module, please email us at `IT Service Desk <ITServiceCentre@unsw.edu.au?subject=Katana Software Install>`_

:ref:`intel_compilers_and_libraries`

:ref:`Java`

:ref:`Python`

    - :ref:`python_virtual_environments`

    - :ref:`jupyter_notebooks`

:ref:`Perl`

:ref:`R`

Bioscience
----------

Bioconductor, BioPerl, BioPython, Blast+, Mothur


.. _CPAN: https://www.cpan.org/
.. _PyPI: https://pypi.org/
.. _CRAN: https://cran.r-project.org/
