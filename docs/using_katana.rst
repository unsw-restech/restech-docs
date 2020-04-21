.. _using_katana:

============
Using Katana
============


.. _about_katana:

About Katana
============

Katana is a shared computational cluster located on campus at UNSW that has been designed to provide easy access to computational resources. With over 3000 CPU cores spread over a large number of compute nodes each with up to 1Tb of memory, Katana provides a flexible compute environment where users can run jobs that wouldn't be possible or practical on their desktop or laptop. For full details of the compute nodes including a full list see the compute node information section below.

Katana can provide a perfect training or development area before moving on to a more powerful system such as Australia's peak HPC system Raijin located at the National Computing Infrastructure (NCI) with the knowledge that has been gained. It can also be the perfect spot if you are uncertain if High Performance Computing is for you or with local support, node mix, long job runtimes and a wide range of software (with a special focus on the biosciences) it may be the perfect location for your research pipeline.

.. _system_configuration:

System Configuration
--------------------

.. _compute_resources:

Compute
-------

.. _gpu_resources:

GPU Compute
-----------

.. _access:

Access
======

Anyone at UNSW can apply for an entry level account on Katana. This is designed for those groups that are thinking that Katana would suit their research needs or will typically use less than 10,000 CPU hours a quarter. Those at this level still get access to the same level of support including software installation and help getting starting running their jobs. The only difference is the number of compute jobs that can be run at any time and how long they can run for.

Some Faculties, Schools and Research Groups have invested in Katana and have a higher level of access. Users in this situation should speak to their supervisor.

.. _requesting_an_account:

Requesting an Account
=====================

To apply for an account you can send an email to the `UNSW IT Service Centre <ITServiceCentre@unsw.edu.au>`__ giving your zID, your role within UNSW and the name of your supervisor or head of your research group.


Connecting to Katana
====================

Once you have an account on Katana then you can log on to it using the instructions in this section or by following the instructions you receive in your introductory email.

Note: When you are connecting to Katana via katana.restech.unsw.edu.au you are connecting to one of two login nodes katana1.restech.unsw.edu.au and katana2.restech.unsw.edu.au. If it is important that you connect to the same login node each time then you should change katana.restech.unsw.edu.au for one of those addresses in the instructions below.

Linux and Mac
-------------

From a Linux or Mac OS machine this can be done as follows:

::

  desktop:~$ ssh z1234567@katana.restech.unsw.edu.au

Windows
-------

From a Windows machine a SSH client such as PuTTY_ or MobaXTerm_ is required. 

.. _Putty: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
.. _MobaXTerm: https://mobaxterm.mobatek.net/
