.. _faqs:

====
FAQS
====

Frequently Asked Questions
==========================

# TODO: These are rubbish. They aren't questions to start with. Fix, remix

There are a number of things that you can do to make the most of Katana and this section lists some of them as well as some of the questions that have been asked regarding Katana in the past.

How do I get help?

The best way to get help or ask a question of the UNSW Research Technology Solutions team is to email the UNSW IT Servicedesk (ITServiceCentre@unsw.edu.au). It is not a good idea to contact a member of the team directly as that person may be on leave or may not be the best person to deal with your request.

When you send your email, please write a clear and detailed description of exactly what you are seeing (including full error messages — use copy-and-paste, not screenshots unless graphics are involved). Something like "It doesn't work" doesn't help us help you! If at all possible, include the exact sequence of steps someone else needs to do to reproduce the problem, the job identifier, the exact date and time of your problem and on which Katana node it occurred, the script filename and the directory you were running from.
Keep your jobs under 12 hours if possible

If you request more than 12 hours of WALLTIME then you can only use the nodes bought by your school or research group, or the Faculty of Science. Keeping your job's run time request under 12 hours means that it can run on any node in the cluster.
Two 10 hour jobs will probably finish sooner that one 20 hour job

In fact, if there is spare capacity on Katana, which there is most of the time, six 10 hours jobs will finish before a single 20 hour job will.
Requesting more resources for your job decreases the places that the job can run

The most obvious example is going over the 12 hour limit which limits the number of compute nodes that your job can run on but it is worth . For example specifying the CPU in your job script restricts you to the nodes with that CPU. A job that requests 20Gb will run on a 128Gb node with a 100Gb job already running but a 30Gb job will not be able to.
Running your jobs interactively makes it hard to manage multiple concurrent jobs

If you are currently only running jobs interactively then you should move to batch jobs which allow you to submit more jobs which then start, run and finish automatically.
If you have multiple batch jobs that are almost identical then you should consider using array jobs

If your batch jobs are the same except for a change in file name or another variable then you should have a look at using array jobs.
If you want to use software that is not already installed

Look at the software page to find out about the module command as it may just need to be loaded. If it is not already installed send an email through to the IT Service Centre (ITServiceCentre@unsw.edu.au) asking for it to be installed.
Where is the best place to store my code?

The best place to store source code is to use a version control server.  This means that you will be able to keep every version of your code and revert to an earlier version if you require.
I just got some money from a grant. What can I spend it on?

There are a number of different options for using research funding to improve your ability to run computationally intensive programs. The best starting point is to speak to the Research Technology Services team to figure out the different options.
Can I access Katana from outside UNSW?

If you have an account then you can connect to the clusters from anywhere. If you are using Windows then download a SSH client such as PuTTY and then connect directly to the cluster. More details above.


Expanding Katana
================
Katana has significant potential for further expansion. It offers a simple and cost-effective way for research groups to invest in a powerful computing facility and take advantage of the economies that come with joining a system with existing infrastructure. A sophisticated job scheduler ensures that users always receive a fair share of the compute resources that is at least commensurate with their research group’s investment in the cluster. For more information please contact us.

Acknowledging Katana
====================
If you use Katana for calculations that result in a publication then you should add the following text to your work.

This research includes computations using the computational cluster Katana supported by Research Technology Services at UNSW Sydney.

For additional attribution information expand this section.

If you are using nodes that have been purchased using an external funding source you should also acknowledge the source of those funds.

For information about acknowledging ARC funding see http://www.arc.gov.au/about_arc/acknowledgementform.htm

Your School or Research Group may also have policies for compute nodes that they have purchased.
Facilities external to UNSW

If you are using facilities at Intersect and NCI in addition to Katana they may also require some form of acknowledgement:

In particular you should look at http://www.intersect.org.au/attribution-policy and http://nf.nci.org.au/policies/nf_usage_policy.php.


