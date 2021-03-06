Workflows
=========

DC Fabric Automation Suite includes turnkey automations required to provision, validate and troubleshoot data center networks spanning Day-0 and Day-N activities.  Designed to work with multiple data center architectures such as IP Fabric, IP Fabric with EVPN overlay, VCS Fabric as well as multiple device families such as VDX, SLX.  

This is a reference documentation organized around key usecases as outlined below.  These can be used as independent workflows, or tied together to form more complex workflows. They can be manually triggered, or they can be tied to :doc:`sensors </sensors>` using rules.

All actions and workflows return a boolean True/False to indicate success or failure of the run.  Some actions, workflows return additional variables which are documented in the corresponding section.

.. contents::
   :local:
   :depth: 2

Manage Fabric Templates
-----------------------

As explained in section :doc:`Setting UP IP Fabric<operation/setup_ipfabric>`, fabric template is used to provision an IP Fabric network.  DC Fabric Suite includes a default fabric template which has a set of predefined parameters used to create the fabric, such as ASN range, IP address ranges, etc.  However, if a different set of configuration parameters are needed, users can create a new IP Fabric template and define the values for the configuration parameters using the actions in this section. For documentation regarding the various fabric parameters, refer to :ref:`IP Fabric parameters<ip_fabric_parameters>`.

.. include:: /_includes/solutions/dcfabric/fabric_add.rst

.. include:: /_includes/solutions/dcfabric/fabric_delete.rst

.. include:: /_includes/solutions/dcfabric/fabric_list.rst

.. include:: /_includes/solutions/dcfabric/fabric_config_set.rst

.. include:: /_includes/solutions/dcfabric/fabric_config_delete.rst


Build IP Fabric Infrastructure
------------------------------

Actions in this section enable the user to register switches into the inventory as part of a pre-defined fabric and automatically configure all the switch interfaces, BGP peering and related settings as per the fabric template parameters.  Please refer to the :doc:`Setting UP IP Fabric<operation/setup_ipfabric>` section for additional details.

.. include:: /_includes/solutions/dcfabric/switch_add.rst

.. include:: /_includes/solutions/dcfabric/switch_delete.rst

.. include:: /_includes/solutions/dcfabric/switch_list.rst

.. include:: /_includes/solutions/dcfabric/switch_update.rst

.. include:: /_includes/solutions/dcfabric/topology_generate.rst

.. include:: /_includes/solutions/dcfabric/configure_fabric_infra.rst

Manage EVPN Tenants and Edge Ports
----------------------------------

Once IP Fabric is provisioned, check out the :doc:`Using IP Fabric<operation/using_ipfabric>` documentation for Day-N service provisioning workflows.  This section includes the actions and workflows to automate Day-N services such as provisioning of tenants, gateways and edge ports to enable the deployment of endpoints such as Servers, Firewalls and Load Balancers etc. on the fabric.

.. include:: /_includes/solutions/dcfabric/create_l2_tenant_evpn.rst

Details
```````

The create_l2_tenant_evpn workflow provisions an L2 domain extension in the BGP EVPN based IP fabric,
on the selected leaves or vLAG pairs.The workflow must be provided with the set of vLAG pairs or
leaf switches between which the layer 2 extension is required.

Requirements
````````````

This workflow is designed for use in IP Fabric EVPN networks only.

Output
``````

   result
       Boolean - True/False, to indicate success or failure of the action.

Error Messages
``````````````

   "Input is not a valid VNI value"
       Returned if VNI value is < 1 or > 16777215

   "EVPN instance not configured under rbridge-id"
       Returned if EVPN instance is not already configured

   "Invalid Input values for VNI <vni> add for evi <evi> under rbridge <rbridge-id>
       Returned if input is invalid.

   "VLAG PAIR must be <= 2 leaf nodes"
       Returned if VLAG pair is more than two nodes

.. include:: /_includes/solutions/dcfabric/create_l3_tenant_evpn.rst

Details
```````

The Create_l3_tenant_evpn workflow provisions the BGP EVPN based IP fabric with an L3 tenant
identified by a VRF. This workflow provisions the vlan, VRF for the Layer 3 tenant at the identified
leaf switches or vLAG pairs, enables routing for the VRF across the IP fabric by enabling the
VRF address family in BGP and creating L3VNI interface and also enables redistribution of
connected routes in the VRF to BGP EVPN.

Requirements
````````````

This workflow is designed for operating in IP Fabric mode.

Output
``````

   result
       Boolean - True/False, to indicate success or failure of the action.


Error Messages
``````````````

   "Not a valid VLAN"
       Returned if VLAN provided are invalid, e.g. > 4094

   "vlan 1 is default vlan"
       Returned if VLAN provided is 1.

   "Vlan cannot be created, as it is not a user/fcoe vlan"
       Returned if VLAN provided is part of user/FCOE VLAN (4087/4096/1002).

