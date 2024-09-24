# Introduction to Cloud Infrastructure Technologies

## 1 - Virtualization

Cloud computing is a computing model that enables remote allocation of computing resources, owned by a third party, to be utilized by end users.

Cloud computing providers offer various services built on top of tools that automate basic provisioning and releasing of resources. Most of these service models fall into one of the following categories, as defined by NIST:

- Infrastructure as a Service (IaaS)
- Platform as a Service (PaaS)
- Software as a Service (SaaS).

In recent years, however, a variety of additional service models have been introduced to describe the needs and requirements of clients of remote computing services:

- Analytics as a Service (AnaaS)
- API as a Service (AaaS)
- Big Data as a Service (BDaaS)
- Business Process as a Service (BPaaS)
- Code as a Service (CaaS)
- Communications Platform as a Service (CPaaS)
- Desktop as a Service (DaaS)
- Database as a Service (DBaaS)
- Function as a Service (FaaS)
- Monitoring as a Service (MaaS)
- Anything as a Service (XaaS).

Another benefit of the cloud service model is the pay-as-you-go model adopted by cloud providers, allowing users to pay only for the cloud resources used during a set time frame. Such an approach to treat computing resources as a utility eliminates the need for high up-front investments in hardware, software, and specialized staff.

Primarily, cloud is deployed under the following models: private cloud, public cloud and hybrid cloud. However, there are other cloud deployment models as well.

![Cloud Computing Models](images/cloud-computing-models.png)

A **private cloud** is designated and operated solely for one organization. It can be hosted internally or externally and managed by internal teams or a third party. A private cloud can be build using a software stack like OpenStack. A **public cloud** is open to the public and anybody can use it, after swiping the credit card, of course. Amazon Web Services, Google Cloud Platform, and Microsoft Azure are examples of public clouds. A **hybrid cloud** is a composition of two or more clouds (private, community, or public) that remain unique entities but are bound together by standardized or proprietary technology that enables data and application portability.

On the other hand, **community cloud** is a cloud infrastructure shared by several organizations and supports a specific community that has shared concerns (e.g., mission, security requirements, policy, and compliance considerations). **Distributed cloud** refers to the distribution of public cloud services to different locations while the originating public cloud provider assumes responsibility for the operation, governance, maintenance, and updates of the services. **Multicloud** is the use of multiple cloud computing services in a single heterogeneous architecture to reduce reliance on single vendors, increase flexibility through choice, and mitigate against disasters. **Polycloud** is the use of multiple cloud computing services in a single heterogeneous architecture to reduce reliance on single vendors, increase flexibility through choice, and mitigate against disasters.

In order to facilitate cloud deployment models and to implement the characteristics of cloud computing, cloud services providers and self managed cloud deployment tools take advantage of specialized software to implement hardware virtualization.

In computing, the capability to create a virtual version of a physical compute resource, including a virtual computer hardware platform, operating system, virtual storage device, and virtual compute resource, is called virtualization.

Virtualization can be achieved at different hardware and software layers, such as the Central Processing Unit (CPU), storage (disk), memory (RAM), filesystems, etc. In this chapter, we will explore a few tools that allow us to create Virtual Machines (VMs) by virtualizing essential hardware components that support the installation of a guest Operating System (OS) on them. A VM (a software equivalent of a hardware built computing machine) represents an isolated collection of emulated resources, behaving like an actual physical system.

Virtual Machines are created with the help of a specialized virtualization software, a hypervisor, that runs on a host machine. The hypervisor is a software capable of creating multiple isolated virtual operating environments, each composed of virtualized resources that are then made available to guest systems. Hypervisors are classified into two main categories.

Type-1 hypervisor (native or bare-metal) runs directly on top of a physical host machine's hardware, without the need for a host OS. Typically, they are found in enterprise settings. Examples of type-1 hypervisors:

- AWS Nitro
- IBM z/VM
- Microsoft Hyper-V
- Nutanix AHV
- Oracle VM Server for SPARC
- Oracle VM Server for x86
- Red Hat Virtualization
- VMWare ESXi
- Xen.

Type-2 hypervisor (hosted) runs on top of the host's OS. Typically, they are for end-users, but they may be found in enterprise settings as well. Examples of type-2 hypervisors:

