Introduction
============

This repo includes the current SolidFire driver for the OpenStack Block Storage Project (Cinder), as well as a brief
description on how to install and use it.

Note that the version in the OpenStack repo is regularly maintained and unless you're looking for a pre-release update
should be sufficient.  In addition, extensive config and usage guides for SolidFire on OpenStack are available from the
SolidFire web-site as well as the block-storage-admin guide on docs.openstack.org.


Overview
========

The SolidFire driver is a tightly integrated OpenStack Block Storage Driver implementing all of the exposed functionality available
from the OpenStack Block Storage API.  This includes all of the basic expected functionality:
    create/delete
	snapshot
	copy-image-to-volume
	copy-volume-to-image
	bootable-volumes

The SolidFire Cluster API is accessible directly via json-rpc over HTTPS, so there's no requirements for extra management configuration
or 3rd party libraries such as SMIS.

Requirements
============

An installed and configured SolidFire cluster, network accessible by your Cinder node.

Supported Operations
====================

All OpenStack Block Storage API operations are supported.

Preparation
===========

Download Cinder driver
----------------------

If you need an updated version of the SolidFire driver, download the one in this repo, otherwise, you can just move
directly to the configuration section.

Updating The Included driver
----------------------------

Clone solidfire.py from this repo (be sure to grab the approrpriate branch [Folsom, Grizzly....]) and replace
the version in your OpenStack distribution.

NOTE:  If the release of OpenStack is newer than the version on this github it's recommended
you do NOT install the version from this repo.

Create a Cluster Admin account on SolidFire for OpenStack to use
----------------------------------------------------------------

The SolidFire Cluster is designed for multi-tenant usage and one of the requirements for this
is multiple admin accounts with varying privelleges.  You'll want to create a Cluster Admin account
that has "reporting, volumes and account" access privelleges.

After creatin the admin account, make a note of the user-name and password, and also
grab the MVIP and SVIP information.

The MVIP is the Management Virtual IP, SolidFire API access is via json-rpc over HTTPS directly
to the cluster via this address.

The SVIP is the Storage Virtual IP, all of the iSCSI interfaces to all nodes in your SolidFire Cluster are
presented via this address.

Configuring the driver in cinder.conf
-------------------------------------

Make the following changes in /etc/cinder/cinder.conf:

```
volume_driver = cinder.volume.drivers.solidfire.SolidFire
san_ip=<SolidFire-MVIP>
san_login=<solidfire-cluster-admin-account-name>
san_password=<solidfire-cluster-admin-account-password>
```

Restart the cinder-volume service
-------------------------------------

```
sudo service cinder-volume restart
```

ROCK ON!!!
----------
