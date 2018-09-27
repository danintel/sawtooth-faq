Unofficial FAQ: Using Docker with Sawtooth
====================
[PREVIOUS_ | HOME_ | NEXT_]

.. contents::

.. Warning::
   **This FAQ was written by a non-expert so may contain both fiction and fact!**

Can I run Sawtooth without Docker?
--------------------------
Yes.

How can I run ``docker`` or ``docker-compose`` without prefixing it with ``sudo``?
--------------------------------------
Sometimes, adding your login to group ``docker`` is recommended, such as with command: ``sudo usermod -aG docker $USER`` . However, this gives ``$USER`` root-equivalent permissions.  A better alternate is to define an alias for docker and docker-compose and add to your ~/.bashrc file:

.. code:: sh

    alias docker='sudo docker'
    alias docker-compose='sudo docker-compose'

For details, see https://docs.docker.com/install/linux/linux-postinstall/#manage-docker-as-a-non-root-user


I get this error running ``docker-compose -f sawtooth-default.yaml up`` : ``Error: files exist, rerun with --force to overwrite existing files``
--------------------------------------
This occurs when docker was not halted cleanly.  Run the following first:

.. code:: sh

    sudo docker-compose -f sawtooth-default.yaml down

Then this:

.. code:: sh

    sudo docker-compose -f sawtooth-default.yaml up

An alternate solution is to force it to ignore the existing files:

.. code:: sh

    docker-compose -f docker/compose/sawtooth-default.yaml up --force

I get this error running ``docker-compose -f sawtooth-default.yaml up`` : ``ERROR: Couldn't connect to Docker daemon at http+docker://localhost - is it running?``
-------------------
If it's at a non-standard location, specify the URL with the DOCKER_HOST environment variable.

You may not have enough permission to run. Try prefixing with sudo: ``sudo docker-compose ...``
To determine if sure docker is running and to start Docker, type:

.. code:: sh

    service docker status
    sudo service docker start

I get this error running ``docker run hello-world`` :  ``Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.37/containers/json: dial unix /var/run/docker.sock: connect: permission denied``
-------------------
Try running with sudo.  For example: sudo docker run hello-world.
Here's a few aliases you can add to your ``~/.bashrc`` file:

.. code:: sh

    alias docker='sudo docker'
    alias docker-compose='sudo docker-compose'


I get this error running ``docker run hello-world`` : ``docker: Error response from daemon: Get https://registry-1.docker.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers).``
-------------------
If it worked before, first try restarting docker:

.. code:: sh

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

I get this error: ``ERROR: repository . . . not found: does not exist or no pull access``
------------
Also a proxy problem--see the answer above.

I get this error: `` ERROR: Service . . . failed to build: Get . . . net/http: request canceled while waiting for connection
------------
Also a proxy problem--see the answer above.

I get this error running docker-compose: ``ERROR: for validator  Cannot create container for service validator: Conflict. The container name "/validator" is already in use by container ...``
-------------------------------
The container already exists.  You need to remove or rename it. To remove:

.. code:: sh

    sudo docker ps -a # list container IDs
    sudo docker stop <container ID>
    sudo docker rm <container ID>

How do I display the logs for a Docker container?
---------------------------
Use the ``sudo docker logs`` command followed by the container name.
The container name may be found with the ``sudo docker ps`` command.
For example: ``sudo docker logs validator`` display the log for the container named ``validator`` .

I get this error running docker-compose: ``ERROR: Version in "./docker-compose.yaml" is unsupported.``
-------------------------------
You may be running an old version of Docker, perhaps from your Linux package manager.  Instead, install Docker from docker.com. Sawtooth requires Docker Engine 17.03.0-ce or better. For Docker CE for Ubuntu, use https://docs.docker.com/install/linux/docker-ce/ubuntu/
Here's a sample script that installs Docker CE on Ubuntu:
https://gist.github.com/askmish/76e348e34d93fc22926d7d9379a0fd08

If I run ``docker`` or ``docker-compose`` it hangs and does nothing.
--------------------------------------
The docker daemons may not be running.  To check, run:

.. code:: sh

     $ ps -ef | grep dockerd

To start, run:

.. code:: sh

    $ sudo systemctl restart docker.service

How do I manually start and stop docker on Linux?
--------------------------------------
.. code:: sh

    $ sudo service docker start
    $ service docker status
    $ sudo service docker stop

How do I enable and disable automatic start of docker on boot on Linux?
--------------------------------------
.. code:: sh

    $ sudo systemctl enable docker
    $ systemctl status docker
    $ sudo systemctl disable docker

Can I connect a client to the REST API running in a Docker container?
-------------------------------
Yes. The ``docker-compose.yaml`` needs the following lines for the REST container:

::

    expose:
      - 8008
    ports:
      - '8008:8008'

Then connect your client to processor to port ``http://localhost:4040``
This might be a command line option for the client
(for example, ``intkey --url http://localhost:4040``).
Otherwise, you need to modify the source if the REST API URL is hard-coded
for your client.


Can I connect a transaction processor to the validator running in a Docker container?
-------------------------------
Yes. The ``docker-compose.yaml`` needs the following lines for the validator container (which maps Docker container TCP port 4004 to external port 4040):

::

    expose:
      - 4004
    ports:
      - '4040:4004'

Then connect your transaction processor to port ``tcp://localhost:4040``
If the port is mapped to 4004 (that is, not mapped to 4040), use ``tcp://localhost:4040``
The port might be a command line option for the TP.
(for example, ``intkey-tp-python -v tcp://localhost:4040`` ).
Otherwise, you need to modify the source if the validator port is hard-coded
for your TP.

I get ``You cannot remove a running container`` error removing docker containers
-------------------
Before running ``docker rm $(docker ps -aq)``, first stop the running containers with
``sudo docker stop $(docker ps -q)``

How do I run Sawtooth with Kubernetes?
-------------------------------------------
Kubernetes requires VirtualBox or some other virtual machine software.
Documentation on using Kubernetes with Minikube for Sawtooth on Linux or Mac hosts is available here:
https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/kubernetes.html
https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/creating_sawtooth_network.html#kubernetes-start-a-multiple-node-sawtooth-network

Can Docker run inside a virtual machine?
----------------------
Yes. For example, I run Docker with Sawtooth containers on a VirtualBox virtual machine instance on a Windows 10 host.

How do I persist data on Docker containers?
----------------------------
You add an external volume. You make a directory for your volume and add it using ``volumes:`` in your Docker .yaml file. For a Sawtooth-specific tutorial, see this blog: http://goshtastic.blogspot.com/2018/04/making-new-transaction-family-on.html
Also see the Docker storage documentation at https://docs.docker.com/storage/

If you do not ``down`` the container or reboot the Docker host, the container will not be destroyed.

For a list of directories used by Sawtooth, see https://github.com/danintel/sawtooth-faq/blob/master/validator.rst#what-files-does-sawtooth-use
It is best to set `$SAWTOOTH_HOME` so all the configuration and data is under one root directory.

[PREVIOUS_ | HOME_ | NEXT_]

.. _PREVIOUS: rest.rst
.. _HOME: README.rst
.. _NEXT: glossary.rst

© Copyright 2018, Intel Corporation.
