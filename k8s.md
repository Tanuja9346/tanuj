1. ClusterIP (for comparison)
Only accessible within the cluster.

Default service type.

Used for internal communication between pods/services.

2. load balancer: Creates an external load balancer (if you're on a supported cloud like AWS, Azure, GCP).

The cloud provider assigns a public IP to the service.

Automatically routes traffic from that IP to your service’s ClusterIP behind the scenes.

type: LoadBalancer
3. nodeport:
Exposes the service on a static port (30000–32767) on each node's IP.

You can access the app using:

<NodeIP>:<NodePort>

Works without a cloud provider—simple and works in any environment.


4. ingress controller and ingress?
Ingress and Ingress Controller are key components in Kubernetes networking, especially for managing external access to services inside the cluster. Here’s a clear breakdown:

What it is: A Kubernetes API object that defines rules for routing external HTTP/S traffic to services within the cluster.

Purpose: Acts like a traffic controller for HTTP requests, allowing you to configure things like:

Host-based routing (e.g., app.example.com)

Path-based routing (e.g., /api, /frontend)

TLS termination

Load balancing

Ingress Controller::
What it is: A Kubernetes controller (i.e., a pod or set of pods) that implements the Ingress rules.

Purpose: Watches the Kubernetes API for Ingress resources and configures a load balancer (like NGINX, Traefik, or AWS ALB) to fulfill the routing.

Common Ingress Controllers:

NGINX Ingress Controller (most popular)

Traefik

HAProxy

AWS ALB Ingress Controller


Ingress = Road signs (rules)

Ingress Controller = Traffic cop or road infrastructure that reads signs and directs traffic accordingly

-----------------------------------

. How can you audit RBAC permissions in a Kubernetes cluster?
Answer:

Use tools like:

kubectl auth can-i -> command

rbac-lookup  --> cli tool

rakkess (RBAC Access Summary)

Audit logs if enabled on the API 

5. How would you give a user read-only access to all pods in a namespace?
Answer:
You can create a Role with get, list, and watch verbs for the pods resource, and bind it to the user using a RoleBinding.
kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  namespace: dev
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: read-pods
  namespace: dev
subjects:
- kind: User
  name: jane
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io

6. What is the purpose of the apiGroup field in RBAC?
Answer:
The apiGroup field specifies the API group of the resource. For example:

Core resources (like pods, services) use "".
Apps (like deployments) use apps.
RBAC resources use rbac.authorization.k8s.io.

8. What are best practices when managing RBAC?
Answer:
Follow the principle of least privilege.

Use Groups rather than binding individual users.
Use ClusterRoles carefully; avoid giving cluster-wide access unless needed.
Regularly review RBAC bindings and audit them.

9. What happens when you run kubectl apply vs kubectl create?
**kubectl create**: Creates a new resource. Fails if the resource already exists.

**kubectl apply**: Creates or updates a resource using the last-applied-configuration. It's idempotent and used for declarative management.

10. How do you manage secrets and ConfigMaps in Kubernetes?
ConfigMap: Stores non-sensitive config data (e.g., environment variables, config files).
Secret: Stores sensitive data (e.g., passwords, tokens) in base64-encoded format.

Ways to use them:
Mount as volumes.
Inject as environment variables.
Reference in command arguments.

refer yaml3reetypes file--- note

11. What is the difference between Deployment, ReplicaSet, and StatefulSet?

Deployment manages ReplicaSets and provides update strategies.

ReplicaSet just ensures the desired number of pod replicas.

StatefulSet is used when pod identity and persistent storage matter.

feature	            Deployment	                       ReplicaSet	                             StatefulSet
Purpose	       Manages stateless applications	   Ensures a specific number of pod replicas	Manages stateful applications
Identity	     No stable identity	                No stable identity	                      Stable hostname & storage
Updates	       Rolling updates supported	        No direct update strategy	                Ordered, graceful updates
Use Case	     Web apps, APIs	                   Underlying mechanism for Deployment	       Databases, Kafka, Zookeeper

12. What is a Pod? How is it different from a container?
A Pod is the smallest deployable unit in Kubernetes and can contain one or more containers.
All containers in a pod:
Share the same network namespace (IP address).
Can communicate via localhost.
Share volumes for storage.

Difference:

A container is an isolated application instance.

A pod is a wrapper that may run multiple containers together that need to share resources or work tightly coupled.

13. Explain the architecture of Kubernetes. What are the main components of a master and worker node?
Kubernetes Architecture follows a master-worker model.

Master Node Components (Control Plane):
API Server: Frontend for the Kubernetes control plane. All requests go through it.

Controller Manager: Runs controllers that handle cluster state (e.g., node controller, replication controller).

Scheduler: Assigns Pods to nodes based on resource availability and constraints.

etcd: Distributed key-value store that holds cluster configuration and state.

Worker Node Components:
kubelet: Agent that runs on each node, ensures containers are running as expected.

kube-proxy: Handles networking and load balancing across services.

Container Runtime: Runs the actual containers (e.g., containerd, Docker, CRI-O).

14. . How does Kubernetes handle service discovery?
Kubernetes uses DNS-based service discovery via the built-in DNS service (like CoreDNS).

Every Service gets a DNS name:
Format: my-service.my-namespace.svc.cluster.local

Pods use this name to communicate with the service instead of IP addresses.
pod ip : unique and service ip stable

Under the hood, Kubernetes maintains an endpoints list(1 or more pods) that maps services to matching pods.

Additionally:

Services are backed by iptables/ipvs rules via kube-proxy, which routes traffic to appropriate pod IPs.

Pods use this dnsname to communicate with the service instead of IP addresses.

Kubernetes gives every Service a DNS name using CoreDNS.

The DNS name is based on the service name and namespace.

It always resolves to the service’s ClusterIP, which forwards traffic to the right pod
Why is it stable?
The service name and namespace don’t change, so the DNS name stays the same.

Even if the pods behind the service change (restarted, rescheduled), the service DNS always points to the new healthy pods automatically.

15. What is kube-proxy and how does it work?
kube-proxy runs on each node and handles network routing for services.

It maintains iptables/ipvs rules that map service IPs to pod IPs.

It watches the Kubernetes API for changes in Services and Endpoints and updates rules accordingly.

It supports both L4 (TCP/UDP) load balancing.

In simple terms, it ensures traffic to a Service is routed to one of the appropriate Pods.

16. 4. Explain how network policies work in Kubernetes.
Network Policies control pod-level traffic flow (ingress and egress).

By default, all pods can talk to each other unless restricted by a policy.

A NetworkPolicy:

Selects target pods using labels.

Specifies allowed traffic (from which pods/namespaces/IPs, and on which ports)

17. How do you expose a service outside the cluster without using a LoadBalancer?
Here are 3 main ways:

NodePort:
--------
Exposes service on a static port (e.g., :30080) on every node.

Access via http://<NodeIP>:<NodePort>

Ingress Controller:
-----
A better, flexible option for HTTP/HTTPS traffic.

Routes external traffic to services using Ingress rules.

Supports domain-based routing, TLS, rate limiting, etc.

Port Forwarding (dev only):
--------
kubectl port-forward svc/my-service 8080:80

For local access during development/testing.

 scheduling and resource management:

18. What are taints and tolerations?
  Taint = A "keep out" sign placed on a node.
 Toleration = A "permission slip" on a pod that says, "I can go to that node even if it has a taint.

*** They are a way for nodes to say “only certain pods are allowed here”, and for pods to say “I’m allowed on that node.”

19. Explain affinity and anti-affinity rules?
Affinity and anti-affinity control how pods are placed on nodes relative to other pods.

📌 Types:
Node Affinity (like nodeSelector but more powerful)

Place pods on specific nodes using labels.

Pod Affinity: Place pod near other pods.

Pod Anti-Affinity: Place pod away from specific pods.

20. How do you set resource requests and limits, and what happens if a container exceeds its limit?

CPU: Throttled (container is slowed down).

Memory: Container is killed (OOMKilled) if it exceeds the memory limit.

✅ Resource Requests:
Minimum resources the pod is guaranteed.

✅ Resource Limits:
Maximum resources the container can use.

21. How does the Kubernetes scheduler choose where to place a pod?
The Kubernetes scheduler decides which node a pod should run on by going through two phases:

1. Filtering (Predicates):
Eliminate nodes that don’t meet requirements, like:

Not enough CPU/memory.

Node selector, taints, affinity rules.

2. Scoring (Prioritization):
Assign scores to the remaining nodes.

Factors include:

Resource balance.

Pod distribution.

Custom weights.

Finally, the pod is scheduled on the highest-scoring node.

storage:
1. What are Persistent Volumes (PV) and Persistent Volume Claims (PVC)?
🔹 Persistent Volume (PV)
A cluster-wide storage resource created by an admin or provisioner.
It represents actual storage like a disk or NFS share.
Exists independently of pods.

🔹 Persistent Volume Claim (PVC)
A request for storage by a user.
Specifies size, access mode (e.g., ReadWriteOnce), and storage class.
The system binds the PVC to a suitable PV automatically.

📦 Analogy:
PV = Pre-installed storage shelf.
PVC = Request to use space on a shelf.
Kubernetes matches the request (PVC) to a shelf (PV).

2. Explain the difference between static and dynamic provisioning in Kubernetes storage.
ProvisioningType	  Description	                Who Creates the PV?	  Use Case
Static	           Admin manually creates PVs	  Admin	        Pre-provisioned NFS or legacy systems
Dynamic	     PV is created automatically when a PVC is made,	Kubernetes (via a StorageClass)	,,Cloud volumes, flexible, modern apps

✅ Dynamic Provisioning Example:

storageClassName: standard
Kubernetes uses the StorageClass to automatically provision a volume using a backend (e.g., AWS EBS, GCE PD, CSI drivers).

3. How would you handle storage in a multi-tenant cluster?
To securely manage storage in a multi-tenant environment:

🔐 Key Strategies:
Separate Namespaces: Each tenant has its own namespace for isolation.

Per-tenant StorageClasses: Define StorageClasses with quota/limits per tenant.

RBAC: Control who can create PVCs or access certain StorageClasses.

Quota Management:


kind: ResourceQuota
spec:
  hard:
    requests.storage: "100Gi"
    persistentvolumeclaims: "10"

Encryption: Use encrypted storage backends or CSI drivers with per-tenant keys.

Dynamic Provisioning with CSI drivers: Use tenant-specific provisioning policies or secrets.

helm:
1. What is Helm? How do you manage Helm chart versions?
🔹 What is Helm?
Helm is the package manager for Kubernetes, like apt for Ubuntu or yum for CentOS.
It lets you define, install, and upgrade complex Kubernetes applications using Helm charts.
A Helm chart is a collection of YAML templates that can be parameterized using values.
✅ Key Features:
Easy deployment of apps (e.g., Prometheus, NGINX Ingress, MySQL).
Handles versioning, upgrades, rollbacks.
Manages app lifecycle with a single command (helm install, helm upgrade, etc.).

🔧 How do you manage Helm chart versions?
Helm charts are versioned using the Chart.yaml file:
version: 1.2.3

When installing a chart:
helm install my-app repo/chart-name --version 1.2.3

You can upgrade to a different version:
helm upgrade my-app repo/chart-name --version 2.0.0
You can pin versions in CI/CD to avoid unexpected updates.

2. What are some use cases for Kubernetes Operators?
🔹 What is an Operator?
A Kubernetes Operator is a custom controller that extends Kubernetes functionality to manage complex stateful applications automatically, using custom resources (CRDs).
| Use Case                         | Explanation                                                                 |
| -------------------------------- | --------------------------------------------------------------------------- |
| **Automated Day-2 Operations**   | Backup, restore, scaling, upgrade of apps like DBs.                         |
| **Managing Stateful Apps**       | Databases like PostgreSQL, MongoDB, or Kafka.                               |
| **Custom App Logic**             | React to app-specific events (e.g., user creation, license sync).           |
| **Self-Healing Apps**            | Automatically restart or reconfigure on failure.                            |
| **Multi-resource orchestration** | Coordinate deployments that span Deployments, Services, PVCs, Secrets, etc. |

1. How do you monitor a Kubernetes cluster? What tools do you use?
✅ Tools commonly used:
Tool	                Purpose
Prometheus	        Collects metrics from K8s and apps (via exporters).
Grafana	            Visualizes metrics with dashboards.
Alertmanager	      Sends alerts based on Prometheus rules.
Kube-State-Metrics	Exposes cluster state info like deployments, pods, etc.
Node Exporter	      Provides node-level metrics like CPU, memory, disk.
cAdvisor	          Container-level metrics, often bundled with kubelet.

🧠 Setup Summary:
Prometheus scrapes metrics from kubelets, pods, nodes.
Grafana connects to Prometheus to visualize dashboards.
Alerts can be configured to notify via Slack, email, PagerDuty, etc.

2. How do you collect and view logs from all pods in a namespace?
✅ Using kubectl:
kubectl logs -n <namespace> --selector app=my-app --tail=100
Use --selector to target specific labels across all pods.

To stream logs from all containers in all pods:


kubectl logs -n <namespace> -l app=my-app --all-containers=true --follow
🔧 Centralized Logging Tools:
ELK Stack (Elasticsearch, Logstash, Kibana)

EFK Stack (Fluentd/Fluent Bit instead of Logstash)

Loki + Grafana (lightweight, Kubernetes-native log aggregation)

Logs are collected via agents like Fluent Bit or Logstash running as DaemonSets on each node.

3. What are liveness and readiness probes, and why are they important?

| Probe Type          | Purpose                                          | Action Taken if Fails                  |
| ------------------- | ------------------------------------------------ | -------------------------------------- |
| **Liveness Probe**  | Checks if the app is **alive** or hung.          | Pod is restarted.                      |
| **Readiness Probe** | Checks if the app is **ready to serve traffic**. | Pod is removed from Service endpoints. |

🧠 Why they're important:
Improve application availability.

Prevent routing traffic to unready or stuck pods.

Enable self-healing and smarter restarts.


--------------------

✅ 1. How do you integrate Kubernetes with Jenkins / GitLab CI / Argo CD?
a) Jenkins + Kubernetes:
Use the Kubernetes plugin to dynamically spin up Jenkins agents (pods) inside the cluster.

