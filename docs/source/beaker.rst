Beaker
======

The Beaker (bkr) provider manages a single resource, ``bkr_server``.

bkr_server
----------

Beaker instances are provisioned using this resource.

* :docs1.5:`Topology Example <workspace/topologies/bkr-new.yml>`

The ansible modules for beaker are written and bundled as part of LinchPin.

* :code1.5:`bkr_server.py <linchpin/provision/library/bkr_server.py>`
* :code1.5:`bkr_info.py <linchpin/provision/library/bkr_info.py>`

Topology Schema
~~~~~~~~~~~~~~~

Within Linchpin, the :term:`bkr_server` :term:`resource_definition` has more
options than what are shown in the examples above. For each :term:`bkr_server`
role definition, the following options are available.

+-------------------+------------+----------+-------------------+-----------------+
| Parameter         | required   | type     | ansible value     | default         |
+===================+============+==========+===================+=================+
| role              | true       | string   | N/A               |                 |
+-------------------+------------+----------+-------------------+-----------------+
| whiteboard        | false      | string   | whiteboard        | Provisioned by  |
|                   |            |          |                   | LinchPin        |
|                   |            |          |                   |                 |
+-------------------+------------+----------+-------------------+-----------------+
| job_group         | false      | string   | job_group         |                 |
+-------------------+------------+----------+-------------------+-----------------+
| cancel_message    | false      | string   | cancel_message    |                 |
+-------------------+------------+----------+-------------------+-----------------+
| max_attempts      | false      | string   | max_attempts      |                 |
+-------------------+------------+----------+-------------------+-----------------+
| attempt_wait_time | false      | integer  | attempt_wait_time |                 |
+-------------------+------------+----------+-------------------+-----------------+
| recipesets        | false      | string   | recipesets        | see table below |
+-------------------+------------+----------+-------------------+-----------------+

recipesets
++++++++++

Because recipesets is how beaker requests systems, it's a large part of what the
topology schema includes. There are several ways to request systems. This table
describes the available recipesets options.

+---------------------+------------+----------+-----------------------------------------+
| Parameter           | required   | type     | sub-field layout options                |
+=====================+============+==========+=========================================+
| distro              | false      | string   | N/A                                     |
+---------------------+------------+----------+-----------------------------------------+
| family              | false      | string   | N/A                                     |
+---------------------+------------+----------+-----------------------------------------+
| tags                | false      | list     | list of strings                         |
+---------------------+------------+----------+-----------------------------------------+
| name                | false      | string   | N/A                                     |
+---------------------+------------+----------+-----------------------------------------+
| ks_meta             | false      | string   | N/A                                     |
+---------------------+------------+----------+-----------------------------------------+
| kernel_options      | false      | string   | N/A                                     |
+---------------------+------------+----------+-----------------------------------------+
| kernel_options_post | false      | string   | N/A                                     |
+---------------------+------------+----------+-----------------------------------------+
| arch                | false      | string   | N/A                                     |
+---------------------+------------+----------+-----------------------------------------+
| variant             | false      | string   | N/A                                     |
+---------------------+------------+----------+-----------------------------------------+
| bkr_data            | false      | string   | N/A                                     |
+---------------------+------------+----------+-----------------------------------------+
| method              | false      | string   | N/A                                     |
+---------------------+------------+----------+-----------------------------------------+
| count               | false      | string   | N/A                                     |
+---------------------+------------+----------+-----------------------------------------+
| ids                 | false      | list     | N/A                                     |
+---------------------+------------+----------+-----------------------------------------+
| taskparam           | false      | list     | list of strings                         |
+---------------------+------------+----------+-----------------------------------------+
| keyvalue            | false      | list     | list of strings                         |
+---------------------+------------+----------+-----------------------------------------+
| hostrequires        | false      | list     | **param** | **required** | **type**     |
+                     +            +          +-----------+--------------+--------------+
|                     |            |          | tag       | true         | string       |
+                     +            +          +-----------+--------------+--------------+
|                     |            |          | op        | false        | string       |
+                     +            +          +-----------+--------------+--------------+
|                     |            |          | value     | false        | int / string |
+                     +            +          +-----------+--------------+--------------+
|                     |            |          | type      | false        | string       |
+                     +            +----------+-----------+--------------+--------------+
|                     |            | dict     | force     | false        | string       |
+                     +            +----------+-----------+--------------+--------------+
|                     |            | dict     | rawxml    | false        | string       |
+---------------------+------------+----------+-----------------------------------------+
| reserve_duration    | false      | int      | N/A                                     |
+---------------------+------------+----------+-----------------------------------------+
| repos               | false      | list     | dict baseurl                            |
+---------------------+------------+----------+-----------------------------------------+
| install             | false      | list     | list of strings                         |
+---------------------+------------+----------+-----------------------------------------+
| ks_append           | false      | list     | list of strings                         |
+---------------------+------------+----------+-----------------------------------------+
| ssh_key             | false      | list     | list of strings                         |
+---------------------+------------+----------+-----------------------------------------+
| kickstart           | false      | string   | absolute path to a kickstart template   |
+---------------------+------------+----------+-----------------------------------------+

Additional Dependencies
-----------------------

The beaker resource group requires several additional dependencies. The
following must be installed.

* beaker-client>=23.3

It is also recommended to install the python bindings for kerberos.

* python-krbV

For a Fedora 26 machine, the dependencies could be installed using dnf.

.. code-block:: bash

  $ sudo dnf install python-krbV
  $ wget https://beaker-project.org/yum/beaker-server-Fedora.repo
  $ sudo mv beaker-server-Fedora.repo /etc/yum.repos.d/
  $ sudo dnf install beaker-client

Alternatively, with pip, possibly within a virtual environment.

.. code-block:: bash

  $ pip install linchpin[beaker]


Credentials Management
----------------------

Beaker provides several ways to authenticate. LinchPin supports these methods.

* Kerberos
* OAuth2

.. note:: LinchPin doesn't support the username/password authentication
   mechanism. It's also not recommended by the Beaker Project, except for
   initial setup.

