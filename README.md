# nmrbox-configuration-htcondor
Overrides to htcondor startup to allow using an Active Directory account.

We prefer to create accounts centrally using LDAP (in our case, Active Directory) server to simplify administration.

We had a problem with the condor service not starting on an Ubuntu 18 (debian) system because the /var/run/condor didn't exist.
/var/run/condor didn't exist because the _systemd-tmpfiles-setup_ service runs before the _sssd_ service, so the creation command 
in the _/usr/lib/tmpfiles.d/condor-tmpfiles.conf_ file installed by htcondor failed because "condor" wasn't recognized as an account 
name.

This solution runs _/bin/systemd-tmpfiles_ a second time after sssd has been started. A systemd override file directs
systemd to execute the service condor-tmpfiles-setup before starting condor.

