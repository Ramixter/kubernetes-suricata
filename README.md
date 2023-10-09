# Suricata & Kubernetes

What we are going to want is to monitor the network traffic towards the nodes of our Kubernetes cluster.

The configuration files that we will use are the following:

- [suricata-suricata.yaml](suricata-suricata.yaml)
- [suricata-rules.yaml](suricata-rules.yaml)
- [suricata-daemonset.yaml](suricata-daemonset.yaml)
- [suricata.yaml](suricata.yaml)
- [suricata.rules](suricata.rules)

---

> <picture>
>   <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Mqxx/GitHub-Markdown/main/blockquotes/badge/light-theme/example.svg">
>   <img alt="Example" src="https://raw.githubusercontent.com/Mqxx/GitHub-Markdown/main/blockquotes/badge/dark-theme/example.svg">
> </picture><br>
>
> Ejemplo


> <picture>
>   <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/Mqxx/GitHub-Markdown/main/blockquotes/badge/light-theme/example.svg">
>   <img alt="Example" src="https://raw.githubusercontent.com/Mqxx/GitHub-Markdown/main/blockquotes/badge/dark-theme/example.svg">
> </picture><br>
>
> Example


> **Example**
Your note text here

# Setting up the configuration

We have the files [suricata.yaml](suricata.yaml) and [suricata.rules](suricata.rules) which are the ones that we will have to modify for the configuration of Suricata inside the nodes.

In our case we are going to create a configuration file [suricata-suricata.yaml](suricata-suricata.yaml) with the content of [suricata.yaml](suricata.yaml) to be able to create additional configurations. In the same way we will create the file [suricata-rules.yaml](suricata-rules.yaml) with the content of [suricata.rules](suricata.rules) to proceed to create the rules that we want suricata to be executing.

  >In our case we are going to create a configuration file [suricata-suricata.yaml](suricata-suricata.yaml) with the content of [suricata.yaml](suricata.yaml) to be able to create additional configurations. In the same way we will create the file [suricata-rules.yaml](suricata-rules.yaml) with the content of [suricata.rules](suricata.rules) to proceed to create the rules that we want suricata to be executing.


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

Now if we ping any of the nodes we can verify that the suricata is recording the activity.

```bash
ping <NODE-IP>
```

```bash
kubectl exec -it <POD_NAME> -- cat /var/log/suricata/fast.log
```

```bash
kubectl exec -it <POD_NAME> -- cat /var/log/suricata/eve.json
```

---

### Example:

Suppose we have 3 nodes in our cluster with which we are going to do the tests to verify that our configuration works.

```text
NAME             READY   STATUS    RESTARTS   AGE   IP             NODE           NOMINATED NODE   READINESS GATES
suricata-7547n   1/1     Running   0          84s   192.168.49.4   master         <none>           <none>
suricata-lrhml   1/1     Running   0          84s   192.168.49.2   worker-1       <none>           <none>
suricata-s4gms   1/1     Running   0          84s   192.168.49.3   worker-2       <none>           <none>
```

```bash
ping 192.168.49.4
PING 192.168.49.4 (192.168.49.4) 56(84) bytes of data.
64 bytes from 192.168.49.4: icmp_seq=1 ttl=64 time=0.637 ms
64 bytes from 192.168.49.4: icmp_seq=2 ttl=64 time=0.128 ms
64 bytes from 192.168.49.4: icmp_seq=3 ttl=64 time=0.112 ms
```

```text
kubectl exec -it suricata-7547n -- cat /var/log/suricata/fast.log
07/20/2023-13:48:08.925591  [**] [1:1000001:1] ICMP Echo Request (Ping) [**] [Classification: (null)] [Priority: 3] {ICMP} 192.168.49.1:8 -> 192.168.49.4:0
07/20/2023-13:48:08.925731  [**] [1:1000002:1] ICMP Echo Reply (Ping Reply) [**] [Classification: (null)] [Priority: 3] {ICMP} 192.168.49.4:0 -> 192.168.49.1:0
07/20/2023-13:48:09.935639  [**] [1:1000001:1] ICMP Echo Request (Ping) [**] [Classification: (null)] [Priority: 3] {ICMP} 192.168.49.1:8 -> 192.168.49.4:0
07/20/2023-13:48:09.935704  [**] [1:1000002:1] ICMP Echo Reply (Ping Reply) [**] [Classification: (null)] [Priority: 3] {ICMP} 192.168.49.4:0 -> 192.168.49.1:0
07/20/2023-13:48:10.949103  [**] [1:1000001:1] ICMP Echo Request (Ping) [**] [Classification: (null)] [Priority: 3] {ICMP} 192.168.49.1:8 -> 192.168.49.4:0
07/20/2023-13:48:10.949164  [**] [1:1000002:1] ICMP Echo Reply (Ping Reply) [**] [Classification: (null)] [Priority: 3] {ICMP} 192.168.49.4:0 -> 192.168.49.1:0
```

---