- Parallels Desktop for Mac
- VirtualBox
- VMware Player
- VMware Workstation.

And then there are the exceptions - hypervisors matching both categories, which can be listed redundantly under hypervisors of both type-1 and type-2. They are Linux kernel modules that act as both type-1 and type-2 hypervisors at the same time. Examples are:

- KVM
- bhyve

Hypervisors enable the virtualization of computer hardware such as CPU, disk, network, RAM, etc., and allow the installation of guest VMs on top of them. We can create multiple guest VMs with different Operating Systems on a single hypervisor. For example, we can use a native Linux machine as a host, and after setting up a type-2 hypervisor, we can create multiple guest machines with different OSes, commonly Linux and Windows.

Hardware virtualization is supported by most modern CPUs, and it is the feature that allows hypervisors to virtualize the physical hardware of a host system, thus sharing the host system's processing resources with multiple guest systems in a safe and efficient manner. In addition, most modern CPUs also support nested virtualization, a feature that enables VMs to be created inside another VM.

While virtualization is the key method that allows for physical hardware to become a virtualized software resource for the purpose of VM provisioning, virtual systems may be created with the help of emulators as well. More specifically software emulation that occurs at both system and user space levels, as supported by a tool such as QEMU, allows for any OS to run on any architecture, or for programs to run on originally unsupported systems. Users can also take advantage of QEMU's flexibility and use it in conjunction with a hypervisor such as KVM to provision VMs.

### KVM

KVM is a loadable virtualization module of the Linux kernel and it converts the kernel into a hypervisor capable of managing guest Virtual Machines. Specific hardware virtualization extensions supported by most modern processors, such as Intel VT and AMD-V, have to be available and enabled for processors to support KVM. Although originally designed for the x86 hardware, it has also been ported to FreeBSD, S/390, PowerPC, IA-64, and ARM as well.

![KVM Architecture](images/kvm.png)

KVM enables device abstraction of network interfaces, disk, but not the processor. Instead, it exposes the /dev/kvm interface that can be used by an external user space host for emulation. KubeVirt, QEMU, and virt-manager are examples of user space tools for KVM Virtual Machine management.

KVM supports nested guests, a feature that allows VMs to run within VMs. It also supports hotpluggable devices such as CPUs and PCI devices. Overcommitting is possible as well for the allocation of additional virtualized resources that may not be available on the system. To achieve overcommitting for a VM, KVM dynamically swaps resources from another guest that is not using the type of resource needed.

Several benefits of using KVM are:

- It is an open source solution, and, as such, free to customize.
- Using KVM is beneficial from a financial perspective, due to zero costs  associated with it.
- It provides efficient hardware-assisted virtualization for an array of - guest OSes, such as Linux, BSD, Solaris, Windows, macOS, ReactOS, and - Haiku.
- It provides para-virtualization of Ethernet cards, disk I/O controllers, and graphical interfaces for guest OSes.
- It is highly scalable.
- KVM employs advanced security features, utilizing SELinux. It provides MAC (Mandatory Access Control) security between Virtual Machines. KVM has received awards for meeting common government and military security standards and for providing open virtualization for homeland security projects.

### VirtualBox

VirtualBox is an x86 and AMD64/Intel64 virtualization product from Oracle, running on Windows, Linux, macOS, and Solaris hosts and supporting guest OSes from Windows, Linux families, and others, such as Solaris, FreeBSD, and DOS. However officially, only a select few of the most common guest OSes are supported and optimized to run on VirtualBox.

It is an easy-to-use multi-platform type-2 hypervisor. Although not part of the mainline Linux kernel, it can be used by compiling and inserting the respective kernel module.

Some of the benefits of using VirtualBox are:

- It is an open source solution.
- It is free to use.
- It runs on Linux, Windows, macOS, and Solaris.
- It provides two virtualization choices: software-based virtualization and hardware-assisted virtualization.
- It is an easy-to-use multi-platform type-2 hypervisor.
- It provides the ability to run virtualized applications side-by-side with normal desktop applications.
- It provides teleportation - live migration.

### Vagrant

