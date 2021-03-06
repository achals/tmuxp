.. _examples:

========
Examples
========

Short hand / inline
-------------------

tmuxp has a short-hand syntax to for those who wish to keep their configs
punctual.

.. sidebar:: short hand

    .. aafig::
       :textual:

        +-------------------+
        | 'did you know'    |
        | 'you can inline'  |
        +-------------------+
        | 'single commands' |
        |                   |
        +-------------------+
        | 'for panes'       |
        |                   |
        +-------------------+

YAML
~~~~

.. literalinclude:: ../examples/shorthands.yaml
    :language: yaml

JSON
~~~~

.. literalinclude:: ../examples/shorthands.json
    :language: json

Blank panes
-----------

No need to repeat ``pwd`` or a dummy command. A ``null``, ``'blank'``,
``'pane'`` are valid.

Note ``''`` counts as an empty carriage return.

YAML
~~~~

.. literalinclude:: ../examples/blank-panes.yaml
    :language: yaml

JSON
~~~~

.. literalinclude:: ../examples/blank-panes.json
    :language: json

2 panes
-------

.. sidebar:: 2 pane

    .. aafig::

        +-----------------+
        | $ pwd           |
        |                 |
        |                 |
        +-----------------+
        | $ pwd           |
        |                 |
        |                 |
        +-----------------+

YAML
~~~~

.. literalinclude:: ../examples/2-pane-vertical.yaml
    :language: yaml

JSON
~~~~

.. literalinclude:: ../examples/2-pane-vertical.json
    :language: json

3 panes
-------

.. sidebar:: 3 panes

    .. aafig::

        +-----------------+
        | $ pwd           |
        |                 |
        |                 |
        +--------+--------+
        | $ pwd  | $ pwd  |
        |        |        |
        |        |        |
        +--------+--------+

YAML
~~~~

.. literalinclude:: ../examples/3-pane.yaml
    :language: yaml

JSON
~~~~

.. literalinclude:: ../examples/3-pane.json
    :language: json

4 panes
-------

.. sidebar:: 4 panes

    .. aafig::

        +--------+--------+
        | $ pwd  | $ pwd  |
        |        |        |
        |        |        |
        +--------+--------+
        | $ pwd  | $ pwd  |
        |        |        |
        |        |        |
        +--------+--------+

YAML
~~~~

.. literalinclude:: ../examples/4-pane.yaml
    :language: yaml

JSON
~~~~

.. literalinclude:: ../examples/4-pane.json
    :language: json

Start Directory
---------------

Equivalent to ``tmux new-window -c <start-directory>``.

YAML
~~~~

.. literalinclude:: ../examples/start-directory.yaml
    :language: yaml

JSON
~~~~

.. literalinclude:: ../examples/start-directory.json
    :language: json

Environment variables
---------------------

tmuxp will replace environment variables wrapped in curly brackets
for the following variables:

- ``start_directory``
- ``before_script``
- ``session_name``
- ``window_name``
- ``shell_command_before``

tmuxp replaces these variables before-hand with variables in the
terminal ``tmuxp`` invokes in.

In this case of this example, assuming the username "user"::

    $ MY_ENV_VAR=foo tmuxp load examples/env-variables.yaml

and your session name will be ``session - user (foo)``. 

Shell variables in ``shell_command`` do not support this type of 
concatenation. ``shell_command`` and ``shell_command_before`` both
support normal shell variables, since they are sent into panes
automatically via ``send-key`` in ``tmux(1)``. See ``ls $PWD`` in 
example.

If you have a special case and would like to see behavior changed,
please make a ticket on the `issue tracker`_.

.. _issue tracker: https://github.com/tony/tmuxp/issues

YAML
~~~~

.. literalinclude:: ../examples/env-variables.yaml
    :language: yaml

JSON
~~~~

.. literalinclude:: ../examples/env-variables.json
    :language: json

Focusing
--------

