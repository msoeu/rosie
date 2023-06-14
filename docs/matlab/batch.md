## Running MatLab Batch jobs on Rosie

The `batch` function runs a job
remotely on Rosie from within your local copy of Matlab.  Before doing this,
you must [configure MatLab](matlab/batch_config.md) appropriately.

The general recipe for this is to

1. Create a cluster object for Rosie
3. Launch the job
4. Wait for it to complete
5. Access the results.

A simple example of a job submission script would then be:

    cluster = parcluster("Rosie");         % step 1
    job = batch(cluster, "myFunction", 1); % step 2: we expect 1 output
    wait(job)                              % step 3
    outputs = fetchOutputs(job);           % step 4
    % print, plot, or otherwise use outputs{1}

Let's look at each step in a little more detail.

### The cluster object

Here we reference the cluster configuration file imported when
configuring MatLab.  We can also specify SLURM job submit arguments as
additional properties of the cluster object.

    cluster = parcluster("Rosie");
    cluster.AdditionalProperties.AdditionalSubmitArgs = '--partition=batch';

### What a job looks like

The `batch` function submits a job to the specified cluster.  A job
can be an expression, a module or a function.  Here we restrict
ourselves to jobs which are functions; although the entry function may
call others.

For a simple example, let's assume we have the module `mywave_fcn.m`,
which has the following contents:

    function A = mywave()
        % A parfor loop uses parallel workers if available
        parfor i = 1:10000000
            A(i) = sin(i*2*pi/2500000)
        end
    end

### Submitting a job

We can submit a job executing the above function as follows

    job = batch(cluster,'mywave_fcn', 1, 'pool', 4, 'AutoAddClientPath', false);

The arguments can be understood as follows:
- `cluster` is the handle for Rosie (as above)
- `'mywave_fcn'` is the file containing the function to run on Rosie
- `1` is the number of values returned from the function
- The remaining arguments are optional; they come in pairs and are
  used to specify additional job parameters.  Here,  
  - `'pool', 4` says to use 4 of Rosie's CPUs to run 4 workers,
    dividing the work.  Really, however, this will use 5 CPUs, as
    one is used to coordinate the others.
  - `'AutoAddClientPath', false` directs MatLab to not copy paths
    from your local machine to the workers on Rosie.  Since Rosie
    has a different file structure, copied paths would make no
    sense.    
  - Additional paired arguments are possible, such as
    `'AttachedFiles'` for files you want to transfer from your local
    machine to Rosie.  See
    [here](https://www.mathworks.com/help/parallel-computing/batch.html)
    for more examples.


### Wait for the job to finish

If the job is likely to finish in a reasonable amount of time, your
script can just wait for it

    wait(job);

Alternately, you can at a later time access the job from the Job
Monitor ("Parallel -> Monitor Jobs").  You can even shut down Matlab
and return at a later time.  After loading the job, you have the same
access as if you'd waited for it.

### Access the results

Once the job is done, we can access the results from the job handle:

    outputs = fetchOutputs(job);

This gives back cells with the return values from the function. They
may be used as you see fit.

    plot(outputs{1});

