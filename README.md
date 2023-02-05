# SUSE
Interview homework

Subject: Request for RCA

Reason:

As our communication with customers is mainly done via email, as part of the recruitment process we would like to evaluate written communication.
The expected outcome from this exercise is for you to prepare a Root Cause Analysis (RCA) for a system crash.
It would ideally include a basic health check of the system, the reasons for the crash, and recommendations to fix this and/or any other evident issue.

For this we have prepared the following simulated request:


Description:

The application system crashed few times today (25th of August 2021) and we would like to know why. Our application team confirmed they didn't do any changes.


Data :

Logs at https://github.com/tomashendrich/SUSE/raw/main/nts_test-vm_210825_1616.txz

Relevant documentation:

Product Support Lifecycle: https://www.suse.com/lifecycle/

List of SUSE Linux Enterprise Server kernel (version and release date):  https://www.suse.com/support/kb/doc/?id=000019587

Holy Supportconfig File:  https://www.suse.com/c/holy-supportconfig-file/

Holy Supportconfig File – Analysis: Part 1: https://www.suse.com/c/holy-supportconfig-file-analysis-part-1/

How to tell if the system got rebooted or crashed: https://www.suse.com/support/kb/doc/?id=000020272


Expected necessary time for this is ~60 min.




Keerthana review:

Hello Team,


From the logs we found that our system is a Physical server ,running on SLES 15 with Basesystem_Module_15_SP1_x86_64.

SUSE Linux Enterprise Server 15 SP1
Dec-17-2019	4.12.14-197.29.1

# Verification Status: Passed -- properly verified and all the repos are in place.

# /bin/uname -a
Linux test-vm 4.12.14-197.29-default #1 SMP Fri Dec 6 12:08:50 UTC 2019 (ca25711) x86_64 x86_64 x86_64 GNU/Linux

as above we can see , that the server is properly verfied but with the older version.

2021-08-25T13:40:41.412013+02:00 tux3 geoclue[1602]: Failed to connect to avahi service: Daemon not running

from above noticed that during system startup a firewal process (avahi) is failing to connect , which might also be a result in sudden crash.

2021-08-25T16:06:53.926247+02:00 tux3 gnome-software[2001]: failed to call gs_plugin_refine on packagekit-refine: failed to get updates for urgency: Download
 (curl) error for 'https://updates.suse.com/SUSE/Updates/SLE-Module-Basesystem/15-SP1/x86_64/update/repodata/repomd.xml?_kvsatywbz52r8bDAPVGIoG6-F9qEweZPCoGy
Mk6pDlAshjVR2n6Ebw0dxfDbsn3TsD6k0Y0CdASiBNmAIVMp420QNNzf2v1az1FLejlVuu7aGXfiQ4CLyXsTOB5WitnpV9A0Pe4NI7dMA_qURz0KJ1OBDjwww':#012Error code: Connection failed#
012Error message:

from the above error in /var/lo/messages file , we also noticed that  while updating the repositories ,the system is facing a curl error where it is failing to update the certificate , hence the sudden crash might have occuerd , the solution is to update the certificate and re-register the server by giving below command.

# SUSEConnect --cleanup
# update-ca-certificates 
# SUSEConnect --regcode <your_registration_code>


2021-08-25T14:40:24.184316+02:00 tux3 /usr/sbin/irqbalance: Balancing is ineffective on systems with a single cpu.  Shutting down

Also i noticed in summary.xml log , enquired that the baselevel of the system is still 1 , since the newer version of the baselevel is installed , hence the server is still running is older baselevel ,which might lead to any antivirus issues , and since there is upgrade in the new patches available for SLES 15 version , it is required to upgrade the system to newer patches.

Hope you have found my RCA a suitable one.

Thankyou 
Keerthana S