Using virtual machines in a development environment has numerous benefits:

- Reproducible environment
- Management of multiple projects, each in its isolated and restricted environment
- Sharing the environment with other teammates
- Keeping the development and deployment environments in sync
- Running consistently the same VM on different OSes leveraging hypervisors such as VirtualBox, VMware, and KVM.

Configuring and sharing one VM is easy, but, when managing multiple VMs for the same project, performing all the build and configuration steps manually can be tiresome. Vagrant by HashiCorp helps us automate VMs management by providing an end-to-end lifecycle utility - the vagrant command line tool. Vagrant is a cross-platform tool, as it can be installed on Linux, macOS, and Windows.

One of the key features of Vagrant is extensibility. By default, Vagrant is shipped with a limited amount of features, but, thanks to various plugins, we can extend its support for custom providers, provisioners, commands, and hosts.

Vagrant has recently added support for Docker, allowing it to manage both Containers and Virtual Machines.

Several benefits of using Vagrant are:

- It automates the setup of one or more VMs, which results in saved time, increased productivity, and lower operational costs.
- It introduces consistency in infrastructure provisioning through Vagrantfile.
- It is a flexible cross-platform tool.
- It provides support for Docker containers in addition to VMs provisioned with VMware, VirtualBox, and Hyper-V.
- It is easy to install and configure.
- It is very useful in multi-developer teams.

## 2 - Infrastructure as a Service (IaaS)

Infrastructure as a Service (IaaS) is a cloud service model that provides on-demand physical and virtual computing resources, storage, network, firewall, and load balancers. To provide virtual computing resources, IaaS uses hypervisors, such as Xen, KVM, VMware ESXi, Hyper-V, or Nitro.

Infrastructure as a Service is the backbone of all other cloud services, providing computing resources. After the provisioning of the computing resources, other services are set up on top. IaaS enables developers to provision and destroy isolated test environments as needed, when needed, quickly, and safely. The provisioning process can be easily reproduced to add consistency to the test environment, not only by the same developer, but by an entire team if desired.

Let's say that you want to have a set of ten Linux systems with 4GB RAM each and two Windows systems with 8GB RAM each to deploy your software. You can go to many IaaS providers and request these systems. Generally, an IaaS provider creates the respective VMs in the background, puts them in the same internal network, and shares any access credentials with you, thus enabling you to access them. Other than VMs, several IaaS providers offer bare-metal machines for provisioning as well.

### Amazon Elastic Compute Cloud (EC2)

Amazon Web Services (AWS) is one of the leading cloud services providers. Its cloud services, whether Platform, Software, DB, Containers, Serverless, Machine Learning, to name a few, rely heavily on its Infrastructure. Part of Amazon's Infrastructure as a Service (IaaS) model is the Amazon Elastic Compute Cloud (Amazon EC2) service. Amazon EC2 service allows individual users and enterprises alike to build a reliable, flexible, and secure cloud infrastructure for their applications and workloads on Amazon's platform. AWS offers an easy to use Console, which is a web interface for cloud resources management.

Amazon EC2 instances are in fact Virtual Machines. When provisioning EC2 instances on the AWS infrastructure, we are provisioning VMs on top of hypervisors that run directly on Amazon's physical infrastructure. In addition to the web console, AWS also offers a command line interface (CLI), a tool to manage resources with command line tools and scripts if necessary.

At the heart of the Amazon EC2 service are various type-1 hypervisors, such as Xen, KVM, and Nitro (a newer KVM-based lightweight hypervisor that delivers close to bare-metal performance).

Although Amazon EC2 is a paid service, new users are encouraged to try the AWS platform for free through AWS Free Tier offerings. While not all of its services qualify for the free tier, the ones that are available for free do come with some limitations. Several such services are offered under short-term free trials, others are offered for 12 months free, while a third group of services may be always free to all users. The three types of free offerings have been designed to encourage users to take advantage of the free tier offerings to become familiar with the most popular services the AWS platform has to offer.

When provisioning Amazon EC2 instances, users are able to manage several aspects of the instance, such as hardware profile, operating system image with software package, additional storage, the network attached to the instance, and firewall rules.

