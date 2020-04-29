.. _about_katana:

============
About Katana
============

.. toctree::
   :maxdepth: 1
   :hidden:
   :glob:

   accessing_katana
   running_jobs



Katana is a shared computational cluster located on campus at UNSW that has been designed to provide easy access to computational resources. With over 4000 CPU cores spread over a large number of compute nodes each with up to 1Tb of memory, Katana provides a flexible compute environment where users can run jobs that wouldn't be possible or practical on their desktop or laptop. For full details of the compute nodes including a full list see the compute node information section below.

Katana can provide a perfect training or development area before moving on to a more powerful system such as Australia's peak HPC system Gadi_, located at NCI_ with the knowledge that has been gained. It can also be the perfect spot if you are uncertain if High Performance Computing is for you or with local support, node mix, long job runtimes and a wide range of software (with a special focus on the biosciences) it may be the perfect location for your research pipeline.

.. _system_configuration:

System Configuration
--------------------

- rpm based Linux OSes. RedHat on the management plane, CentOS on the nodes.
- PBSPro_ version 19.1.3
- Large global scratch at /srv/scratch, local scratch at $TMPDIR
- 12,24,48,100,200 hour queues with prioritisation


.. _compute_resources:

Compute
-------

- hetergenous hardware: Dell, Lenovo, Huawei


.. _gpu_resources:

GPU Compute
-----------

- four GPU capable nodes, Tesla V100-SXM2, 32GB
- three are dedicate
- one is general use

.. _Gadi: https://nci.org.au/our-systems/hpc-systems
.. _NCI: https://nci.org.au/
.. _PBSPro: https://www.pbspro.org/
