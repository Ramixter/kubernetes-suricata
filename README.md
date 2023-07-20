# Suricata & Kubernetes

What we are going to want is to monitor the network traffic towards the nodes of our Kubernetes cluster.

The configuration files that we will use are the following:

- [suricata-configmap.yaml](suricata-configmap.yaml)
- [suricata-rules.yaml](suricata-rules.yaml)
- [suricata-daemonset.yaml](suricata-daemonset.yaml)
- [suricata.yaml](suricata.yaml)
- [suricata.rules](suricata.rules)

---

We have the files [suricata.yaml](suricata.yaml) and [suricata.rules](suricata.rules) which are the ones that we will have to modify for the configuration of Suricata inside the nodes.


