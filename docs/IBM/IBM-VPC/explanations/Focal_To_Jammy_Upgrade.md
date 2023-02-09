# Upgrading from Focal to Jammy on IBM VPC

## General Advice
When deciding how to upgrade the main consideration is whether your system can be setup/deployed with automation or requires manual configuration.

Running [`do-release-upgrade`](https://manpages.ubuntu.com/manpages/focal/man8/do-release-upgrade.8.html) requires some manual intervention ([see Manual upgrade steps](#manual-upgrade-steps)), which makes it a good option for systems which require manual configuration and cannot be easily created or destroyed.

For system deployments which are fully automated it is recommended to redeploy with new Jammy instances instead of upgrading from Focal.

## Manual upgrade steps
If you are upgrading from Focal to Jammy you can expect to run into the following prompts requesting manual input.

### Additional SSH daemon
When upgrading in a session over SSH there is an inherent risk of losing access if something goes wrong with the SSH daemon. To mitigate this risk an additional SSH daemon is started on a different port as a backup.
![A warning regarding the risk of upgrading in a session over SSH. The prompt is notifying the user that an additional SSH daemon will be started to mitigate the risk. The user is prompted whether they would like to continue or cancel the upgrade.](Focal_To_Jammy_Upgrade_Images/0_additional_ssh_daemon.png)

### Update sources.list
Since the IMB VPC Focal image is configured to use internal mirrors by default, the `sources.list` entries will likely need to be updated from 'focal' to 'jammy'. Confirm 'Y' on the prompt to automatically update the `sources.list` entires
![A prompt asking to update source.list file entries from focal to jammy.  Information is provided that needs to be done when running an interal mirror.  The options are Yes or No.](Focal_To_Jammy_Upgrade_Images/1_sources_list.png)

### Start upgrade
A final prompt before starting the upgrade. Information regarding the amount of changes and estimated time are provided because once you start the upgrade process it cannot be cancelled.
![A prompt asking if the user would like to start the upgrade process. Some information is provided regarding the amount of changes and estimated time to complete. The user is prompted to continue, cancel or see additional details.](Focal_To_Jammy_Upgrade_Images/2_start_upgrade.png)

### Restart services automatically
Some services need to be restarted when certain libraries are upgraded. The user has the option to allow the system to automatically restart these services or to be asked after every library upgrade which services they want to be restarted.
![A prompt asking the user if they would like services to be restarted automatically during package upgrades. If no is selected there will be prompts later for which services to restart on each library upgrade.](Focal_To_Jammy_Upgrade_Images/3_restart_services.png)


### SSHD configuration modified
Canonical makes changes to `/etc/ssh/sshd_config` for IBM VPC images. As a result when upgrading there will be a prompt asking whether to keep this version, use the default in the newer version or take some other action.
![A prompt notifying the user that there is a newer version available of the sshd_config file. Options include keeping the local version, using the default and a couple other actions.](Focal_To_Jammy_Upgrade_Images/4_sshd_modified_config.png)


### Restart to finish upgrade
A restart will be necessary for some parts of the upgrade to be applied. If you select no to the restart you can check `/var/run/reboot-required.pkgs` to see which things need a reboot to be applied.
![A prompt asking the user to restart the system because it is required to complete the upgrade.](Focal_To_Jammy_Upgrade_Images/5_finish_upgrade.png)