# 42 Important Kubernetes Interview Questions

<br>

## 1. What is _Kubernetes_, and why is it used for container _orchestration_?

**Kubernetes** provides a robust platform for deploying, scaling, and managing containerized applications. Unlike Docker Swarm, which is specifically designed for Docker containers, Kubernetes is container-agnostic, supporting runtimes like Containerd and CRI-O. Moreover, Kubernetes is more feature-rich, offering features such as service discovery, rolling updates, and automated scaling.

### Key Kubernetes Features

#### Pod Management

- **Why**: Pods are the smallest deployable units in Kubernetes, encapsulating one or more containers. This brings flexibility and makes it effortless to manage multi-container applications.
- **Core Concepts**: Deployments, Replication Controllers, and ReplicaSets ensure that a specific number of replicas for the Pod are running at all times.
- **Code Example**: In YAML, a Pod with two containers might look like:

  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: pod-with-two-containers
  spec:
    containers:
      - name: container1
        image: image1
      - name: container2
        image: image2
  ```

#### Networking

- **Why**: Kubernetes assigns each Pod its unique IP address, ensuring communication across Pods. It also abstracts the underlying network, simplifying the deployment process.
- **Core Concepts**: Services, Ingress, and Network Policies provide different levels of network abstraction and control.

#### Persistent Storage

- **Why**: It offers a straightforward, standardized way of managing storage systems, making it a great solution for databases and stateful applications.
- **Core Concepts**: Storage classes, Persistent Volumes (PVs) and Persistent Volume Claims (PVCs) abstract the underlying storage technologies and provide dynamic provisioning and access control.

#### Cluster Scaling

- **Why**: Kubernetes can automate the scaling of Pods based on CPU or memory usage, ensuring optimal resource allocation and performance.
- **Core Concepts**: Horizontal Pod Autoscaler (HPA) dynamically scales the number of replica Pods in a Deployment, ReplicaSet, or StatefulSet. Cluster autoscaler scales underlying infrastructure, including nodes.

#### Resource Management

- **Why**: Kubernetes makes it easy to manage and allocate resources (CPU and memory) to different components of an application, ensuring that no single component degrades overall performance.
- **Core Concepts**: Resource Quotas and Limit Ranges help define the upper limits of resources that each object can consume.

#### Batch Execution

- **Why**: For quick, efficient tasks, Kubernetes provides a Job and a CronJob API to manage such tasks.
- **Core Concepts**: Jobs and CronJobs manage the execution of Pods over time, guaranteeing the desired state.

#### Clusters Maintenance

- **Why**: Kubernetes enables non-disruptive updates, ensuring cluster maintenance without downtime.
- **Core Concepts**: Rolling updates for Deployments and Pod disruption budgets during updates help maintain availability while updating.

#### Health Checks

- **Why**: Kubernetes actively monitors and checks the health of containers and workloads, ensuring quick remediation in case of issues.
- **Core Components**: Liveness and Readiness probes are used to determine the health of container-based applications. Kubernetes restarts containers that don't pass liveness probes and stops routing traffic to those that don't pass readiness probes.

#### Secrets Management

- **Why**: Kubernetes provides a secure way to manage sensitive information, such as passwords, OAuth tokens, and SSH keys.
- **Core Components**: Secrets and ConfigMaps are different types of objects used to centrally manage application configuration and secrets.

#### Automated Deployments

- **Why**: Kubernetes facilitates gradual, controlled updates of applications, reducing the risk of downtime or failure during deployment.
- **Core Concepts**: Deployments, with their built-in features like rolling updates and rollbacks, provide a mechanism for achieving zero-downtime deployments.

#### Metadata and Labels

- **Why**: Labels help organize and select and workloads in a Kubernetes cluster, and annotations can attach arbitrary non-identifying information to objects.
- **Core Components**: Labels are key/value pairs that are attached to Kubernetes resources to give them identifying attributes. Annotations are used to attach arbitrary non-identifying metadata to objects.

#### Mechanism to Extend Functionality

- **Why**: **Kubernetes** is designed to be extensible, allowing developers to write and register custom controllers or operators that can manage any object.

### Multi-Cluster Deployment

Kubernetes provides capabilities for effective multi-cluster deployment and scaling, be it within a single cloud provider or across hybrid and multi-cloud environments. With tools like **Kubefed**, you can define and manage resource configurations that are shared across clusters.

### Cost and Productivity Considerations

Kubernetes also offers significant cost efficiencies. It optimizes resource utilization, utilizing CPU and memory more effectively, reducing the need for over-provisioning.

On the productivity front, Kubernetes streamlines the development and deployment pipelines, facilitating agility and enabling rapid, consistent updates across different environments.

### Why Choose Kubernetes for Orchestration?

- **Portability**: Kubernetes is portable, running both on-premises and in the cloud.
- **Scalability**: It's proven its mettle in managing massive container workloads efficiently, adapting to varying resource demands and scheduling tasks effectively.
- **Community and Ecosystem**: A vast and active community contributes to the platform, ensuring it stays innovative and adaptable to evolving business needs.
- **Extensive Feature Set**: Kubernetes offers rich capabilities, making it suitable for diverse deployment and operational requirements.
- **Reliability and Fault Tolerance**: From self-healing capabilities to rolling updates, it provides mechanisms for high availability and resilience.
- **Automation and Visibility**: It simplifies operational tasks and offers insights into the state of the system.
- **Security and Compliance**: It provides role-based access control, network policies, and other security measures to meet compliance standards.

### Master-Node Architecture

Kubernetes follows a master-node architecture, divided into:

- **Master Node**: Manages the state and activities of the cluster. It contains essential components like the API server, scheduler, and controller manager.

  - **API Server**: Acts as the entry point for all API interactions.
  - **Scheduler**: Assigns workloads to nodes based on resource availability and specific requirements.
  - **Controller Manager**: Maintains the desired state, handling tasks like node management and endpoint creation.
  - **etcd**: The distributed key-value store that persists cluster state.

- **Worker Nodes**: Also called minions, these are virtual or physical machines that run the actual workloads in the form of containers. Each worker node runs various Kubernetes components like Kubelet, Kube Proxy, and a container runtime (e.g., Docker, containerd).

### Need for Orchestration

Containers, while providing isolation and reproducibility, need a way to be managed, scaled, updated, and networked. This is where **Orchestration platforms** like Kubernetes come in, providing a management layer for your containers, ensuring that they all work together in harmony.
<br>

## 2. Describe the roles of _master_ and _worker nodes_ in the _Kubernetes_ architecture.

In a **Kubernetes** cluster, each **type of node** has a distinct responsibility:

- **Master Node**:

  - Also known as **Control Plane Node**, it manages the entire cluster.
  - Houses the **API Server**, **Controller Manager**, and **Scheduler**, primarily responsible for cluster orchestration.
  - It initializes and configures workloads, spans across several Master nodes to ensure high availability, and typically doesn't run user applications.
  - Also, the **etcd** database is often hosted separately or as a clustered data store.

- **Worker Node**:

  - Also called **Minion** or **Ingress Node**.
  - Serves as the **compute** layer of the cluster and by design hosts the containers that make up the actual workloads ('pods') or app services.
  - Acts as a **bridge** to offload the networking and monitoring overhead from the Master node and to ensure the security of the cluster.

- **Other Nodes**:
  - Special-purpose nodes or clusters may introduce other node types, such as dedicated storage or networking elements. However, these are not universal and won't be found in most standard Kubernetes setups.

It is essential to understand the distinct roles of these node types for effectively managing a Kubernetes cluster.

### Kubernetes Cluster Overview

- **Nodes**: Physical or virtual servers that form the worker or master nodes.

  - May also include additional specialized nodes for storage, networking, or other purposes.

- **Kubelet**: An agent installed on each worker node and helps the node connect with the main control panel. It takes `PodSpecs`, which define a group of containers that require to be coordinated, and guarantees that the identified containers are running and healthy.

- **Kube-Proxy**: A network agent on each node for managing network connectivity to local deployments.

- **Control Panel**: A collection of critical processes steamy on a cluster's master nodes to regulate the cluster management and API server.

  - **API Server**: Resembles the front door to the cluster. All external communications cease here.
  - **Scheduler**: Picks the best node for a program to run on.
  - **Controller manager**: Monitors the present state of the cluster and attempts to bring it to the desired state.

- **External Cloud**: Offers the physical hardware where your cluster will run.
  <br>

## 3. How do _Pods_ facilitate the management of _containers_ in _Kubernetes_?

In **Kubernetes**, a **Pod** serves as the smallest deployable unit, generally encapsulating a single application. The primary function of the Pod is to provide a cohesive context for executing one or more containers.

### Key Characteristics

- **Context Sharing**: Containers within the same Pod share a common network namespace, hostname, and storage volumes, optimizing their interactions.

- **Logical Bundling**: Designates one of the containers as the primary application with the remaining containers often providing supplementary services or shared resources.

- **Atomicity**: Containers in a single Pod are scheduled onto the same node and execute in close proximity. This ensures that they are either all running or all terminated.

### Core Functionalities

1. **Networking**: Containers within the same Pod are reachable via `localhost` and have a single, shared IP address. This design simplifies internal communication.

2. **Resource Sharing**: All containers in a Pod have identical resource-sharing provisions, ensuring that they have the same CPU and memory limits.

3. **Storage Volumes**: Shared volumes can be mounted across all containers within a Pod to promote data sharing among its constituent containers.

4. **Lifecycle Coordination**: Defines the life cycle for all containers in a Pod, such that if one container exits, the termination affects the entire Pod, consistent with the atomic execution concept.

### Core Relationship with Workloads

- **Dedicated Workloads**: Pods that host a single container are often used for self-sufficient, standalone tasks. These tasks have specific resource requirements and are designed to run independently.

- **Coordinated Workloads**: Pods with multiple containers emphasize complementarity. These containers often tackle related tasks, such as syncing data, logging, or serving as administrative dashboards.

### YAML Configuration

Here is the YAML configuration:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: primary-container
      image: primary-image
    - name: secondary-container
      image: secondary-image
  volumes:
    - name: shared-data
      emptyDir: {}
```

In this instance, `primary-container` is the main application container, and `secondary-container` is the auxiliary container. Both containers share the same storage volume `shared-data`.

### Management Tools

Popular dev-ops tools, like Helm, provide higher-level abstractions for managing Kubernetes resources, such as the Pod. This simplifies deployment and scaling tasks, allowing for centralized configuration management.

### Benefits of Pod:

- **Resource Efficiency**: Pods refrain from having resource duplication. Each constituent container operates within the same resource environment, reducing wastage.

- **Lifecycle Consistency**: All containers in a Pod are deployed, scaled, and managed as a single unit, promoting consistency in their execution.
  <br>

## 4. What types of _Services_ exist in _Kubernetes_, and how do they facilitate _pod communication_?

**Kubernetes** offers different types of **services** for flexible & reliable communication between pods. Let's explore each.

### Service Types

#### ClusterIP

- **Role**: Internal communication within the cluster.
- **How it Works**: Pods communicate with the Service's fixed IP and port. Service distributes traffic among pods based on defined selector.
- **Practical Use**: Ideal for back-end services where direct access from external sources isn't necessary.

#### NodePort

- **Role**: Exposes the Service on a static port on each node of the cluster.
- **How it Works**: In addition to the ClusterIP functionality, it also opens a specific port on each node. This allows external traffic to reach the service through the node's IP address and the chosen port.
- **Practical Use**: Useful for reaching the service from outside the cluster during development and testing phases.

#### LoadBalancer

- **Role**: Makes the Service externally accessible through a cloud provider's load balancer.
- **How it Works**: In addition to NodePort, this service provides a cloud load balancer's IP address, which then distributes traffic across Service Pods.
- **Practical Use**: Especially useful in cloud settings for a public-facing, production deployment.

#### ExternalName

- **Role**: Maps the Service to an external service using a DNS name.
- **How it Works**: When the Service is accessed, Kubernetes points it directly to the external (DNS) name provided.
- **Practical Use**: Useful if you want environments to have a uniform way of accessing external services and don't want to use environment-specific methods like altering /etc/hosts.

#### Headless

- **Role**: Disables the cluster IP, allowing direct access to the Pods selected by the Service.
- **How it Works**: This type of Service doesn't allocate a virtual IP (ClusterIP) to the Service, letting the clients directly access the Pods. It returns the individual Pod IPs.
- **Practical Use**: Useful for specialized use cases when direct access to Pod IPs is necessary.

### Selectors and Endpoints

For many types of Service, the **selector** identifies which Pods to include in the service. The matching Pods are called **endpoints** for the Service.

Kubernetes retrieves these endpoints from the API server, and the Service kube-proxy sets up appropriate iptables rules (or equivalent on other platforms or using `ipvs`) to match the Service behavior.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
    type: Frontend
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```

Here, the Service 'my-service' selects all the Pods with labels `app=MyApp` and `type=Frontend` and forwards the traffic on the port `80` to the `targetPort` `9376`.
<br>

## 5. Explain the functionality of _Namespaces_ in _Kubernetes_.

**Kubernetes** uses **Namespaces** to create separate virtual clusters on top of a physical cluster. This enables multi-tenancy, providing a powerful way to manage diverse workloads efficiently.

### Key Features

- **Resource Isolation**: Namespaces offer a level of separation, ensuring resources like pods, services, and persistent volumes are distinct to a Namespace.

- **Network Isolation**: Each Namespace has its IP, enabling isolated network policies and container-to-container communication.

### Use-Cases

#### Development, Testing, and Staging

- **Multi-Environment Segregation**: Namespaces can delineate development, testing, and staging environments.

#### Multi-Team and Multi-Project Support

- **Multi-Tenancy**: Namespaces allow multiple teams to work independently in the same cluster.

- **Client Isolation**: Service objects in a Namespace are only visible to clients in the same Namespace, providing clear-cut network boundaries.

#### Security and Policy Enforcement

- **Resource Quotas**: Namespaces help establish quotas to govern resource consumption for projects or teams.

- **Limit Ranges**: Namespaces can define minimum and maximum limitations on resource memory and CPU for each container.

#### Network Segmentation and IP Management

- **Ingress Configuration**: Namespaces assist in external-to-internal network mapping and traffic routing.

- **IP Management**: Each Namespace can have distinct IP ranges to uniquely identify services and pods.
  <br>

## 6. Differentiate between _Deployments_, _StatefulSets_, and _DaemonSets_.

**Kubernetes** offers various abstractions to manage containerized applications according to specific operational needs. **Deployments**, **StatefulSets**, and **DaemonSets** are all essential controllers in this regard.

### Key Distinctions

- **Deployments** are suitable for stateless, replicated applications, and mainly focus on the management of pods.
- **StatefulSets** are designed for stateful applications requiring stable, unique network identities and persistent storage.

- **DaemonSets** are for running agents on each node for system-level tasks.

### Deployments

- **State**: Stateless

  These are ideal for microservices that do not store data and can be horizontally scaled.

- **Pod Management**: Managing a replica set of pods.

- **Inter-Pod Communication**: Achieved through services.

- **Storage**: Volatile. Data does not persist beyond the pod's lifecycle.

### StatefulSets

- **State**: Stateful

  Suitable for applications that store persistent data, control startup order, and require unique network identities.

- **Pod Management**: Provides sticky identity, persistence, and orderly deployment and scaling.

- **Inter-Pod Communication**: Managed through stable network identities.

- **Storage**: Provides mechanisms for persistent storage.

### DaemonSets

- **State**: Node-Focused

  Ideal for workloads with demon-like functionalities that are needed on each node (e.g. log collection, monitoring).

- **Pod Management**: Ensures one pod per node.

- **Inter-Pod Communication**: Not a primary concern.

- **Storage**: Depends on the specific use case.
  <br>

## 7. How do _ReplicaSets_ ensure _pod availability_?

**Kubernetes** provides robust mechanisms, such as **ReplicaSets**, to ensure consistent pod availability. In the context of failure scenarios or manual scaling, it is essential to understand how **ReplicaSets** guarantee pods are up and running according to the defined configuration.

### Key Components

- **Pod Template**: It specifies the required state for individual pods within the configured ReplicaSet.

- **Controller-Reconciler Loop**: This Control Plane component continuously monitors the cluster, compares the observed state against the desired state specified in the ReplicaSet, and takes corrective actions accordingly. If there's a mismatch, this loop is responsible for making the necessary adjustments.

- **Replica Level**: Each ReplicaSet specifies the desired number of replicas. It's the responsibility of the Controller-Reconciler Loop to ensure that this count is maintained.

### Action Workflow

#### Initial Deployment

During the initial setup, the ReplicaSet creates the specified number of pods and ensures they are in an `Up` state.

#### Observation and Feedback Loop

The Controller-Reconciler continuously monitors pods. If the observed state deviates from the Pod Template, corrective action is initiated to bring the system back to the specified configuration.

- **Failure Detection**: If a pod is unavailable or not matching the defined template, the Controller-Reconciler identifies the anomaly in the system.

- **Self-Healing**: The Controller-Reconciler instantiates new pods, replaces unhealthy ones, or ensures the required number of pods is available, maintaining the ReplicaSet's defined state.

#### Horizontal Scaling

The ReplicaSet allows for dynamic scaling of pods in response to changes in demand.

- **Auto-Scaling**: The Controller-Reconciler automatically scales the number of pods to match the configured thresholds or metrics.

- **Manual Scaling**: Administrators can manually adjust the number of replicas.

### Code Example: Availability Reporting System

Kubernetes YAML:

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: av-replicaset
  labels:
    tier: frontend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: availability
  template:
    metadata:
      labels:
        app: availability
    spec:
      containers:
        - name: av-reporter
          image: av-reporter:v1
```

