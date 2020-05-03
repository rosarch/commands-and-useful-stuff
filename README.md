* TOC
{:toc}

# k8 related stuff


### kubectl get pods
`kgp` 


```

The **main.tf** file is where terraform will start its executing. 

The **modules** directory contains re-usable modules for each part of our infrastructure. Each module has configurable variables which are then configured according to each environment - for example **dev.tfvars**

The **state** directory contains the terraform configuration for initialising the remote state.

## Instructions

### Preparation

**Output your public key**

`cat ~/.ssh/id_rsa_eks_cluster.pub`

**Update code with your public key**

Locate the environments/dev/variables.tf file

Replace the `ssh-rsa REPLACE_ME` with your public key.

So it looks something like below.

NOTE: There are no new lines in the value.

```

}
```

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

**Navigate into the state directory**

```
cd state
```

**Initialise the terraform AWS provider**

```
terraform init
```

**Run a plan**

It'll ask you a bucket name - enter something unique such as using your name.

```
terraform plan
```

**Apply that plan to create your state buckets**

It'll ask you a bucket name - use the same name as the previous step.

```
terraform apply
```

**Make a note of your bucket name**


```
terraform output terraform_bucket_name
```

Now wait a few moments for the S3 bucket to be fully ready before moving on.

### Provisioning our cluster and all required services

Finally we can create our cluster

**Navigate to dev environment folder**

```
cd ../environments/dev
```

**Update bucket name**

Edit the **backend.tf** file and update the bucket name by replacing the 'bucket' with the output of your previous `terraform output...` command.

**Initialise Terraform**

This will pull down the required providers (AWS and HTTP)

```
terraform init
```
***note you might need to use -reconfigure if you come across forbidden issues

**Run a plan**

```
terraform plan
```

**Apply your changes**

This might take between 5 and 15 minutes depending on AWS.

```
# commands

k get pods

k describe pod <pod-name>
	
k port-forward <pod-name> 8080: <pod-port>

k logs <pod-name> 

k logs <pod-name> -p

k get pods -o wide (shows more info including nodes)

kgp --all-namespaces -o wide |grep -i 'consumer-gateway\|api-gateway\|tpf-web'

<How to work out what the secret password is>
k get secret guaranteed-valuation-service -o yaml
echo "password" | base64 --decode  (ignore the last percentage sign)

k describe ingress

k describe service

k port-forward <ingresss-pod-name> 8080:<ingress-port>

k port-forward service/<service-name>  8080:<service-port>

k get sites.cloudflare.com (on an external facing namespace)


alias kl='k logs' # kubectl logs - <pod>
 59 alias klf='k logs -f'
 60 alias kn='kubens' # Change namespace
 61 alias kt='kubectx' # Change contex / cluster
 62 alias kd='k describe pod'
 63 alias kds='k describe service | ccze -A'
 64 alias kdi='k describe igress | ccze -A'
 65 alias kgscf='k get sites.cloudflare.com | ccze -A'
 66 alias kgpw="kubectl get pods --sort-by='.status.containerStatuses[0].restartCount' -o wide| ccze -A"
 67 alias ke='k exec -ti'
 68 alias kname='kgp --all-namespaces -o wide |grep -i'
 69 alias kgn='k get nodes | ccze -A'
 70 alias kr='kubectl get pods --all-namespaces --sort-by=".status.containerStatuses[0].restartCount" -o wide'
 
 
 
 DOCKER
 docker-compose up --force-recreate --build
docker image prune -f

`kubectl apply -f *.yaml` # Apply or update a configuration to a resource by filename or stdin (-f dash file)

`kubectl get <*>` # Display one or many resources

`kubectl get pods -o wide` # Displays details of pods and IP addresses

`kubectl get replicasets` # shoes replica count, desired against actual

`kubectl get replicasets -w` # does the same appart from watches for changes

Service - example service.yaml (for nginx)

apiVersion: v1

kind: Service

metadata:

name: nginx-service

spec:

selector:

app: nginx

ports:

- protocol: TCP

port: 80

targetPort: 80

nodePort: 31050

type: NodePort

`kubectl get services` # Lists running services

NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE

kubernetes ClusterIP 10.100.0.1 <none> 443/TCP 9h

nginx-service NodePort 10.100.16.98 <none> 80:31050/TCP 8h

