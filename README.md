# ansible-docker-swarm-tls Readme
Ansible Playbooks to Deploy Docker Swarm with TLS (including certificate creation)

# Configuration Settings

There are a few settings that can be configured in site.yml:
* Certificate metadata
* discovery_fqdn: defaults to consul discovery service, can be commented out and replaced with existing service (ie zookeeper)
* docker_conf: filename for docker configuration template in docker/templates directory. Defaults to standard settings with discovery info
	* An additional config file, docker.devicemapper.conf.j2, is also present that uses the devicemapper storage opt with a default thinpool path of docker-thinpool

# Deploy New Swarm

'''Currently runs only on CentOS'''

Specify the server you would like to use to generate certificates in the [ca] group. These will be downloaded to the Ansible hosts in a playbook subdirectory named client-certs that will be created at runtime. Variables for CSRs are located in site.yml under the cert role (admin email, organization name, and organizational unit). Additional certificate parameters can be added to the openssl.cnf.j2 file in the cert templates directory.

The current playbook setup will use FQDNs for the Swarm consul, managers, and nodes. If your servers do not have a resolvable FQDN, the playbook can be updated to use host IPv4 by editing the references in the swarm-discovery and swarm-nodes tasks.

```shell
cd newswarm-playbook
ansible-playbook -i hosts -k site.yml
```

# Deploy New Nodes

To deploy additional nodes, make sure that the CA's private and public key are in the client-certs subdirectory of the playbook.

```shell
cd addnode-playbook
ansible-playbook -i newnodes -k site.yml
```