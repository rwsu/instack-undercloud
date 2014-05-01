[← README](README.md)

Run the instack-test-overcloud script to launch a Fedora image on the overcloud and wait until it pings successfully

    instack-test-overcloud

If your overcloud contains a Block Storage node, the <code>instack-test-overcloud</code> script will test that node by:

 - Creating a new volume
 - Attaching the volume to the Compute instance,  and then
 - Using ssh to log on to the instance and partition, format, and mount the volume


If your overcloud contains a Swift (Object) Storage node, the <code>instack-test-overcloud</code> script will test that node by:

 - Uploading a file with data to the node
 - Testing the data content downloaded from the node