Amazon Machine Images (AMI) are preconfigured images with the information needed to launch EC2 instances. They include an operating system and a collection of software packages. AMIs are stored in an AWS repository and are used to quickly launch instances. There are default images created and maintained by AWS, the user community, or custom images we can create ourselves.

Instance Types determine the virtualized hardware profile of the instance to be launched. Each instance type is preconfigured with different compute, memory, and storage capabilities. They are categorized in instance families such as general purpose instances or optimized instances for:

- Compute (CPU) - for CPU-intensive applications such as high-performance computing (HPC), machine learning (ML), batch workloads, and media transcoding.
- Accelerated Computing (GPU) - for graphically intensive, scientific, and engineering applications, and for machine learning.
- Memory (RAM) - for in-memory databases, in-memory caches, and real-time data processing.
- Machine Learning (ML).
- Storage (SSD) - for data-intensive workloads and distributed filesystems.
When launching an instance, select an instance type based on the requirements of the application or software expected to run on the instance.

Additional configurable aspects of EC2 instances, related services, and tools may include:

- Virtual Private Cloud (VPC) for network isolation.
- Security Groups for VPC networks to control EC2 inbound/outbound traffic.
- Amazon Elastic Block Store (EBS) for persistent storage attachment.
- Dedicated hosts to provision instances on a physical machine reserved for our use.
- Elastic IP to remap a Static IP address automatically.
- CloudWatch for monitoring resources and applications.
- Auto Scaling to dynamically resize resources.

### Azure Virtual Machines

Microsoft Azure is a leading cloud services provider, with products in different domains, such as compute, web and mobile, data and storage, Internet of Things (IoT), and many others. The Azure Virtual Machine service allows individual users and enterprises alike to provision and manage compute resources, both from a web Portal or the Azure Cloud Shell, a flexible command line utility configurable to use either Bash or PowerShell.

Azure cloud services are enabled by the Azure Hypervisor, a customized version of Microsoft Hyper-V type-1 hypervisor. The Azure Hypervisor promises high efficiency, performance, and scalability over a small footprint thanks to the optimization of the custom Hyper-V hypervisor coupled with its tight integration with the physical infrastructure.

The Azure Virtual Machine is a paid service, however, the Azure free account offering aims to attract new users and encourage them to explore the cloud services the Azure platform has to offer. Between an initial credit limited to the first 30 days following the opening of the free account, popular services free for 12 months, and a set of always free services, users have several options enabling them to become familiar with the most popular services the Azure cloud platform has to offer.

During the VM provisioning process, Azure users are able to manage various instance aspects such as the Operating System, VM size, storage, networking, and firewall rules.

The VM Image field defines the base Operating System or the application for the VM. Both Linux VMs and Windows VMs can be launched from images available through the Azure Marketplace.

The VM size determines the type and capacity of the compute, memory, storage and network resources for the VM to be launched. VMs are grouped in families based on their intended usage. General purpose for development and testing environments, low traffic web servers, and small databases or optimized VMs for:

- Compute (for CPU intensive applications) for medium traffic web servers, batch processes and application servers.
- GPU for heavy graphic rendering, video editing, and deep learning.
- Memory (RAM) for relational databases and in-memory analysis.
- High Performance Compute (HPC).
- Storage for data warehousing and large transactional databases.
A
dditional configurable aspects of VMs:

- Network security groups to manage network traffic.
- SSD or HDD for persistent storage attachment, with optional encryption.
- Dedicated hosts to provision VMs on a physical machine reserved for our use.
- Accelerated networking for low latency and high throughput.
- Virtual network for network isolation.
- Monitoring resources and applications.
- Resource Manager templates for VM deployment.
- Seamless hybrid connections.
- Automated backups.

### DigitalOcean Droplets

DigitalOcean is a leading cloud services provider, aiming its cloud platform at both individual users and enterprises. DigitalOcean helps you create a simple cloud quickly, as it promises IaaS virtual instances set up within seconds. DigitalOcean cloud services enable application deployments and scaling on a cloud infrastructure available worldwide, leveraging network and storage flexibility together with security and monitoring tools.

