[← README](README.md)
## Deploying an Undercloud

### Virtualization Undercloud Preparation
If you're using the [Virtualization](README-virt.md) instructions, then complete this section before moving on the

The Undercloud image on the instack virtual machine is a minimal install of Fedora 20 with yum-utils and net-tools
installed.  This section will walk you through installing the instack-undercloud package and then running instack to
apply packages.

1. Log into your instack virtual machine as the stack user via the IP you retrieved earlier.

2. Create the virtual-power-key and copy it to the virt host.  The user in ssh-copy-id should match the the user you
created on the host earlier.  Make a note of the user and ip you used here.  They will be the VIRTUAL_POWER_USER and
VIRTUAL_POWER_HOST values in the instack.answers file discussed later.

    ssh-keygen -t rsa -N '' -C virtual-power-key -f virtual-power-key
    ssh-copy-id -i virtual-power-key.pub stack@192.168.122.1


## Installing the Undercloud

Ensure you're logged in with your non-root user.

1. Enable the rdo-icehouse repository

    sudo yum install -y http://rdo.fedorapeople.org/openstack-icehouse/rdo-release-icehouse.rpm

1. Install instack-undercloud

    sudo yum -y install instack-undercloud

1. Create and edit your answers file. The descriptions of the parameters that can be set are in the sample answers file.
Make sure that at least VIRTUAL_POWER_USER and VIRTUAL_POWER_HOST match the values you noted earlier.

    # Answers file must exist in home directory for now
    # Use either the baremetal or virt sample answers file
    # cp /usr/share/doc/instack-undercloud/instack-baremetal.answers.sample ~/instack.answers
    # cp /usr/share/doc/instack-undercloud/instack-virt.answers.sample ~/instack.answers
    # Perform any answer file edits

1. Run script to install undercloud. The script will produce a lot of output on the sceen. It also logs to
~/.instack/install-undercloud.log. You should see `install-undercloud Complete!` at the end of a successful run.

    instack-install-undercloud-packages

1. Once the install script has run to completion, you should take note to secure and save the file  `/root/stackrc` and
`/root/tripleo-undercloud-passwords`. Both these files will be needed to interact with the installed undercloud. You may
copy these files to your home directory to make them easier to source later on, but you should try to keep them as secure
and backed up as possible.

You now have a running Undercloud.  Next steps: [Deploying an Overcloud](README-deploy-overcloud.md)

## Optional: Accessing Undercloud Dashboard
To access horizon on the undercloud, create an ssh tunnel on the virt host where 192.168.122.55 should be changed to
reflect your instack virtual machine's actual IP address.  This will allow you to use horizon on instack from your virt
host.  If you need to connect remotely through the virt host, you can chain ssh tunnels as needed.  Note: Depending on
your virt host configuration, you may need to open up the correct port(s) in iptables.

    ssh -g -N -L 8080:192.168.122.55:80 `hostname`

The default user and password are found in the stackrc file on the instack virtual machine, OS_USERNAME and OS_PASSWORD.
You can read more about using the dashboard in the [User Guide](http://docs.openstack.org/user-guide/content/log_in_dashboard.html).