`kubectl describe service nginx-service` # Get more details on a service, verify endpoint IP's

Name: nginx-service

Namespace: default

Labels: <none>

Annotations: kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","kind":"Service","metadata":{"annotations":{},"name":"nginx-service","namespace":"default"},"spec":{"ports":[{"nodePort":31050,"port...

Selector: app=nginx

Type: NodePort

IP: 10.100.16.98

Port: <unset> 80/TCP

TargetPort: 80/TCP

NodePort: <unset> 31050/TCP

Endpoints: 10.244.35.4:80,10.244.49.2:80,10.244.49.3:80

Session Affinity: None

External Traffic Policy: Cluster

Events: <none>

`kubectl get pods --namespace kube-system` # List the pods in the kube-system namespace

`kubectl apply -f namespace-example.yaml` # Add a new namespace via version control

apiVersion: v1

kind: Namespace

metadata:

name: namespace-example

`kubectl apply -f pod-quota.yaml` # Add a resource quota via version control

apiVersion: v1

kind: ResourceQuota

metadata:

name: pod-demo

spec:

hard:

pods: "2" 

`kubectl apply -f pod-quota.yaml --namespace=namespace-example` # apply a quota config to a namespace

`kubectl apply -f nginx-deployment.yaml --namespace=namespace-example` # apply a yaml config against a set namespace

`kubectl get deployment --namespace=namespace-example` # Get deploymet details from a non default namespace

NAME DESIRED CURRENT UP-TO-DATE AVAILABLE AGE

nginx-deployment 3 2 2 2 1m

`kubectl get deployment nginx-deployment --namespace=namespace-example --output=yaml` # add an --output=yaml to get an log output

`kubectl exec nginx -- ls -lh /etc/nginx` # Just like docker exec, you can do the same with kubectl

`kubectl get pod <pod>`: retrieve only information about the specific <pod> (the option -o wide can be used here too).

`kubectl describe pod <pod>`: show detailed information about the specific <pod>.

`kubectl logs <pod>`: print the logs from a container in the <pod> (1 container case).

`kubectl logs <pod> -c <container>`: print the logs from a specific container in the <pod> (multi-container case).

``kubectl exec <pod> -- <command>`: execute a command on a container in the <pod> (1 container case).

`kubectl exec <pod> -c <container> -- <command>`: execute a command on a specific container in the <pod> (multi-container case).

`kubectl top pods`: show metrics for all running pods (requires Heapster running).

`kubectl top pod <pod>`: show metrics for the specific <pod> (requires Heapster running).

kubectl (k)

k get pods

DOCKER

# Docker

`docker images` **#lists all images**

`docker ps` **#list Show RUNNING containers**

`docker ps -a` **#Shows ALL containers**

`docker login` **#Login to Docker Hub**

`docker exec -it <container> bash` **#connect to bash on container**

`docker container top <Container name>` **#view running process**

`docker container prune` **#<Remove all stopped containers**

`docker container stats` **#Display a live stream of container(s) resource usage statistics**

`docker container run -ti --name=GIVE_IT_A_NAME IMAGE_NAME:IMAGE_TAG` **#Download and run a container interactively and remain within the new container.**

`docker container run -ti --name=ubuntu --rm ubuntu:14.04` **#If you don't want to keep a container after you exit it, use the --rm flag, when you exit the container will delete**

`--name` **#Flag to set a name for the container upon creation.**

`-p HOST_PORT:CONTAINER_PORT` **#Bind container port to host port, by default, Docker will bind TCP ports. If you want to bind UDP ports, use the udp suffix: -p514:514/udp**

`docker container exec -ti mysql /bin/bash` **#The docker container exec command runs a command inside a running container. In this case, the command was /bin/bash, which will return you a shell session inside the container, but you could use it for any other command like ls, cat, etc. The -t (or --tty) option allocates a pseudo-TTY and the -i (or --interactive) option keeps STDIN open even if not attached.**

`-v ~/docker-volume-example:/var/lib/mysql` **#Create persistant storage directory on server, use -v command to mount persistent volume**

`-e MYSQL_ROOT_PASSWORD=ROOT` **#To set an env variable use -e** 

`docker container exec -ti mysql env` **#Check environment variables on running container**

`docker network ls` **#lists available networks**

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

