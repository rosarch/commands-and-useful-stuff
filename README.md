* TOC
{:toc}

# K8

## Kubectl
```
k ### kubectl
```
```
kgp ## kubectl get pods ### (List all pods in the current namespace)
```
```
kga ## kubectl get pods --all-namespaces ### List all pods in all namespaces
```
```
kgpwide ## kubectl get pods -o wide ### (List all pods in the current namespace, with more details)
```
``` 
kl <pod> <container> ## kubectl logs ### (Get logs from single container)
```
```
kgp --all-namespaces -o wide |grep -i '<namespace>\|<namesspace>\|<namesspace>' ### Grep pods from defined workspaces
```
```
kgs ## kubectl get services ### (List all services in the namespace)
```
```
klf <pod> <container> ## kubectl logs -f ### (Similar to tail -f (one container))
```
```
kdp <pod> ## kubectl describe pod <pod>
```
```
kds ## kubectl describe service ##
```
```
kdi ## kubectl describe ingress ###
```
```
k port-forward <pod-name> 8080: <pod-port>
```
```
k port-forward <ingresss-pod-name> 8080:<ingress-port>
```
```
k port-forward service/<service-name>  8080:<service-port>
```
```
kgscf ## k get sites.cloudflare.com (on an external facing namespace)
```
```
live ## kubectl describe pod |grep -i -m 1 liveness ### (List details of liveness probe)
```
```
ready ## kubectl describe pod |grep -i -m 1 readiness ### (List details of readiness probe)
```
```
kgp <pod> -o yaml ## kubectl get <pod> -o yaml ### (Get a pod's YAML)
```
```
kr ## kubectl get pods --all-namespaces --sort-by=".status.containerStatuses[0].restartCount" -o wide ###
```
```
kubectl get replicasets` ### Shows replica count, desired against actual, add -w to watch
```
```
ks --replicas=0 deployment/<namespace> ## kubectl scale --replicas=0 deployment/<namespace> ### Manualy scale amount of replicas in namespace
```
```
ke <pod> <container> /bash ## kubectl exec -ti <pod> <container> /bash ### Exec into a container
```
```
k get secret <namespace> -o yaml
echo "password" | base64 --decode  ### Ignore the last percentage sign from output
```
```
kgn ## kubectl get nodes ###
```
```
kt ## kubectx ## (Change contex / cluster)
```
```
kn ## kubens ## (Change namespace)
```

## Stern

```
s <namespace> ## stern <namespace> ### Tail multiple pods and containers from Kubernetes
```

## Helm

```
h ## helm
```
```
hs ## helm secrets view secrets.yaml ### View secrets in secrets.yaml
```
```
hdp <namespace> ## helm delete --purge <namespace> ### Delete and purge a deployment
```
```
hh <namespace> ## helm history <namespace> ### See release history (in a namespace)
``
``
hr <namespace> <revision> ## helm rollback <namespace> <revision> ### Helm release rollback
```

# Docker

docker-compose up -d` ### Start docker via compose script

## Containers
```
start <container> ## docker stop <container> ### Stop a container 
```
```
stop <container> ## docker start <container> ### Start a container 
```
```
restart <container> ## docker start <container> ### Restart a container 
```
```
dsa ## docker stop $(docker ps -a -q) ### Stops all docker containers
```
```
dra ## docker rm $(docker ps -a -q) ### Remove all containers
```
```
dps ## docker ps ### Lists all RUNNING containers
```
```
dpsa ## docker ps -a ### Lists all containers
```
```
dce <container> /bash ## docker exec -it <container> /bash ### Jump into a /bash shell on a container
```
```
dcp ## docker container prune ### Prune all stopped containers, -f to force
```
```
dss ## docker stop squad squad2 ## Stops Squad servers
```
```
dcs ## docker container stats ## # Display a live stream of container(s) resource usage statistics
```
```
dl <container> ## docker logs <container> ## Show container logs
```
```
dlf <container> ## docker logs -f <container> ### Tail -f container logs
```
```
dcs <container> ## docker container stats ### Display a live stream of container(s) resource usage statistics
```
```
dci <container> ## docker container inspect <container> ### Display detailed information on one or more containers
```
```
dcr <container> ## docker container rm <container> ### Remove one or more containers
```

## Images
```
dib ## docker image build ### Build an image from a Dockerfile
```
```
dih <image> ## docker image history <image> ### Show the history of an image
```
```
dii <image> ## docker image inspect <image> ### Display detailed information on one or more images
```
```
di ## docker image ls ### List images
```
```
diprune ## docker image prune ### Remove unused images
```
```
dip ## docker image pull ### Pull an image or a repository from a registry
```
```
dipush ## docker image push ### Push an image or a repository to a registry
```
```
dir <image> ## docker image rm <image> ## Remove one or more images
```

