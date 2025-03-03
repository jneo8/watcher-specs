..
 This work is licensed under a Creative Commons Attribution 3.0 Unported
 License.

 http://creativecommons.org/licenses/by/3.0/legalcode

===========================================
Host maintenance strategy disable migration
===========================================

Include the URL of your launchpad blueprint:

https://blueprints.launchpad.net/watcher/+spec/example

Problem description
===================

Host maintenance is a migration strategy designed for maintaining a compute node.
It triggers either live or cold migration for all instances on the node, assuming that both migration methods are available.
However, this may not apply to deployments where live or cold migration is not supported.

Use Cases
----------

- As a Cloud Administrator, if live migration is not supported in my OpenStack deployment, I want to stop instance and apply cold migration to instance.
- As a Cloud Administrator, if cold migration is not supported in my OpenStack deployment, I want to skip the migration.

Proposed change
===============

Estimated changes are going to be in the following places:

* Host maintenance strategy

  * Input parameters **disable_cold_migration** and **disable_live_migration** to disable the migration.
    
    * If **disable_live_migration** is given, stop the instance and perform a cold migration.
    * If **disable_cold_migration** is given, skip the migration of the instance.

* New start and stop actions in applier

  * Action to stop the instance

Alternatives
------------

None

Data model impact
-----------------

None

REST API impact
---------------

None

Security impact
---------------

None

Notifications impact
--------------------

None

Other end user impact
---------------------

Two new input parameters for host maintenance strategy,
the behavior is expected as the same if no parameters are provided, so no breaking change.

Performance Impact
------------------

None

Other deployer impact
---------------------

None

Developer impact
----------------

None

Implementation
==============

Assignee(s)
-----------

Primary assignee:
  <jneo8>

Work Items
----------

1. New applier action to stop the instance.

2. Modify the function which creates migration action for instance.

Dependencies
============

* https://specs.openstack.org/openstack/watcher-specs/specs/queens/approved/cluster-maintenance-strategy.html

Testing
=======

* Unit tests on the Watcher Decision Engine and Applier.

* Integration tests

  * Launch an audit with the **disable_live_migration** input parameter enabled.
  * Launch an audit with the **disable_cold_migration** input parameter enabled.
  * Launch an audit with both **disable_live_migration**, **disable_cold_migration** input parameters enabled.

Documentation Impact
====================

Need to update `Host Maintenance Strategy documentation`_.

References
==========

None

History
=======

None

.. _Host Maintenance Strategy documentation: https://docs.openstack.org/watcher/latest/strategies/host_maintenance.html
