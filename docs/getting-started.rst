Getting started
===============

.. contents:: Sections
   :local:

.. _ulogd__ref_ulogd_entry:

ULOGD - concept
---------------

``ulogd`` wants to provide a flexible, almost universal logging daemon for
netfilter logging. This encompasses both packet-based logging (logging of policy
violations) and flow-based logging, e.g. for accounting purpose.

ulogd consists of a small core and a number of plugins. All the real power lies
in the plugins, and in the user who configures the interactions between those
plugins.

Input Plugins

    Input plugins acts data source. They get data from somewhere outside of
    ulogd, and convert it into a list of ulogd keys.

Filter Plugins

    Filter plugins interpret and/or filter data that was received from the
    Input Plugin. A good example is parsing a raw packet into IPv4 / TCP /
    ... header information.

Output Plugins

    Output plugins describe how and where to put the information gained by
    the Input Plugin and processed by one or more Filter Plugins. The
    easiest way is to build a line per packet and fprint it to a file. Some
    people might want to log into a SQL database or want an output
    conforming to the IETF IPFIX language.

    By means of the configuration file, the administrator can build any
    number of Plugin Stacks. A plugin stack is a series of plugins,
    starting with an Input plugin, none, one or multiple filter plugins,
    and one output plugin on top.

.. _ulogd__ref_ulogd__debops:

ULOGD in DebOps
---------------

The DebOps playbooks use a set of Ansible inventory variables to provide custom
stacks of log levels. Syntax of all possible values is descibed on the next
page.

.. _ulogd__ref_ulogd__dependency:

Usage as a role dependency
--------------------------

Other roles can use ``debops.ulogd`` role in their own playbooks
to idempotently use ``ulogd`` logging mechanism, define stacks
and their plugins.

The variables defined by roles override variables defined through Ansible
inventory.

.. _ulogd__ref_ulogd__example:

Example inventory
-----------------
It's an example use case for defining and configuring two stacks.

.. epigraph::
 Attention: by limitation of this role, names of plugin instances can not start
 with a capital letter or a number!

.. code-block:: yaml

   ulogd__stack_shorewall:
     - log1: 'BASE'
       options:
         group: 0
         netlink_qtimeout: 100
     - base1: 'BASE'
     - ifi1: 'IFINDEX'
     - emu1: 'LOGEMU'
       options:
         file: '/var/log/ulogg/syslogemu.log'
         sync: 1

   ulogd__stack_ferm:
     - log2: 'BASE'
     - ifi2: 'IFINDEX'

   ulogd__stacks:
     - '{{ ulogd__stack_shorewall  }}'
     - '{{ ulogd__stack_ferm  }}'

..
 Local Variables:
 mode: rst
 ispell-local-dictionary: "british"
 End:
