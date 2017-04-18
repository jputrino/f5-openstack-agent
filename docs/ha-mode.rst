.. _ha-mode:

.. index::
   single: openstack-bigip-ctlr; BIG-IP HA

.. index::
   single: openstack-bigip-ctlr; BIG-IP High Availability

.. index::
   pair: openstack-bigip-ctlr; High Availability; HA

Set up BIG-IP High Availability mode
====================================

Overview
--------

:term:`HA`, or, 'high availability', mode refers to high availability of the BIG-IP device(s). The |agent-long| can configure BIG-IP to operate in :term:`standalone`, :term:`pair`, or :term:`scalen` mode. The |agent-long| configures LBaaS objects on HA BIG-IP devices in real time.

Use Case
--------

High availability modes provide redundancy, helping to ensure service interruptions don't occur if a device goes down.

* :term:`standalone` mode utilizes a single BIG-IP device; here, 'high availability' means that BIG-IP core services are up and running, and VLANs are able to send and receive traffic to and from the device.

* :term:`pair` mode requires two (2) BIG-IP devices and provides :term:`active-standby` operation.
  When an event occurs that prevents the 'active' BIG-IP device from processing network traffic, the 'standby' device immediately begins processing that traffic so users experience no interruption in service.
  The standby device takes over the entire traffic load, avoiding a loss in performance.

* :term:`scalen` mode requires a :term:`device service cluster` of two (2) - four (4) BIG-IP devices.
   Scalen allows you to configure multiple active devices, each of which can fail over to other available active devices (:term:`active-active` mode).
   For example, if two BIG-IP devices are using active-active mode, both devices in the pair actively handling traffic.
   If an event occurs that prevents one device from processing traffic, that traffic automatically directs to the other active device.

   .. note::

      When failover occurs on an active-active cluster, a secondary device takes over the peer traffic load in addition to its current load.
      Depending on device configuration and capabilities, there may be a reduction in performance.


.. figure:: /_static/media/f5-lbaas-ha-active-standby-pair.png
   :alt: BIG-IP HA pair using active-standby mode
   :scale: 60%

   BIG-IP HA pair using active-standby mode


Caveats
-------

- If you only have one (1) BIG-IP deployed, you must use ``standalone`` mode.
- In this context, HA pertains to the BIG-IP device(s), not to the |agent-long|.


Configuration
-------------

#. Edit the :ref:`Agent Configuration File`:

   .. code-block:: console

      vi /etc/neutron/services/f5/f5-openstack-agent.ini

#. Set the :ref:`Device driver settings <device-driver-settings>`.

#. Set :code:`f5_ha_type` as appropriate for your environment.

    - ``standalone``: A single BIG-IP device
    - ``pair``: An :term:`active-standby` pair of BIG-IP devices
    - ``scalen``: An active :term:`device service cluster` of 2 to 4 BIG-IP devices

   .. code-block:: text

      #
      # HA mode
      #
      f5_ha_type = standalone
      #

#. Set up the |agent-long| to use :ref:`L2-adjacent mode <l2-adjacent-mode>` or :ref:`Global Routed mode <global-routed-mode>`.
