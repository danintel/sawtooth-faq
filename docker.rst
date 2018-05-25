FAQ: Using Docker with Sawtooth
====================

I get this error running "docker-compose -f sawtooth-default.yaml up" Error: files exist, rerun with --force to overwrite existing files
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

I get this error running "docker-compose -f sawtooth-default.yaml up" ERROR: Couldn't connect to Docker daemon at http+docker://localhost - is it running?
-------------------
If it's at a non-standard location, specify the URL with the DOCKER_HOST environment variable.

You may not have enough permission to run. Try "sudo docker-compose ..."
See if sure docker is running with and start Docker with:

::

    service docker status
    sudo service docker start

I get this error running "docker run hello-world": Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.37/containers/json: dial unix /var/run/docker.sock: connect: permission denied
-------------------

Try running with sudo.  For example: sudo docker run hello-world

I get this error running ``docker run hello-world`` : ``docker: Error response from daemon: Get https://registry-1.docker.io/v2/: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers). ``
-------------------

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


If I run docker or docker-compose it hangs and does nothing.
-------------------

The docker daemons may not be running.  To check, run:

::

     $ ps -ef | grep dockerd

To start, run:

::

    $ sudo systemctl restart docker.service

How do I manually start and stop docker on Linux?
-------------------
::

    $ sudo service docker start
    $ service docker status
    $ sudo service docker stop

How do I enable and disable automatic start of docker on boot on Linux?
-------------------
::

    $ sudo systemctl enable docker
    $ systemctl status docker
    $ sudo systemctl disable docker

© Copyright 2018, Intel Corporation.