.. include:: /_includes/solutions/dcfabric/add_multihomed_endpoint.rst

.. include:: /_includes/solutions/dcfabric/add_multihomed_endpoint_and_gw_evpn.rst

Details
```````
The add_multihomed_endpoint_and_gw_evpn workflow automates the configuration of the edge ports of the
BGP EVPN based IP fabric. The workflow automates creation of port-channel interfaces (LAGs and
vLAGs), configuration of the port-channel interface as access or trunk, creation and association
of VLANs with the port-channel interfaces, validation of the port channel state as well as
creation of layer 3 gateway using Anycast Gateway protocol and modify ARP ND ageing on the
vLAG pair or leaf switch and association of the layer 3 gateways with a VRF. 

Requirements
````````````
This workflow is designed for use in IP Fabric with EVPN networks. 

Output
``````
   Result
       Boolean - True/False, to indicate success or failure of the action.

Error Messages
``````````````
   "Not a valid VLAN"
       Returned if VLAN provided are invalid, e.g. > 4094

   "vlan 1 is default vlan"
       Returned if VLAN provided is 1.

   "Vlan cannot be created, as it is not a user/fcoe vlan"
       Returned if VLAN provided is part of user/fcoe vlan (4087/4096/1002).

    Invalid IP “anycast_address”

.. note::
   To re-run this workflow, autopick_port_channel_id flag must be unset and port-channel ID must be specified.


IP Fabric Validation and Troubleshooting
----------------------------------------

.. include:: /_includes/solutions/dcfabric/get_flow_trace_ip_fabric.rst

Details
```````
Based on source and destination information provided by the user, this workflow determines the path of the packet flow in the IP Fabric topology and connects and runs diagnostic scripts on ingress leaf, spine and egress leaf.   Diagnostic actions on each node are intended for use by this workflow and not to be directly run by the end user.

Output
``````
    Result
       Boolean - True/False, to indicate success or failure of the action.

Error Messages
``````````````
   "The host ip address 10.18.245.150 is not the fabric host list"
       Returned if the input switch IP address is not part of fabric topology
       
   "the connection parameters  are incorrect"
       Returned if any of the connection parameters (Username, Password or Host IP) is incorrect.

   "username for %s not found."
       Returned if authentication fails
       
   "password for %s not found."
       Returned if authentication fails

   "Unsupported version running on the switch"
       Returned if the switch software version is < 7.0.1a

   "Length of the description is more than the allowed size"
       Returned if interface description length is more than 63.

   "The mgmt_ip could not find for loopback_ip"
       Loopback IP address provided in the debug cannot be found from topology

   "Not able to open the file located at /tmp/debug_ingress"
       Returned if the debug file cannot be opened

   "Failed to run on switch 10.18.245.166 due to exception"
       Returned if the command is not be able to run on the switch because of some exceptions.

.. include:: /_includes/solutions/dcfabric/query_topology.rst

.. include:: /_includes/solutions/dcfabric/show_config_bgp.rst

.. include:: /_includes/solutions/dcfabric/show_lldp_links.rst

.. include:: /_includes/solutions/dcfabric/show_vcs_links.rst


Manage VCS Fabric Tenants and Edge Ports
----------------------------------------

Brocade VCS fabric automatically forms with minimal Day-0 configurations.  This section includes the actions and workflows to automate Day-N services such as provisioning of tenants, gateways and edge ports to enable the deployment of endpoints such as Servers, Firewalls and Load Balancers etc. on a VCS fabric.  Refer to :doc:`Brocade VCS Fabric<overview>` for various deployment models.

.. include:: /_includes/solutions/dcfabric/add_multihomed_endpoint.rst

Details
```````
The add_multihomed_endpoint workflow automates the configuration of the edge ports. 
The workflow automates creation of port-channel interfaces (LAGs and vLAGs), 
configuration of the port-channel interface as access or trunk, creation and
association of VLANs with the port-channel interfaces as well as validation of
the port channel state.

Requirements
````````````
This workflow is designed for operating on edge devices of IP Fabric, EVPN or VCS networks.

Output
``````
   Result
       Boolean - True/False, to indicate success or failure of the action.

Error Messages
``````````````
   "Not a valid VLAN"
       Returned if the VLAN provided is invalid, e.g. > 4094

   "vlan 1 is default vlan"
       Returned if VLAN provided is 1.

   "Vlan cannot be created, as it is not a user/fcoe vlan"
       Returned if VLAN provided is part of user/FCOE VLAN (4087/4096/1002).
	
   "Input is not a valid Interface"
       Returned if interface name is not a valid port number.

   “SWITCHING_NOT_ENABLED | %Error: Interface not configured for switching”
       Returned if given interfaces are already part of a port-channel 
 
.. include:: /_includes/solutions/dcfabric/add_multihomed_endpoint_and_gw.rst

Details
```````
The add_multihomed_endpoint_and_gw workflow automates the addition of an endpoint which needs Layer
3 termination within the fabric. It automates the provisioning of both the edge ports 
as well as the VRRP-E based redundant gateway.

