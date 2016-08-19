Introduction
============

The ``debops.ulogd`` role can is used to manage ``ulogd``, which wants to
provide a flexible, almost universal logging daemon for netfilter logging.
This encompasses both packet-based logging (logging of policy violations)
and flow-based logging, e.g. for accounting purpose.

``ulogd`` consists of a small core and a number of plugins. All the real power
lies in the plugins, and in the user who configures the interactions between
those plugins.

The role can also be used by other Ansible roles as a dependency to create
netfliter logging envornment, that preserves idempotency.

Installation
~~~~~~~~~~~~

This role requires at least Ansible ``v2.0.0``. To install it, run:

    ansible-galaxy install debops.ulogd

..
 Local Variables:
 mode: rst
 ispell-local-dictionary: "british"
 End:
