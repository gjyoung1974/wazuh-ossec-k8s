# wazuh-kubernetes
Wazuh OSSEC for Kubernetes with a embedded ELK stack.

## Abstract
Wazuh best practices recommend deploying multiple instances of the Wazuh manager so it can support a larger amount of events and can be fault tolerant.
* `Master` node - intended to expose the Wazuh API, manage agents registration
* `Client` nodes - intended to receive agents events

Based on [Wazuh documentation](https://documentation.wazuh.com/current/user-manual/manager/wazuh-cluster.html) 

Master and client nodes are all behind an internal AWS elastic load balancer, so the events from agents are dispatched to any available nodes in the cluster. Kubernetes will ensure that the cluster stays highly available.

This deployment leverages  the Docker images from:    
[wazuh-docker](https://github.com/wazuh/wazuh-docker). 


* The Wazuh API will be available at wazuh-api.{some-domain.com}:55000
* All manager nodes of your Wazuh manager cluster should be reachable at wazuh-manager.{some-domain.com}:1514
* Kibana and the Wazuh Kibana application should be available at https://wazuh.some-domain.com:443

## Wazuh agents deployment
This repository does not show how to deploy the Wazuh agent in a Kubernetes cluster. Normally, we would use a DaemonSet to deploy the agent on each Kubernetes node. To do that, we would need a Docker image with the Wazuh agent installed on it and then we would need to mount almost every folder of your host inside that container (`/bin`, `/etc`, `/var/log`, etc.). It would be a very complicated task since you cannot simply mount the `/bin` folder of your host in the `/bin` folder of your container. Therefore, creating such Docker image and using it in a Kubernetes DaemonSet is not the ideal way to deploy a Wazuh agent. Instead, you should take a look at the [Wazuh Ansible playbooks project](https://github.com/wazuh/wazuh-ansible) or at the [Wazuh Puppet module project](https://github.com/wazuh/wazuh-puppet) to deploy your Wazuh agents.

## TODO
* Secure the Wazuh API: Harden + integrate SSO
* In production: use an enterprise class ELK cluster instead of a single node.

```
2018 Gordon Young gjyoung1974@gmail.com
```

`//TODO`
* Use EFK vs Logstash    
* Make the Kibana deployment highly available.    
* Provide a host DaemonSet example for coverage of the k8s nodes
===