Use kubectl or helm in Jenkins pipelines to deploy apps.

Example pipeline step:

sh 'kubectl apply -f deployment.yaml'

b) GitLab CI + Kubernetes:
GitLab has native Kubernetes integration.
You can configure a Kubernetes cluster in the GitLab UI under Infrastructure > Kubernetes.
GitLab Runners (as pods) deploy using kubectl, helm, or GitOps.

Example .gitlab-ci.yml:

deploy:
  script:
    - kubectl apply -f k8s/

c) Argo CD + Kubernetes:

Argo CD is a GitOps tool for continuous delivery.

You deploy by pushing YAML/Helm manifests to Git — Argo CD syncs them to the cluster.

Supports automated sync, health checks, and rollback.

Integration flow:

Define your app in Git.

Argo CD watches the Git repo.

Argo CD applies changes to the cluster automatically or manually.

---------------------

🚀 2. How do you perform blue-green or canary deployments in Kubernetes?
a) Blue-Green Deployment:
Run two versions of the app: Blue (old) and Green (new).

Use 2 Deployments or 2 Services.

Switch traffic from blue to green by updating the Service selector.

Steps:

Deploy new version as green.

Test green via internal service or temporary URL.

Switch service from blue to green.

Delete blue deployment if all is good.

b) Canary Deployment:
Gradually send a small percentage of traffic to the new version.

