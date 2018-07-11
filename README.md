# EDCOP Kibana Guide

Table of Contents
-----------------
 
* [Configuration Guide](#configuration-guide)
	* [Image Repository](#image-repository)
	* [Networks](#networks)
	* [Node Selector](#node-selector)
	* [Ingress](#ingress)
	
# Configuration Guide

Within this configuration guide, you will find instructions for modifying Kibana's helm chart. All changes should be made in the *values.yaml* file.
Please share any bugs or features requests via GitHub issues.
 
## Image Repository

By default, the Kibana image is pulled from Elastic.co directly. If you're changing this value, make sure you use the full repository name.
 
```
image:
  kibana: docker.elastic.co/kibana/kibana:6.2.4
```

## Networks

Kibana only uses an overlay network to communicate because it isn't a network sensing tool and doesn't monitor any traffic. By default, the overlay network is named *calico*. 

```
networks:
  overlay: calico
```
 
To find the names of your networks, use the following command:
 
```
# kubectl get networks
NAME		AGE
calico		1d
passive		1d
inline-1	1d
inline-2	1d
```

## Node Selector

This value tells Kubernetes which hosts the deployment should be deployed to by using labels given to the hosts. Hosts without the defined label will not receive pods. By default Kibana will be deployed to any system labelled as infrastructure equal to true.
 
```
nodeSelector:
  nodetype: infrastructure
```
 
To find out what labels your hosts have, please use the following:
```
# kubectl get nodes --show-labels
NAME		STATUS		ROLES		AGE		VERSION		LABELS
master 		Ready		master		1d		v1.9.1		...,nodetype=master
minion-1	Ready		<none>		1d		v1.9.1		...,nodetype=minion
minion-2	Ready		<none>		1d		v1.9.1		...,nodetype=minion
```
## Ingress

This value must be set to function correctly.  This is the hostname that will be used to access Kibana.  The value must inside a valid sub domain of you edcop cluster.

```
ingress:
  # Enter the FQDN of how you will access kibana.
  host: kibana.physical.edcop.io  
```
