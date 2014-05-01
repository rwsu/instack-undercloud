[← README](README.md)

## Minimum System Requirements
In order to produce a usable OpenStack install this setup requires five baremetal machines: one machine for the
undercloud,  and one machine  for each of the overcloud controller, compute, block storage and object storage nodes.
However, the setup can be scaled down to run on just three machines and include only the undercloud, overcloud
controller and overcloud compute nodes. Similarly, the overcloud nodes can be scaled up to include multiple compute,
object storage or block storage nodes.

The setup requires baremetal machines with the following minimum specifications:
* multi-core CPU
* 4GB memory
* 60GB free disk space


The undercloud machine needs to run Fedora 20 x86_64, which is discussed more below.

## Preparing the Baremetal Environment

### Networking
The overcloud nodes will be deployed from the undercloud machine and therefore the machines need to have  have their
network settings modified to allow for the overcloud nodes to be PXE boot'ed using the undercloud machine.  As such,
the setup requires that:

* all overcloud machines in the setup must support IPMI

* one NIC from every machine needs to be on its own broadcast domain.  In the tested environment,  this required setting up a new VLAN on the switch. Note that you should use the "same" NIC on each of the overcloud machines ( for example: use the second NIC on each overcloud machine). This is because during installation we will need to refer to that NIC using a single name across all overcloud machines e.g. em2

* the overcloud machines can PXE boot off the NIC that is on the private VLAN. In the tested environment, this required disabling network booting in the BIOS  for all NICs other than the one we wanted to boot and then ensuring that the chosen NIC is at the top of the boot order (ahead of the local hard disk drive and CD/DVD drives).

* For each overcloud machine you have:
** the MAC address of the NIC that will PXE boot
** the IPMI information for the machine (i.e. IP address of the IPMI NIC, IPMI username and password)

### Setting Up the Undercloud Machine

- Select a machine within the baremetal environment on which to install the undercloud.
- Install [https://fedoraproject.org/en/get-fedora Fedora 20 x86_64] on this machine.  (Instack installs have been tested on Fedora 20 x86_64 only.)
- Add a user account and configure this user to have passwordless sudo access. This user will execute the steps for
installation and deployment. The following example should be executed by the <code>root</code> user. It adds the user "stack".</p>
<pre>useradd stack
passwd stack # specify a password
echo "stack        ALL=(ALL)       NOPASSWD: ALL" > /etc/sudoers.d/stack
chmod 0440 /etc/sudoers.d/stack</pre>


## Installing the Undercloud with Instack

- Make sure you are logged in as the non-root user you created above.
- Enable the RDO icehouse repository</p>

    sudo yum install -y http://rdo.fedorapeople.org/openstack-icehouse/rdo-release-icehouse.rpm

- Install instack-undercloud</p>

    sudo yum -y install instack-undercloud

- Create and edit your answers file. The descriptions of the parameters that can be set are in the sample answers file.
<pre># Answers file must exist in home directory for now
# Use the baremetal answers file
cp /usr/share/doc/instack-undercloud/instack-baremetal.answers.sample ~/instack.answers
# Perform any answer file edits.
The values in the answer file should be suitable for your environment.
In particular, check that  the  LOCAL_INTERFACE setting matches the Network Interface on the undercloud used to handle PXE boots.</pre></li>
<li><p>Run script to install undercloud. The script will produce a lot of output on the screen. It also logs to <code>~/.instack/install-undercloud.log</code>. You should see <code>install-undercloud Complete!</code> at the end of a successful run.</p>
<pre>instack-install-undercloud-packages</pre></li>
<li><p>Once the install script has run to completion, you should take note to secure and save the files <code>/root/stackrc</code> and <code>/root/tripleo-undercloud-passwords</code>. Both these files will be needed to interact with the installed undercloud. You may copy these files to your home directory to make them easier to source later on, but you should try to keep them as secure and backed up as possible. </p>
<p>That completes the undercloud install and now you should have a running undercloud. For the next steps, see: [Deploying an Overcloud](README-deploy-overcloud.md)
<p>If your install does not complete successfully please see the [FAQ](README-instack-faq.md) page for potential solutions.

