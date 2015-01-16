Usage
=====

Tunir is a CI system which can run a set of commands/tests in a new cloud vm, or bare metal,
or in Docker containers based on the job configurations. It uses fabric project to connect
to the vm or bare metal tests.

The current version can be used along with cron ton run in predefined times.

Configuring a new job
----------------------

For each different job, two files are required. For example *default* job has two files,
**default.json** and **default.txt**.

jobname.json
-------------

This file is the main configuration for the job. Below is the example of one such job.

::

    {
      "name": "jobname",
      "type": "vm",
      "image": "file:///home/vms/Fedora-Cloud-Base-20141203-21.x86_64.qcow2",
      "ram": 2048,
      "user": "fedora",
      "password": "passw0rd"
    }

The possible keys are mentioned below.

name
    The name of the job, must match the filename.

type
    The type of system in which the tests will run. Possible values are vm, docker, bare.

image
    Path to the cloud image in case of a vm, you can provide docker image there for Docker based tests. It can also provide the ip/hostname of the bare metal box.

ram
    The amount of ram for the vm. Optional for bare or docker types.

user
    The username to connect to.

password
    The password of the given user. Right now for cloud vm(s) it is set to *passw0rd*.

jobname.txt
------------

This text file contains the bash commands to run in the system. One command on each line. In case you are
rebooting the system, you may want to use **SLEEP NUMBER_OF_SECONDS** command there.

If a command starts with @@ sign, it means the command is supposed to fail. Generally we check the return codes
of the commands to find if it failed, or not. For Docker container based systems, we track the stderr output.


Create the database schema
---------------------------
::

    $ python createdb.py

The path of the database is configured in the tunirlib/default_config.py file. Currently it stays under /tmp.


Start a new job
---------------

::

    $ sudo ./tunir_main.py --job jobname


Find the result from a job
--------------------------

::

    $ ./tunir_main.py --result JOB_ID

The above command will create a file *result-JOB_ID.txt* in the current directory. It will overwrite any file (if exits).
In case you want to just print the result on the console use the following command.::

    $ ./tunir_main.py --result JOB_ID --text

