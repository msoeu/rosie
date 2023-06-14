## Configuring Matlab to Submit Batch Jobs to Rosie

Rosie is currently running MatLab Parallel Server, version R2022a.
While it is possible to [run Matlab desktop directly](./desktop), you
can also submit batch jobs from your desktop Matlab.  For this guide,
I will assume you are running Matlab on your own computer on the MSOE
network.

Before you can submit batch jobs from your desktop Matlab to Rosie,
you must perform some one-time setup.

1. Rosie is running MatLab Parallel Server, version R2022a.  You
   should also be running R2022a on your machine.  See
   [here](https://helpdesk.msoe.edu/support/solutions/articles/1000036642-matlab-desktop-getting-started)
   for help installing MatLab locally.  **However,** you should
   also select and install MatLab Parallel Toolbox as part of the
   installation.  (It can be installed later.)

2. Now go to "Add-Ons" and, in the Add-On Explorer, search for
   "Parallel Computing Toolbox plugin for MATLAB Parallel Server with
   Slurm" Install this plugin.  It will attempt to run a wizard to
   configure settings after installing; cancel out, as you will be
   using the provided [configuration file] (./Rosie.mlsettings)

   If you didn't install Parallel Computing Toolbox in
   the previous step, you should install it now as an add-on, as well.

1. Download the [cluster configuration file](./Rosie.mlsettings)


2. In the "Parallel" settings of Matlab, select "Create and Manage
      Clusters..." to bring up the Cluster Profile Manager.

3. In the profile manager, import the configuration file you just
      downloaded. **Do not** validate the profile: the profile notes
      that there are potentially 1,440 worker threads; a job
      *using* that many will require sole access to Rosie, and be
      forced to wait indefinitely.

4. In the lower-right, click "Edit" to edit the profile.  In the
      Additional Properties" of the Scheduler Plugin, change **both**
      occurrences of `USERNAME` to your Rosie account name.   

   If you are using a Macintosh or have installed MATLAB in a
   non-standard location on your Windows machine, you will also
   need to edit the PluginScriptLocation to the appropriate
   directory.
   
5. Use "Rename" to name the cluster "Rosie."  This is not required, but
      all further examples assume your configuration is named "Rosie"

Once your MatLab desktop is configured correctly, you can use it to
[submit batch jobs to Rosie](./batch).