In this example, we ensure that two pods of the `av-reporter:v1` image are continuously available, serving as a live status reporter for an availability system.
<br>

## 8. What are _Jobs_ in _Kubernetes_, and when is it suitable to use them?

In **Kubernetes**, a **job** object is used to run a specific task to completion or a certain number of times. It's ideal for tasks that are rather short and encapsulate work that isn't part of the ongoing application processes.

### Key Characteristics of Jobs

- **Pod Management**: Jobs create one or more Pods and manage their lifecycle, ensuring successful completion.

- **Completions**: You can specify the number of successful completions, especially useful for batch tasks.

- **Parallelism**: Control how many Pods run concurrently. This feature allows for efficient management of resources.

- **Pod Cleanup**: After the task has been completed, Jobs ensure that related Pods are terminated. They might also garbage collect completed Jobs, depending on your settings.

- **Auto-Restart**: Jobs do not restart by default if successful. They can be configured to restart on failure.

### The Three Main Job Types

- **Serial Jobs**: Ensure tasks are completed exactly once.

- **Parallel Jobs**: Suitable for tasks where some level of parallel processing can be beneficial for performance.

- **Work Queues**: Suitable for tasks where a specific number of parallel tasks is defined and managed.

### Scenario-Specific Best Fits

1. **Data Processing**: For processing a batch of records or data sets. For example, a tech company might use it in a data pipeline to process thousands of records in chunks.

2. **Clean-up Tasks**: For periodic clean-up, such as an e-commerce site cleaning up expired user data.

3. **Software Compilation**: Useful in CI/CD pipelines to parallelize software builds.

4. **Cron Jobs**: For scheduling recurring batch processes, such as taking database backups nightly.

5. **Metering or Accounting**: Useful for counting or tallying records, possibly in near-real-time.

6. **Health Checks**: Occasionally, when more sophisticated health checks are needed, for tasks perhaps beyond the remit of a Liveness and Readiness check.

7. **Resource Acquisition**: For occasional resource acquisition tasks – imagine a scenario where a system occasionally scales on demand and requires a specific number of resources at run-time.
   <br>

## 9. How do _Labels_ and _Selectors_ work together in _Kubernetes_?

In Kubernetes, **Labels** are key-value pairs attached to resources, and **Selectors** are the tools used to manage and interact with these labeled resources.

### Why Use Labels and Selectors?

- **Efficient Grouping**: Labels group resources, simplifying management and configuration.
- **Decoupling**: Resources are decoupled from consuming services through selectors, making management flexible and robust.

- **Queries**: Selectors filter resources by matching label sets, enabling focused actions.

### Core Concepts

#### Labels

Kubernetes resources, such as Pods or Services, are associated with labels to indicate attributes. A `spec` section can include labels during resource definition.

Example labels:

```yaml
metadata:
  labels:
    environment: prod
    tier: frontend
```

#### Resources

Select:

- **All** resources without any specific labels are significant when not tagged.
- **Specific Resources**: Indicate labels to MATCH (AND condition) for resource retrieval.

Example, to find all "frontend" Pods in the "prod" environment:

```yaml
matchLabels:
  environment: prod
  tier: frontend
```

#### Resources that Supports Labels and Selectors

1. **Services**: Use to route and expose network traffic.

2. **ReplicaSets/Horizontally Scaling Controllers**: Helps in load balancing, scaling, and ensuring availability.

3. **DaemonSets/Node Level Controllers**: For system-level operations on all or specific nodes.

4. **Deployments/Rolling Updates and Rollbacks**: Manages dynamic scaling and maintains consistent state.

5. **Jobs/CronJobs**: Facilitates task scheduling and execution.

6. **StatefulSets/Persistent Storage**: Ensures stable and unique network identities for stateful applications.

7. **Pods/Microservices**: Basic building blocks for containerized applications.

8. **Ingress**: Routes external HTTP and HTTPS to Services.

9. **Network Policies**: Defines network access policies.

10. **Endpoints**: Populates the subset of backend Pods for a Service.

11. **PersistentVolumeClaims**: Dynamic storage provisioning and management.

12. **Virtual Machines Deployment**: Uses for virtual machines creation and management.

13. **CronJobs**: Facilitates job scheduling.

14. **PodDisruptionBudgets**: Ensures efficient resource allocation during disruptions.

15. **ServiceMonitor/EndpointSlice**: Specific to monitoring and service discovery.
    <br>

## 10. What would you consider when performing a _rolling update_ in _Kubernetes_?

In **Kubernetes**, a **rolling update** ensures your application is updated without downtime. This process carefully replaces old pods with new ones, employing a **health check mechanism** for a smooth transition.

A rolling update involves several components, including the **Replica Set**, the **Update Control Loop**, the **Pod Update Process**, and **Health Checking** of pods. Here are their details:

### Upgrade Process Flow

1. **Replica Set**: Initiates the update by altering the pods it supervises.

2. **Pod Lifecycle**: Old pods gradually terminate while new ones start up according to the update strategy, such as `maxUnavailable` and `maxSurge`.

3. **Readiness Probes**: Each new pod is verified against its readiness to serve traffic, ensuring the application is live and responsive before the transition.

4. **Liveness Probes**: These checks confirm the stability of new pods. If a pod fails a liveness probe, it's terminated and replaced with a new version, maintaining application reliability.

5. **Rolling Update Status** `status`: Informs about the progress of the update.

6. **Annotations from Provider**: Kubernetes providers may supply additional insights, such as `spec.rollingUpdate.strategy`.

### Code Example: Rolling Update Process

Here is the Kubernetes YAML code:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  strategy:
    rollingUpdate:
      maxUnavailable: 2
      maxSurge: 1
      type: RollingUpdate
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app-container
          image: my-app:v2
          readinessProbe:
            httpGet:
              path: /ready
              port: 8080
          livenessProbe:
            httpGet:
              path: /live
              port: 8080
```

<br>

## 11. How do _Services_ route traffic to _pods_ in _Kubernetes_?

In **Kubernetes**, Services serve as an abstraction layer, enabling consistent access to Pods. Traffic is **routed** to Pods primarily via **Selectors** and **Endpoints**.

### Label Selectors

- **Purpose**: Establish traffic endpoints based on matching labels.
- **Workflow**: Pods are labelled, and Services are configured to match these labels. Upon connectivity, the Service pairs the request to Pods having corresponding labels.
- **Configuration**: Defined in the Service configuration file.

### Endpoints

- **Dynamic Mapping**: Enables fine-grained control over which Pods receive traffic.
- **Workflow**: Automatically managed and updated. When Pods are created or terminated, corresponding Endpoints are adjusted to ensure traffic flow continuity.

### Routing Modes

1. ClusterIP: The default behavior, where each Service is assigned a stable internal IP, accessible only within the cluster.
2. NodePort: Exposes the Service on each Node's IP at a specific port, allowing external access.
3. LoadBalancer: Provisioned by an external cloud provider, creating a load balancer for accessing the Service from outside the cluster.
4. ExternalName: Maps a Service to a DNS name, effectively making the Service accessible from inside the cluster using that DNS name.

### Session Affinity

- **Purpose**: Grants control over the duration for which subsequent requests from the same client are sent to the same Pod.
- **Measured Using Cookies**: When set to `ClientIP`, the user's IP address is used to direct future requests to the same Pod. Using `None` ensures that each request is independently routed.

### Service Types in code and YAML

#### Kubernetes YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
  type: NodePort
```

#### Code Example: Select All Pods with App: MyApp Label

Kubernetes Service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 9376
  type: LoadBalancer
```

Python code that Accesses the Service:

```python
import requests

response = requests.get('http://my-service/')
print(response.text)
```

<br>

## 12. Describe the purpose of _Ingress_ and its key components.

**Ingress** is a powerful Kubernetes service that acts as an HTTP and HTTPS **load balancer** and provides **routing** for external traffic to services in a cluster. Let's look at its key components:

### Key Components

- **Ingress Resource**: This is a Kubernetes object that acts as a configuration file for managing external access to services in a cluster. It defines rules and settings for how traffic should be routed.

- **Ingress Controller**: The Ingress Controller is in charge of obeying the Ingress Resource's rules and configuring the load balancer and traffic routing. Most often, the Ingress Controller is implemented through a Kubernetes extension or a third-party tool, like NGINX or Traefik.

- **Load Balancer**: Operating at layer 7 of the OSI model, the Load Balancer directs traffic to available services based on rules. The Ingress Controller configures and manages this Load Balancer on behalf of the cluster.

  **Note**: A cloud provider might offer its own Load Balancer, but the Ingress Controller can also use standard cloud provider tools or its own mechanism if the cluster isn't on a cloud platform.

- **Rules**: These are defined within the Ingress Resource and specify how incoming traffic should be handled. Rules typically match a specific path or domain and define the associated backend Kubernetes service.

### When to Use Ingress

Ingress is an ideal fit for clusters needing to route HTTP or HTTPS traffic. It's especially beneficial in microservices architectures, as it centralizes and simplifies access to services. This, in turn, improves security, ease of management, and facilitates traffic optimizations like caching.

It should be noted that newer configurations, such as gateway API, offer more extensibility and include features such as traffic splitting and enhanced security measures, which might make them a better choice in certain contexts.
<br>

## 13. Explain _Kubernetes DNS_ resolution for _services_.

**Kubernetes** leverages a consistent, centrally managed Domain Name System (DNS) for containers and services, offering ease of discovery and effictive communication within the cluster.

### Core DNS vs. Kubernetes DNS

In early versions of Kubernetes, **SkyDNS** powered service discovery. However, **CoreDNS** has succeeded it as the default DNS server.

- **CoreDNS** is more extendable and easier to manage.
- Its modular nature means you enable specific features through plugins.

### DNS Resolution Workflow

1. Nodes and pods use `kube-dns` or `coredns` as their predefined DNS servers.
2. The DNS server typically resides within a Kubernetes cluster and knows all service names and their IP addresses.
3. On receiving a DNS query, the DNS server tracks IP changes and ensures name-to-IP mapping.

### Example: Query Flow

1. **Pod Initiates DNS Request**: A pod wants to connect to a service inside or outside the cluster.
2. **DNS Query**: The pod sends a DNS query via the specified server (K8s or custom).
3. **DNS Server**: The server processes the query.
4. **Query Results**: Based on pod's namespace, service name, and domain suffix, the DNS server returns the corresponding IP(s).

### DNS Requirements in the Cluster

- **Service Discovery**: Pods need to locate services. DNS offers an effective way, abstracting the complexity of directly handling service discovery.
- **Name Resolution**: Pods and other entities use DNS to get a service's IP address. The DNS server ensures efficient updates, so pods always have the most accurate IP.

### Considerations for Inter-Pod Communication

1. **Direct Cluster IP**: Services communicate via Cluster IP.
2. **Unrestricted or Port-Defined Communication**: Use the service type of "ClusterIP".
3. **Custom Domains**: For custom domains, specify appropriate service names so the DNS server properly resolves their IPs.

### Namespace Segregation

Without namespace information, separate services with the same name and different namespaces might be unreachable. Including namespace info ensures accuracy.

### Key Configuration Parameters

- **pod** / `spec.dnsPolicy`: Set `ClusterFirst` to utilize the default DNS service.
- **pod** / `spec.dnsConfig`: Specify configs for custom DNS.
- **service** : Utilize `spec.clusterIP` for manual IP assignments. This avoids potential IP address reassignment.

### Inter-Kubernetes Cluster Communication

- For multi-cluster communication, several solutions are available, including direct IP endpoint access and Ingress. DNS resolution strategies can consider these factors.

#### Code Example: Access Service from a Pod

Here is the Kubernetes YAML configuration:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
  namespace: test-ns
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376

---
apiVersion: v1
kind: Pod
metadata:
  name: dns-example
  namespace: test-ns
spec:
  containers:
    - name: test
      image: your-image
      command: ["nslookup", "my-service"]
```

<br>

## 14. What is the _Container Network Interface (CNI)_, and how does it integrate with _Kubernetes_?

The **Container Network Interface (CNI)** is a standard for connecting containers and pods in Kubernetes with underlying networking hardware.

### Integrating with Kubernetes

Containers within a Pod share network namespaces, making inter-container communication straightforward. However, Pods require network isolation, which CNI providers, like Calico, address.

Kubernetes initiates and manages the **CNI connectivity process** as follows:

1. **Network Attachment Definitions (NADs)**: Kubernetes 1.18 and later supports a custom resource called Network Attachment Definitions. This allows operators to specify that a Pod should have a particular network interface.

2. **Multus CNI**: This open-source CNI plugin enables Kubernetes Pods to have multiple network interfaces. With Multus, you can use different CNI plugins to assign either overlay or underlay network interfaces to the Pods.

3. **Kubelet Configuration**: Kubelet, an essential Kubernetes component on each node, is responsible for integrating CNI providers. Configuration commands, typically introduced in the kubelet.service file, facilitate this compatibility.

   ```json
   {
     "cniConfigFilePath": "/etc/cni/net.d/",
     "networkPluginName": "cni-type",
     "featureGates": {
       "CSIMigration": true,
       "CSIMigrationAWS": true,
       "CSIMigrationAzureDisk": true,
       "CSIMigrationAzureFile": true,
       "CSIMigrationGCEPD": true,
       "SupportPodPidsLimit": true
     }
   }
   ```

   Here, `cniConfigFilePath` specifies the path for CNI configuration files, while `networkPluginName` names the CNI plugin Kubernetes should use.

4. **API Server Triggers**: By interacting with the pod lifecycle events, the API server can trigger CNI when pods require network interfaces.

5. **CoreDNS Integration**: CoreDNS acts as a plugin for Kubernetes DNS, providing a unified method for service discovery.

6. **Use Throughout the Stack**: After establishing a network, CNI serves an essential role in the Kubernetes stack, including in Service and Ingress controller configurations.

7. **DaemonSets**: Operators use DaemonSets to run CNI plugins on every Kubernetes node. This ensures network configuration consistency across the cluster.

