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

# Docker
```
di ## docker images ### Lists all docker images
```
```
dps ## docker ps ### Lists all RUNNING containers
```
```
dpsa ## docker ps -a ### Lists all containers
```
```
de <container> /bash ## docker exec -it <container> /bash ### Jump into a /bash shell on a container
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
## Networks

The bridge network represents the docker0 network present in all Docker installations. Unless you specify otherwise with the docker container run --net=<NETWORK> option, the Docker daemon connects containers to this network by default.

```
dnls ## docker network ls ### Lists available networks
```
```
dni <network> ## docker network inspect <network> ### Inspect config of x <network>
```




docker-compose up --force-recreate --build




`docker login` **#Login to Docker Hub**



`docker network inspect bridge` **#The bridge network represents the docker0 network present in all Docker installations. Unless you specify otherwise with the docker container run --net=<NETWORK> option, the Docker daemon connects containers to this network by default.**

!!! NOTE "The Bridge Network - The bridge network will enable the connectivity to the other interfaces of the host machine as well as among containers in the same subnet. In other words, it means that a container connected to a bridge network has connectivity to the host machine, the external network (Internet) and other containers in the same subnet."

`docker network create my-network` **#Creates a network (rm_ command to remove)**

`docker network create --subnet=172.20.0.0/16 my-network` **#When creating* a Docker network, you can specify the subnet in CIDR format**

`docker container run -ti --name=ubuntu --rm --net=my-network ubuntu:14.04` **#Adds a container to the network and jump straight in**

`docker container run -ti --name=ubuntu --rm --net=my-network --ip=172.20.0.100 ubuntu:14.04` **#Assign a static IP to a container**

#Popular Commands

**Images**

`docker image build`: **Build an image from a Dockerfile**

`docker image history`: **Show the history of an image**

`docker image inspect`: **Display detailed information on one or more images**

`docker image ls`: **List images**

`docker image prune`: **Remove unused images**

`docker image pull`: **Pull an image or a repository from a registry**

`docker image push`: **Push an image or a repository to a registry**

`docker image rm`: **Remove one or more images**

**Containers**

`docker container attach`: **Attach local standard input, output, and error streams to a running container**

`docker container exec`: **Run a command in a running container**

`docker container inspect`: **Display detailed information on one or more containers**

`docker container kill`: **Kill one or more running containers**

`docker container logs`: **Fetch the logs of a container** (add -f)

`docker container ls`: **List containers**

`docker container prune`: **Remove all stopped containers**

`docker container restart`: **Restart one or more containers**

`docker container rm`: **Remove one or more containers**

`docker container run`: **Run a command in a new container**

`docker container stats`: **Display a live stream of container(s) resource usage statistics**

`docker container top`: **Display the running processes of a container**

`docker-compose up -d` **#Start docker via compose script**

# Curl


***Accept invalid certs***
```
-k
```
***Show verbose output (headers)***
```
-v
```
***/dev/null send the output of the request to dev null (the html etc)***
```
-o
```
***I then peg on headers i'm interested on with:***
```
-H 'name: value
```
***So an example:***
```
curl -o /dev/null -v -H "x-worker-debug: true" https://m-qa.atcdn.co.uk/media/a/media/w400h300/f5bd53d2c66a481f8993e3adfc251c0d.jpg
```
***This gets an image from our image server, but puts the actual image in /dev/null, but shows us the headers.  I pass the x-worker-debug: true header in the request too***




DOCKER

# Docker



ZSH

Delete a line --- CTRL + U

s

ITERM

• CMD + D = Vertical terminal split

• CMD + T = Add a new tab

• CMD + ; = Auto complete menu

• CMD + SHIFT + H = Paste history

• CMD + / = Shows where cursor is

CMD + SHIFT + E = Search all tabs

SYNOLOGY

How to get netdata working

sudo /var/packages/debian-chroot/scripts/start-stop-status chroot

/usr/sbin/netdata

/opt/netdata

MISC

Update locate.db --- sudo /usr/libexec/locate.updatedb



- ![#f03c15](https://via.placeholder.com/15/f03c15/000000?text=+) `#f03c15`
- ![#c5f015](https://via.placeholder.com/15/c5f015/000000?text=+) `#c5f015`
- ![#1589F0](https://via.placeholder.com/15/1589F0/000000?text=+) `#1589F0`

