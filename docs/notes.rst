From running_jobs_on_katana

If you want to know what resources you used when your job completes then you can add -M me@unsw.edu.au to your qsub command and you will receive an email when your job completes and we have:

::
 
   z1234567@katana1:~$ qsub -I -l select=1:ncpus=1:mem=8gb,walltime=1:00:00 -M me@unsw.edu.au
    qsub: waiting for job 1234.kman.restech.unsw.edu.au to start
    qsub: job 1234.kman.restech.unsw.edu.au ready
     
    z1234567@k201:~$ 


=========================
TODO: REPLACE with FAQ?
=========================
.. _using_keepalive:

Keeping SSH connected whilst you are running an interactive job
---------------------------------------------------------------

With all networks there is a limit to how long a connection between two computers will stay open if no data is travelling between them. This can cause problems when you are connected to Katana to run interactive jobs or even if you step away from your computer. The solution to this problem is to set the SSH keepalive variable to 60 seconds.

.. _using_tmux:

Keeping things running while you disconnect
-------------------------------------------

=========================
END TODO
=========================

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

Explain more:

qsub conf:
Mail -b
----
If you want the output and error files available as soon as your job starts then you can add the qsuboption -k oed and then watch the files as they change using the tail -f command while the job is still running.
---
and the following commands will save standard output and standard error to 2 separate files.

#PBS -e /home/z1234567/results/Error_Report
#PBS -o /home/z1234567/results/Output_Report

Array Jobs

Couldn't make this work
::

    #PBS -v MY_VAR
    #PBS -N ARRAY4 - $MY_VAR     

to the start of the job below. The first is necessary for making the variable available to the job. The second line changes the name of the job in the queue so you can tell them apart. The jobs will be called 


REMOVED ALL THIS - didn't work. Does now, see below


If you have two independent parameters that you want to cycle through then there is a number of different ways to do it. The simplest way is to create an array job and then use the :code:`bash` command line to submit multiple array jobs. For example if you have data files :code:`red_1, ..., red_12, green_1, ..., green_12, blue_1, ..., blue_12, yellow_1, ..., yellow_12`

.. code-block:: bash
   :caption: array.sh 

    for MY_VAR in red green blue yellow 
    do 
        export $MY_VAR; qsub -V array.pbs 
    done

To make the variable :code:`$MY_VAR` usable within the job script :code:`array.pbs` you need to supply the :code:`-V` flag to qsub. 

.. code-block:: bash

    #!/bin/bash

    #PBS -l select=1:ncpus=1:mem=1gb
    #PBS -l walltime=1:00:00
    #PBS -j oe

    #PBS -v MY_VAR
    #PBS -N ARRAY4 - $MY_VAR     
    #PBS -J 1-12
     
    cd $HOME
     
    ./my_prog ${MY_VAR}_${PBS_ARRAY_INDEX}

Note: If you use an array job to start more than one copy of a program then, depending on the application, you may run into problems as multiple nearly identical jobs start all at once. If this occurs you can simply add a random wait in your script by adding the following line in your script immediately before the line where the application is launched.

::

    sleep $((RANDOM % 240))


Here's how to make it work

for MY_VAR in green yellow blue red; do qsub -v var_col=$MY_VAR test.pbs; done

#!/bin/bash

#PBS -M lachlan.simpson@unsw.edu.au
#PBS -m ae
# PBS -v MY_VAR
#PBS -J 1-2

echo "MY_VAR is $MY_VAR"
echo "var_col is $var_col"
echo ${PBS_ARRAY_INDEX}
Removed from Monitoring jobs

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