8. **Namespace Segmentation**: Kubernetes frequently utilizes CNI to prevent traffic from leaking between namespaces, ensuring network security. CNI is primarily responsible for performing network segmentation to meet these policy requirements.
   <br>

## 15. How do you implement _Network Policies_, and what are their benefits?

**Kubernetes Network Policies** regulate traffic within the cluster, boosting both security and performance.

### Core Elements

- **Network Policy:** The rule-set defining traffic behavior.
- **Selector:** Identifies pods to which the policy applies.
- **Gateways / Egress Points:** Regulate outgoing traffic.
- **Ingress Definition:** Specifies allowed inbound connections.

### Policy Types

- **Allow:** Permits specific traffic types.
- **Deny:** Blocks specified traffic types.
- **Blacklist / Whitelist:** Contradictory to Allow & Deny, they either block everything except what's listed (Whitelist) or block specific things except what's listed (Blacklist).

### Policy Language

Different Kubernetes tools and implementations have their own way of writing policies.

- **Cilium:** Uses rich BPF rule expression to define advanced rules and manage application-level policies.
- **Calico:** Adds its security and network features, with a powerful policy engine allowing you to express complex network rules.

### Best Practices

1. **Start Simple:** Build policies gradually, testing after each rule addition.
2. **Documentation:** Detailed policy design and updates encourage consistent and secure practices.
3. **Review Regularly:** Network requirements can change, necessitating policy updates.
4. **Testing:** Use tools like `kube-router` to verify policy application.

### What It Means for Businesses

By implementing **Network Policies**, businesses can ensure more secure and efficient internal Kubernetes communication, meeting compliance and governance requirements.
<br>

- Kubectl Kubernetes CheatSheet :Cloud:

** Common Commands
| Name | Command |
|--------------------------------------+-------------------------------------------------------------------------------------------|
| Run curl test temporarily | =kubectl run --generator=run-pod/v1 --rm mytest --image=yauritux/busybox-curl -it= |
| Run wget test temporarily | =kubectl run --generator=run-pod/v1 --rm mytest --image=busybox -it wget= |
| Run nginx deployment with 2 replicas | =kubectl run my-nginx --image=nginx --replicas=2 --port=80= |
| Run nginx pod and expose it | =kubectl run my-nginx --restart=Never --image=nginx --port=80 --expose= |
| Run nginx deployment and expose it | =kubectl run my-nginx --image=nginx --port=80 --expose= |
| List authenticated contexts | =kubectl config get-contexts=, =~/.kube/config= |
| Set namespace preference | =kubectl config set-context <context_name> --namespace=<ns_name>= |
| List pods with nodes info | =kubectl get pod -o wide= |
| List everything | =kubectl get all --all-namespaces= |
| Get all services | =kubectl get service --all-namespaces= |
| Get all deployments | =kubectl get deployments --all-namespaces= |
| Show nodes with labels | =kubectl get nodes --show-labels= |
| Get resources with json output | =kubectl get pods --all-namespaces -o json= |
| Validate yaml file with dry run | =kubectl create --dry-run --validate -f pod-dummy.yaml= |
| Start a temporary pod for testing | =kubectl run --rm -i -t --image=alpine test-$RANDOM -- sh= |
| kubectl run shell command | =kubectl exec -it mytest -- ls -l /etc/hosts= |
| Get system conf via configmap | =kubectl -n kube-system get cm kubeadm-config -o yaml= |
| Get deployment yaml | =kubectl -n denny-websites get deployment mysql -o yaml= |
| Explain resource | =kubectl explain pods=, =kubectl explain svc= |
| Watch pods | =kubectl get pods -n wordpress --watch= |
| Query healthcheck endpoint | =curl -L http://127.0.0.1:10250/healthz= |
| Open a bash terminal in a pod | =kubectl exec -it storage sh= |
| Check pod environment variables | =kubectl exec redis-master-ft9ex env= |
| Enable kubectl shell autocompletion | =echo "source <(kubectl completion bash)" >>~/.bashrc=, and reload |
| Use minikube dockerd in your laptop | =eval $(minikube docker-env)=, No need to push docker hub any more |
| Kubectl apply a folder of yaml files | =kubectl apply -R -f .= |
| Get services sorted by name | kubectl get services --sort-by=.metadata.name |
| Get pods sorted by restart count | kubectl get pods --sort-by='.status.containerStatuses[0].restartCount' |
| List pods and images | kubectl get pods -o='custom-columns=PODS:.metadata.name,Images:.spec.containers[*].image' |
| List all container images | [[https://github.com/dennyzhang/cheatsheet-kubernetes-A4/blob/master/list-all-images.sh#L14-L17][list-all-images.sh]] |
| kubeconfig skip tls verification | [[https://github.com/dennyzhang/cheatsheet-kubernetes-A4/blob/master/skip-tls-verify.md][skip-tls-verify.md]] |
| [[https://kubernetes.io/docs/tasks/tools/install-kubectl/][Ubuntu install kubectl]] | ="deb https://apt.kubernetes.io/ kubernetes-xenial main"= |
| Reference | [[https://github.com/kubernetes/kubernetes/tags][GitHub: kubernetes releases]] |
| Reference | [[https://cheatsheet.dennyzhang.com/cheatsheet-minikube-A4][minikube cheatsheet]], [[https://cheatsheet.dennyzhang.com/cheatsheet-docker-A4][docker cheatsheet]], [[https://cheatsheet.dennyzhang.com/cheatsheet-openshift-A4][OpenShift CheatSheet]] |
** Check Performance
| Name | Command |
|----------------------------------------------+------------------------------------------------------|
| Get node resource usage | =kubectl top node= |
| Get pod resource usage | =kubectl top pod= |
| Get resource usage for a given pod | =kubectl top <podname> --containers= |
| List resource utilization for all containers | =kubectl top pod --all-namespaces --containers=true= |
** Resources Deletion
| Name | Command |
|-----------------------------------------+----------------------------------------------------------|
| Delete pod | =kubectl delete pod/<pod-name> -n <my-namespace>= |
| Delete pod by force | =kubectl delete pod/<pod-name> --grace-period=0 --force= |
| Delete pods by labels | =kubectl delete pod -l env=test= |
| Delete deployments by labels | =kubectl delete deployment -l app=wordpress= |
| Delete all resources filtered by labels | =kubectl delete pods,services -l name=myLabel= |
| Delete resources under a namespace | =kubectl -n my-ns delete po,svc --all= |
| Delete persist volumes by labels | =kubectl delete pvc -l app=wordpress= |
| Delete state fulset only (not pods) | =kubectl delete sts/<stateful_set_name> --cascade=false= |
#+BEGIN_HTML
<a href="https://cheatsheet.dennyzhang.com"><img align="right" width="185" height="37" src="https://raw.githubusercontent.com/dennyzhang/cheatsheet.dennyzhang.com/master/images/cheatsheet_dns.png"></a>
#+END_HTML
** Log & Conf Files
| Name | Comment |
|---------------------------+---------------------------------------------------------------------------|
| Config folder | =/etc/kubernetes/= |
| Certificate files | =/etc/kubernetes/pki/= |
| Credentials to API server | =/etc/kubernetes/kubelet.conf= |
| Superuser credentials | =/etc/kubernetes/admin.conf= |
| kubectl config file | =~/.kube/config= |
| Kubernetes working dir | =/var/lib/kubelet/= |
| Docker working dir | =/var/lib/docker/=, =/var/log/containers/= |
| Etcd working dir | =/var/lib/etcd/= |
| Network cni | =/etc/cni/net.d/= |
| Log files | =/var/log/pods/= |
| log in worker node | =/var/log/kubelet.log=, =/var/log/kube-proxy.log= |
| log in master node | =kube-apiserver.log=, =kube-scheduler.log=, =kube-controller-manager.log= |
| Env | =/etc/systemd/system/kubelet.service.d/10-kubeadm.conf= |
| Env | export KUBECONFIG=/etc/kubernetes/admin.conf |
** Pod
| Name | Command |
|------------------------------+-------------------------------------------------------------------------------------------|
| List all pods | =kubectl get pods= |
| List pods for all namespace | =kubectl get pods --all-namespaces= |
| List all critical pods | =kubectl get -n kube-system pods -a= |
| List pods with more info | =kubectl get pod -o wide=, =kubectl get pod/<pod-name> -o yaml= |
| Get pod info | =kubectl describe pod/srv-mysql-server= |
| List all pods with labels | =kubectl get pods --show-labels= |
| [[https://github.com/kubernetes/kubernetes/issues/49387][List all unhealthy pods]] | kubectl get pods --field-selector=status.phase!=Running --all-namespaces |
| List running pods | kubectl get pods --field-selector=status.phase=Running |
| Get Pod initContainer status | =kubectl get pod --template '{{.status.initContainerStatuses}}' <pod-name>= |
| kubectl run command | kubectl exec -it -n "$ns" "$podname" -- sh -c "echo $msg >>/dev/err.log" |
| Watch pods | =kubectl get pods -n wordpress --watch= |
| Get pod by selector | kubectl get pods --selector="app=syslog" -o jsonpath='{.items[*].metadata.name}' |
| List pods and images | kubectl get pods -o='custom-columns=PODS:.metadata.name,Images:.spec.containers[*].image' |
| List pods and containers | -o='custom-columns=PODS:.metadata.name,CONTAINERS:.spec.containers[*].name' |
| Reference | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]] |
** Label & Annotation
| Name | Command |
|----------------------------------+-------------------------------------------------------------------|
| Filter pods by label | =kubectl get pods -l owner=denny= |
| Manually add label to a pod | =kubectl label pods dummy-input owner=denny= |
| Remove label | =kubectl label pods dummy-input owner-= |
| Manually add annotation to a pod | =kubectl annotate pods dummy-input my-url=https://dennyzhang.com= |
** Deployment & Scale
| Name | Command |
|------------------------------+--------------------------------------------------------------------------|
| Scale out | =kubectl scale --replicas=3 deployment/nginx-app= |
| online rolling upgrade | =kubectl rollout app-v1 app-v2 --image=img:v2= |
| Roll backup | =kubectl rollout app-v1 app-v2 --rollback= |
| List rollout | =kubectl get rs= |
| Check update status | =kubectl rollout status deployment/nginx-app= |
| Check update history | =kubectl rollout history deployment/nginx-app= |
| Pause/Resume | =kubectl rollout pause deployment/nginx-deployment=, =resume= |
| Rollback to previous version | =kubectl rollout undo deployment/nginx-deployment= |
| Reference | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]], [[https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#pausing-and-resuming-a-deployment][Link: Pausing and Resuming a Deployment]] |
#+BEGIN_HTML
<a href="https://cheatsheet.dennyzhang.com"><img align="right" width="185" height="37" src="https://raw.githubusercontent.com/dennyzhang/cheatsheet.dennyzhang.com/master/images/cheatsheet_dns.png"></a>
#+END_HTML
** Quota & Limits & Resource
| Name | Command |
|-------------------------------+-------------------------------------------------------------------------|
| List Resource Quota | =kubectl get resourcequota= |
| List Limit Range | =kubectl get limitrange= |
| Customize resource definition | =kubectl set resources deployment nginx -c=nginx --limits=cpu=200m= |
| Customize resource definition | =kubectl set resources deployment nginx -c=nginx --limits=memory=512Mi= |
| Reference | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]] |
** Service
| Name | Command |
|---------------------------------+-----------------------------------------------------------------------------------|
| List all services | =kubectl get services= |
| List service endpoints | =kubectl get endpoints= |
| Get service detail | =kubectl get service nginx-service -o yaml= |
| Get service cluster ip | kubectl get service nginx-service -o go-template='{{.spec.clusterIP}}' |
| Get service cluster port | kubectl get service nginx-service -o go-template='{{(index .spec.ports 0).port}}' |
| Expose deployment as lb service | =kubectl expose deployment/my-app --type=LoadBalancer --name=my-service= |
| Expose service as lb service | =kubectl expose service/wordpress-1-svc --type=LoadBalancer --name=ns1= |
| Reference | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]] |
** Secrets
| Name | Command |
|----------------------------------+-------------------------------------------------------------------------|
| List secrets | =kubectl get secrets --all-namespaces= |
| Generate secret | =echo -n 'mypasswd', then redirect to base64 --decode= |
| Get secret | =kubectl get secret denny-cluster-kubeconfig= |
| Get a specific field of a secret | kubectl get secret denny-cluster-kubeconfig -o jsonpath="{.data.value}" |
| Create secret from cfg file | kubectl create secret generic db-user-pass --from-file=./username.txt |
| Reference | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]], [[https://kubernetes.io/docs/concepts/configuration/secret/][Link: Secrets]] |
** StatefulSet
| Name | Command |
|------------------------------------+----------------------------------------------------------|
| List statefulset | =kubectl get sts= |
| Delete statefulset only (not pods) | =kubectl delete sts/<stateful_set_name> --cascade=false= |
| Scale statefulset | =kubectl scale sts/<stateful_set_name> --replicas=5= |
| Reference | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]] |
** Volumes & Volume Claims
| Name | Command |
|---------------------------+--------------------------------------------------------------|
| List storage class | =kubectl get storageclass= |
| Check the mounted volumes | =kubectl exec storage ls /data= |
| Check persist volume | =kubectl describe pv/pv0001= |
| Copy local file to pod | =kubectl cp /tmp/my <some-namespace>/<some-pod>:/tmp/server= |
| Copy pod file to local | =kubectl cp <some-namespace>/<some-pod>:/tmp/server /tmp/my= |
| Reference | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]] |
** Events & Metrics
| Name | Command |
|---------------------------------+------------------------------------------------------------|
| View all events | =kubectl get events --all-namespaces= |
| List Events sorted by timestamp | kubectl get events --sort-by=.metadata.creationTimestamp |
** Node Maintenance
| Name | Command |
|-------------------------------------------+-------------------------------|
| Mark node as unschedulable | =kubectl cordon $NODE_NAME= |
| Mark node as schedulable | =kubectl uncordon $NODE_NAME= |
| Drain node in preparation for maintenance | =kubectl drain $NODE_NAME= |
** Namespace & Security
| Name | Command |
|-------------------------------+-----------------------------------------------------------------------------------------------------|
| List authenticated contexts | =kubectl config get-contexts=, =~/.kube/config= |
| Set namespace preference | =kubectl config set-context <context_name> --namespace=<ns_name>= |
| Switch context | =kubectl config use-context <context_name>= |
| Load context from config file | =kubectl get cs --kubeconfig kube_config.yml= |
| Delete the specified context | =kubectl config delete-context <context_name>= |
| List all namespaces defined | =kubectl get namespaces= |
| List certificates | =kubectl get csr= |
| [[https://kubernetes.io/docs/concepts/policy/pod-security-policy/][Check user privilege]] | kubectl --as=system:serviceaccount:ns-denny:test-privileged-sa -n ns-denny auth can-i use pods/list |
| [[https://kubernetes.io/docs/concepts/policy/pod-security-policy/][Check user privilege]] | =kubectl auth can-i use pods/list= |
| Reference | [[https://cheatsheet.dennyzhang.com/kubernetes-yaml-templates][Link: kubernetes yaml templates]] |
** Network
| Name | Command |
|-----------------------------------+----------------------------------------------------------|
| Temporarily add a port-forwarding | =kubectl port-forward redis-134 6379:6379= |
| Add port-forwarding for deployment | =kubectl port-forward deployment/redis-master 6379:6379= |
| Add port-forwarding for replicaset | =kubectl port-forward rs/redis-master 6379:6379= |
| Add port-forwarding for service | =kubectl port-forward svc/redis-master 6379:6379= |
| Get network policy | =kubectl get NetworkPolicy= |
| Get ingress controller | =kubectl get ingress= |
| Get ingress classes | =kubectl get ingressclasses= |  
** Patch
| Name | Summary |
|-------------------------------+---------------------------------------------------------------------|
| Patch service to loadbalancer | kubectl patch svc $svc_name -p '{"spec": {"type": "LoadBalancer"}}' |
** Extenstions
| Name | Summary |
|-----------------------------------------+----------------------------|
| Enumerates the resource types available | =kubectl api-resources= |
| List api group | =kubectl api-versions= |
| List all CRD | =kubectl get crd= |
| List storageclass | =kubectl get storageclass= |
#+BEGIN_HTML
<a href="https://cheatsheet.dennyzhang.com"><img align="right" width="185" height="37" src="https://raw.githubusercontent.com/dennyzhang/cheatsheet.dennyzhang.com/master/images/cheatsheet_dns.png"></a>
#+END_HTML
** Components & Services \*** Services on Master Nodes
| Name | Summary |
|--------------------------+--------------------------------------------------------------------------------------------|
| [[https://github.com/kubernetes/kubernetes/tree/master/cmd/kube-apiserver][kube-apiserver]] | API gateway. Exposes the Kubernetes API from master nodes |
| [[https://coreos.com/etcd/][etcd]] | reliable data store for all k8s cluster data |
| [[https://github.com/kubernetes/kubernetes/tree/master/cmd/kube-scheduler][kube-scheduler]] | schedule pods to run on selected nodes |
| [[https://github.com/kubernetes/kubernetes/tree/master/cmd/kube-controller-manager][kube-controller-manager]] | Reconcile the states. node/replication/endpoints/token controller and service account, etc |
| cloud-controller-manager | |
\*\*\* Services on Worker Nodes
| Name | Summary |
|-------------------+----------------------------------------------------------------------------------------------|
| [[https://github.com/kubernetes/kubernetes/tree/master/cmd/kubelet][kubelet]] | A node agent makes sure that containers are running in a pod |
| [[https://github.com/kubernetes/kubernetes/tree/master/cmd/kube-proxy][kube-proxy]] | Manage network connectivity to the containers. e.g, iptable, ipvs |
| [[https://github.com/docker/engine][Container Runtime]] | Kubernetes supported runtimes: dockerd, cri-o, runc and any [[https://github.com/opencontainers/runtime-spec][OCI runtime-spec]] implementation. |

\*\*\* Addons: pods and services that implement cluster features
| Name | Summary |
|-------------------------------+---------------------------------------------------------------------------|
| DNS | serves DNS records for Kubernetes services |
| Web UI | a general purpose, web-based UI for Kubernetes clusters |
| Container Resource Monitoring | collect, store and serve container metrics |
| Cluster-level Logging | save container logs to a central log store with search/browsing interface |

**\* Tools
| Name | Summary |
|-----------------------+-------------------------------------------------------------|
| [[https://github.com/kubernetes/kubernetes/tree/master/cmd/kubectl][kubectl]] | the command line util to talk to k8s cluster |
| [[https://github.com/kubernetes/kubernetes/tree/master/cmd/kubeadm][kubeadm]] | the command to bootstrap the cluster |
| [[https://kubernetes.io/docs/reference/setup-tools/kubefed/kubefed/][kubefed]] | the command line to control a Kubernetes Cluster Federation |
| Kubernetes Components | [[https://kubernetes.io/docs/concepts/overview/components/][Link: Kubernetes Components]] |
** More Resources
License: Code is licensed under [[https://www.dennyzhang.com/wp-content/mit_license.txt][MIT License]].

https://kubernetes.io/docs/reference/kubectl/cheatsheet/

https://codefresh.io/kubernetes-guides/kubernetes-cheat-sheet/

#+BEGIN_HTML
<a href="https://cheatsheet.dennyzhang.com"><img align="right" width="201" height="268" src="https://raw.githubusercontent.com/USDevOps/mywechat-slack-group/master/images/denny_201706.png"></a>
<a href="https://cheatsheet.dennyzhang.com"><img align="right" src="https://raw.githubusercontent.com/dennyzhang/cheatsheet.dennyzhang.com/master/images/cheatsheet_dns.png"></a>

<a href="https://www.linkedin.com/in/dennyzhang001"><img align="bottom" src="https://www.dennyzhang.com/wp-content/uploads/sns/linkedin.png" alt="linkedin" /></a>
<a href="https://github.com/dennyzhang"><img align="bottom"src="https://www.dennyzhang.com/wp-content/uploads/sns/github.png" alt="github" /></a>
<a href="https://www.dennyzhang.com/slack" target="_blank" rel="nofollow"><img align="bottom" src="https://www.dennyzhang.com/wp-content/uploads/sns/slack.png" alt="slack"/></a>
#+END_HTML

- org-mode configuration :noexport:
  #+STARTUP: overview customtime noalign logdone showall
  #+DESCRIPTION:
  #+KEYWORDS:
  #+LATEX_HEADER: \usepackage[margin=0.6in]{geometry}
  #+LaTeX_CLASS_OPTIONS: [8pt]
  #+LATEX_HEADER: \usepackage[english]{babel}
  #+LATEX_HEADER: \usepackage{lastpage}
  #+LATEX_HEADER: \usepackage{fancyhdr}
  #+LATEX_HEADER: \pagestyle{fancy}
  #+LATEX_HEADER: \fancyhf{}
  #+LATEX_HEADER: \rhead{Updated: \today}
  #+LATEX_HEADER: \rfoot{\thepage\ of \pageref{LastPage}}
  #+LATEX_HEADER: \lfoot{\href{https://github.com/dennyzhang/cheatsheet-kubernetes-A4}{GitHub: https://github.com/dennyzhang/cheatsheet-kubernetes-A4}}
  #+LATEX_HEADER: \lhead{\href{https://cheatsheet.dennyzhang.com/cheatsheet-kubernetes-A4}{Blog URL: https://cheatsheet.dennyzhang.com/cheatsheet-kubernetes-A4}}
  #+AUTHOR: Denny Zhang
  #+EMAIL: denny@dennyzhang.com
  #+TAGS: noexport(n)
  #+PRIORITIES: A D C
  #+OPTIONS: H:3 num:t toc:nil \n:nil @:t ::t |:t ^:t -:t f:t \*:t <:t
  #+OPTIONS: TeX:t LaTeX:nil skip:nil d:nil todo:t pri:nil tags:not-in-toc
  #+EXPORT_EXCLUDE_TAGS: exclude noexport
  #+SEQ_TODO: TODO HALF ASSIGN | DONE BYPASS DELEGATE CANCELED DEFERRED
  #+LINK_UP:
  #+LINK_HOME:
- # --8<-------------------------- separator ------------------------>8-- :noexport:
- DONE Misc scripts :noexport:
  CLOSED: [2018-11-17 Sat 12:23]

* Tail pod log by label
  #+BEGIN_SRC sh
  namespace="mynamespace"
  mylabel="app=mylabel"
  kubectl get pod -l "$mylabel" -n "$namespace" | tail -n1 \
   | awk -F' ' '{print $1}' | xargs -I{} \
      kubectl logs -n "$namespace" -f {}
  #+END_SRC

* Get node hardware resource utilization
  #+BEGIN_SRC sh
  kubectl get nodes --no-headers \
   | awk '{print $1}' | xargs -I {} \
   sh -c 'echo {}; kubectl describe node {} | grep Allocated -A 5'

kubectl get nodes --no-headers | awk '{print $1}' | xargs -I {} \
 sh -c 'echo {}; kubectl describe node {} | grep Allocated -A 5 \
 | grep -ve Event -ve Allocated -ve percent -ve -- ; echo'
#+END_SRC

- Apply the configuration in manifest.yaml and delete all the other configmaps that are not in the file.

#+BEGIN_EXAMPLE
kubectl apply --prune -f manifest.yaml --all --prune-whitelist=core/v1/ConfigMap
#+END_EXAMPLE

- [#A] Kubernetes :noexport:IMPORTANT:
  https://github.com/dennyzhang/cheatsheet-kubernetes-A4

k8s provides declarative primitives for the "desired state"

- Self-healing
- Horizontal scaling
- Automatic binpacking
- Service discovery and load balancing
  ** Names of certificates files
  https://github.com/kubernetes/kubeadm/blob/master/docs/design/design_v1.9.md
  Names of certificates files:
  ca.crt, ca.key (CA certificate)
  apiserver.crt, apiserver.key (API server certificate)
  apiserver-kubelet-client.crt, apiserver-kubelet-client.key (client certificate for the apiservers to connect to the kubelets securely)
  sa.pub, sa.key (a private key for signing ServiceAccount )
  front-proxy-ca.crt, front-proxy-ca.key (CA for the front proxy)
  front-proxy-client.crt, front-proxy-client.key (client cert for the front proxy client)
  ** TODO update k8s cheatsheet github: https://github.com/alex1x/kubernetes-cheatsheet
  ** TODO Setting up MySQL Replication Clusters in Kubernetes: https://blog.kublr.com/setting-up-mysql-replication-clusters-in-kubernetes-ab7cbac113a5
  ** TODO MySQL on Docker: Running Galera Cluster on Kubernetes
  https://severalnines.com/blog/mysql-docker-running-galera-cluster-kubernetes
  ** TODO Try Functions as a Service - a serverless framework for Docker & Kubernetes http://docs.get-faas.com/
  https://blog.alexellis.io/first-faas-python-function/
  ** TODO [#A] k8s clustering elasticsearch
  https://blog.alexellis.io/kubernetes-kubeadm-video/
  ** TODO k8s scale with redis
  ** TODO k8s scale with mysqld
  ** TODO [#A] k8s: https://5pi.de/2016/11/20/15-producation-grade-kubernetes-cluster/
  ** TODO Try kops with k8s
  ** TODO k8s free course: https://classroom.udacity.com/courses/ud615
  ** TODO feedbackup for k8s study project
  Aaron Mulholland [1:18 AM]
  So it looks pretty good. Got some good concepts in early on. Couple of suggestions for further work;

Potentially the following scenarios;
_ Setting up ingresses and TLS
_ Fully configure something like Nginx Ingress Controller or Traefik.
_ Create TLS Secrets within Kubernetes, and use them in your ingress controller.
_ Managing RBAC (Don't know enough about this one, but sounds like a good concept to include) \* Creating new roles, etc

I'll have a think and if anymore come to me, I'll let you know.

Denny Zhang (Github . Blogger)
[1:19 AM]
:thumbsup:

Will update per your suggestions tomorrow, Aaron
\*\* TODO k8s add DNS challenges
Gui [4:01 PM]
Getting familiar with the concepts like pod, service, RC, deployment, etc.

[4:02]
Try volume

[4:02]
DNS.

Denny Zhang (Github . Blogger)
[4:02 PM]
I'm trying to cover the volume via mysql scenarios

Gui [4:02 PM]
And other addons
1 reply Today at 4:03 PM View thread

Denny Zhang (Github . Blogger)
[4:02 PM]
For DNS, not sure whether I get your point

Gui [4:03 PM]
I haven't tried a lot myself.
1 reply Today at 4:03 PM View thread

[4:03]
Like every pod and service has an DNS name to talk to each other.

Denny Zhang (Github . Blogger) [4:04 PM]
Yes, that makes sense

[4:04]
For addons, do you have any recommended scenario?
\*\* TODO k8s add challenge of addon
https://www.cncf.io

https://kubernetes.io/docs/concepts/cluster-administration/addons/
** TODO k8s networking models
** TODO k8s example: https://github.com/kubernetes/examples
** TODO Blog: Wordpress powered by k8s, docker swarm
** # --8<-------------------------- separator ------------------------>8-- :noexport:
** TODO [#A] absord: https://github.com/kubecamp/kubernetes_in_one_day
** TODO [#A] absord: https://github.com/kubecamp/kubernetes_in_2_days
** DONE kubectl config view
CLOSED: [2017-12-31 Sun 10:40]
** DONE [#A] kubernetes persistent volume claim pending
CLOSED: [2017-12-31 Sun 11:32]
https://github.com/openshift/origin/issues/7170

kubectl get pvc
kubectl get pv

#+BEGIN_EXAMPLE
ubuntu@k8s1:~$ kubectl describe pvc
Name: ironic-gerbil-jenkins
Namespace: default
StorageClass:
Status: Pending
Volume:
Labels: app=ironic-gerbil-jenkins
chart=jenkins-0.10.2
heritage=Tiller
release=ironic-gerbil
Annotations: <none>
Capacity:
Access Modes:
Events:
Type Reason Age From Message

---

Normal FailedBinding 37s (x261 over 2h) persistentvolume-controller no persistent volumes available for this claim and no storage class is set

Name: my-mysql-mysql
Namespace: default
StorageClass:
Status: Pending
Volume:
Labels: app=my-mysql-mysql
chart=mysql-0.3.2
heritage=Tiller
release=my-mysql
Annotations: <none>
Capacity:
Access Modes:
Events:
Type Reason Age From Message

---

Normal FailedBinding 7s (x5 over 1m) persistentvolume-controller no persistent volumes available for this claim and no storage class is set
#+END_EXAMPLE
** DONE kubernetes start a container for testing: kubectl run -i --tty ubuntu --image=ubuntu:16.04 --restart=Never -- bash -il
CLOSED: [2017-12-31 Sun 11:26]
** DONE [#A] ReplicaSet is the next-generation Replication Controller.
CLOSED: [2017-12-04 Mon 11:26]
The only difference between a ReplicaSet and a Replication Controller right now is the selector support.

https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/

https://github.com/arun-gupta/oreilly-kubernetes-book/blob/master/ch01/wildfly-replicaset.yml
Next generation Replication Controller

Set-based selector requirement

- Expression: key, operator, value
- Operators: In, NotIn, Exists, DoesNotExist

▪Generally created with Deployment
▪Enables Horizontal Pod Autoscaling
** DONE k8s yaml API version: https://kubernetes.io/docs/reference/federation/extensions/v1beta1/definitions/
CLOSED: [2017-12-03 Sun 12:50]
** DONE k8s cronjob
CLOSED: [2018-01-03 Wed 12:26]
https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/

kubectl create -f ./cronjob.yaml
kubectl get cronjob hello
kubectl get jobs --watch
kubectl delete cronjob hello

#+BEGIN*EXAMPLE
apiVersion: batch/v1beta1
kind: CronJob
metadata:
name: hello
spec:
schedule: "*/1 \_ \* \* \*"
jobTemplate:
spec:
template:
spec:
containers: - name: hello
image: busybox
args: - /bin/sh - -c - date; echo Hello from the Kubernetes cluster
restartPolicy: OnFailure
#+END_EXAMPLE
** DONE [#B] check k8s status: kubectl get cs
CLOSED: [2018-01-03 Wed 11:57]
** BYPASS crictl not found in system path: warning
CLOSED: [2018-01-03 Wed 12:36]
** DONE kubernetes default service type: ClusterIP
CLOSED: [2018-01-02 Tue 11:07]
** DONE kubectl get nodes: Unable to connect to the server: x509: certificate signed by unknown authority: incorrect /etc/kubernetes/admin.conf
CLOSED: [2018-01-04 Thu 00:09]

root@k8s1:~# kubectl get nodes
Unable to connect to the server: x509: certificate signed by unknown authority (possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "kubernetes")
root@k8s1:~# echo $KUBECONFIG

root@k8s1:~# export KUBECONFIG=/etc/kubernetes/admin.conf
root@k8s1:~# kubectl get nodes
NAME STATUS ROLES AGE VERSION
k8s1 Ready master 29m v1.9.0
k8s2 NotReady <none> 17m v1.9.0
** DONE [#A] kubernetes-the-hard-way: https://github.com/kelseyhightower/kubernetes-the-hard-way
CLOSED: [2017-12-04 Mon 15:49] \*** CANCELED k8s hardway: etcdctl: Error: context deadline exceeded
CLOSED: [2017-12-04 Mon 17:54]
https://github.com/kelseyhightower/kubernetes-the-hard-way/blob/e8d728d0162ebcdf951464caa8be3a5b156eb463/docs/07-bootstrapping -etcd.md
#+BEGIN_EXAMPLE
mac@controller-0:~$ ETCDCTL_API=3 etcdctl member list
Error: context deadline exceeded
#+END_EXAMPLE

#+BEGIN_EXAMPLE
mac@controller-0:~$ kubectl get componentstatuses
NAME STATUS MESSAGE ERROR
etcd-2 Unhealthy Get https://10.240.0.12:2379/health: dial tcp 10.240.0.12:2379: getsockopt: connection refused
controller-manager Healthy ok
etcd-1 Unhealthy Get https://10.240.0.11:2379/health: dial tcp 10.240.0.11:2379: getsockopt: connection refused
scheduler Healthy ok
etcd-0 Unhealthy Get https://10.240.0.10:2379/health: net/http: TLS handshake timeout
#+END_EXAMPLE
\*\* DONE k8s livenessProbe(when to restart a Container), readinessProbe(when is ready to accept requests)
CLOSED: [2018-01-08 Mon 07:41]
https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/
http://kubernetesbyexample.com/healthz/
https://kubernetes-v1-4.github.io/docs/user-guide/liveness/
https://github.com/arun-gupta/kubernetes-java-sample/blob/master/wildfly-pod-hc-http.yaml
http://kubernetesbyexample.com/healthz/

Probes have a number of fields that you can use to more precisely control the behavior of liveness and readiness checks:

initialDelaySeconds: Number of seconds after the container has started before liveness or readiness probes are initiated.
periodSeconds: How often (in seconds) to perform the probe. Default to 10 seconds. Minimum value is 1.
timeoutSeconds: Number of seconds after which the probe times out. Defaults to 1 second. Minimum value is 1.
successThreshold: Minimum consecutive successes for the probe to be considered successful after having failed. Defaults to 1. Must be 1 for liveness. Minimum value is 1.
failureThreshold: When a Pod starts and the probe fails, Kubernetes will try failureThreshold times before giving up. Giving up in case of liveness probe means restarting the Pod. In case of readiness probe the Pod will be marked Unready. Defaults to 3. Minimum value is 1.

#+BEGIN_EXAMPLE
apiVersion: v1
kind: Pod
metadata:
labels:
test: liveness
name: liveness-exec
spec:
containers:

- args: - /bin/sh - -c - echo ok > /tmp/health; sleep 10; rm -rf /tmp/health; sleep 600
  image: gcr.io/google_containers/busybox
  livenessProbe:
  exec:
  command: - cat - /tmp/health
  initialDelaySeconds: 15
  timeoutSeconds: 1
  name: liveness
  #+END_EXAMPLE
  \*\* DONE list all critical pods
  CLOSED: [2018-01-04 Thu 10:10]
  kubectl --namespace kube-system get pods

for pod in $(kubectl --namespace kube-system get pods -o jsonpath="{.items[*].metadata.name}"); do
    node_info=$(kubectl --namespace kube-system describe pod $pod | grep "Node:")
echo "Pod: $pod, $node_info"
done
** DONE k8s cheatsheet: kube-shell https://github.com/cloudnativelabs/kube-shell
CLOSED: [2017-12-31 Sun 10:47]
** DONE k8s configmap
CLOSED: [2018-01-08 Mon 10:32]
https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/
| Name | Summary |
|-----------------------------------------------------+---------|
| kubectl get configmaps my-wordpress-mariadb -o yaml | |
** DONE [#A] k8s initContainers debug: kubectl logs <pod-name> -c <init-container-2>
CLOSED: [2018-01-05 Fri 16:29]
https://kubernetes.io/docs/tasks/debug-application-cluster/debug-init-containers/
** DONE Use GCE to setup k8s cluster deployment
CLOSED: [2018-01-07 Sun 07:26]
https://github.com/kelseyhightower/kubernetes-the-hard-way

https://cloud.google.com/
source /Users/mac/Downloads/google-cloud-sdk/completion.bash.inc
source /Users/mac/Downloads/google-cloud-sdk/path.bash.inc
\*\*\* doc: gcloud setup
#+BEGIN_EXAMPLE
[28] us-central1-f
[29] us-central1-c
[30] us-central1-b
[31] us-east1-d
[32] us-east1-c
[33] us-east1-b
[34] us-east4-c
[35] us-east4-a
[36] us-east4-b
[37] us-west1-a
[38] us-west1-c
[39] us-west1-b
[40] Do not set default zone
Please enter numeric choice or text value (must exactly match list
item): 36

Your project default Compute Engine zone has been set to [us-east4-b].
You can change it by running [gcloud config set compute/zone NAME].

Your project default Compute Engine region has been set to [us-east4].
You can change it by running [gcloud config set compute/region NAME].

Created a default .boto configuration file at [/Users/mac/.boto]. See this file and
[https://cloud.google.com/storage/docs/gsutil/commands/config] for more
information about configuring Google Cloud Storage.
Your Google Cloud SDK is configured and ready to use!

- Commands that require authentication will use denny.zhang001@gmail.com by default
- Commands will reference project `denny-k8s-test1` by default
- Compute Engine commands will use region `us-east4` by default
- Compute Engine commands will use zone `us-east4-b` by default

Run `gcloud help config` to learn how to change individual settings

This gcloud configuration is called [default]. You can create additional configurations if you work with multiple accounts and/or projects.
Run `gcloud topic configurations` to learn more.

Some things to try next:

- Run `gcloud --help` to see the Cloud Platform services you can interact with. And run `gcloud help COMMAND` to get help on any gcloud command.
- Run `gcloud topic -h` to learn about advanced features of the SDK like arg files and output formatting
  #+END_EXAMPLE
  **\* TODO [#A] can't find gcloud :IMPORTANT:
  source /Users/mac/Downloads/google-cloud-sdk/completion.bash.inc
  source /Users/mac/Downloads/google-cloud-sdk/path.bash.inc
  ** DONE kubectl get pod
  CLOSED: [2018-04-28 Sat 09:28]
  /etc/kubernetes/admin.conf /etc/kubernetes/kubelet.conf /etc/kubernetes/bootstrap-kubelet.conf /etc/kubernetes/controller-manager.conf /etc/kubernetes/scheduler.conf]

#+BEGIN_EXAMPLE
Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

mkdir -p $HOME/.kube
   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
   sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
https://kubernetes.io/docs/concepts/cluster-administration/addons/
#+END_EXAMPLE
\*\* DONE pod CrashLoopBackOff: starting, then crashing, then starting again and crashing again.

CLOSED: [2018-01-05 Fri 15:47]
https://www.krenger.ch/blog/crashloopbackoff-and-how-to-fix-it/

https://kubernetes.io/docs/tasks/debug-application-cluster/debug-init-containers/

| Status | Meaning |
|----------------------------+-------------------------------------------------------------|
| Init:N/M | The Pod has M Init Containers, and N have completed so far. |
| Init:Error | An Init Container has failed to execute. |
| Init:CrashLoopBackOff | An Init Container has failed repeatedly. |
| Pending | The Pod has not yet begun executing Init Containers. |
| PodInitializing or Running | The Pod has already finished executing Init Containers. |
** DONE k8s ImagePullBackOff: describe pod $pod_name; No space
CLOSED: [2018-06-25 Mon 14:28]
** DONE default pods for single node installation
CLOSED: [2018-04-28 Sat 08:49]
#+BEGIN_EXAMPLE
root@mdm-k8s-node2:~# docker ps
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
75d08dd2b171 k8s.gcr.io/kube-proxy-amd64@sha256:c7036a8796fd20c16cb3b1cef803a8e980598bff499084c29f3c759bdb429cd2 "/usr/local/bin/ku..." 16 hours ago Up 16 hours k8s_kube-proxy_kube-proxy-jmcs9_kube-system_02a0eac8-4a75-11e8-afce-7aa5a78d07bd_0
0a769558ec4f k8s.gcr.io/pause-amd64:3.1 "/pause" 16 hours ago Up 16 hours k8s_POD_kube-proxy-jmcs9_kube-system_02a0eac8-4a75-11e8-afce-7aa5a78d07bd_0
2af1fbfd581a k8s.gcr.io/kube-apiserver-amd64@sha256:1ba863c8e9b9edc6d1329ebf966e4aa308ca31b42a937b4430caf65aa11bdd12 "kube-apiserver --..." 16 hours ago Up 16 hours k8s_kube-apiserver_kube-apiserver-mdm-k8s-node2_kube-system_fee65b809c1e455cf1672ebe7efc4bc7_0
63c214ac8d1b k8s.gcr.io/kube-controller-manager-amd64@sha256:922ac89166ea228cdeff43e4c445a5dc4204972cc0e265a8762beec07b6238bf "kube-controller-m..." 16 hours ago Up 16 hours k8s_kube-controller-manager_kube-controller-manager-mdm-k8s-node2_kube-system_5ad7a10c5a8589117db7258c7d499a33_0
324ff1a8d357 k8s.gcr.io/kube-scheduler-amd64@sha256:5f50a339f66037f44223e2b4607a24888177da6203a7bc6c8554e0f09bd2b644 "kube-scheduler --..." 16 hours ago Up 16 hours k8s_kube-scheduler_kube-scheduler-mdm-k8s-node2_kube-system_aa8d5cab3ea096315de0c2003230d4f9_0
dce77d944669 k8s.gcr.io/etcd-amd64@sha256:68235934469f3bc58917bcf7018bf0d3b72129e6303b0bef28186d96b2259317 "etcd --listen-cli..." 16 hours ago Up 16 hours k8s_etcd_etcd-mdm-k8s-node2_kube-system_59f847fe34319ab1263f0b3ee03df8a3_0
2af621e52e11 k8s.gcr.io/pause-amd64:3.1 "/pause" 16 hours ago Up 16 hours k8s_POD_kube-apiserver-mdm-k8s-node2_kube-system_fee65b809c1e455cf1672ebe7efc4bc7_0
bdc64588b27d k8s.gcr.io/pause-amd64:3.1 "/pause" 16 hours ago Up 16 hours k8s_POD_kube-controller-manager-mdm-k8s-node2_kube-system_5ad7a10c5a8589117db7258c7d499a33_0
14dd26427abf k8s.gcr.io/pause-amd64:3.1 "/pause" 16 hours ago Up 16 hours k8s_POD_kube-scheduler-mdm-k8s-node2_kube-system_aa8d5cab3ea096315de0c2003230d4f9_0
17bfbb8af205 k8s.gcr.io/pause-amd64:3.1 "/pause" 16 hours ago Up 16 hours k8s_POD_etcd-mdm-k8s-node2_kube-system_59f847fe34319ab1263f0b3ee03df8a3_0
#+END_EXAMPLE
** DONE One pod may have multiple containers
CLOSED: [2018-06-19 Tue 14:31]
If a pod has more than 1 containers then you need to provide the name of the specific container.
** DONE kubectl edit deployment parameters
CLOSED: [2018-04-15 Sun 21:49]
https://github.com/kubernetes/helm/issues/2464
kubectl -n kube-system patch deployment tiller-deploy -p '{"spec": {"template": {"spec": {"automountServiceAccountToken": true}}}}'

kubectl --namespace=kube-system edit deployment/tiller-deploy and changed automountServiceAccountToken to true.
\*\* DONE [#A] k8s sidecar
CLOSED: [2018-07-15 Sun 22:50]
https://k8s.io/examples/admin/logging/two-files-counter-pod-streaming-sidecar.yaml
#+BEGIN_EXAMPLE
apiVersion: v1
kind: Pod
metadata:
name: counter
spec:
containers:

- name: count
  image: busybox
  args:
  - /bin/sh
  - -c
  - > i=0;
    > while true;
    > do
        echo "$i: $(date)" >> /var/log/1.log;
        echo "$(date) INFO $i" >> /var/log/2.log;
        i=$((i+1));
        sleep 1;
    done
    volumeMounts:
  - name: varlog
    mountPath: /var/log
- name: count-log-1
  image: busybox
  args: [/bin/sh, -c, 'tail -n+1 -f /var/log/1.log']
  volumeMounts:
  - name: varlog
    mountPath: /var/log
- name: count-log-2
  image: busybox
  args: [/bin/sh, -c, 'tail -n+1 -f /var/log/2.log']
  volumeMounts:
  - name: varlog
    mountPath: /var/log
    volumes:
- name: varlog
  emptyDir: {}
  #+END_EXAMPLE
  ** TODO [#A] k8s debug why termination takes time
  ** TODO Kubernetes availability
  **\* TODO Building High-Availability Clusters: https://kubernetes.io/docs/admin/high-availability/
  ** TODO [#A] Blog: Kubernetes Service Type: NodePort, ClusterIP and Loadbalancer?
  #+BEGIN_EXAMPLE
  https://kubernetes.io/docs/concepts/services-networking/service/

Publishing services - service types
For some parts of your application (e.g. frontends) you may want to expose a Service onto an external (outside of your cluster) IP address.

Kubernetes ServiceTypes allow you to specify what kind of service you want. The default is ClusterIP.

Type values and their behaviors are:

ClusterIP: Exposes the service on a cluster-internal IP. Choosing this value makes the service only reachable from within the cluster. This is the default ServiceType.
NodePort: Exposes the service on each Node's IP at a static port (the NodePort). A ClusterIP service, to which the NodePort service will route, is automatically created. You'll be able to contact the NodePort service, from outside the cluster, by requesting <NodeIP>:<NodePort>.
LoadBalancer: Exposes the service externally using a cloud provider's load balancer. NodePort and ClusterIP services, to which the external load balancer will route, are automatically created.
ExternalName: Maps the service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value. No proxying of any kind is set up. This requires version 1.7 or higher of kube-dns.
#+END\*EXAMPLE
\*\*\* Type: Loadbalancer
_** Type: ClusterIP
**_ Type: NodePort
If you set the type field to "NodePort", the Kubernetes master will allocate a port from a flag-configured range (default: 30000-32767)
\_\*\* # --8<-------------------------- separator ------------------------>8-- :noexport:
\*\*\* TODO Now if i access IP:NodePort, will it balance the load across multiple pods ?
https://kubernetes.io/docs/tasks/access-application-cluster/load-balance-access-application-cluster/
#+BEGIN_EXAMPLE
Vivek Yadav [8:34 AM]
Hey Denny, quick question -

```
---
 apiVersion: v1
 kind: Service
 metadata:
   name: span
   labels:
     app: span
 spec:
   type: NodePort
   ports:
     - port: 80
       nodePort: 30080
   selector:
     app: spa

---
 apiVersion: apps/v1beta2
 kind: Deployment
 metadata:
   name: spa
 spec:
   replicas: 2
   selector:
     matchLabels:
       app: spa
   template:
     metadata:
       labels:
         app: spa
     spec:
       containers:
         - name: py
           image: viveky4d4v/local-simple-python:latest
           ports:
             - containerPort: 8080
         - name: nginx
           image: viveky4d4v/local-nginx-lb:latest
           ports:
             - containerPort: 80
       imagePullSecrets:
         - name: regsecret

```

Now if i access IP:NodePort, will it balance the load across multiple pods ?

Denny Zhang (Github . Blogger) [8:35 AM]
I don't think so
#+END\*EXAMPLE
\*\*\* TODO How Does NodePort work behind the scene?
_** # --8<-------------------------- separator ------------------------>8-- :noexport:
**_ TODO How Loadbalancer is implemented in code?
_** # --8<-------------------------- separator ------------------------>8-- :noexport:
**_ TODO Does Loadbalancer works only for public cloud?
\_\*\* TODO How I configure Ingress?
\*\* TODO [#A] NodePort VS clusterIP :IMPORTANT:
https://stackoverflow.com/questions/41509439/whats-the-difference-between-clusterip-nodeport-and-loadbalancer-service-types
http://weezer.su/kubernetes-1.html
https://docs.openshift.com/container-platform/3.3/dev_guide/getting_traffic_into_cluster.html

clusterIP: You can only access this service while inside the cluster.
** TODO [#A] k8s feature watch list \*** I want to check pod initContainer logs, but I don't want to specify initContainer by name
#+BEGIN_EXAMPLE
macs-MacBook-Pro:Scenario-401 mac$ kubectl logs my-jenkins-jenkins-89889ddb7-ct7jw -c 1
Error from server (BadRequest): container 1 is not valid for pod my-jenkins-jenkins-89889ddb7-ct7jw
macs-MacBook-Pro:Scenario-401 mac$ kubectl logs my-jenkins-jenkins-89889ddb7-ct7jw -c copy-default-config
Error from server (BadRequest): container "copy-default-config" in pod "my-jenkins-jenkins-89889ddb7-ct7jw" is waiting to start: PodInitializing
macs-MacBook-Pro:Scenario-401 mac$ kubectl logs my-jenkins-jenkins-89889ddb7-ct7jw -c copy-default-config
Error from server (BadRequest): container "copy-default-config" in pod "my-jenkins-jenkins-89889ddb7-ct7jw" is waiting to start: PodInitializing
#+END_EXAMPLE
**\* Support using environment variables inside deployment yaml file
https://github.com/kubernetes/kubernetes/issues/52787
** TODO pod error: CreateContainerConfigError
https://github.com/kubernetes/minikube/issues/2256
#+BEGIN_EXAMPLE
bash-3.2$ kubectl get pod my-wordpress-wordpress-df987548d-btvf5
NAME READY STATUS RESTARTS AGE
my-wordpress-wordpress-df987548d-btvf5 0/1 CreateContainerConfigError 0 2m
bash-3.2$
#+END_EXAMPLE

#+BEGIN_EXAMPLE
bash-3.2$ kubectl describe pod/my-wordpress-wordpress-df987548d-btvf5
Name: my-wordpress-wordpress-df987548d-btvf5
Namespace: default
Node: minikube/192.168.99.102
Start Time: Fri, 05 Jan 2018 16:41:27 -0600
Labels: app=my-wordpress-wordpress
pod-template-hash=895431048
Annotations: kubernetes.io/created-by={"kind":"SerializedReference","apiVersion":"v1","reference":{"kind":"ReplicaSet","namespace":"default","name":"my-wordpress-wordpress-df987548d","uid":"910e01e0-f269-11e7-b6d8...
Status: Pending
IP: 172.17.0.6
Created By: ReplicaSet/my-wordpress-wordpress-df987548d
Controlled By: ReplicaSet/my-wordpress-wordpress-df987548d
Containers:
my-wordpress-wordpress:
Container ID:
Image: bitnami/wordpress:4.9.1-r1
Image ID:
Ports: 80/TCP, 443/TCP
State: Waiting
Reason: CreateContainerConfigError
Ready: False
Restart Count: 0
Requests:
cpu: 300m
memory: 512Mi
Liveness: http-get http://:http/wp-login.php delay=120s timeout=5s period=10s #success=1 #failure=6
Readiness: http-get http://:http/wp-login.php delay=30s timeout=3s period=5s #success=1 #failure=3
Environment:
ALLOW_EMPTY_PASSWORD: yes
MARIADB_ROOT_PASSWORD: <set to the key 'mariadb-root-password' in secret 'my-wordpress-mariadb'> Optional: false
MARIADB_HOST: my-wordpress-mariadb
MARIADB_PORT_NUMBER: 3306
WORDPRESS_DATABASE_NAME: bitnami_wordpress
WORDPRESS_DATABASE_USER: bn_wordpress
WORDPRESS_DATABASE_PASSWORD: <set to the key 'mariadb-password' in secret 'my-wordpress-mariadb'> Optional: false
WORDPRESS_USERNAME: admin
WORDPRESS_PASSWORD: <set to the key 'wordpress-password' in secret 'my-wordpress-wordpress'> Optional: false
WORDPRESS_EMAIL: contact@dennyzhang.com
WORDPRESS_FIRST_NAME: FirstName
WORDPRESS_LAST_NAME: LastName
WORDPRESS_BLOG_NAME: My DevOps Blog!
SMTP_HOST:
SMTP_PORT:
SMTP_USER:
SMTP_PASSWORD: <set to the key 'smtp-password' in secret 'my-wordpress-wordpress'> Optional: false
SMTP_USERNAME:
SMTP_PROTOCOL:
Mounts:
/bitnami/apache from wordpress-data (rw)
/bitnami/php from wordpress-data (rw)
/bitnami/wordpress from wordpress-data (rw)
/var/run/secrets/kubernetes.io/serviceaccount from default-token-tc8kd (ro)
Conditions:
Type Status
Initialized True
Ready False
PodScheduled True
Volumes:
wordpress-data:
Type: PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
ClaimName: my-wordpress-wordpress
ReadOnly: false
default-token-tc8kd:
Type: Secret (a volume populated by a Secret)
SecretName: default-token-tc8kd
Optional: false
QoS Class: Burstable
Node-Selectors: <none>
Tolerations: <none>
Events:
Type Reason Age From Message

---

Normal Scheduled 1m default-scheduler Successfully assigned my-wordpress-wordpress-df987548d-btvf5 to minikube
Normal SuccessfulMountVolume 1m kubelet, minikube MountVolume.SetUp succeeded for volume "pvc-910644d3-f269-11e7-b6d8-08002782d6cd"
Normal SuccessfulMountVolume 1m kubelet, minikube MountVolume.SetUp succeeded for volume "default-token-tc8kd"
Normal Pulled 1s (x7 over 1m) kubelet, minikube Container image "bitnami/wordpress:4.9.1-r1" already present on machine
Warning Failed 1s (x7 over 1m) kubelet, minikube Error: lstat /tmp/hostpath-provisioner/pvc-910644d3-f269-11e7-b6d8-08002782d6cd: no such file or directory
Warning FailedSync 1s (x7 over 1m) kubelet, minikube Error syncing pod
bash-3.2$
#+END_EXAMPLE
\*\* TODO [#A] Certified Kubernetes Administrator (CKA) :IMPORTANT:
https://www.cncf.io/certification/expert/

https://github.com/cncf/curriculum/blob/master/certified_kubernetes_administrator_exam_v1.8.0.pdf

It is an online, proctored, performance-based test that requires solving multiple issues from a command line.

Candidates have 3 hours to complete the tasks.
** HALF Difference in between selectors and labels
** TODO [#A] kubernetes mount a file to pod :IMPORTANT:
https://stackoverflow.com/questions/33415913/whats-the-best-way-to-share-mount-one-file-into-a-pod
https://www.linkedin.com/feed/update/urn:li:activity:6355445509146107904/
\*\* TODO K8S label & Selector
https://github.com/dennyzhang/dennytest/tree/master/cheatsheet-kubernetes-A4][challenges-leetcode-interesting]]

- [#A] k8s metric server :noexport:IMPORTANT:
  Metrics Server is a cluster-wide aggregator of resource usage data.

Metrics Server registered in the main API server through Kubernetes aggregator.

https://github.com/kubernetes-incubator/metrics-server
https://github.com/kubernetes-incubator/metrics-server/tree/master/deploy/1.8%2B

https://kubernetes.io/docs/tasks/debug-application-cluster/core-metrics-pipeline/
| Name | Summary |
|----------------+-------------------------------------------------------------------|
| Core metrics | node/container level metrics; CPU, memory, disk and network, etc. |
| Custom metrics | refers to application metrics, e.g. HTTP request rate. |

Today (Kubernetes 1.7), there are several sources of metrics within a Kubernetes cluster
| Name | Summary |
|----------------+---------------------------------------------------------------------|
| Heapster | k8s add-on |
| Cadvisor | a standalone container/node metrics collection and monitoring tool. |
| Kubernetes API | does not track metrics. But can get real time metrics |
\*\* metric server
Resource Metrics API is an effort to provide a first-class Kubernetes API (stable, versioned, discoverable, available through apiserver and with client support) that serves resource usage metrics for pods and nodes.

- metric server is sort of a stripped-down version of Heapster
- The metrics-server will collect "Core" metrics from cAdvisor APIs (currently embedded in the kubelet) and store them in memory as opposed to in etcd.
- The metrics-server will provide a supported API for feeding schedulers and horizontal pod auto-scalers
- All other Kubernetes components will supply their own metrics in a Prometheus format
  ** Cadvisor
  Cadvisor monitors node and container core metrics in addition to container events.
  It natively provides a Prometheus metrics endpoint
  The Kubernetes kublet has an embedded Cadvisor that only exposes the metrics, not the events.
  ** heapster
  Heapster is an add on to Kubernetes that collects and forwards both node, namespace, pod and container level metrics to one or more "sinks" (e.g. InfluxDB).

It also provides REST endpoints to gather those metrics. The metrics are constrained to CPU, filesystem, memory, network and uptime.

Heapster queries the kubelet for its data.

Today, heapster is the source of the time-series data for the Kubernetes Dashboard.
** # --8<-------------------------- separator ------------------------>8-- :noexport:
** TODO How to query metric server
\*\* TODO Key scenarios of metric server
The metrics-server will provide a much needed official API for the internal components of Kubernetes to make decisions about the utilization and performance of the cluster.

- HPA(Horizontal Pod Autoscaler) need input to do good auto-scaling
  ** TODO There are plans for an "Infrastore", a Kubernetes component that keeps historical data and events
  ** # --8<-------------------------- separator ------------------------>8-- :noexport:
  ** TODO why from heapster to k8s metric server?
  ** TODO kube-aggregator
  \*\* TODO what is prometheus format?
  #+BEGIN_EXAMPLE
  Denny Zhang [12:34 AM]
  An easy introduction about k8s metric server. (It will replace heapster)

https://blog.freshtracks.io/what-is-the-the-new-kubernetes-metrics-server-849c16aa01f4

> All other Kubernetes components will supply their own metrics in a Prometheus format

In logging domain, we can say `syslog` is the standard format

In metric domain, maybe we can choose `prometheus` as the standard format.
#+END_EXAMPLE
\*\* Metrics Use Cases
https://github.com/kubernetes/community/blob/master/contributors/design-proposals/instrumentation/resource-metrics-api.md

https://docs.giantswarm.io/guides/kubernetes-heapster/

#+BEGIN_EXAMPLE
Horizontal Pod Autoscaler: It scales pods automatically based on CPU or custom metrics (not explained here). More information here.
Kubectl top: The command top of our beloved Kubernetes CLI display metrics directly in the terminal.
Kubernetes dashboard: See Pod and Nodes metrics integrated into the main Kubernetes UI dashboard. More info here
Scheduler: In the future Core Metrics will be considered in order to schedule best-effort Pods.
#+END_EXAMPLE
\*\* useful link
https://blog.freshtracks.io/what-is-the-the-new-kubernetes-metrics-server-849c16aa01f4
https://blog.outlyer.com/monitoring-kubernetes-with-heapster-and-prometheus
https://www.outcoldman.com/en/archive/2017/07/09/kubernetes-monitoring-resources/

- k8s loadbalancer :noexport:
  \*\* DONE k8s service: loadbalancer
  CLOSED: [2018-06-19 Tue 13:51]
  #+BEGIN_EXAMPLE
  cat > service.yml <<EOF
  apiVersion: v1
  kind: Service
  metadata:
  name: lb
  namespace: logging
  spec:
  selector:
  app: kibana
  ports:
  - protocol: TCP
    port: 5601
    type: LoadBalancer
    EOF
    #+END_EXAMPLE
- k8s DaemonSet :noexport:
  \*\* DONE k8s daemonsets: ensures that all (or some) Nodes run a copy of a Pod.
  CLOSED: [2018-06-19 Tue 13:28]
  https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/

As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.

Some typical uses of a DaemonSet are:

- running a cluster storage daemon, such as glusterd, ceph, on each node.
- running a logs collection daemon on every node, such as fluentd or logstash.
  - running a node monitoring daemon on every node, such as Prometheus Node Exporter, collectd, Datadog agent, New Relic agent, or Ganglia gmond.

* [#A] etcd :noexport:
  https://coreos.com/etcd/docs/latest/dev-guide/interacting_v3.html
  https://coreos.com/etcd/docs/latest/v2/README.html
* [#B] k8s addons :noexport:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/
  \*\* DONE k8s install add-on: dashboard
  CLOSED: [2018-01-03 Wed 12:19]

- Install, then use kubectl-proxy to start
- Create user and binding, then use token to login

#+BEGIN_EXAMPLE
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
nohup kubectl proxy --port=8001 --address=0.0.0.0 &

curl http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/

#+END_EXAMPLE

#+BEGIN_EXAMPLE

# https://github.com/kubernetes/dashboard/wiki/Creating-sample-user

cat > user.yaml <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
name: admin-user
namespace: kube-system

---

apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
name: admin-user
roleRef:
apiGroup: rbac.authorization.k8s.io
kind: ClusterRole
name: cluster-admin
subjects:

- kind: ServiceAccount
  name: admin-user
  namespace: kube-system
  EOF
  #+END_EXAMPLE

kubectl apply -f user.yaml
kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')

https://github.com/kubernetes/dashboard#kubernetes-dashboard
https://blog.frognew.com/2017/09/kubeadm-install-kubernetes-1.8.html#8dashboard%E6%8F%92%E4%BB%B6%E9%83%A8%E7%BD%B2
\*\*\* DONE kubectl proxy listen on all network nics
CLOSED: [2018-01-03 Wed 12:12]
https://github.com/kubernetes/kubectl/issues/142
kubectl proxy --port=8001 --address=0.0.0.0

- [#A] k8s volumes :noexport:
  CLOSED: [2017-12-01 Fri 22:45]
  https://kubernetes.io/docs/concepts/storage/volumes
  https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/
  https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes

https://blog.couchbase.com/stateful-containers-kubernetes-amazon-ebs/
https://stackoverflow.com/questions/37555281/create-kubernetes-pod-with-volume-using-kubectl-run
https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/

▪Directory accessible to the containers in a pod
▪Volume outlives any containers in a pod
▪Common types
hostPath
nfs
awsElasticBlockStore
gcePersistentDisk

#+BEGIN_EXAMPLE
Creating and using a persistent volume is a three step process:

1. Provision: Administrator provision a networked storage in the cluster, such as AWS ElasticBlockStore volumes. This is called as PersistentVolume.
2. Request storage: User requests storage for pods by using claims. Claims can specify levels of resources (CPU and memory), specific sizes and access modes (e.g. can be mounted once read/write or many times write only).
   This is called as PersistentVolumeClaim.
3. Use claim: Claims are mounted as volumes and used in pods for storage.
   #+END_EXAMPLE
   \*\* DONE persistence.accessMode ReadWriteOnce or ReadOnly: https://github.com/kubernetes/charts/tree/master/cheatsheet-kubernetes-A4][challenges-leetcode-interesting]]
   CLOSED: [2018-01-02 Tue 16:52]
   The access modes are:

ReadWriteOnce - the volume can be mounted as read-write by a single node
ReadOnlyMany - the volume can be mounted read-only by many nodes
ReadWriteMany - the volume can be mounted as read-write by many nodes

- [#B] k8s security: secrets, authentication & authorization :noexport:
  ** what's service account: In contrast, service accounts are users managed by the Kubernetes API.
  https://kubernetes.io/docs/admin/authentication/
  https://github.com/kubernetes/kubernetes/blob/master/examples/elasticsearch/service-account.yaml
  https://kubernetes.io/docs/admin/authorization/
  ** serviceaccount, clusterrolebinding
  https://blog.frognew.com/2017/12/its-time-to-use-helm.html
  #+BEGIN_EXAMPLE
  apiVersion: v1
  kind: ServiceAccount
  metadata:
  name: tiller
  namespace: kube-system

---

apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
name: tiller
roleRef:
apiGroup: rbac.authorization.k8s.io
kind: ClusterRole
name: cluster-admin
subjects:

- kind: ServiceAccount
  name: tiller
  namespace: kube-system
  #+END_EXAMPLE
  \*\* k8s secrets: intended to hold sensitive information, such as passwords, OAuth tokens, and ssh keys.
  https://github.com/arun-gupta/vault-kubernetes/blob/master/secrets.yaml
  http://kubernetesbyexample.com/secrets/

- Secrets are namespaced objects, that is, exist in the context of a namespace
- You can access them via a volume or an environment variable from a container running in a pod
- The secret data on nodes is stored in tmpfs volumes

kubectl create secret generic mysecret --from-literal=mysql_root_password=my-secret-pw
kubectl get secret mysecret

#+BEGIN_EXAMPLE
apiVersion: v1
kind: Pod
metadata:
name: secret-env-pod
spec:
containers:

- name: mycontainer
  image: redis
  env: - name: SECRET_USERNAME
  valueFrom:
  secretKeyRef:
  name: mysecret
  key: username - name: SECRET_PASSWORD
  valueFrom:
  secretKeyRef:
  name: mysecret
  key: password
  restartPolicy: Never
  #+END_EXAMPLE

* HPA: Horizontal Pod Autoscaler :noexport:
* Uncertainty & Uncomfortable things with K8S :noexport:
  ** Destroy namespace takes more than 15 minutes, with nowhere to check
  Testing in minikube
  ** Pod stucks in containercreating for a long time
* HALF kubectl apply to a list of folder: kubectl apply -R -f namespace-drain-manifests/manifests :noexport:
* GKE user access :noexport:
  #+BEGIN_EXAMPLE
  If y'all run into the following error: `is forbidden: attempt to grant extra privileges:` when trying to run `kubectl apply -R -f ~/workspace/namespace-drain/manifests/` against a GKE cluster, then run the following command.

`kubectl create clusterrolebinding cluster-admin-binding --clusterrole cluster-admin --user $(gcloud config get-value account)`
#+END_EXAMPLE

- Blog: How Enterprise Do XXX in Container world? :noexport:
- TODO [#A] Blog: interview candidates for k8s experience :noexport:
  ** Explain concepts \*** What's k8s context. Why we need it?
  **_ What's initContainer? Why we need it?
  _** Network policy
  ** Comparision \*** configmap vs secrets
  \*\*\* labels vs annotations
  What are k8s Annotations? What differences it is compared with labels:

* Like labels, annotations are key/value pairs. Where labels have length limits, annotations can be quite large.
* you can't query or select objects based on annotations.
* Are used for non-identifying information. Stuff not used internally by k8s.

https://codeengineered.com/blog/2017/kubernetes-labels-annotations/
https://vsupalov.com/kubernetes-labels-annotations-difference/ (edited)
**_ clusterip, service, loadbalancer
_** ClusterRole vs Role
**\* serviceaccount vs useraccount
** Scenarios/Experience
**_ tell me about k8s security model
_** tell me about k8s scheduling model
**_ tell me about k8s HA model
_** tell me about k8s trouble shooting experience
** Your Wish List \*** layer of yaml
**_ ABBA on volumes
_** apply one configmap to all namespace

- k8s workflow: every 3 months has one new release :noexport:
  https://github.com/kubernetes/kubeadm/blob/master/docs/release-cycle.md
- Blog: Kubernetes Limitation List :noexport:

* Starting with Kubernetes 1.6 we support 5000 nodes clusters with 30 pods per node. ([[https://github.com/kubernetes/community/blob/master/contributors/design-proposals/instrumentation/metrics-server.md#scalability-limitations][link]])

- # --8<-------------------------- separator ------------------------>8-- :noexport:
- DONE Why we need Static Pods :noexport:
  CLOSED: [2019-01-04 Fri 15:04]
  https://kubernetes.io/docs/tasks/administer-cluster/static-pod/
  Denny Zhang [2:26 PM]
  Fan, ever heard of `Static Pods` in k8s?

If yes, could you give me two use scenarios why I would use it.

Fan Zhang [3:00 PM]
我听说过
其实就是 kubelet 直接管理的 pod

Denny Zhang [3:01 PM]
是的,文档是这么说的.

Fan Zhang [3:01 PM]
我觉得这个是 DeamonSet 的补充

Denny Zhang [3:01 PM]
我在尝试理解这个背后的应用场景

Fan Zhang [3:02 PM]
因为有时候在 node 上需要有一些 particular 的 service,但又不希望被 kubernetes 的 schecular 管理

Denny Zhang [3:02 PM]
将 OS 的进程容器化
但这些只是 OS 级别,而不是 k8s 系统或 app 应用级别的进程
可以这样理解吗？

Fan Zhang [3:03 PM]
否则 drain 之后 就没有了
可以这样理解

Denny Zhang [3:04 PM]
所以 drain node 不会把 static pod 删掉？

- TODO Why need kubernetes/apiserver: https://github.com/kubernetes/apiserver :noexport:
  Library for writing a Kubernetes-style API server.

https://github.com/kubernetes/kube-aggregator

- TODO [#A] Questions :noexport:
  \*\* pod type
  https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/#my-service-is-missing-endpoints
  #+BEGIN_EXAMPLE
  ...
  spec:
  - selector:
    name: nginx
    type: frontend
    #+END_EXAMPLE

kubectl get pods --selector=name=nginx,type=frontend
\*\* Containers inside a Pod can communicate with one another using localhost.
https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/

Networking
Each Pod is assigned a unique IP address. Every container in a Pod shares the network namespace, including the IP address and network ports. Containers inside a Pod can communicate with one another using localhost. When containers in a Pod communicate with entities outside the Pod, they must coordinate how they use the shared network resources (such as ports).
\*\* How to restart a container inside a Pod?
https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/

Restarting a container in a Pod should not be confused with restarting the Pod. The Pod itself does not run, but is an environment the containers run in and persists until it is deleted.
** explain k8s components: apiserver, scheduler, controller-manager, kube-proxy
** get logs of failed container
https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/#my-pod-is-crashing-or-otherwise-unhealthy
#+BEGIN_EXAMPLE
If your container has previously crashed, you can access the previous container's crash log with:

$ kubectl logs --previous ${POD_NAME} ${CONTAINER_NAME}
#+END_EXAMPLE
\*\* Why k8s dashboard get deprecated?
https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

- TODO k8s architecture :noexport:
  https://www.youtube.com/watch?v=_WfJz5VS_cU&list=PLj6h78yzYM2NGwRwkBPxigKio2r0XHPl9
- TODO k8s scenario problems :noexport:
  ** TODO export k8s dashboard: kube proxy VS ingress
  ** TODO how to back and restore etcd
  https://kubernetes-incubator.github.io/kube-aws/advanced-topics/etcd-backup-and-restore.html
- TODO Apply yamls file recursively :noexport:
  #+BEGIN_SRC sh

# create

time ls -1 _/_.yml | grep -v namespace | xargs -I{} kubectl apply -f {}

# delete

time ls -1r _/_.yml | grep -v namespace | xargs -I{} kubectl delete -f {}
#+END_SRC

- TODO devstats: https://k8s.devstats.cncf.io/d/12/dashboards?refresh=15m&orgId=1 :noexport:
- TODO create a ingress service for clusterip service :noexport:
- TODO kubectl -vvv :noexport:
- TODO kubectl get application --all-namespaces :noexport:
- TODO kubectl delete namespace in GKE is extremely slow :noexport:
- TODO try more with ReplicaSet :noexport:
- TODO try PodDisruptionBudget: https://hackernoon.com/top-10-kubernetes-tips-and-tricks-27528c2d0222 :noexport:
- TODO [#A] k8s services :noexport:
  https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0
- [#A] ClusterIP :noexport:
  ** TODO kubernetes clusterip
  ** TODO Is k8s ClusterIP SPOF?
  https://mp.weixin.qq.com/s?__biz=MzIzNjUxMzk2NQ==&mid=2247486025&idx=1&sn=1f95917918a3217bb92b97113c81b6c8&chksm=e8d7f58bdfa07c9dedbfbe4f39687ea5d467ec371ecb2dea5dd13101a46d3bb754d6738e481f&scene=27#wechat_redirect
  \*\* TODO Use ExternalName to avoid ClusterIP SPOF
- TODO k8s cpu 88m? :noexport:
  #+BEGIN_EXAMPLE
  Limits:
  cpu: 48m
  memory: 104Mi
  Requests:
  cpu: 48m
  memory: 104Mi

#+END_EXAMPLE

- TODO autoscaling pod: try auto scaling :noexport:
- TODO k8s volume: readwriteonce, readwritemany? :noexport:
- # --8<-------------------------- separator ------------------------>8-- :noexport:
- TODO grant more privileges to a given serviceaccount :noexport:
  kubectl get serviceaccount --all-namespaces

prometheus-1-prometheusserviceaccount-e1fd

system:kubelet-api-admin

- TODO Question: PodDisruptionBudget: https://docs.pivotal.io/runtimes/pks/1-2/troubleshoot-issues.html#upgrade-drain-hangs :noexport:
  If Kubernetes is unable to unschedule a pod, then the drain hangs indefinitely.

One reason why Kubernetes may be unable to unschedule the node is if
the PodDisruptionBudget object has been configured in a way that
allows 0 disruptions and only a single instance of the pod has been
scheduled.

- TODO k8s events :noexport:
  https://solinea.com/blog/tapping-kubernetes-events
- TODO kubectl from worker vm, I don't seem to need a kubeconfig :noexport:
- TODO kubectl apply -f - :noexport:
- TODO How does "kubectl delete - f -" works? :noexport:
- TODO devstats: https://k8s.devstats.cncf.io/d/12/dashboards?refresh=15m&orgId=1 :noexport:
- TODO Is it possible to assign a DNS address to Kubernetes service :noexport:
- TODO k8s template templateinstance :noexport:
- TODO [#A] k8s yaml create a loadbalancer :noexport:
- TODO github improvememnt: update k8s cheatsheet: https://blog.billyc.io/notes/kubectl-notes/ :noexport:
  https://kubernetes.io/docs/reference/kubectl/cheatsheet/
- [#A] Google Kubernetes :noexport:IMPORTANT:
  No.2 Kubernetes

Kubernetes 是一个编排（orchestration）工具,类似运行于 Apache Mesos 之上的 Marathon,但是它是专门为 Docker 容器而创建的.

Kubernetes is an open-source platform for automating deployment, scaling, and operations of application containers across clusters of hosts, providing container-centric infrastructure

Kubernetes 来自 Google,除了能在他们自己的 Google Container Engine 上工作之外,还支持 VMware vSphere, Mesos, or Mesosphere DCOS,以及很多公有云,包括 Amazon Web Services 等.

Kubernetes 具备完善的集群管理能力,包括多层次的安全防护和准入机制`多租户应用支撑能力`透明的服务注册和服务发现机制`内建负载均衡器`故障发现和自我修复能力`服务滚动升级和在线扩容`可扩展的资源自动调度机制`多粒度的资源配额管理能力.

Kubernetes 还提供完善的管理工具,涵盖开发`部署测试`运维监控等各个环节.

每个 API 对象都有 3 大类属性:元数据 metadata`规范 spec 和状态 status

- Concepts: Pod, Service, Labels 和单 Pod 单 IP
  \*\* Installing and Setting Up kubectl
  https://kubernetes.io/docs/tasks/tools/install-kubectl/

curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
\*\* kubectl --help
kubectl controls the Kubernetes cluster manager.

Find more information at https://github.com/kubernetes/kubernetes.

Basic Commands (Beginner):
create Create a resource by filename or stdin
expose Take a replication controller, service, deployment or pod and expose it as a new Kubernetes Service
run Run a particular image on the cluster
set Set specific features on objects

Basic Commands (Intermediate):
get Display one or many resources
explain Documentation of resources
edit Edit a resource on the server
delete Delete resources by filenames, stdin, resources and names, or by resources and label selector

Deploy Commands:
rollout Manage a deployment rollout
rolling-update Perform a rolling update of the given ReplicationController
scale Set a new size for a Deployment, ReplicaSet, Replication Controller, or Job
autoscale Auto-scale a Deployment, ReplicaSet, or ReplicationController

Cluster Management Commands:
certificate Modify certificate resources.
cluster-info Display cluster info
top Display Resource (CPU/Memory/Storage) usage.
cordon Mark node as unschedulable
uncordon Mark node as schedulable
drain Drain node in preparation for maintenance
taint Update the taints on one or more nodes

Troubleshooting and Debugging Commands:
describe Show details of a specific resource or group of resources
logs Print the logs for a container in a pod
attach Attach to a running container
exec Execute a command in a container
port-forward Forward one or more local ports to a pod
proxy Run a proxy to the Kubernetes API server
cp Copy files and directories to and from containers.
auth Inspect authorization
Advanced Commands:
apply Apply a configuration to a resource by filename or stdin
patch Update field(s) of a resource using strategic merge patch
replace Replace a resource by filename or stdin
convert Convert config files between different API versions

Settings Commands:
label Update the labels on a resource
annotate Update the annotations on a resource
completion Output shell completion code for the specified shell (bash or zsh)

Other Commands:
api-versions Print the supported API versions on the server, in the form of "group/version"
config Modify kubeconfig files
help Help about any command
version Print the client and server version information

Use "kubectl <command> --help" for more information about a given command.
Use "kubectl options" for a list of global command-line options (applies to all commands).
** kubernetes: The connection to the server localhost:8080 was refused - did you specify the right host or port?
https://github.com/kubernetes/kubernetes/issues/23092
** Layers

- Nucleus: API And Execution
- Application layer: deployment and running
- Governance layer: automation and policy enforcement
- Interface layer: client libraries and tools
- Ecosystem
  ** healthcheck: LivenessProbe, ReadinessProbe
  ** 核心组件
  Kubernetes 主要由以下几个核心组件组成:
- etcd 保存了整个集群的状态;
- apiserver 提供了资源操作的唯一入口,并提供认证`授权`访问控制`API 注册和发现等机制;
- controller manager 负责维护集群的状态,比如故障检测`自动扩展`滚动更新等;
- scheduler 负责资源的调度,按照预定的调度策略将 Pod 调度到相应的机器上;
- kubelet 负责维护容器的生命周期,同时也负责 Volume（CVI）和网络（CNI）的管理;
- Container runtime 负责镜像管理以及 Pod 和容器的真正运行（CRI）;
- kube-proxy 负责为 Service 提供 cluster 内部的服务发现和负载均衡
  ** helloworld
  https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/
  ** useful link
  https://kubernetes.io
  https://www.reddit.com/r/devops/comments/51ra9q/moving_from_docker_to_rkt/
  http://blog.dataman-inc.com/67/
  http://jpadilla.com/post/161144157937/update-kubernetes-deployment-after-pushing-image
  https://spacelift.io/blog/kubernetes-cheat-sheet

http://www.oschina.net/news/70140/infoworlds-2016-technology-of-the-year-award-winners?p=3#comments
** DONE Principle: API 的操作复杂度不能超过 O(N)
CLOSED: [2017-06-10 Sat 15:24]
https://kubernetes.feisky.xyz/architecture/concepts.html
API 操作复杂度与对象数量成正比.这一条主要是从系统性能角度考虑,要保证整个系统随着系统规模的扩大,性能不会迅速变慢到无法使用,那么最低的限定就是 API 的操作复杂度不能超过 O(N),N 是对象的数量,否则系统就不具备水平伸缩性了.
** Principle: API 对象状态不能依赖于网络连接状态
https://kubernetes.feisky.xyz/architecture/concepts.html
** # --8<-------------------------- separator ------------------------>8--
** TODO [#A] fail to start minikube: "VBoxManage not found. Make sure VirtualBox is installed and VBoxManage is in the path".
root@totvsjenkins:/tmp# minikube start
Starting local Kubernetes v1.6.4 cluster...
Starting VM...
E0610 20:14:57.518198 27907 start.go:127] Error starting host: Error creating host: Error with pre-create check: "VBoxManage not found. Make sure VirtualBox is installed and VBoxManage is in the path".

Retrying.
E0610 20:14:57.519201 27907 start.go:133] Error starting host: Error creating host: Error with pre-create check: "VBoxManage not found. Make sure VirtualBox is installed and VBoxManage is in the path"
** TODO how kubernetes use etcd
** TODO how healthcheck is implemented
** TODO What about alerting and reporting
** TODO what's fluentd
** # --8<-------------------------- separator ------------------------>8--
** TODO [#A] k8s support rolling deployment :IMPORTANT:
https://www.youtube.com/watch?v=7TOWLerX0Ps
Kubernetes: zero downtime update at 1 million requests per second
https://www.youtube.com/watch?v=9C6YeyyUUmI
Kubernetes: zero downtime update at 10 million QPS
** TODO [#A] How to scale Pods with volumes configured :IMPORTANT:
** What is Kubernetes
https://www.youtube.com/watch?v=R-3dfURb2hA
What is Kubernetes

Deployment, Scaling, Monitoring
\*\* DONE Kubernetes hellworld
CLOSED: [2017-07-11 Tue 08:42]
https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/#create-a-minikube-cluster

# build image

docker build -t hello-node:v1 .

# create deployment

kubectl run hello-node --image=hello-node:v1 --port=8080

# View the Deployment

kubectl get deployments

# Create service

kubectl expose deployment hello-node --type=LoadBalancer
** TODO [#A] Install minikube in headless Ubuntu server :IMPORTANT:
| Name | Summary |
|-----------------+---------|
| minikube status | |
** DONE [#A] Ubuntu install kubernetes for all-in-one POC: minikube
CLOSED: [2017-07-11 Tue 08:43]
https://blog.jetstack.io/blog/k8s-getting-started-part2/
https://github.com/kubernetes/minikube
https://stackoverflow.com/questions/38528762/kubernetes-on-ubuntu-16-04
https://hxquangnhat.com/2016/12/21/tutorial-deploy-a-kubernetes-cluster-on-ubuntu-16-04/
\*\*\* TODO minikube fail to start
#+BEGIN_EXAMPLE
root@totvsjenkins:/home/denny/minikube# ./minikube start --vm-driver=none --use-vendored-driver
Starting local Kubernetes v1.6.4 cluster...
Starting VM...
Moving files into cluster...

Setting up certs...
Starting cluster components...
Connecting to cluster...
Setting up kubeconfig...
Kubectl is now configured to use the cluster.
===================
WARNING: IT IS RECOMMENDED NOT TO RUN THE NONE DRIVER ON PERSONAL WORKSTATIONS
The 'none' driver will run an insecure kubernetes apiserver as root that may leave the host vulnerable to CSRF attacks
#+END_EXAMPLE
\*\*\* useful link
https://www.youtube.com/watch?v=PH-2FfFD2PU
Kubernetes in 5 mins
https://www.youtube.com/watch?v=DC7NECq3Ghs
Setting up and using a single node Kubernetes cluster.
https://www.youtube.com/watch?v=BDrcUjOczsE
Kubernetes - Local Testing

https://www.youtube.com/watch?v=R-3dfURb2hA
The Illustrated Children's Guide to Kubernetes

- TODO [#A] Run a task on every node in a cluster :noexport:
- TODO kubectl get all won't get psp :noexport:
  #+BEGIN_EXAMPLE
  root@009069ee-95d5-49a2-6b82-67aff8eb6737:/tmp/build/4ecf0f02# kubectl get all --all-namespaces
  NAMESPACE NAME READY STATUS RESTARTS AGE
  kube-system pod/heapster-6d5f964dbd-2xxcm 1/1 Running 0 1d
  kube-system pod/kube-dns-6b697fcdbd-c4rmm 3/3 Running 0 1d
  kube-system pod/kubernetes-dashboard-785584f46b-9wmqj 1/1 Running 0 1d
  kube-system pod/metrics-server-6bbb689cf9-swtxc 1/1 Running 0 1d
  kube-system pod/monitoring-influxdb-76fd8dcff6-qws9m 1/1 Running 0 1d
  kube-system pod/wavefront-proxy-8498d5bbf4-gl6sw 4/4 Running 0 4m
  test-afjogacpjsqfetejycxx pod/busybox-io-ftpz8 1/1 Running 0 1d

NAMESPACE NAME DESIRED CURRENT READY AGE
test-afjogacpjsqfetejycxx replicationcontroller/busybox-io 1 1 1 1d

NAMESPACE NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE
default service/kubernetes ClusterIP 10.100.200.1 <none> 443/TCP 1d
kube-system service/heapster ClusterIP 10.100.200.123 <none> 8443/TCP 1d
kube-system service/kube-dns ClusterIP 10.100.200.10 <none> 53/UDP,53/TCP 1d
kube-system service/kubernetes-dashboard NodePort 10.100.200.8 <none> 443:32433/TCP 1d
kube-system service/metrics-server ClusterIP 10.100.200.102 <none> 443/TCP 1d
kube-system service/monitoring-influxdb ClusterIP 10.100.200.89 <none> 8086/TCP 1d

NAMESPACE NAME DESIRED CURRENT UP-TO-DATE AVAILABLE AGE
kube-system deployment.apps/heapster 1 1 1 1 1d
kube-system deployment.apps/kube-dns 1 1 1 1 1d
kube-system deployment.apps/kubernetes-dashboard 1 1 1 1 1d
kube-system deployment.apps/metrics-server 1 1 1 1 1d
kube-system deployment.apps/monitoring-influxdb 1 1 1 1 1d
kube-system deployment.apps/wavefront-proxy 1 1 1 1 4m

NAMESPACE NAME DESIRED CURRENT READY AGE
kube-system replicaset.apps/heapster-6d5f964dbd 1 1 1 1d
kube-system replicaset.apps/kube-dns-6b697fcdbd 1 1 1 1d
kube-system replicaset.apps/kubernetes-dashboard-785584f46b 1 1 1 1d
kube-system replicaset.apps/metrics-server-6bbb689cf9 1 1 1 1d
kube-system replicaset.apps/monitoring-influxdb-76fd8dcff6 1 1 1 1d
kube-system replicaset.apps/wavefront-proxy-8498d5bbf4 1 1 1 4m
root@009069ee-95d5-49a2-6b82-67aff8eb6737:/tmp/build/4ecf0f02# kubectl get psp
NAME PRIV CAPS SELINUX RUNASUSER FSGROUP SUPGROUP READONLYROOTFS VOLUMES
kube-system-psp false \* RunAsAny RunAsAny RunAsAny RunAsAny false configMap,emptyDir,projected,secret,downwardAPI
root@009069ee-95d5-49a2-6b82-67aff8eb6737:/tmp/build/4ecf0f02# kubectl get all --all-namespaces | grep kube-system-psp
#+END_EXAMPLE

- TODO where is k8s job log? :noexport:
  http://kubernetesbyexample.com/jobs/
- TODO kubectl logs --previous nginx-app-zibvs :noexport:
  https://jimmysong.io/cheatsheets/kubernetes-kubectl
- TODO [#A] play with k8s ingress service :noexport:
- TODO Vanilla CNCF Certified Kubernetes :noexport:
- TODO [#A] try admission controller :noexport:
- HALF Accessing Kubernetes API from pods :noexport:
  curl -k -v --cacert /var/run/secrets/kubernetes.io/serviceaccount/ca.crt -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)" https://<mycluster>
- TODO k8s training course from linux foundation: https://training.linuxfoundation.org/training/introduction-to-kubernetes/ :noexport:
- # --8<-------------------------- separator ------------------------>8-- :noexport:
- TODO consolidate: https://codefresh.io/kubernetes-tutorial/page/4/ :noexport:
- TODO consolidate: https://info.shadow-soft.com/hubfs/Kubernetes-Cheatsheet-Mesosphere.pdf :noexport:
- TODO consolidate: https://kapeli.com/cheat_sheets/Kubernetes.docset/Contents/Resources/Documents/index :noexport:
- TODO consolidate: https://lzone.de/cheat-sheet/kubernetes :noexport:
- TODO consolidate: http://www.productiondown.com/devops/2018/08/02/Kubernetes-Commands-Cheatsheet.html :noexport:
- TODO consolidate cheatsheet: https://github.com/LeCoupa/awesome-cheatsheets/blob/master/tools/kubernetes.sh :noexport:
- TODO consolidate: http://kubernetesbyexample.com/ :noexport:
- TODO consolidate https://jimmysong.io/cheatsheets/kubernetes-tricks :noexport:
- # --8<-------------------------- separator ------------------------>8-- :noexport:
- HALF use kubectl to pull docker images, instead of ssh to vm :noexport:
- HALF use kubectl to cleanup docker images, instead of ssh to vm :noexport:
  https://github.com/onfido/k8s-cleanup/blob/master/docker-clean.yml
- # --8<-------------------------- separator ------------------------>8-- :noexport:
- TODO pv termination hangs there forever :noexport:
  #+BEGIN_EXAMPLE
   /Users/zdenny/git_code/codecommit/devops_blog/k8s  kubectl get pv   master ✘ ✹  ✔ 0
  NAME CAPACITY ACCESS MODES RECLAIM POLICY STATUS CLAIM STORAGECLASS REASON AGE
  db-pv-volume 400Gi RWO Retain Available 12h
  pvc-bbddb940-5f43-11e9-ba3c-42010a800085 1Gi RWO Delete Bound denny-websites/cdn-pv-claim standard 12h
  website-pv-volume 10Gi RWO Retain Terminating denny-websites/mysql-pv-claim standard 12h
  #+END_EXAMPLE
- TODO k8s configmap can't be changed :noexport:
  #+BEGIN_EXAMPLE
   /Users/zdenny/git_code/codecommit/devops_blog/k8s  kubectl logs -n denny-websites pod/nginx-b88c67f77-dkw64   master ✘ ✖ ✹ ✭  ✔ 0
  Update /etc/nginx/conf.d/default.conf

* echo 'Update /etc/nginx/conf.d/default.conf'
* sed -i s/http_port_here/80/g /etc/nginx/conf.d/default.conf
  sed: cannot rename /etc/nginx/conf.d/sedz2uuPB: Device or resource busy
  #+END_EXAMPLE

- TODO [#A] k8s mount configmap file, then edit it when process boostrap :noexport:
- TODO gce disk: how and when the filesystem formating happens? :noexport:
- # --8<-------------------------- separator ------------------------>8-- :noexport:
- TODO k8s pod share volume within containers :noexport:
- TODO gce use one disk in a small chunks :noexport:
- TODO k8s mount jenkins home volume, then dockerfile copy/jenkins groovy. How to align? :noexport:
  COPY resources/jobs/ /usr/share/jenkins/ref/jobs/
- # --8<-------------------------- separator ------------------------>8-- :noexport:
- TODO k8s: when jenkins pod gets recreated, jenkins secret parameters need to be reconfigured :noexport:
- TODO k8s: instruct application to run a clean shutdown or a safe restart :noexport:
  https://support.cloudbees.com/hc/en-us/articles/115003926511-Best-Practices-for-Jenkins-Updates-Patches-and-Maintenance
- # --8<-------------------------- separator ------------------------>8-- :noexport:
- HALF doc: configmap cannot be mounted as a file :noexport:
  https://stackoverflow.com/questions/44325048/kubernetes-configmap-only-one-file

ConfigMaps must be mounted as directories

https://github.com/kubernetes/kubernetes/issues/45000
https://stackoverflow.com/questions/44325048/kubernetes-configmap-only-one-file

- HALF doc: mount configmap as a seperate file :noexport:
- TODO How to pass credentials to yaml in a secured way? :noexport:
- TODO kubectl cluster-info only get recent information :noexport:
- DONE k8s pod dns :noexport:
  CLOSED: [2019-05-25 Sat 08:21]
  https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/

\_my-port-name.\_my-port-protocol.my-svc.my-namespace.svc.cluster.local

curl -I http://jenkins-lb.my-testbed.svc.cluster.local

- DONE why one pod has two docker images :noexport:
  CLOSED: [2019-08-01 Thu 14:31]
  One pod with two containers
  #+BEGIN_EXAMPLE
  root@422e158feb46fff15217b24e4f8ad20b [ ~ ]# kubectl get pods -o='custom-columns=PODS:.metadata.name,Images:.spec.containers[*].image' --all-namespaces | grep sche
  kube-scheduler-422e158feb46fff15217b24e4f8ad20b my/kube-scheduler:v1.13.1,my/wcp-schedext:0.0.1.26323453
  #+END_EXAMPLE
- DONE kubectl get port nodeport :noexport:
  CLOSED: [2020-04-16 Thu 10:57]
  kubectl get service/wordpress -n blog -o json | jq '.spec.ports[].nodePort'

- # --8<-------------------------- separator ------------------------>8-- :noexport:
- TODO [#B] Create PVC workflow :noexport:
- TODO [#B] Create CRD workflow :noexport:
- # --8<-------------------------- separator ------------------------>8-- :noexport:
- TODO Why we need kube-controller-manager :noexport:
- TODO Why we need cluster-controller-manager :noexport:
- # --8<-------------------------- separator ------------------------>8-- :noexport:
- TODO k8s volume: CSI, vmdk, NFS :noexport:
- TODO k8s dynamic PV provision vs static PV provision :noexport:
- TODO [#A] k8s delete namespace hang :noexport:
  Related resources need to be deleted first
- TODO [#A] k8s debugging loadbalancer service: external ip in <pending> state :noexport:
  #+BEGIN_EXAMPLE
  $ kubectl get svc -n blog
  NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE
  mysql ClusterIP 100.69.173.237 <none> 3306/TCP 12m
  wordpress LoadBalancer 100.66.31.190 <pending> 80:30407/TCP 12m

$ kubectl describe service/wordpress -n blog
Name: wordpress
Namespace: blog
Labels: app=wordpress
Annotations: Selector: app=wordpress
Type: LoadBalancer
IP: 100.66.31.190
Port: <unset> 80/TCP
TargetPort: 80/TCP
NodePort: <unset> 30407/TCP
Endpoints: 100.117.221.13:80
Session Affinity: None
External Traffic Policy: Cluster
Events: <none>
10:34

$ cat 21-wordpress-service.yaml
apiVersion: v1
kind: Service
metadata:
labels:
app: wordpress
namespace: blog
name: wordpress
spec:
type: LoadBalancer
ports: - port: 80
targetPort: 80
protocol: TCP
selector:
app: wordpress
#+END_EXAMPLE

- TODO K8s networking :noexport:

* container-to-container communication
* pod-to-pod communication
  K8s itself won't do it for you. And CNI can be used to configure the network of a pod and provide a single IP per pod.
  CNI doesn't help you with pod-to-pod communication across nodes.
* external-to-pod communication

- Questions forked from CKA preparation :noexport:
  ** TODO how etcd is designed and implemented?
  ** TODO [#A] Only one IP address per Pod. How multiple containers talk with each other inside one pod?
  Two containers share the same network namespace of the thrid container, known as the pause container.

* The pause container is used to get an IP address, then all containers in the pod will uses its network namespace.
* To communicate with each other, containers can use the loopback interface, write to files on a common filesystem, or via IPC
  ** TODO Why ipv6 doesn't gain popularity
  ipv6 not backward compatible
  NAT
  ipv4 better management
  ** TODO How K8s reconcilation is done?
  \*\* TODO How the feature of cluster ip is implemented?
