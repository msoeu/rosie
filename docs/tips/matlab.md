## Deep Learning Toolbox

### Rosie VNC Desktop in local browser (recommended method)

1. [Launch interactive desktop on compute node using VNC.](https://dh-ood.hpc.msoe.edu/pun/sys/dashboard/batch_connect/sys/rosie_vnc_desktop/session_contexts/new) Use your username *without* @msoe.edu to log in.
1. Select 1 GPU and the number of hours you need. Select "Launch."
1. It may take a minute to prepare your session. Select "Launch Rosie VNC Desktop" when given the option.
1. Open a terminal window on the remote desktop. You can do this by clicking the footprint icon in the upper left and searching for "term." Select "MATE Terminal."
1. See procedure below to get a license file for MATLAB.
1. `singularity exec --nv /data/containers/matlab-r2022a.sif bash`
1. `matlab&`
   * If you receive an error that the license for a different computer is installed, `/opt/matlab/R2022a/bin/activate_matlab.sh` to manually start the activation process, which will allow you to select a different license file.
1. Chose "Activate manually without the Internet", "Next", "Enter the full path...", "Browse" and then point MATLAB to the license file you downloaded from MathWorks and uploaded to Rosie. Click "Next," receive success message, and click "Finish" to exit.
1. `matlab&`
1. The MATLAB GUI appears and you have access to:
   * [Deep Learning Toolbox](https://www.mathworks.com/help/deeplearning/getting-started-with-deep-learning-toolbox.html)
   * Signal Processing Toolbox
   * Statistics and Machine Learning Toolbox
   * Image Processing Toolbox
   * and much more

### Alternate method (X11 forwarding)

This method forwards the GUI calls from your node on Rosie, through a Rosie management node, back to your local computer. It requires an X Server such as [VcXsrv for Windows](https://sourceforge.net/projects/vcxsrv/). MATLAB licensing is handled as above. The key steps follow.

1. ssh into a Rosie management node (dh-mgmtN.hpc.msoe.edu, where N is between 1 and 4.).
   * Use -X (capital), or the equivalent option in your ssh client, to enable X11 forwarding.
   * Use -C (capital), or the equivalent option in your ssh client, to enable compression.
1. `srun --time=04:00:00 --pty --partition=teaching --gres=gpu:t4:1 --x11 singularity exec --nv /data/containers/matlab-r2022a.sif bash`, set to more than 4 hours if needed
1. `matlab&`

## Get MATLAB license file

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