In the DigitalOcean ecosystem, the virtual compute instances are called droplets, and they are Linux-based VMs launched on top of the KVM type-1 hypervisor, with SSD (Solid-State Drive) as their primary storage disk.

DigitalOcean offers paid cloud services, but users can take advantage of a short-term credit towards cloud services, part of a free tier offering aimed to encourage users to explore and become familiar with its offerings.

DigitalOcean droplets are easy to configure, users being able to manage the resource profile, guest Operating System, application server, security, backup, monitoring, and more.

Resource profiles are directly associated with DigitalOcean cost plans and are categorized in Shared CPU and Dedicated CPU plans.

The Shared CPU plan includes the Basic Droplets with burstable vCPU and memory that can be configured to support the running of web servers, forums, and blogs.

The Dedicated CPU plan includes dedicated vCPUs and a specific amount of memory per each vCPU that further determines the droplet type:

- General Purpose with 4GB of memory for each vCPU, to support high traffic web servers, e-commerce, and SaaS.
- CPU-Optimized with 2GB of memory for each vCPU, to support Machine Learning, CI/CD, and video encoding.
- Memory-Optimized with 8GB of memory for each vCPU to support high-performance DBs, and real-time big data processing.

The droplet guest OS can be picked from a list of Linux distribution images, such as CentOS, CoreOS, Debian, Fedora, FreeBSD, and Ubuntu. While custom images can be built and used, droplets can be pre-configured to run applications such as Docker, LAMP, MongoDB, MySQL, and Node.js.

Users can take advantage of additional configurable services and features:

- Monitoring to collect performance metrics.
- Cloud Firewalls to secure the network infrastructure.
- Backups that can be automated, allowing for easy droplet restores or to launch new pre-configured droplets.
- Snapshots to be used as restore points after a failed upgrade or configuration change.
- Team Management for collaboration.
- Block Storage for droplet storage.
- Spaces for scalable and secure storage solutions aimed to store and deliver data.
- Load Balancers for traffic distribution.
- Floating IPs for flexibility when assigning IPs to droplets and to release them when no longer needed.
- APIs for programmatic droplet launching.
- Networking features, such as DNS, IPv6, private networking.

### Google Compute Engine (GCE)

Google Cloud Platform (GCP) is one of the leading cloud services providers. Its infrastructure is the foundation for all other cloud services, whether Platform, Software, Containers, Serverless, Artificial Intelligence, Machine Learning, to name a few. Part of Google's Infrastructure as a Service (IaaS) model is the Compute Engine service. Google Compute Engine (GCE) service allows individual users and enterprises alike to build a reliable, flexible, and secure cloud infrastructure for their applications and workloads on Google's platform. GCP offers an easy to use Console, which is a web interface for cloud resources management.

GCE instances are in fact Virtual Machines. When provisioning GCE instances on the GCP infrastructure, we are provisioning VMs on top of hypervisors that run directly on Google's physical infrastructure. In addition to the web Console, GCP also offers a command line interface (CLI), a tool to manage resources with command line tools and scripts if necessary.

GCE services are enabled by the KVM type-1 hypervisor, running directly on Google's physical infrastructure and allowing for VMs to be launched with Linux and Windows guest Operating Systems.

GCE is a paid service, however, new users are encouraged to try the GCP platform for free through the GCP Free Tier offering. The Free Tier offers new users a chance to become familiar with the most popular services the Google Cloud Platform has to offer with the help of short-term initial free credits and several products that are always free within predefined usage limits.

Google Compute Engine service allows users to configure many of the VM instance characteristics, such as machine profile, image, storage, network, security, and access control.

GCE Machine Types determine the virtualized hardware configuration for VMs to be provisioned. Based on available configurations, VMs are categorized in various machine type families aimed to support specific types of applications and workloads:

- General-purpose machines offer the best price-performance ratio. This family includes machine types N1, N2, N2D, E2.
- Memory-optimized designed for memory-intensive workloads.
- Compute-optimized for compute-intensive workloads.
- Accelerator-optimized for machine learning (ML), high-performance computing (HPC), and GPU-dependent workloads.
- Shared-core for cost-effective light-weight applications. Several machine types from the N1 and E2 machine families share a physical core to minimize costs.