## Networks

> The bridge network represents the docker0 network present in all Docker installations.
> The bridge network will enable the connectivity to the other interfaces of the host machine as well as among containers in the same subnet. 
> In other words,it means that a container connected to a bridge network has connectivity to the host machine, the external network (Internet) 
> and other containers in the same subnet.
> Unless you specify otherwise with the docker container run --net=<NETWORK> option.

```
dnls ## docker network ls ### Lists available networks
```
```
dni <network> ## docker network inspect <network> ### Inspect config of x <network>
```
```
dnc <network-name> ## docker network create <network-name> ### Creates a network
```
```
dnc --subnet=172.20.0.0/16 <network-name> ## docker network create --subnet=172.20.0.0/16 <network-name>` ### Creates a network in CIDR format
```
## Healthchecks

> The process started by the ENTRYPOINT or CMD of the Dockerfile runs as a PID 1. PID is an acronym for process identification number and is automatically assigned to each process when it’s created. 
> Each process has a unique PID on any Unix-like system. A process running as PID 1 inside a container is treated specially as it ignores any signal, such as SIGINT or SIGTERM, and won’t terminate unless it’s coded to do so.
> As long as PID 1 is up and running, the Docker engine will keep reporting the container as being up and running too.

> A custom health check is specified in a Dockerfile using the HEALTHCHECK directive. The check command is executed inside the container, so make sure it’s available. 
> As for the type of check it performs, it can be anything you might think of as long as it returns an appropriate exit-status code.

> The HEALTHCHECK command can also be used with four different options:
> --interval=DURATION (default: 30s)
> --timeout=DURATION (default: 30s)
> --start-period=DURATION (default: 0s)
> --retries=N (default: 3)

> The interval option specifies the number of seconds to initially wait before executing the health check and then the frequency at which subsequent health checks will be performed.

> The timeout option specifies the number of seconds Docker awaits for your health check command to return an exit code before declaring it as failed (and your container as unhealthy).

> The start-period option specifies the number of seconds your container needs to bootstrap. During this period, health checks with an exit code greater than zero won’t mark the container as unhealthy; however, a status code of 0 will mark the
> container as healthy.

> The retries option specifies the number of consecutive health check failures required to declare the container as unhealthy.

> Example - add to Dockerfile

```
RUN apt-get update && apt-get install -y wget

HEALTHCHECK CMD wget -q --method=HEAD localhost/system-status.txt
```
# Curl

> cURL which is a tool/command for transferring data and is handy for troubleshooting issues on the internet 

> [Using Curl](https://ec.haxx.se/usingcurl)


> Handy flags

```
-k ### Accept invalid certs
```
```
-v ### Show verbose output (headers)
```
```
-o ### output, /dev/null send the output of the request to dev null (the html etc)
```
```
-H ### Add relevant headers
```

> Example

```
curl -o /dev/null -v -H "x-worker-debug: true" https://google.com
```

# DNS

## Dig Command

> Dig stands for (Domain Information Groper) is a network administration command-line tool for querying Domain Name System (DNS) name servers. It is useful for verifying and troubleshooting DNS problems and also to perform DNS lookups and displays the answers that are returned from the name server that were queried.

> Basic command, this queries the 'A' record of example.com

```
➜ dig example.com
```
```
; <<>> DiG 9.10.6 <<>> example.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 29434
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4000
;; QUESTION SECTION:
;example.com.			IN	A

;; ANSWER SECTION:
example.com.		283	IN	A	93.184.216.34

;; Query time: 54 msec
;; SERVER: 172.30.20.131#53(172.30.20.131)
;; WHEN: Mon May 18 22:00:59 BST 2020
;; MSG SIZE  rcvd: 56
```

> Dig + short - just brings back A record result without any other info

```
➜ dig example.com +short
```
```
93.184.216.34
```

> Querying MX Record for Domain

```
➜ dig example.com MX
```
```

; <<>> DiG 9.10.6 <<>> example.com MX
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 34817
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4000
;; QUESTION SECTION:
;example.com.			IN	MX

;; ANSWER SECTION:
example.com.		300	IN	MX	0 .

;; Query time: 161 msec
;; SERVER: 172.30.20.131#53(172.30.20.131)
;; WHEN: Mon May 18 22:02:49 BST 2020
;; MSG SIZE  rcvd: 55
```
<<<<<<< HEAD
=======

>>>>>>> df92bbf803dbad7296f455840f90af480c310a95
# ZSH

# MAC

# Ubuntu 
