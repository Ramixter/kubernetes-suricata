# Suricata & Kubernetes

What we are going to want is to monitor the network traffic towards the nodes of our Kubernetes cluster.

The configuration files that we will use are the following:

- [suricata-suricata.yaml](suricata-suricata.yaml)
- [suricata-rules.yaml](suricata-rules.yaml)
- [suricata-daemonset.yaml](suricata-daemonset.yaml)
- [suricata.yaml](suricata.yaml)
- [suricata.rules](suricata.rules)

---

# Setting up the configuration

We have the files [suricata.yaml](suricata.yaml) and [suricata.rules](suricata.rules) which are the ones that we will have to modify for the configuration of Suricata inside the nodes.

In our case we are going to create a configuration file [suricata-suricata.yaml](suricata-suricata.yaml) with the content of [suricata.yaml](suricata.yaml) to be able to create additional configurations. In the same way we will create the file [suricata-rules.yaml](suricata-rules.yaml) with the content of [suricata.rules](suricata.rules) to proceed to create the rules that we want suricata to be executing.

# Applying the configuration

We will have the option to apply the configuration in our kubernetes cluster with the following options:

## Option 1:

``` bash
kubectl create -f suricata-suricata.yaml
kubectl create -f suricata-rules.yaml
kubectl create -f suricata-daemonset.yaml
```

## Option 2:

``` bash
kubectl create configmap suricata-config --from-file=suricata.yaml=suricata.yaml
kubectl create configmap suricata-rules --from-file=suricata.rules
kubectl create -f suricata-daemonset.yaml
```

# Checking Suricata

The rules that we have configured are to perform a test using ICMP packets through Ping that have been configured in the [suricata.rules](suricata.rules)[[suricata-rules.yaml](suricata-rules.yaml)] file.

Suppose we have 3 nodes in our cluster with which we are going to do the tests to verify that our configuration works.