tmuxp allows ``focus: true`` for assuring windows and panes are attached /
selected upon loading.

YAML
~~~~

.. literalinclude:: ../examples/focus-window-and-panes.yaml
    :language: yaml

JSON
~~~~

.. literalinclude:: ../examples/focus-window-and-panes.json
    :language: json

Window Index
------------

You can specify a window's index using the ``window_index`` property. Windows
without ``window_index`` will use the lowest available window index.

YAML
~~~~

.. literalinclude:: ../examples/window-index.yaml
    :language: yaml

JSON
~~~~

.. literalinclude:: ../examples/window-index.json
    :language: json

Automatic Rename
----------------

YAML
~~~~

.. literalinclude:: ../examples/automatic-rename.yaml
    :language: yaml

JSON
~~~~

.. literalinclude:: ../examples/automatic-rename.json
    :language: json

Main pane height
----------------

YAML
~~~~

.. literalinclude:: ../examples/main-pane-height.yaml
    :language: yaml

JSON
~~~~

.. literalinclude:: ../examples/main-pane-height.json
    :language: json

Super-advanced dev environment
------------------------------

.. seealso::
    :ref:`tmuxp developer config` in the :ref:`developing` section.

YAML
~~~~

.. literalinclude:: ../.tmuxp.yaml
    :language: yaml

JSON
~~~~

.. literalinclude:: ../.tmuxp.json
    :language: json

Bootstrap project before launch
-------------------------------

You can use ``before_script`` to run a script before the tmux session
starts building. This can be used to start a script to create a virtualenv
or download a virtualenv/rbenv/package.json's dependency files before
tmuxp even begins building the session.

It works by using the `Exit Status`_ code returned by a script. Your
script can be any type, including bash, python, ruby, etc.

A successful script will exit with a status of ``0``.

Important: the script file must be chmod executable ``+x`` or ``755``.

Run a python script (and check for it's return code), the script is
*relative to the ``.tmuxp.yaml``'s root* (Windows and panes omitted in 
this example):

.. code-block:: yaml

    session_name: my session
    before_script: ./bootstrap.py
    # ... the rest of your config

.. code-block:: json

    {
        "session_name": "my session",
        "before_script": "./bootstrap.py"
    }

Run a shell script + check for return code on an absolute path. (Windows
and panes omitted in this example)

.. code-block:: yaml

    session_name: another example
    before_script: /absolute/path/this.sh # abs path to shell script
    # ... the rest of your config

.. code-block:: json

    {
        "session_name": "my session",
        "before_script": "/absolute/path/this.sh"
    }

.. _Exit Status: http://tldp.org/LDP/abs/html/exit-status.html

Per-project tmux config
-----------------------

You can load your software project in tmux by placing a ``.tmuxp.yaml`` or
``.tmuxp.json`` in the project's config and loading it.

tmuxp supports loading configs via absolute filename with ``tmuxp load``
and via ``$ tmuxp load .`` if config is in directory.

.. code-block:: bash

    $ tmuxp load ~/workspaces/myproject.yaml

See examples of ``tmuxp`` in the wild. Have a project config to show off?
Edit this page.

* https://github.com/tony/dockerfiles/blob/master/.tmuxp.yaml
* https://github.com/tony/pullv/blob/master/.tmuxp.yaml
* https://github.com/tony/sphinxcontrib-github/blob/master/.tmuxp.yaml

You can use ``start_directory: ./`` to make the directories relative to
the config file / project root.

Kung fu
-------

.. note::

    tmuxp sessions can be scripted in python. The first way is to use the
    ORM in the :ref:`API`. The second is to pass a :py:obj:`dict` into
    :class:`tmuxp.WorkspaceBuilder` with a correct schema. See:
    :meth:`tmuxp.config.validate_schema`.

Add yours? Submit a pull request to the `github`_ site!

.. _github: https://github.com/tony/tmuxp
