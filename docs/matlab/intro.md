## MATLAB on Rosie, an Introduction

MATLAB is a commonly used platform for numeric computing and simple
programming. Currently, Rosie has MATLAB r2022a installed.  MATLAB
jobs can be run in different ways on the cluster.

* A job can be run using the [MATLAB desktop](matlab/desktop.md), generally
  through the web portal.
* A job can be run "headless" using the command line (not documented here).
* Rosie can be used as a worker pool for [batch jobs](matlab/batch.md)
  submitted from a desktop running MATLAB Parallel Toolbox

For either of the first two, you must have a license for the
particular node of the cluster that your job is running on.  Batch
jobs do not require an individual license.

### MATLAB Licensing

MSOE has a university-wide license for MATLAB.  However, each MSOE
user must obtain a separate license for each computer that they use to
run MATLAB.  Because Rosie is a cluster, MATLAB may run on different
nodes (computers) each time.  Therefore, you may need to acquire a new
license when starting a job (if it isn't on a node for which you
already have a license).

If you do not have a license, you can activate it by running the
activate script (`/opt/matlab/R2022a/bin/activate/activate_matlab.sh`
in the container, or `/data/bin/matlab/R2022a/bin/activate_matlab.sh`
if running directly on the cluster). The script will prompt you to
activate the license interactively, by logging into your
(MSOE-affiliated) MathWorks account, or manually.  For a manual
installation, you must first download and install the license, then
activate it. The MATLAB app in OpenOnDemand should run the activation
script automatically if necessary.

### Manual License File

Once you know which node you're on, you need to generate a MATLAB license file (unless you previously generated one for this particular node).

You should complete these steps directly on your laptop using Chrome. This does not need to be done on Rosie.

These instructions are adapted from [this MathWorks Central post](https://www.mathworks.com/matlabcentral/answers/235126-how-do-i-generate-a-matlab-license-file#answer_190013).

1. Go to the [License Center](https://www.mathworks.com/licensecenter/licenses)
1. Log into (or create) your MathWorks Account with your @msoe.edu address if you are not already logged in
1. Select your license number from the list. If you don't see your license, use the links in the upper left-hand corner to toggle between Licenses, Trials, and Prereleases
1. Select the "Install and Activate" tab
1. Click the "Activate to Retrieve License File" link under "Related Tasks" on the right side of the page.
1. On the page titled "Activated Computers," click the blue "Activate a Computer" button on the right side of the page.
1. Enter the required information to complete activation. Release = R2022a, Operating System = Linux
1. For the Host ID, enter the MAC address of your assigned node. `/sbin/ifconfig eth0` on your assigned Rosie node. Copy the 6-byte address that shows up after `ether`
1. For the Computer Login Name, enter your MSOE username (e.g., doej)
1. For the Activation Label, make it correspond your node number, e.g., "R2022a Rosie dh-nodeNN" to aid in finding it later
1. MathWorks will verify your MSOE account through MSOE SSO. This sometimes generates an error message when attempting to open the SSO URL. If this happens, copy the entire (very long) URL from the error message and open it in a new tab. You should be told that verification was successful (you may need to enter your MSOE username and password if not already authenticated). At this point, you should be able to go back to the MathWorks window and finish downloading your license file.
1. Select "Yes" when asked "Is the software installed?"
1. Download the license file. Recommended naming convention: license-username-node.lic, e.g., license-durant-dh-node18.lic
1. Upload to Rosie. Recommended method is selecting Files, Home Directory from your [Rosie Dashboard](https://dh-ood.hpc.msoe.edu/pun/sys/dashboard), but you could also use a SecureFTP program connected to one of the management nodes.

Once uploaded, you can activate the license using the script:
1. Choose "Activate manually without the Internet", then "Next"
1. "Enter the full path...", "Browse"
1. Point MATLAB to the license file you downloaded from MathWorks and
   uploaded to Rosie. Click "Next"
1. After receiving the success message, click "Finish" to exit.