Requires tools like:

Service mesh (e.g., Istio, Linkerd)

Ingress controller (e.g., NGINX with traffic splitting)

Flagger (with Argo Rollouts or Helm)

Example with Argo Rollouts:


apiVersion: argoproj.io/v1alpha1
kind: Rollout
spec:
  strategy:
    canary:
      steps:
        - setWeight: 10
        - pause: { duration: 2m }
        - setWeight: 50
        - pause: { duration: 5m }
        - setWeight: 100
This slowly increases traffic to the new version, allowing monitoring and rollback at each step.

------------------

troubleshoot:

✅ 1. What steps do you take when a pod is stuck in CrashLoopBackOff?
A pod in CrashLoopBackOff means it keeps crashing and Kubernetes is backing off from restarting it.

🔍 Troubleshooting steps:
Check logs:

kubectl logs <pod-name> --previous
Use --previous to see logs from the previous crash.

Describe the pod:

kubectl describe pod <pod-name>
Look for events like:
OOMKilled
Failed to pull image
Liveness/readiness probe failures

**Check the container command/environment:

Wrong startup command?
Missing config?
Misconfigured secrets?

Review resource limits:
The container may be getting killed for exceeding memory.

Disable probes temporarily (for testing):
Liveness/readiness probes may be misconfigured and cause restarts.

