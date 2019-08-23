Sawtooth FAQ: Installation and Configuration
============================================

[PREVIOUS_ | FAQ_ | NEXT_]

.. contents::


How do I list and install Sawtooth packages?
--------------------------------------------
The following setups the Sawtooth stable repository, lists the packages,
and installs the core packages
(sawtooth, python3-sawtooth-cli, python3-sawtooth-sdk, python3-sawtooth-signing):

.. code:: sh

    $ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 8AA7AF1F1091A5FD
    $ sudo add-apt-repository 'deb [arch=amd64] http://repo.sawtooth.me/ubuntu/bumper/stable xenial universe'
    $ sudo apt update
    $ apt install sawtooth python3-sawtooth-*
    $ apt search sawtooth

For more, up-to-date installation information see
https://sawtooth.hyperledger.org/docs/core/releases/latest/sysadmin_guide/installation.html

How do I edit Sawtooth settings?
--------------------------------
With ``.toml`` configuration files in ``/etc/sawtooth`` .
Examples are in the directory as ``.toml.example`` .
For details, see
https://sawtooth.hyperledger.org/docs/core/nightly/master/sysadmin_guide/configuring_sawtooth.html

Configuration files include:

``validator.toml``
	Validator configuration file
``rest_api.toml``
	REST API configuration file
``cli.toml``
	Sawtooth CLI configuration file
``poet_enclave.toml``
	PoET SGX Enclave configuration file
``path.toml``
	Directory path configuration (or use ``$SAWTOOTH_HOME``)
``identity.toml``
	Identity TP configuration file
``settings.toml``
	Settings TP configuration file
``log_config.toml``
	Log configuration file

More transaction-processor specific configuration files may be present.

I get this error installing Ubuntu packages: ``Could not get lock /var/lib/dpkg/lock``
--------------------------------------------------------------------------------------
The file lock is probably left over from a previous failed install.
The solution is ``sudo rm  /var/lib/dpkg/lock`` .
This assumes you're not multitasking and installing something else in another terminal on the same host.

I get this error installing ``sawtooth-cxx-sdk``: ``Depends: protobuf but it is not installable``
-------------------------------------------------------------------------------------------------
The C++ SDK package is in the nightly repository.
Until the package dependency is fixed, here's a workaround to force an install:


.. code:: sh

    $ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 44FC67F19B2466EA
    $ sudo apt-add-repository "deb [trusted=yes] http://repo.sawtooth.me/ubuntu/nightly bionic universe"
    $ sudo apt update
    $ apt download sawtooth-cxx-sdk
    $ sudo dpkg -i  sawtooth-cxx-sdk_1.1.1.dev808_amd64.deb
    $ pkg contents sawtooth-cxx-sdk

I get this error when running ``sawtooth setting list`` or ``xo list`` : ``Error 503: Service Unavailable``
-----------------------------------------------------------------------------------------------------------
This usually occurs when there is no genesis node created. To create, type the following:

.. code:: sh

    # Create the genesis node:
    sawtooth keygen
    sawset genesis
    sudo -u sawtooth sawadm genesis config-genesis.batch
    # Start the validator:
    sudo sawadm keygen
    sudo -u sawtooth sawtooth-validator -vv

I get this error when running ``sudo -u sawtooth sawadm genesis config-genesis.batch`` : ``Permission denied``
--------------------------------------------------------------------------------------------------------------
Change to a sawtooth user-writable directory before running the command and make sure file `config-genesis.batch` does not already exist: ``cd /tmp; ls config-genesis.batch``

I get a ``Key file is not readable`` error when starting ``sudo -u sawtooth sawtooth-validator -vv``
----------------------------------------------------------------------------------------------------
The validator key file permissions are wrong. To fix it, type:

.. code:: sh

    $ sudo
    $ sudo chown root:sawtooth /etc/sawtooth/keys /etc/sawtooth/keys/*
    $ sudo chmod 755 /etc/sawtooth/keys
    $ sudo chmod 640 /etc/sawtooth/keys/validator.priv
    $ sudo chmod 644 /etc/sawtooth/keys/validator.pub
    $ ls -la /etc/sawtooth/keys
    drwxr-xr-x 2 root sawtooth 4096 Jan 11 11:35 .
    -rw-r----- 1 root sawtooth   65 Jan 11 11:35 validator.priv
    -rw-r--r-- 1 root sawtooth   67 Jan 11 11:35 validator.pub

If the validator key files are missing, type ``sudo sawadm keygen``

How to I delete all blockchain data?
------------------------------------
Type the following: ``sudo -u sawtooth rm -rf /var/lib/sawtooth/*``
This deletes the entire database--for development and purposes.

How do I delete or change a specific block in the blockchain?
-------------------------------------------------------------
You cannot delete blocks--they are immutable by design.
You can create a new transaction (or block of transactions)
that reverse a previous transaction.

I get a usage error running ``sawnet peers`` or ``sawnet list-blocks``
----------------------------------------------------------------------
You should upgrade to the current release. These commands were added after the Sawtooth 1.0.4 release and are not available in earlier releases.

How can I change on-chain settings without deleting the blockchain?
-------------------------------------------------------------------
Use the ``sawset`` command. This allows you to change settings such
as maximum batches per block or target wait time.

I got error ``gpg: keyserver receive failed: keyserver error`` when executing ``sudo apt-key adv --keyserver hkp://keyserver...``
---------------------------------------------------------------------------------------------------------------------------------
This error means your machine couldn't add the supplied key to trusted list. This key is later used to authenticate and get sawtooth package.
One of the possible reason for this error is that your machine is trying to connect to keyserver through a proxy server. Add proxy server details in the command to solve this issue. For example, ``sudo apt-key adv --keyserver-options http-proxy=http://[username:password]@<proxyserver>:<port> --keyserver hkp://keyse...`` (notice usage of flag ``--keyserver-options`` here).

What does this error mean ``repository ... xenial InRelease' doesn't support architecture 'i386'``?
---------------------------------------------------------------------------------------------------
Following could be possibilities:

#. You installed on a 32-bit-only system. Install on a 64-bit system.

#. You are using 64-bit system, but your linux variant has enabled additional architecture i386. ``apt`` is expecting the repository for all configured architectures on your machine. One safe way to solve this error would be to tell ``apt`` to get only 64-bit repository. For example, ``sudo add-apt-repository 'deb [arch=amd64] http://repo.sawtooth.me/ubuntu.....'``.

I get this error running ``sawset``: ``ModuleNotFoundError: No module named 'colorlog'``
----------------------------------------------------------------------------------------
Something went wrong with installing Python dependencies or they were removed.
In this case, install ``colorlog`` with ``sudo apt install python3-colorlog`` or with``pip3 install colorlog``

I get this error starting Sawtooth: ``lmdb.DiskError: /var/lib/sawtooth/poet-key-state-03efb2aa.lmdb: No space left on device``
-------------------------------------------------------------------------------------------------------------------------------
Besides the obvious problem of no disk space, it could be your OS or filesystem does not support sparse files. The LMDB databases used by Sawtooth are 1TB sparse (mostly unallocated) files.

I get this error when running ``sawtooth sawadm genesis config-genesis.batch``:  ``Processing config-genesis.batch... Error: Unable to read config-genesis.batch``
------------------------------------------------------------------------------------------------------------------------------------------------------------------
This error can occur when there is no sawtooth user and group.
This should have been done by the package ``postinst`` script.
To add, type ``addgroup --system sawtooth; adduser --system --ingroup sawtooth sawtooth`` .

It could be a file or directory permission problem--try changing the file ownership with ``chown sawtooth:sawtooth config-genesis.batch`` and move it to a sawtooth-writable directory. For example ``mv config-genesis.batch /tmp; cd /tmp``

Another cause is the file doesn't exist. Create it with ``sawset genesis`` .


How do I install Sawtooth on Ubuntu?
------------------------------------
Follow the instructions at https://sawtooth.hyperledger.org/docs/core/releases/latest/app_developers_guide/ubuntu.html

These instructions are missing steps for installing and starting the DevMode consensus engine. If the consensus engine is not started, no new blocks can be published. The missing steps are:

* After the "Install Sawtooth" step, install the DevMode consensus engine package.

.. code:: sh

    $ sudo apt-get install sawtooth-devmode-engine-rust 

* After "Step 5: Start the Validator", start the DevMode consensus engine

.. code:: sh

    $ sudo -u sawtooth devmode-engine-rust -vv --connect tcp://localhost:5050

* A "Consensus engine registered" message should appear indicating the consensus engine connected with validator TCP port 5050 (for consensus messages).

How do I install Sawtooth on AWS?
---------------------------------
* Sign up for a free AWS Free Tier account, if you don't have an account. The AWS Free Tier is free for qualifying developers. This gives you 1 Micro instance (or any combination of instances up to 750 hours/month) for 12 months. See https://aws.amazon.com/free/
* Create your instance from the Hyperledger Sawtooth product page on AWS Marketplace, at https://aws.amazon.com/marketplace/pp/B075TKQCC2
* Follow instructions to launch an AWS Marketplace instance at
  https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launch-marketplace-console.html
* Then follow the instructions for using your Sawtooth AWS instance at
  https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/aws.html


How do I use ssh with AWS?
--------------------------
By default ssh access to AWS instances are disabled.
To enable, first paste the contents of your public key, at `` ~/.ssh/id_rsa.pub`` , to ``Key Pairs``  under your EC2 Dashboard. Use this key when creating your Sawtooth instance.

After creating your AWS Sawtooth instance, go to your EC2 Dashboard and click on the security group for your instance (usually ``default``). Select the ``Inbound`` tab and  ``Edit``. Add ``SSH`` (TCP port 22) and Source ``Anywhere`` (or ``My IP`` and your IP address) and save. I had to reboot the instance (Actions --> InstanceState --> Reboot) to get it to work.

How to I build Sawtooth from source?
------------------------------------
Run ``sudo docker-compose build`` from the root source directory. For instructions
see https://github.com/hyperledger/sawtooth-core/blob/master/BUILD.md

How do I install Sawtooth on FreeBSD?
-------------------------------------
Sawtooth is supported for Ubuntu Linux with binary packages.
For other other \*IX-like systems, including FreeBSD, you can build from source.
The following blog may help:
https://wiki.freebsd.org/HyperledgerSawtooth
This is based on FreeBSD 11.1. Docker is not required to run Sawtooth.
See also this bug for the status of the FreeBSD Sawtooth port:
https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=228581

I get this error while installing Sawtooth: ``Error starting userland proxy: listen tcp 0.0.0.0:8080: bind: address already in use``
------------------------------------------------------------------------------------------------------------------------------------
You already have a program running that uses TCP port 8080. Either kill it or change the port you use to something else.
To find the process(es) that have port 8080 open, type ``sudo lsof -t -i:8080``
Then kill the processes. Check again that they have not restarted. Also check that they are not Docker containers that have restarted.

I get this error after setting up a Sawtooth network: ``Can't send message PING_RESPONSE back to ... because connection OutboundConnectionThread- tcp://192.168.0.100:8800 not in dispatcher``
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
The usual problem when you get this message is configuring the peer endpoints

* If you are using Ubuntu directly instead of Docker, use the Validator's hostname or IP address instead of the default (``validator``), which only works with Docker, or ``localhost``, which may not be routable

* If you are using Docker, make sure the Docker ports are mapped to the Ubuntu OS, and that the OS IP address/port is routable between the two machines. Check the ``expose:`` and ``ports:`` entries in your ``docker-compose.yaml`` file or similar file

* Verify network connectivity to the remote machine with ``ping``

* Verify port connectivity ``telnet aremotehostname 8800`` (replace ``aremotehostname`` with the remote peer's hostname or IP address). Control-c out if it connects

* Verify network and port connectivity in the other direction (remote to local)

* Check peer configuration in your local and remote ``/etc/sawtooth/validator.toml`` files. Check the ``peering`` and ``endpoint`` lines. Check the ``seeds`` line (for dynamic peering) or ``peers`` line (for static peering)

* If you are connecting to external nodes on the network, bind to 0.0.0.0 (which means every IP address) or your external IP address instead of 127.0.0.1 (localhost only). Be careful doing this as may open up this port to external networks.

I get ``unmet dependencies`` errors installing Sawtooth on Ubuntu 18.04 LTS.
----------------------------------------------------------------------------
Ubuntu 18.04 LTS is supported only in the nightly development packages. Use Ubuntu 16.04 LTS for the stable release packages.
You can also install Sawtooth with Docker. See:
https://sawtooth.hyperledger.org/docs/core/releases/latest/app_developers_guide/docker.html

If you wish to install the nightly development packages on Ubuntu 18.04 LTS (Bionic), then, for now, specify the individual packages you wish to install instead of parent package ``sawtooth``.  For example, ``sudo apt-get install python3-sawtooth-cli python3-sawtooth-integration python3-sawtooth-rest-api python3-sawtooth-sdk python3-sawtooth-settings python3-sawtooth-signing python3-sawtooth-validator sawtooth-devmode-engine-rust``

For details, see https://jira.hyperledger.org/projects/STL/issues/STL-1465

I get this error installing Sawtooth: ``No matching distribution found for sawtooth_rest_api``
----------------------------------------------------------------------------------------------
You tried to install Sawtooth using Python pip.
I don't know if this could work. I know installing Sawtooth using Ubuntu/Debian installation tools (such as apt, apt-get, dpkg, aptitude) works OK.

I get this error with ``add-apt-repository``: Couldn't connect to accessibility bus: Failed to connect to socket . . . Connection refused
-----------------------------------------------------------------------------------------------------------------------------------------
It is just a warning and you can ignore it. Verify the Sawtooth repository was added in ``/etc/apt/sources.list`` The cause is the command tried to start a graphic display (probably over SSH) when it was not available. A workaround to remove the warning is to add ``export NO_AT_BRIDGE=1`` to ``~/.bashrc``

I get this error starting Sawtooth on Docker: ``No response from ... beginning heartbeat pings``
------------------------------------------------------------------------------------------------
It means that there has been no message exchange between the validators for 10 seconds. There is not necessarily a problem, but it could mean the the genesis node and peer nodes are not connecting.

How do I list sawtooth command line options?
--------------------------------------------
For the Sawtooth CLIs (sawadm, sawset, sawnet, sawtooth), append ``-h`` after the command to list subcommands (for example, ``sawadm -h`` ). For the Sawtooth subcommands, append ``-h`` after the subcommand (for example, ``sawadm keygen -h`` ).

Why do I get a ``No module named ...`` error running a Sawtooth program?
------------------------------------------------------------------------
The ``No module named`` error occurs in Python when a Python module is missing. The usual fix is to install the corresponding Python package. Something you need to prepend ``python3-`` to the name. So, for example, if you get a ``No module named 'netifaces'`` error, install the missing package with something like ``apt install python3-netifaces``

How do I fix this error: ``no transaction processors registered for processor type sawtooth_settings`` ??
--------------------------------------------------------------------------------------------------------------
You start the Settings TP, as follows ``sudo -u sawtooth settings-tp -v`` .
The Settings TP is always required for all Sawtooth nodes, even if you did not add or change any settings.

I get ``unmet dependencies`` errors on ``python3-sawtooth-poet-cli`` and other packages with ``sudo apt-get install -y sawtooth`` on Ubuntu Xenial
--------------------------------------------------------------------------------------------------------------------------------------------------
The Sawtooth PoET packages are not yet available for the unreleased Sawtooth nightly builds on Ubuntu 18.x LTS (Xenial). As a workaround do not install the meta-package ``sawtooth``.  Instead list the Sawtooth packages and install the packages you need. For example:

.. code:: sh

    $ apt search sawtooth # List Sawtooth packages (optional)
    $ sudo apt-get install python3-sawtooth-cli python3-sawtooth-integration \
        python3-sawtooth-rest-api python3-sawtooth-sdk \
        python3-sawtooth-settings python3-sawtooth-signing \
        python3-sawtooth-validator sawtooth-devmode-engine-rust

I get ``Unable to start validator; sawtooth.consensus.algorithm.name and sawtooth.consensus.algorithm.version must be set in the genesis block`` when installing Sawtooth nightly builds
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
The installation instructions for "Step 3" at
https://sawtooth.hyperledger.org/docs/core/nightly/master/app_developers_guide/ubuntu.html
are incomplete for Sawtooth nightly builds.
They work for Sawtooth 1.1, but for nightly builds the consensus engine setting is now required. The correct instructions in Step 3 for nightly builds are:

.. code:: sh

    $ cd /tmp
    $ sudo -u sawtooth sawset genesis -k /etc/sawtooth/keys/validator.priv
    $ sudo -u sawtooth sawset proposal create \
         -k /etc/sawtooth/keys/validator.priv \
         sawtooth.consensus.algorithm.name=Devmode \
         sawtooth.consensus.algorithm.version=0.1 -o config.batch
    $ sudo -u sawtooth sawadm genesis config-genesis.batch config.batch 

[PREVIOUS_ | FAQ_ | NEXT_]

.. _PREVIOUS: sawtooth.rst
.. _FAQ: README.rst
.. _NEXT: transaction-processing.rst

© Copyright 2018, Intel Corporation.
