I got this error when i write  this command to terminal:
sudo dpkg --configure -a

And then I get this:
Setting up samba-common-bin (2:4.8.2+dfsg-1) ...
Checking smb.conf
ERROR: Unable to load default file
dpkg: error processing package samba-common-bin (--configure):
 installed samba-common-bin package post-installation script subprocess returned error exit status 255
dpkg: dependency problems prevent configuration of samba:
 samba depends on samba-common-bin (= 2:4.8.2+dfsg-1); however:
  Package samba-common-bin is not configured yet.

dpkg: error processing package samba (--configure):
 dependency problems - leaving unconfigured
Errors were encountered while processing:
 samba-common-bin
 samba