GCE Images determine the guest Operating System of the VMs. While there are several public images available to launch VMs, users can create custom Images as well.

Additional configurable aspects of GCE VMs, and related services:

- Storage Disks for VM attached storage volumes.
- Networking VPC and Firewalls for network isolation and security.
- Snapshots for fast GCE persistent disk backups and recovery.
- Cloud Security Scanner to scan applications for security vulnerabilities.
- Health Checks to probe VMs health.
- Sole-tenant Nodes for dedicated physical Compute Engines.
- Network Endpoint Group representing collections of IP addresses for load balancing, firewalls, and logging purposes.

### IBM Cloud Virtual Servers

IBM Cloud is one of the leading cloud services providers for enterprises and academic institutions. It includes the typical service models, the Infrastructure as a Service (IaaS), Software as a Service (SaaS), and Platform as a Service (PaaS) offered through public, private, and hybrid cloud delivery models. Part of the IBM Cloud IaaS model is the IBM Cloud Virtual Servers service, also known as Virtual Machines, one of the many services providing cloud compute services. IBM Cloud Virtual Servers provide interoperability between virtual and bare-metal servers by being deployed on the same VLANs as the physical servers. When you create an IBM Cloud Virtual Server, you can choose between multi-tenancy or single-tenancy environments, and also high-performance local disks or enterprise SAN storage.

IBM Cloud is the successor of two joined technologies - the IBM mainframe and virtualization. IBM treats virtualization with some level of flexibility. While it uses IBM z/VM and IBM PowerVM, two powerful hypervisors, to manage its own virtual workload, the users of IBM Cloud are allowed to choose between XenServer, VMware, and Hyper-V hypervisors when managing bare-metal instances. This level of hypervisor flexibility is one of the advantages of IBM Cloud, and one of the top reasons why users pick IBM Cloud over another cloud service provider.

IBM Cloud services are available at a cost, however, the IBM Cloud Free Tier offer allows users to explore always free cloud services in combination with a predetermined amount of free credits available short-term to be used towards specific cloud resources.

When provisioning IBM Cloud Virtual Servers, users are able to manage different server aspects, such as profile, image, software package add-ons, attached storage, network interface bandwidth, internal and external firewall rules, IP addresses, and VPN. Most of these server aspects are available for the four supported types of virtual servers:

- Public for multi-tenants
- Dedicated for single-tenants
- Transient for ephemeral multi-tenants
- Reserved for multi-tenants committed for a pre-specified term.

Profiles specify the size of the virtual server and are associated with predefined resource amounts that the servers get launched with. While there are popular profiles specifying a balanced collection of compute, memory, and storage resources, there are optimized profiles aimed at specific workload types:

- Balanced for common cloud workloads requiring a balance between performance and scalability, with network-attached storage (NAS).
- Balanced local storage for medium to large databases that require high I/O, with local HDD storage.
- Compute for compute-intensive deployments such as moderate to high traffic web servers.
- Memory for caching and analytics workloads.
- Variable compute for workloads without constant high-CPU performance.
- GPU for high-performance deployments.

Virtual server images are preconfigured with the information needed to launch IBM Cloud Virtual Servers. They include an operating system such as a Linux distribution or Windows, and optional software package add-ons.

### Linode Compute Instances

Linode, presently a subsidiary of Akamai Technologies, is the cloud computing service provider at the core of Akamai Connected Cloud. Linode offers a variety of cloud computing plan types that range from Shared CPU to GPU for most demanding compute workloads. Linode offers users a stable infrastructure on Linux VMs that is advertised as a discounted solution for infrastructure provisioning and hosting, aggressively challenging cloud platforms such as AWS and Azure.

Linode combines a diverse set of offerings with the user friendliness of an easy to use management dashboard. Its popular compute, storage, network, and resource management tools blended with Akamai’s advanced content delivery network (CDN) and security services turned the joint venture into a powerful cloud services provider. Users can take advantage of the Linode Compute service to provision robust, secure, yet inexpensive infrastructure that can satisfy a vast array of computing requirements. The following plan types are available to provision compute infrastructure:

