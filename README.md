#Undercloud Install via instack

This project allows you to deploy a [TripleO](https://wiki.openstack.org/wiki/TripleO) Undercloud and Overcloud Instack
on an all baremetal or an all virtual environment.

TripleO is a program aimed at installing, upgrading and operating OpenStack clouds using OpenStack's own cloud
facilities as the foundations - building on nova, neutron and heat to automate fleet management at datacenter scale
(and scaling down to as few as 2 machines).

Instack executes [diskimage-builder](https://github.com/openstack/diskimage-builder) style elements on the current system.
This enables a current running system to have an element applied in the same way that diskimage-builder applies the
element to an image build.

## System Preparation
To use instack-undercloud in an all virtual environment, perform the setup using the [Virtualization](README-virt.md)
instructions.  If you're working with baremetal, see the [Baremetal](README-baremetal.md) setup instructions.

## Deploying an Undercloud
[Installing the Undercloud](README-deploy-undercloud.md)

## Deploying an Overcloud
[Overcloud Deployment](README-deploy-overcloud.md)

## Testing the Overcloud
[Overcloud Testing](README-test-overcloud.md)

## FAQ
[Tips, Examples, Workarounds and Answers](README-instack-faq.md) for working with instack-undercloud.