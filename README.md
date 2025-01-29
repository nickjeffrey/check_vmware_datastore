# check_vmware_datastore
nagios check for VMware datastore via HTTPS REST API

# Requirements
perl, curl, low-privilege read-only nagios userid on vCenter server, vCenter 6.7 or later (for REST API)

# Preparation
Create a user called nagios in vCenter
<br><img src=images/vcenter_create_user.png>

Assign the Read-Only role.  This makes the nagios user on vCenter a low-privilege read-only role that cannot make any changes to vCenter.
<br><img src=images/vcenter_user_role.png>


Copy the check script to the plugins directory
```
su - nagios
cd /tmp
git clone https://github.com/nickjeffrey/check_vmware_datastore
cd check_vmware_datastore
cp check_vmware_datastore     /usr/local/nagios/libexec/check_vmware_datastore
chown nagios:nagios           /usr/local/nagios/libexec/check_vmware_datastore
chmod 755                     /usr/local/nagios/libexec/check_vmware_datastore
```

Create the /usr/local/nagios/etc/private/vcenter.auth file with the credentials for the above user
```
username=nagios@vsphere.local
password=SuperSecretPassword#123!
```

Create a section similar to the following in the services.cfg file:
```
define service {
       # syntax is check_vmware_datastore!warn%free!crit%free!includeds1,includeds2!excludeds1,excludeds2
       # all the additional parameters are optional
        use                             generic-service
        host_name                       vcenter01.example.com
        service_description             VMWare datastores
        check_command                   check_vmware_datastore!10!5
        }
```

Create a section similar to the following in the commands.cfg file:
```
# 'check_vmware_datastore' command definition
define command {
       command_name    check_vmware_datastore
       command_line    $USER1$/check_vmware_datastore --host=$HOSTADDRESS$  --warn=$ARG1$ --crit=$ARG2$ --include=$ARG3 --exclude=$ARG4$
       }
```

# Output
<br><img src=images/output.png>