Check dependencies:
Database not reachable?
Missing configMap or secret?

✅ 2. A service is not reachable from another namespace—how would you debug this?
🧪 Step-by-step:
Use full DNS name:

Kubernetes DNS is namespace-scoped by default.

Use:

<service-name>.<namespace>.svc.cluster.local

Check DNS resolution inside pod:
kubectl exec -it <pod> -- nslookup <service-name>.<namespace>

Verify service and endpoints:
kubectl get svc -n <namespace>
kubectl get endpoints <service-name> -n <namespace>

Ensure correct ports:
Service and pod must expose the same ports/protocols.

Network Policies:
Check if any NetworkPolicy is blocking cross-namespace traffic.

Connectivity test:
Use a test pod like busybox or curl to try connecting manually.

✅ 3. How do you troubleshoot a node not joining the cluster?
🛠️ Steps:
Check kubelet logs on the node:

journalctl -u kubelet

Verify kubeadm token/join command:

On master:
kubeadm token list
kubeadm token create --print-join-command
On node: Did you run the correct kubeadm join?

Check firewall rules:
Required ports open between control plane and worker? (e.g., 6443)

Check hostname and DNS resolution:
The hostname must resolve correctly, especially in cloud environments.

Verify container runtime:
Ensure containerd or Docker is installed and running.

