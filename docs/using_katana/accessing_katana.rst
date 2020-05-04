.. _accessing_katana:

================
Accessing Katana
================

Anyone at UNSW can apply for an entry level account on Katana. This is designed for those groups that are thinking that Katana would suit their research needs or will typically use less than 10,000 CPU hours a quarter. Those at this level still get access to the same level of support including software installation and help getting starting running their jobs. The only difference is the number of compute jobs that can be run at any time and how long they can run for.

Some Faculties, Schools and Research Groups have invested in Katana and have a higher level of access. Users in this situation should speak to their supervisor.

.. _requesting_an_account:

Requesting an Account
=====================

To apply for an account you can send an email to the `UNSW IT Service Centre <ITServiceCentre@unsw.edu.au>`_ giving your zID, your role within UNSW and the name of your supervisor or head of your research group.

.. _connecting_to_katana:

Connecting to Katana
====================

.. note:: 
    When you are connecting to Katana via :code:`katana.restech.unsw.edu.au` you are connecting to one of two login nodes :code:`katana1.restech.unsw.edu.au` and :code:`katana2.restech.unsw.edu.au`. If you have a long running :ref:`tmux_session` session open, you will need to login to the node on which it was started.

Linux and Mac
-------------

From a Linux or Mac OS machine this can be done as follows:

::

  desktop:~$ ssh z1234567@katana.restech.unsw.edu.au

Windows
-------

From a Windows machine a SSH client such as PuTTY_ or MobaXTerm_ is required. 

.. _graphical_session:

Graphical sessions
------------------

Some software - :ref:`Matlab`, :ref:`r` and :ref:`jupyter_notebooks` being among the most popular - are easier with a graphical session. If you require an interactive graphical session to Katana then you can use the X2Go_ client.

If you have connected from a Linux machine (or a Mac with X11 support via X11.app or XQuartz) then connecting via SSH will allow you to open graphical applications from the command line. To run these programs you should start an interactive job on one of the compute nodes so that none of the computational processing takes place on the head node.

Start X2Go and create a session for Katana. The details that you need to enter for the session are:

:: 

    Session name: Katana
    Host: katana.restech.unsw.edu.au
    Login: zID
    Session type: Mate

.. image:: ../_static/x2go.png

.. note:: 
    If you use X2Go from a Mac then you may get the following errors:

    ::

        SSH daemon failed to open the application's public host key.
        Connection failed Cannot open file -

    This happens because of missing SSH key files on the Mac client. To force the Mac to generate these keys log in over SSH from a Windows computer using PuTTY (or Linux computer using SSH) which will generate the missing SSH key files.

.. danger::
    TODO: Lachlan thinks this advice is incorrectly worded. I haven't checked but I presume the files need to be created on Katana during a regular ssh login process, and are from then available to be read (on the login node) by the Mac x2go client.

.. warning:: 
    The usability of a graphical connection to Katana is highly dependent on network latency and performance.

.. _Putty: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
.. _MobaXTerm: https://mobaxterm.mobatek.net/
.. _X2Go: http://wiki.x2go.org/doku.php
