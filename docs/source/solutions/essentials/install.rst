Installation
============

.. warning::
    If you had previously installed DC Fabric Automation Suite TP release, please do not install this pack, wait for the new release of DC Fabric Automation Suite.  When you install DC Fabric Automation Suite, it installs the compatible version of Network Essentials pack automatically.

Installing Pre-requisites
-------------------------
Network Essentials pack requires gcc plus the libxml2-dev and libxslt1-dev libraries.  Before installing Network Essentials pack, please run these commands:

On Ubuntu:

.. code-block:: bash

  sudo apt-get install build-essential libxml2-dev libxslt1-dev

On Redhat/CentOS:

.. code-block:: bash

  sudo yum install libxml2-dev libxslt1-dev
  sudo yum install gcc

Installing Network Essentials
-----------------------------
After installing the above pre-requisites, install Network Essentials pack using:

.. code-block:: bash

  st2 pack install network_essentials

To install DC Fabric Suite, follow the installation instructions for the :doc:`DC Fabric Automation Suite <../dcfabric/install>`

* New to |bwc|? Go to fundamentals - start with :doc:`/start`.
* Understand the Network Essentials :doc:`Actions and Workflows<workflows>`.
