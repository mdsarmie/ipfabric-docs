.. NOTE: This file has been generated automatically, don't manually edit it

delete_l2_port_channel
~~~~~~~~~~~~~~~~~~~~~~

**Description**: Delete the port channel interface and delete the port chanel configuration from all the member ports 

.. table::

   ================================  ======================================================================
   Parameter                         Description
   ================================  ======================================================================
   **mgmt_ip**                       Management IP address of the target device

                                     Type: ``string``
   *username*                        Login user name to connect to the device

                                     Type: ``string``

                                     **Default**: admin
   *password*                        Login password to connect to the device

                                     Type: ``string``

                                     **Default**: password
   **port_channel_id**               Port-channel interface number <NUMBER:1-6144>

                                     Type: ``integer``
   ================================  ======================================================================

