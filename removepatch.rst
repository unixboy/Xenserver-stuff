How to remove pending updates in Citrix XenServer
=================================================

**Applying an incorrect update**

When you apply an update that is meant for a different version of XenServer you should receive a warning similar to the one below.

::

   [root@xenserver ~]# xe patch-upload file-name=XS62ESP1027.xsupdate
   06b86256-274d-444e-aa08-a635717e68d1


::
 
   [root@xenserver ~]# xe patch-pool-apply uuid=06b86256-274d-444e-aa08-a635717e68d1
   The patch precheck stage failed: the server is of an incorrect version.
   patch: 06b86256-274d-444e-aa08-a635717e68d1 (XS62ESP1027)
   found_version: 6.5.0
   required_version: ^6\.2\.0$


Remove the pending update

 Removing the update is actually quite simple using the command line on any XenServer node within the pool.

 First you need to identify the UUID of the patch, if you uploaded it recently and still have the UUID you can use that, otherwise run the ‘xe patch-list’ command which will output all patches. You can grep for the patch using -B1 which will display the UUID.

::

  [root@xenserver ~]# xe patch-list | grep -B1 XS62ESP1027
  uuid ( RO)                    : 06b86256-274d-444e-aa08-a635717e68d1
              name-label ( RO): XS62ESP1027


With the UUID of the patch in question we can use the ‘xe patch-destroy’ command as shown below to remove the patch.


::

  root@xenserver ~]# xe patch-destroy uuid=06b86256-274d-444e-aa08-a635717e68d1 

As soon as this is completed the patch will no longer be present on any XenServer node within the pool and XenCenter will no longer list it as an update that has not yet been applied.



