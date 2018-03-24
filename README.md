# Wazuh containers for Docker

[![Slack](https://img.shields.io/badge/slack-join-blue.svg)](https://goo.gl/forms/M2AoZC4b2R9A9Zy12)
[![Email](https://img.shields.io/badge/email-join-blue.svg)](https://groups.google.com/forum/#!forum/wazuh)
[![Documentation](https://img.shields.io/badge/docs-view-green.svg)](https://documentation.wazuh.com)
[![Documentation](https://img.shields.io/badge/web-view-green.svg)](https://wazuh.com)

In this repository you will find the containers to run:

* wazuh: It runs the Wazuh manager, Wazuh API and Filebeat (for integration with Elastic Stack)
* wazuh-logstash: It is used to receive alerts generated by the manager and feed Elasticsearch using an alerts template
* wazuh-kibana: Provides a web user interface to browse through alerts data. It includes Wazuh plugin for Kibana, that allows you to visualize agents configuration and status.

In addition, a docker-compose file is provided to launch the containers mentioned above. It also launches an Elasticsearch container (working as a single-node cluster) using Elastic Stack Docker images.

## Current release

Containers are currently tested on Wazuh version 3.2.1 and Elastic Stack version 6.2.2. We will do our best to keep this repository updated to latest versions of both Wazuh and Elastic Stack.

## Installation notes

To run all docker instances you can just run ``docker-compose up``, from the directory where you have docker-compose.yml file. The following is part of the expected behavior when setting up the system:

* Both wazuh-kibana and wazuh-logstash containers will run multiple queries to Elasticsearch API using curl, to learn when Elasticsearch is up. It is expected to see several ``Failed to connect to elasticsearch port 9200`` log messages, until Elasticesearch is started. Then the set up process will continue normally.
* Kibana container can take a few minutes to install Wazuh plugin, this takes place after ``Optimizing and caching browser bundles...`` is printed out.
* It is recommended to set Docker host preferences to give at least 4GB memory per container (this doesn't necessarily mean they all will use it, but Elasticsearch requires them to work properly).

Once installed you can browse through the interface at: https://127.0.0.1.

## Mount custom Wazuh configuration files

To mount custom Wazuh configuration files in the Wazuh manager container, mount them in the `/wazuh-config-mount` folder. For example, to mount a custom `ossec.conf` file, mount it in `/wazuh-config-mount/etc/ossec.conf` and the [run.sh](wazuh/config/run.sh) script will copy the file at the right place on boot while respecting the destination file permissions.

Here is an example of a `/wazuh-config-mount` folder used to mount some common custom configuration files:
```
root@wazuh-manager:/# tree /wazuh-config-mount/
/wazuh-config-mount/
└── etc
    ├── ossec.conf
    ├── rules
    │   └── local_rules.xml
    └── shared
        └── default
            └── agent.conf

4 directories, 3 files
```

In that case, you will see this in the Wazuh manager logs on boot:
```
Identified Wazuh configuration files to mount...
'/wazuh-config-mount/etc/ossec.conf' -> '/var/ossec/data/etc/ossec.conf'
'/wazuh-config-mount/etc/rules/local_rules.xml' -> '/var/ossec/data/etc/rules/local_rules.xml'
'/wazuh-config-mount/etc/shared/default/agent.conf' -> '/var/ossec/data/etc/shared/default/agent.conf'
```

## More documentation

* [Wazuh full documentation](http://documentation.wazuh.com)
* [Wazuh documentation for Docker](https://documentation.wazuh.com/current/docker/index.html)
* [Docker hub](https://hub.docker.com/u/wazuh)

## Credits

These Docker containers are based on:

*  "deviantony" dockerfiles which can be found at [https://github.com/deviantony/docker-elk](https://github.com/deviantony/docker-elk)
*  "xetus-oss" dockerfiles, which can be found at [https://github.com/xetus-oss/docker-ossec-server](https://github.com/xetus-oss/docker-ossec-server)

We thank you them and everyone else who has contributed to this project.

## Wazuh official website

[Wazuh website](http://wazuh.com)
