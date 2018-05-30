Preliminary FAQ: Using Docker with Sawtooth
====================
.. contents::

.. **Warning**::

   This FAQ was written by a non-expert so may be both fiction and fact!

How can I run ``docker`` or ``docker-compose`` without prefixing it with ``sudo``?
--------------------------------------
Sometimes, adding your login to group ``docker`` is recommended, such as with command: ``sudo usermod -aG docker $USER`` . However, this gives ``$USER`` root-equivalent permissions.  A better alternate is to define an alias for docker and docker-compose and add to your ~/.bashrc file:

::

    alias docker='sudo docker'
    alias docker-compose='sudo docker-compose'

For details, see https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user

I get this error running ``docker-compose -f sawtooth-default.yaml up`` : ``Error: files exist, rerun with --force to overwrite existing files``
-------------------

This occurs when docker was not halted cleanly.  Run the following first:

::

    sudo docker-compose -f sawtooth-default.yaml down

Then this:

::

    sudo docker-compose -f sawtooth-default.yaml up

An alternate solution is to force it to ignore the existing files:

::

    docker-compose -f docker/compose/sawtooth-default.yaml up --force

I get this error running ``docker-compose -f sawtooth-default.yaml up`` : ``ERROR: Couldn't connect to Docker daemon at http+docker://localhost - is it running?``
-------------------
If it's at a non-standard location, specify the URL with the DOCKER_HOST environment variable.

You may not have enough permission to run. Try prefixing with sudo: ``sudo docker-compose ...``
To determine if sure docker is running and to start Docker, type:

::

    service docker status
    sudo service docker start

I get this error running ``docker run hello-world`` :  ``Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.37/containers/json: dial unix /var/run/docker.sock: connect: permission denied``
-------------------

Try running with sudo.  For example: sudo docker run hello-world

I get this error running ``docker run hello-world`` : ``docker: Error response from daemon: Get https://registry-1.docker.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers).``
-------------------
If it worked before, first try restarting docker:

::

   sudo service docker start; sudo service docker stop

If you are behind a network firewall, it is usually a proxy problem.
Proxy configurations are firewall-dependent, but this might serve as a pattern:

::

    # /etc/default/docker
    export http_proxy="http://proxy.mycompany.com:911/"
    export https_proxy="https://proxy.mycompany.com:912/"
    export no_proxy=".mycompany.com,10.0.0.0/8,192.168.0.0/16,localhost,127.0.0.0/8"

::

    # /etc/systemd/system/docker.service.d/override.conf
    Environment="HTTP_PROXY=http://proxy.mycompany.com:911/"
    Environment="HTTPS_PROXY=http://proxy.mycompany.com:912/"
    Environment="FTP_PROXY=http://proxy.mycompany.com:911/"
    Environment="NO_PROXY=.mycompany.com,10.0.0.0/8,192.168.0.0/16,localhost,127.0.0.0/8"

I get this error running docker-compose:
``ERROR: for validator  Cannot create container for service validator: Conflict. The container name "/validator" is already in use by container ...``
-------------------------------
The container already exists.  You need to remove or rename it. To remove:

::

    sudo docker ps -a # list container IDs
    sudo docker stop <container ID>
    sudo docker rm <container ID>

I get this error running docker-compose: ``ERROR: Version in "./docker-compose.yaml" is unsupported.``
-------------------------------
You may be running an old version of Docker, perhaps from your Linux package manager.  Instead, install Docker from docker.com. Sawtooth requires Docker Engine 17.03.0-ce or better. For Docker CE for Ubuntu, use https://docs.docker.com/install/linux/docker-ce/ubuntu/
Here's a sample script that installs Docker CE on Ubuntu:
https://gist.github.com/askmish/76e348e34d93fc22926d7d9379a0fd08

If I run ``docker`` or ``docker-compose`` it hangs and does nothing.
--------------------------------------
The docker daemons may not be running.  To check, run:

::

     $ ps -ef | grep dockerd

To start, run:

::

    $ sudo systemctl restart docker.service

How do I manually start and stop docker on Linux?
-------------------------------------- 
::

    $ sudo service docker start
    $ service docker status
    $ sudo service docker stop

How do I enable and disable automatic start of docker on boot on Linux?
-------------------------------------- 
::

    $ sudo systemctl enable docker
    $ systemctl status docker
    $ sudo systemctl disable docker

[`PREVIOUS`_] [`NEXT`_]
=========

.. _PREVIOUS: rest.rst
.. _NEXT: glossary.rst

© Copyright 2018, Intel Corporation.