- Shared CPU Plans are the most inexpensive type, aimed to support development and staging environments, and web applications.
- Dedicated CPU Plans are designed for production environments and high traffic database applications.
- Premium Plans are targeted at enterprise level business use cases, and latency sensitive application deployments.
- High Memory Plans are optimized for memory intensive use cases such as in-memory databases and caching, and big data processing.
- GPU Plans, based on NVIDIA RTX 6000 hardware, are suited for complex parallel processing needs, AI, ML, big data analysis, and graphics/video processing.

### Oracle Cloud Compute Virtual Machines

Oracle is a leading cloud services provider through its Oracle Cloud offering. Oracle's IaaS cloud service model is implemented by a wide range of Cloud Infrastructure products. A commonly used infrastructure product is Cloud Compute, which includes several options to support various types of I/O intensive workloads, high-performance computing (HPC), and artificial intelligence (AI). These options are represented by Cloud Compute Instances that include Virtual Machines (VMs), Bare Metal Servers, GPU-optimized VMs and Bare Metal Servers, alongside Autonomous Linux instances, and instances optimized for Container Registries and Container Engines for Kubernetes.

Oracle Cloud Infrastructure Compute Virtual Machines are offered in many shapes with Oracle Compute Units (OCPUs) to support a wide range of workloads and software platforms, allowing for storage support customization from remote block storage to high local SSD storage capacity.

At the core of the Oracle Cloud Infrastructure are Bare Metal Servers capable of supporting Nested Virtualization when paired with a hypervisor such as KVM.

Oracle Cloud is also a paid offering, however, similar to several other cloud providers, Oracle too has a Free Tier offering. The free tier is composed of a set of Always Free services, coupled with free credit towards additional eligible infrastructure services, available for a limited time upon signing up. The free tier encourages users to navigate the Oracle Cloud services and to become familiar with the most popular options and features.

Oracle Cloud Infrastructure VMs offer flexible performance paired with strong isolation thanks to cloud-optimized hardware, software, and networking, all at an advantageous cost.

Infrastructure VMs are offered in several shapes, which are determined by the virtual hardware profile of the instance:

- Standard, or general-purpose instances, offering a balance between compute, memory, and networking resources, suitable for most common use cases.
- Dense IO, or high-performance instances, paired with large local non-volatile memory express (NVMe) SSD storage, suitable for large database applications.
When provisioning Cloud VMs, users are able to manage additional instance aspects and related services:

- Flexible image management by allowing users to choose between images based on enterprise Linux distributions or Windows Server, to bring their own custom guest operating systems, or to choose an image from an Oracle partner.
- Low latency block storage for boot volumes, for increased performance and reliability, and to facilitate backups and restores.
- Secure and flexible network through fully customizable private Virtual Cloud Networks (VCN).
- High availability by distributing application deployments in multi-regions, multi-availability domains or multi-fault domains, ensuring fault isolation, and low latency across availability domains.

### OpenStack

We have seen examples for consuming the services of different cloud providers to provision our infrastructure. What if we want to become a cloud provider and offer cloud computing services?

OpenStack allows users to build a cloud computing platform for public and private clouds. OpenStack was started as a joint project between Rackspace and NASA in 2010. In 2012, a non-profit corporate entity, the OpenStack Foundation, was formed to manage it while receiving the support of more than 500 organizations. OpenStack is an open source software platform released under an Apache 2.0 License, and it is currently part of the OpenInfra Foundation.

In addition to providing a IaaS solution, OpenStack has evolved over time to provide other services, such as Database, Storage, and Networking.

The modular nature of OpenStack allows users to design and implement components for specific features or functionality. OpenStack components provide APIs for accessing infrastructure resources by cloud end users.

