## MATLAB Desktop

There are three methods to run a MATLAB desktop on Rosie:
1. Using the MATLAB app in the [portal](https://dh-ood.hpc.msoe.edu)
1. Using a local browser and the VNC Desktop App.
1. Using X11 forwarding

### Rosie MATLAB app

1. [Launch MATLAB app on compute node](https://dh-ood.hpc.msoe.edu/pun/sys/dashboard/batch_connect/sys/rosie_matlab/session_contexts/new)
1. If you need to activate a license, you should be prompted to do so at the start of the session.

### Rosie VNC Desktop in local browser

1. [Launch interactive desktop on compute node using VNC.](https://dh-ood.hpc.msoe.edu/pun/sys/dashboard/batch_connect/sys/rosie_vnc_desktop/session_contexts/new) Use your username *without* @msoe.edu to log in.
1. Select 1 GPU and the number of hours you need. Select "Launch."
1. It may take a minute to prepare your session. Select "Launch Rosie VNC Desktop" when given the option.
1. Open a terminal window on the remote desktop. You can do this by clicking the footprint icon in the upper left and searching for "term." Select "MATE Terminal."
1. If you are manually activating the license, [get the license file](intro?id=manual-license-file) now.
1. `singularity exec --nv /data/containers/matlab-r2022a.sif bash`
1. `matlab&`
   * If you receive an error that the license for a different computer is installed, [run the license activation script](intro.md?id=matlab-licensing)
   * `matlab&`

### Alternate method (X11 forwarding)

This method forwards the GUI calls from your node on Rosie, through a Rosie management node, back to your local computer. It requires an X Server such as [VcXsrv for Windows](https://sourceforge.net/projects/vcxsrv/). MATLAB licensing is handled as before. The key steps follow.

1. ssh into a Rosie management node (dh-mgmtN.hpc.msoe.edu, where N is between 1 and 4.).
   * Use -X (capital), or the equivalent option in your ssh client, to enable X11 forwarding.
   * Use -C (capital), or the equivalent option in your ssh client, to enable compression.
1. `srun --time=04:00:00 --pty --partition=teaching --gres=gpu:t4:1 --x11 singularity exec --nv /data/containers/matlab-r2022a.sif bash`, set to more than 4 hours if needed
1. `matlab&`