Requirements
````````````
This workflow is designed for use in IP Fabric (no EVPN) and VCS Fabric networks.

Output
``````

   result
       Boolean - True/False, to indicate success or failure of the action.

   ve_ip: IP address assigned to the VE interface

   vrid: VRRPe router ID assigned to the VE interface

.. include:: /_includes/solutions/dcfabric/add_singlehomed_endpoint.rst

.. include:: /_includes/solutions/dcfabric/configure_vrrpe_gw.rst

Requirements
````````````

This workflow is designed for use in IP Fabric, EVPN and VCS networks.

Output
``````

   result
       Boolean - True/False, to indicate success or failure of the action.

   ve_ip: IP address assigned to the VE interface

   vrid: VRRPe router ID assigned to the VE interface

   virtual_ip: VRRPe virtual IP assigned to the VE interface

   virtual-mac: VRRPe virtual MAC assigned to the VE interface

Error Messages
``````````````
   "Not a valid VLAN"
       Returned if VLAN provided is invalid, e.g. > 4094

   "vlan 1 is default vlan"
       Returned if VLAN provided is 1.

   "Vlan cannot be created, as it is not a user/fcoe vlan"
       Returned if VLAN provided is part of user/FCOE VLAN (4087/4096/1002).

   "Pls specify a valid description"
       Returned if interface description length is less than 1.

   "Length of the description is more than the allowed size"
       Returned if interface description length is more than 63.

   "rbridge_id and ip_address lists are not matching"
       Returned if given rbridge_id and ip_address lists are not matching

   "Invalid IP address <ip-address>"
       Returned if ip address format is wrong e.g. 10.0.0.10.1

   "Pass IP address along with netmask.(ip-address/netmask)"
       Returned if IP address input is without netwmask e.g. 10.0.0.1.

   "Invalid Input values while creating to Ve"
       Returned if any one of the input is invalid.

   "Invalid Input values while assigning IP address to Ve"
       Returned if any one of the input is invalid.

   "Invalid Input values while configuring IPV6 link local"
       Returned if input is invalid.

VLAN to VNI Mapping
-------------------
Prior releases of DC Fabric Automation Suite (version < 1.2) only supported auto-mapping of VLAN to VNI, i.e., configure_fabric_infra workflow configures VLAN to VNI mapping as auto under overlay gateway on all the leaf switches in the fabric.  When auto-mapping is enabled, overlay gateway associates the VLANs the VNIs with the same ID.  Auto-mapping is convenient in some deployments, however, in deployments where VLAN scaling and overlapping is required, users need to use manual mapping.  This release provides user configurable option in fabric template so that users can choose auto or manual VLAN to VNI mapping during the fabric deployment.

A new fabric configuration parameter vlan_vni_auto_map has been introduced as part of fabric template, which can be set as ‘Yes/No’ while configuring the fabric settings. By default auto-mapping is enabled, however, users can create a fabric and set the value to No for manual mapping option.

NOTE: If a switch has been deployed using DC Fabric Automation Suite (version < v1.2), switch will contain VLAN to VNI mapping as auto under the overlay gateway configuration.  To change from auto to manual mapping, auto mapping configured previously needs to be disabled on the switch using delete_vlan_vni_mapping action.

.. include:: /_includes/solutions/dcfabric/configure_vlan_vni_mapping.rst

.. include:: /_includes/solutions/dcfabric/delete_vlan_vni_mapping.rst

Actions
-------
This section includes various building block actions that are used by the workflows above.  These are provided as a reference, can be used to build workflows for any custom scenarios.

.. include:: /_includes/solutions/dcfabric/configure_anycast_gateway_evpn.rst

.. include:: /_includes/solutions/dcfabric/configure_anycast_gw_mac_evpn.rst

.. include:: /_includes/solutions/dcfabric/configure_conversational_arp_evpn.rst

.. include:: /_includes/solutions/dcfabric/configure_conversational_mac_evpn.rst

.. include:: /_includes/solutions/dcfabric/create_vrf_evpn.rst

.. include:: /_includes/solutions/dcfabric/configure_arp_nd_suppression.rst

.. include:: /_includes/solutions/dcfabric/configure_bgp_redistribute_connected.rst

.. include:: /_includes/solutions/dcfabric/configure_evpn_instance.rst

.. include:: /_includes/solutions/dcfabric/configure_evpn_vtep.rst

.. include:: /_includes/solutions/dcfabric/modify_arp_nd_aging_ve.rst

.. include:: /_includes/solutions/dcfabric/provision_evpn_instance.rst

.. include:: /_includes/solutions/dcfabric/redistribute_connected_bgp_vrf.rst

.. include:: /_includes/solutions/dcfabric/set_max_path_bgp.rst