Check Kubernetes version compatibility:
Node version should be close to the control plane version.

Check node certificate issues:
Node may be blocked due to expired or missing certificates.


------------------------
1. rollout : kubectl rollout restart deployement filename -n namespace
2. rollback a faulty deployement: kubectl rollout undo deployement filename -n namespace
3. rool out history: kubectl rollout history deployement filename -n 
4. check mounted volumes: login inside the pod with namespace and df -ht | grep /var/lib/mysql
5. get decode values of secrets: kubectl get secret poad-name -n namspace -o jsonpath="{.data.keynmae}" | base64 --decode/-d
6. logs of previous crash before pod restarted: kubectl exec -it podname -n namespace --kill 1
now run : kubectl logs -p podname -n namespace
7. excute readiness probe manually: kubectl exec -it podname -n namespace -- wget -qo- http://localhost:8080/login 
8. test internal dns resolution of service: kubectl run mysql-client -it -rm --restart=never --image=which one u want see image -n namespacd -- mysql -h service name -u root <pswrdof>   note{connect mysql server}
9. list pvc mount paths in the pod: kubectl describe pod podname -n namespace | grep -A10 "mounts"

 grep -A10 "mounts":
grep searches for the string "mounts"
-A10 means:
Show 10 lines After the matched line

10. get service endpoints and actual pod ips: kubectl get endpoints service -o wide -n namespace
11. see all events in namespace, sorted by time: kubectl get events -n namespace --sort-by=.metadata.creationTimestamp
12. patch pvc for resize the volume: kubectl get pvc pvcname -n namespace -o jsonpath="{.spec.volumename}"
kubectl patch storageclass scname -p '{"allowvolumeexpansion": true}'
*** kubectl patch pvc pvcname -n namespace \ --patch '{"spec": {"resources": {"requests": {"storage": 15Gi}}}}'
13. simulate creashllopback error: 
14. enable hpa based on cpu usage: kubectl autoscale deployement deplname --cpu-percent=60 --min=1 --max=5 -n namespace 
15. livestream pod cpu and memory usage: watch kubectl top pods -n namespace
setup metrics server in cluster.{install through github yaml file}
16. backup mysql volume(ebs pvc) to local:
kubectl cp podnamemysql:/var/lib/mysql(volumetorage) ./mysql.txt  {note directory and filename where u want backup}

17. drain a node before az maintenance or upgrade:
kubectl drain workernodename --ignore-daemonsets --delete-emptydir-data  (all resources are evicted and node is drained)
18. restart all depolyements in a namespace: kubectl rollout restart deployement -n namespace
19. list all deployements with image versions: 
kubectl get deployments --all-namespaces -o=jsonpath="{range .items[*]}{.metadata.namespace}{'\t'}{.metadata.name}{'\t'}{range .spec.template.spec.containers[*]}{.image}{'\n'}{end}{end}"

20. get on which node a pod is running on: 
kubectl get pod podname -n namespace -o wide 

21. enable sechduling back on nodes after draining: kubectl uncordon <node-name>

22. force delete a pod: kubectl delete pod <pod-name> -n <namespace> --grace-period=0 --force
23. compare live cluster and manifest file: kubectl diff -f <manifest.yaml>
