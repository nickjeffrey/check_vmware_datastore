# check_vmware_datastore
nagios check for VMware datastore via HTTPS REST API

# Requirements
perl, curl, low-privilege read-only nagios userid on vCenter server, vCenter 6.7 or later (for REST API)

# Preparation
Create a user called nagios in vCenter
<br><img src=images/vcenter_create_user.png>

Assign the Read-Only role.  This makes the nagios user on vCenter a low-privilege read-only role that cannot make any changes to vCenter.
<br><img src=images/vcenter_user_role.png>


Create the /usr/local/nagios/etc/private/vcenter.auth file with the credentials for the above user
```
username=nagios@vsphere.local
password=SuperSecretPassword#123!
```

Create a section similar to the following in the services.cfg file:
```
define service {
        use                             generic-service
        host_name                       vcenter01.example.com
        service_description             VMWare datastores
        check_command                   check_vmware_datastore
        }
```

Create a section similar to the following in the commands.cfg file:
```
# 'check_vmware_datastore' command definition
define command {
       command_name    check_vmware_datastore
       command_line    $USER1$/check_vmware_datastore --host=$HOSTADDRESS$
       }
```

# Output
<br><img src=images/output.png>
