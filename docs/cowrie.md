Cowrie Honeypot
===============

The CommunityHoneyNetwork Cowrie Honeypot is an implementation of [@micheloosterhof's Cowrie](https://github.com/micheloosterhof/cowrie), configured to report logged attacks to the CommunityHoneyNetwork management server.

> "Cowrie is a medium interaction SSH and Telnet honeypot designed to log brute force attacks and the shell interaction performed by the attacker."

## Configuring Cowrie to talk to the CHN management server

Prior to starting, Cowrie will parse some options from `/etc/sysconfig/cowrie` for RedHat-based or `/etc/default/cowrie` for Debian-based systems or containers.  The following is an example config file:

```
#
# This can be modified to change the default setup of the cowrie unattended installation

DEBUG=false

# Server to stream data to
FEEDS_SERVER="http://localhost"
FEEDS_SERVER_PORT="10000"

# Deploy key from the FEEDS_SERVER administrator
# This is a REQUIRED value
DEPLOY_KEY=

# Registration information file
# If running in a container, this needs to persist
# COWRIE_JSON="/etc/cowrie/cowrie.json

# SSH Listen Port
# Can be set to 22 for deployments on real servers
# or left at 2222 and have the port mapped if deployed 
# in a container
SSH_LISTEN_PORT=2222

```

### Configuration Options

The following options are supported in the `/etc/sysconfig/cowrie` and `/etc/default/cowrie files`:

* DEBUG: (boolean) Enable more verbose output to the console
* FEEDS_SERVER: (string) The hostname or IP address of the HPFeeds server to send logged events.  This is likely going to be the CHN management server.
* FEEDS_SERVER_PORT: (integer) The HPFeeds port.  Default is 10000.
* DEPLOY_KEY: (string; REQUIRED) The deploy key provided by the feeds server administration for registration during the first startup.  This key is **required** for registration.
* COWRIE_JSON: (string) The location to store the registration information returned from the HPFeeds server.
* SSH_LISTEN_PORT: (integer) The port for the Cowrie daemon to listen on for SSH connections.  In containerized applications, this is _inside the container_, and the port can still be mapped to a different port on the host.

## Deploying Cowrie with Docker and docker-compose

Deploying Cowrie with Docker and docker-compose is covered in the [Your First Honeypot](firstpot.md) documentation.

## Deploying Cowrie without Docker

While deploying Cowrie in a container is the strongly suggested method, it can be deployed directly on a host, without the use of Docker, by running an [Ansible](https://www.ansible.com/) playbook.

_The warnings, prerequisites, supported operating systems and other notes from the overall [Deploying Without Docker](nondocker.md) documentation apply._

To deploy the latest CommunityHoneyNetwork Cowrie honeypot without Docker:

    cd /opt
    git clone https://github.com/CommunityHoneyNetwork/cowrie/
    cd cowrie
    echo "localhost ansible_connection=local" inventory.txt
    ansible-playbook cowrie.yml -i inventory.txt -c local

After installation, adjust the `/etc/sysconfig/cowrie` or `/etc/default/cowrie` file as described above to register the honeypot with the CHN management server.

As with other CommunityHoneyNetwork projects, Cowrie relies on [Runit](http://smarden.org/runit/) as its process manager.   See the [Runit Section](nondocker.md#Runit) section of the Deploying Without Docker documentation for more information.

## Acknowledgements

CommunityHoneyNetwork Cowrie container is an adaptation of [@micheloosterhof's Cowrie](https://github.com/micheloosterhof/cowrie) Cowrie software and [Threatstream's Modern Honey Network](https://threatstream.github.io/mhn/) Cowrie & HPFeeds work, among other contributors and collaborators.

## License

The Cowrie software is [Copyright (c) 2009 Upi Tamminen](https://raw.githubusercontent.com/micheloosterhof/cowrie/master/LICENSE.md) All rights reserved.

The ThreatStream implementation of Cowrie with HPFeeds, upon which CommunityHoneyNetwork is based is licensed under the [GNU LESSER GENERAL PUBLIC LICENSE Version 2.1](https://raw.githubusercontent.com/threatstream/mhn/master/LICENSE)

The [CommunityHoneyNetwork Cowrie deployment model and code](https://github.com/CommunityHoneyNetwork/cowrie) is therefore also licensed under the [GNU LESSER GENERAL PUBLIC LICENSE Version 2.1](https://raw.githubusercontent.com/CommunityHoneyNetwork/cowrie/master/LICENSE)