- **Nova** is a compute service that implements scalable and on-demand access to compute resources, including bare metal, virtual machines, and containers.
- **Ironic** is a bare metal provisioning service part of Hardware lifecycle services.
- **Cyborg** is a lifecycle management service for GPU and ASIC-based devices, part of Hardware lifecycle services.
- **Swift** is the object store part of Storage services that provides a highly available, distributed, eventually consistent object/blob store.
- **Cinder** is the block storage part of Storage services that provides an abstraction layer over the management of block storage devices through a self-service API.
- **Manila** is the shared filesystem part of Storage services, provides coordinated access to shared or distributed filesystems.
- **Neutron** is a networking service, a Software Defined Networking (SDN) delivering networking as a service (NaaS) in virtual compute environments.
- **Octavia** is the load balancer part of Networking services, delivering on-demand and horizontal scaling load balancing as a service (LBaaS) to fleets of VMs, containers, or bare-metal servers.
- **Designate** is the DNS-as-a-Service part of Networking services.
- **Keystone** is the identity service part of Shared services, provides authentication, service discovery, and authorization. It supports LDAP, OAuth, OpenID Connect, SAML, and SQL.
- **Glance** is the image service part of Shared services, provides VM image discovery, registration, and retrieval.
- **Heat** is the orchestration service provides orchestration of infrastructure resources for cloud applications that also supports autoscaling.
- **Senlin** is the clustering service part of Orchestration services eases the orchestration of clusters of similar objects.
- **Magnum** is the container orchestration engine provisioning service part of Workload provisioning services provisions Docker Swarm, Kubernetes or Apache Mesos to run on a cluster of VMs or bare-metal servers.
- **Sahara** is the framework provisioning service for the Big Data processing part of Workload provisioning services.
- **Freezer** is the Backup, Restore, and Disaster Recovery service part of Application lifecycle services, provides efficient and flexible block-based backups, file-based incremental backups, synchronized backups over multiple nodes, while it supports multiple OSes (Linux, Windows, macOS).
- **Horizon** is the dashboard service part of Web frontend services provides an extensible web-based user interface to manage OpenStack services.
- **Ceilometer** is the metering and data collection service, an operations tool part of Monitoring services, provides efficient data collection, normalization, and transformation for telemetry purposes.
- **Monasca** is the monitoring service, an operations tool part of Monitoring services, provides highly-scalable, fault-tolerant monitoring as a service solution (MaaS), together with high-speed metrics processing, querying, alarm, and notification engines.
- **Cloudkitty** is the billing and chargeback service, an operations tool part of Billing and Business logic services, introduces rating as a service (RaaS) to convert rating metrics into service pricing.
- **Rally** is used for benchmarking and performance analysis; it is an operations tool part of Testing/Benchmarking services that automates the detection of the impact changes in software architecture and hardware have on the OpenStack performance.
- **Kuryr** is the networking integration enabler for containers running on OpenStack.

### Vultr Cloud Compute Virtual Machines

Vultr is a younger cloud services provider that aims to simplify the way cloud is consumed by developers and businesses. Through its worldwide network of datacenters, Vultr enables its users to provision public cloud, cloud storage, single-tenant bare metal systems, and highly reliable high performance cloud compute environments.

Vultr supports a wide range of business solutions, from simple Web Hosting, Machine Learning (ML), and AI to Blockchain, and High Performance Computing (HPC). It integrates shared virtual CPUs, AMD CPUs, Intel CPUs, and GPUs with regular SSD and NVMe SSD into its infrastructure offering a wide array of Cloud Compute Virtual Machines.

Vultr is a paid offering; however (as of October 2024), it has a free offering through free credit, limited to the first 30 days of a newly opened account, aimed to encourage users to explore the Vultr Cloud Compute and Cloud Storage services and to become familiar with its most popular options and features.

Factoring in all the possible combinations of compute and storage hardware in Vultr’s inventory, there are various Cloud Compute instance categories available to end-users:

- Cloud Compute VMs suitable for low traffic websites, blogs, and small databases. They leverage shared virtual CPUs and include Regular Performance, High Performance, and High Frequency VMs.
- Optimized Cloud Compute VMs are designed for specialized use cases. They include General Purpose suitable for E-Commerce, game servers, video streaming; CPU Optimized suitable for video encoding, HPC, analytics processing; Memory Optimized suitable for in-memory databases and caches, real-time analytics; and Storage Optimized suitable for non-relational databases, and high frequency online transaction processing (OLTP).
- Cloud GPU leveraging NVIDIA hardware suitable for AI, HPC, and virtual desktops.
- Bare Metal physical instances satisfying challenging single-tenant requirements of most demanding businesses.

## 3 - Platform as a Service (PaaS)