# Overview

This playbook deploys the Globus Connect Server on a Centos 7 server, configures it based on host variables and then registers an endpoint in Globus.  It assumes that you can already SSH to the CentOS 7 server, and can become `root` via `sudo`.  If the server is protected by a firewall or other security devices, the following rules shall be applied in advance:

| Source Network / Host        | Destination Network / Host       | Traffic Type  |  Port(s)       | Application             |
|------------------------------|----------------------------------|---------------|----------------|-------------------------|
| Any (Internet)               | Globus Endpoint                  | TCP           | 2811           | GridFTP control channel |
| Any (Internet)               | Globus Endpoint                  | TCP           | 50000-51000    | GridFTP data channel    |
| Globus Endpoint              | Any (Internet)                   | TCP           | 50000-51000    | GridFTP data channel    |
| Globus Endpoint              | ANY (Internet)                   | HTTPS         | 443            | Globus REST API         |
| Internal Network             | Globus Endpoint                  | HTTPS         | 443            | MyProxy OAuth           |


To execute this playbook, please run:

```shell
ansible-playbook ./deploy-globus-connect-server.yml --ask-valut-pass (optional: --limit hostname)
```

Argument `--ask-valut-pass` will prompt you for a password to decrypt encrypted files in the playbook.  You really should encrypt file `vars/secret.yml` by using `ansible-valut`.

The current version of this playbook has been developed to deploy a Globus endpoint on a single server and use *MyProxy OAuth* hosted on the sever itself for user authentication. You will need to change these scripts if you are using this playbook for different kinds of deployments.

## Globus Endpoint Creation and Registration

The task to register the target host with Globus is optional.  It only gets executed if host variable **create_endpoint** is defined and set to **true** on the host.

## Documentation for Globus Connect Server

* Go to [https://docs.globus.org/globus-connect-server-installation-guide/](https://docs.globus.org/globus-connect-server-installation-guide/), for documentation related to the deployment of a Globus Connect Server;
