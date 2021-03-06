********
Examples
********

A good source of examples are the `scenario`_ functional tests.

.. _`scenario`: https://github.com/metacloud/molecule/tree/master/test/scenarios/driver

Monolith Repo
=============

Molecule is generally used to test roles in isolation.  However, it can also
test roles from a monolith repo.

::

    $ tree monolith-repo -L 3 --prune
    monolith-repo
    ├── library
    │   └── foo.py
    ├── plugins
    │   └── filters
    │       └── foo.py
    └── roles
        ├── bar
        │   └── README.md
        ├── baz
        │   └── README.md
        └── foo
            └── README.md

The role initialized with Molecule (baz in this case) would simply reference
the dependant roles via it's `playbook.yml` or meta dependencies.

Molecule can test complex scenarios leveraging this technique.

.. code-block:: bash

    $ cd monolith-repo/roles/baz
    $ molecule test

Molecule is simply setting the `ANSIBLE_*` environment variables.  To view the
environment variables set during a Molecule operation pass the ``--debug``
flag.

.. code-block:: bash

    $ molecule --debug test

    DEBUG: ANSIBLE ENVIRONMENT
    ---
    ANSIBLE_CONFIG: /private/tmp/monolith-repo/roles/baz/molecule/default/.molecule/ansible.cfg
    ANSIBLE_FILTER_PLUGINS: /Users/jodewey/.pyenv/versions/2.7.13/lib/python2.7/site-packages/molecule/provisioner/ansible/plugins/filters:/private/tmp/monolith-repo/roles/baz/plugins/filters:/private/tmp/monolith-repo/roles/baz/molecule/default/.molecule/plugins/filters
    ANSIBLE_LIBRARY: /Users/jodewey/.pyenv/versions/2.7.13/lib/python2.7/site-packages/molecule/provisioner/ansible/plugins/libraries:/private/tmp/monolith-repo/roles/baz/library:/private/tmp/monolith-repo/roles/baz/molecule/default/.molecule/library
    ANSIBLE_ROLES_PATH: /private/tmp/monolith-repo/roles:/private/tmp/monolith-repo/roles/baz/molecule/default/.molecule/roles

Molecule can be customized any number of ways.  Updating the provisioner's env
section in `molecule.yml` to suit the needs of the developer and layout of the
project.

.. code-block:: yaml

  provisioner:
    name: ansible
    env:
      ANSIBLE_$VAR: $VALUE
