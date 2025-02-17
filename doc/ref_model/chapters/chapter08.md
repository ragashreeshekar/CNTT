[<< Back](../../ref_model)

# 8. Hybrid Multi-Cloud: Data Centre to Edge

## Table of Contents
* [8.1. Introduction](#8.1)
* [8.2. Hybrid Multi-Cloud Architecture](#8.2)
  * [8.2.1. Characteristics of a Federated Cloud](#8.2.1)
  * [8.2.2. Telco Cloud](#8.2.2)
  * [8.2.3. Telco Operator Platform Conceptual Architecture](#8.2.3)
* [8.3. Telco Edge Cloud](#8.3)
  * [8.3.1. Telco Edge Cloud: Deployment Environment Characteristics](#8.3.1)
  * [8.3.2. Telco Edge Cloud: Infrastructure Characteristics](#8.3.2)
  * [8.3.3. Telco Edge Cloud: Infrastructure Profiles](#8.3.3)
  * [8.3.4. Telco Edge Cloud: Platform Services Deployment](#8.3.4)
  * [8.3.5. Comparison of Deployment Topologies and Edge terms](#8.3.5)


<a name="8.1"></a>
## 8.1 Introduction
The [Reference Model Chapter 3](./chapter03.md) focuses on cloud infrastructure abstractions. While these are generic abstractions they and the associated capabilities of the cloud infrastructure are specified for data centres, central office and colocation centres. The environmental conditions, facility and other constraints, and the variability of deployments on the edge are significantly different and, thus, require separate consideration.

It is unrealistic to expect that a private cloud can cost effectively meet the need of all loads, including peak loads and disaster recovery. It is for that reason that enterprises will implement a hybrid cloud.  In a hybrid cloud deployment, at least two or more distinct cloud infrastructures are inter-connected together.  In a multi-cloud the distinct cloud infrastructures of the hybrid cloud may be implemented using one or more technologies.  The hybrid multi-cloud infrastructure has differences requiring different abstractions. These hybrid multi-clouds can be considered to be federated.

In the [Reference Model Chapter 3](./chapter03.md), the cloud infrastructure is defined. The tenants are required to provide certain needed services (such as Load Balancer (LB), messaging). Thus, the VNF/CNFs incorporate different versions of the same services with the resultant issues related to an explosion of services, their integration and management complexities. To mitigate these issues, the Reference Model must specify the common services that every Telco cloud must support and thereby require workload developers to utilise these pre-specified services.

A generic Telco cloud is a hybrid multi-cloud or a Federated cloud that has deployments in large data centres, central offices or colocation facilities, and the edge. This chapter discusses the characteristics of Telco Edge and hybrid multi-cloud.

<a name="8.2"></a>
## 8.2 Hybrid Multi-Cloud Architecture
The GSMA whitepaper on "Operator Platform Concept Phase 1: Edge Cloud Computing" (January 2020) states, "Given the wide diversity of use cases that the operators will tasked to address, from healthcare to industrial IoT, it seems logical for operators to create a generic platform that can package the existing assets and capabilities (e.g., voice messaging, IP data services, billing, security, identity management, etc. ...) as well as the new ones that 5G makes available (e.g., Edge cloud, network slicing, etc.) in such a way as to create the necessary flexibility required by this new breed of enterprise customers."

Cloud computing has evolved and matured since 2010 when [NIST](http://csrc.nist.gov/publications/nistpubs/800-145/SP800-145.pdf) published its definition of cloud computing, with its 5 essential characteristics, 3 service models and 4 deployment models.

The generic model for an enterprise cloud has to be "hybrid" with the special cases of purely private or public clouds as subsets of the generic hybrid cloud deployment model. In a hybrid cloud deployment, at least two or more distinct cloud infrastructures are inter-connected together.

Cloud deployments can be created using a variety of technologies  (e.g., OpenStack, Kubernetes) and commercial technologies (e.g., VMware, AWS, Azure, etc.). A multi-cloud deployment can consist of the use of more than one technology.

A generic Telco cloud is a hybrid multi-cloud. A better designation would be a federation of clouds - a federated cloud:
   - a collection of cooperating, interoperable autonomous component clouds
   -  the component clouds perform their local operations (internal requests) while also participating in the federation and responding to other component clouds (external requests)
        - the component clouds are autonomous in terms of, for example, execution autonomy; please note that in a centralised control plane scenario (please see the section "Centralised Control Plane" in the "[Edge Computing: Next Steps in Architecture, Design and Testing](https://www.openstack.org/use-cases/edge-computing/edge-computing-next-steps-in-architecture-design-and-testing/)" whitepaper [26]) the edge clouds do not have total autonomy and are subject to constraints (e.g., workload LCM)  
        - execution autonomy is the ability of a component cloud to decide the order in which internal and external requests are performed
   - the component clouds are loosely coupled where no changes are required to participate in a federation
        - also, a federation controller does not impose changes to the component cloud except for running some central component(s) of the federated system (for example, a broker agent – executes as a workload)
   - the component clouds are likely to differ in, for example, infrastructure resources and their cloud platform software
   - workloads may be distributed on single or multiple clouds, where the clouds may be collocated or geographically distributed
   - component clouds only surface NBIs (Please note that VMware deployed in a private and  a public cloud can be treated as a single cloud instance)

<a name="8.2.1"></a>
### 8.2.1 Characteristics of a Federated Cloud
In this section we will further explore the characteristics of the federated cloud architecture, and architecture building blocks that constitute the  federated cloud. For example, Figure 8-1 shows a Telco Cloud that consists of 4 sub-clouds: Private on premise, Cloud Vendor provided on premise, Private outsourced (Commercial Cloud Provider such as a Hyperscaler Cloud Provider (HCP), and Public outsourced (see diagram below). Such an implementation of a Telco Cloud allows for mix'n'match of price points, flexibility in market positioning and time to market, capacity with the objective of attaining near "unlimited" capacity, scaling within a sub-cloud or through bursting across sub-clouds, access to "local" capacity near user base, and access to specialised services.

<p align="center"><img src="../figures/RM-Ch08-HMC-Image-1.png" alt="Example Hybrid Multi-Cloud Component Cloud"></p>
<p align="center"><b>Figure 8-1:</b> Example Hybrid Multi-Cloud Component Cloud</p>

<a name="8.2.2"></a>
### 8.2.2 Telco Cloud
The Figure 8-2 presents a visualisation of a Telco operator cloud (or simply, Telco cloud) with clouds and cloud components distributed across Regional Data Centres, Metro locations (such as Central Office or a Colocation site) and at the Edge, that are interconnected using a partial mesh network. Please note that at the Regional centre level the interconnections are likely to be a "fuller" mesh while being a sparser mesh at the Edges.

<p align="center"><img src="../figures/RM-Ch08-Multi-Cloud-DC-Edge.png" alt="Telco Cloud: Data Centre to Edge"></p>
<p align="center"><b>Figure 8-2:</b> Telco Cloud: Data Centre to Edge</p>

The Telco Operator may own and/or have partnerships and network connections to utilize multiple Clouds for network services, IT workloads, and external subscribers. The types of the component clouds include:
   - On Premise Private
        - Open source; Operator or Vendor deployed and managed  | OpenStack or Kubernetes based
        - Vendor developed; Operator or Vendor deployed and managed  | Examples: Azure on Prem, VMware, Packet, Nokia, Ericsson, etc.
   - On Premise Public: Commercial Cloud service hosted at Operator location but for both Operator and Public use | Example: AWS Wavelength
   - Outsourced Private: hosting outsourced; hosting can be at a Commercial Cloud Service | Examples: Equinix, AWS, etc.
   - (Outsourced) Public: Commercial Cloud Service | Examples: AWS, Azure, VMware, etc.
   - Multiple different Clouds can be co-located in the same physical location and may share some of the physical infrastructure (for example, racks)

In general, a Telco Cloud consists of multiple interconnected very large data centres that serve trans-continental areas (Regions). A Telco Cloud Region may connect to multiple regions of another Telco Cloud via large capacity networks. A Telco Cloud also consists of interconnected local/metro sites (multiple possible scenarios). A local site cloud may connect to multiple Regions within that Telco Cloud or another Telco Cloud. A Telco Cloud also consists of a large number of interconnected edge nodes where these edge nodes maybe impermanent. A Telco Cloud's Edge node may connect to multiple local sites within that Telco Cloud or another Telco Cloud; an Edge node may rarely connect to a Telco Cloud Region.

Table 8-1 captures the essential information about the types of deployments, and responsible parties for cloud artefacts.



Type | System Developer | System Maintenance | System Operated & Managed by | Location where Deployed | Primary Resource Consumption Models
---|---|---|---|---|---
Private (Internal Users) | Open Source | Self/Vendor | Self/Vendor | On Premise | Reserved, Dedicated
Private | Vendor  \|  HCP | Self/Vendor | Self/Vendor | On Premise | Reserved, Dedicated
Public | Vendor  \|  HCP | Self/Vendor | Self/Vendor | On Premise | Reserved, On Demand
Private | HCP | Vendor | Vendor | Vendor Locations | Reserved, Dedicated
Public (All Users) | HCP | Vendor | Vendor | Vendor Locations | On Demand, Reserved
<p align="center"><b>Table 8-1. Cloud Types and the Parties Responsible for Artefacts</b></p>

<a name="8.2.3"></a>
### 8.2.3 Telco Operator Platform Conceptual Architecture
Figure 8-3 shows a conceptual Telco Operator Platform Architecture. The Cloud Infrastructure Resources Layer exposes virtualised (including containerised) resources on the physical infrastructure resources and also consists of various virtualisation and management software (see details later in this chapter). The Cloud Platform Components Layer makes available both elementary and composite objects for use by application and service developers, and for use by Services during runtime.  The Cloud Services Layer exposes the Services and Applications that are available to the Users; some of the Services and Applications may be sourced from or execute on other cloud platforms. Please note that while the architecture is shown as a set of layers, this is not an isolation mechanism and, thus, for example, Users may access the Cloud Infrastructure Resources directly without interacting with a Broker.

<p align="center"><img src="../figures/RM-Ch08-Telco-Operator-Platform.png" alt="Conceptual Architecture of a Telco Operator Platform"></p>
<p align="center"><b>Figure 8-3:</b> Conceptual Architecture of a Telco Operator Platform</p>

The Cloud Services and the Cloud Resources Brokers provide value-added services in addition to the fundamental capabilities like service and resource discovery.  These Brokers are critical for a multi-cloud environment to function and utilise cloud specific plugins to perform the necessary activities. These Brokers can, for example, provision and manage environments with resources and services for Machine Learning (ML) services, Augmented/Virtual Reality, or specific industries.

<a name="8.3"></a>
## 8.3 Telco Edge Cloud
This section presents the characteristics and capabilities of different Edge cloud deployment locations, infrastructure, footprint, etc. Please note that in the literature many terms are used and, thus, this section includes a table that tries to map these different terms.

<a name="8.3.1"></a>
### 8.3.1. Telco Edge Cloud: Deployment Environment Characteristics
Telco Edge Cloud (TEC) deployment locations can be environmentally friendly such as indoors (offices, buildings, etc.) or environmentally challenged such as outdoors (near network radios, curb side, etc.) or environmentally harsh environments (factories, noise, chemical, heat and electromagnetic exposure, etc). Some of the more salient characteristics are captured in Table 8-2.

| | Facility Type | Environmental Characteristics | Capabilities | Physical Security | Implications | Deployment Locations
-----|-----|-----|-----|-----|-----|------
Environmentally friendly | Indoors: typical commercial or residential structures | Protected<br>Safe for common infrastructure | Easy access to continuous electric power<br>High/Medium bandwidth Fixed and/or wireless network access | Controlled Access | Commoditised infrastructure with no or minimal need for hardening/ruggedisation<br>Operational benefits for installation and maintenance | Indoor venues: homes, shops, offices, stationary and secure cabinets<br>Data centers, central offices, co-location facilities, Vendor premises, Customer premises |
Environmentally challenged | Outdoors and/or exposed to environmentally harsh conditions | maybe unprotected<br>Exposure to abnormal levels of noise, vibration, heat, chemical, electromagnetic pollution | May only have battery power<br>Low/Medium bandwidth Fixed and/or mobile network access | No or minimal access control | Expensive ruggedisation<br>Operationally complex | Example locations: curb side, near cellular radios, |
<p align="center"><b>Table 8-2. TEC Deployment Location Characteristics & Capabilities</b></p>

<a name="8.3.2"></a>
### 8.3.2 Telco Edge Cloud: Infrastructure Characteristics
Commodity hardware is only suited for environmentally friendly environments. Commodity hardware have standardised designs and form factors. Cloud deployments in data centres typically use such commodity hardware with standardised configurations resulting in operational benefits for procurement, installation and ongoing operations.

In addition to the type of infrastructure hosted in data centre clouds, facilities with smaller sized infrastructure deployments, such as central offices or co-location facilities, may also host non-standard hardware designs including specialised components. The introduction of specialised hardware and custom configurations increases the cloud operations and management complexity.

At the edge, the infrastructure may further include ruggedised hardware for harsh environments and hardware with different form factors.

<a name="8.3.3"></a>
### 8.3.3 Telco Edge Cloud: Infrastructure Profiles
The [Cloud Infrastructure Profiles](./chapter04.md#4.2) section specifies two infrastructure profiles:

The **Basic** cloud infrastructure profile is intended for use by both IT and Network Function workloads that have low to medium network throughput requirements.

The **High Performance** cloud infrastructure profile is intended for use by applications that have high network throughput requirements (up to 50Gbps).

The High Performance profile can specify extensions for hardware offloading; please see [Hardware Acceleration Abstraction](./chapter03.md#3.8). The Reference Model High Performance profile includes an initial set of [High Performance profile extensions](./chapter04.md#4.2.3).

Based on the infrastructure deployed at the edge, Table 8-3 specifies the [Infrastructure Profile features and requirements](./chapter05.md) that would need to be relaxed.

**Table 8-3. TEC Exceptions to [Infrastructure Profile features and requirements](./chapter05.md)**

| Reference | Feature | Description | As Specified in RM Chapter 05| | Exception for Edge | |
|----|----|----|----|----|----|----|
| | | | **Basic Type** | **High Performance** | **Basic Type** | **High Performance** |
| infra.stg.cfg.003 | Storage with replication |  | N | Y | N | Optional |
| infra.stg.cfg.004 | Storage with encryption |  | Y | Y | N | Optional |
| infra.hw.cpu.cfg.001 | Minimum Number of CPU sockets | This determines the minimum number of CPU sockets within each host | 2 | 2 | 1 | 1 |
| infra.hw.cpu.cfg.002 | Minimum Number of cores per CPU | This determines the number of cores needed per CPU. | 20 | 20 | 1 | 1 |
| infra.hw.cpu.cfg.003 | NUMA alignment | NUMA alignment support and BIOS configured to enable NUMA | N | Y | N | Y<sup>*</sup> |

<sup>*</sup> immaterial if the number of CPU sockets (infra.hw.cpu.cfg.001) is 1

Please note that none of the listed parameters form part of a typical OpenStack flavour except that the vCPU and memory requirements of a flavour cannot exceed the available hardware capacity.

<a name="8.3.4"></a>
### 8.3.4  Telco Edge Cloud: Platform Services Deployment
This section characterises the hardware capabilities for different edge deployments and the Platform services that run on the infrastructure. Please note, that the Platform services are containerised to save resources, and benefit from intrinsic availability and auto-scaling capabilities.

**Table 8-4. Characteristics of Infrastructure nodes**

| | Platform Services | | | | | | | | Storage | | | | Network Services | | |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|  | Identity | Image | Placement | Compute | Networking | Message Queue | DB Server | | Ephemeral | Persistent Block | Persistent Object | | Management | Underlay (Provider) | Overlay |
| Control Nodes | &#9989; | &#9989; | &#9989; | &#9989; | &#9989; | &#9989; | &#9989; | | | &#9989; | | | &#9989; | &#9989; | &#9989; |
| Workload Nodes<br>(Compute) |  |  |  | &#9989; | &#9989; |  |  | | &#9989; | &#9989; | &#9989; | | &#9989; | &#9989; | &#9989; |
| Storage Nodes |  |  |  |  |  |  |  | | | &#9989; | &#9989; | | &#9989; | &#9989; | &#9989; |


Depending on the facility capabilities, deployments at the edge may be similar to one of the following:
- Small footprint edge device
- Single server: deploy multiple (one or more) workloads
- Single server: single Controller and multiple (one or more) workloads
- HA at edge (at least 2 edge servers): Multiple Controller and multiple workloads


<a name="8.3.5"></a>
### 8.3.5 Comparison of Deployment Topologies and Edge terms


**Table 8-5. Comparison of Deployment Topologies**

| This Specification | Compute | Storage | Networking | RTT | Security | Scalability | Elasticity | Resiliency | Preferred Workload Architecture | Upgrades |  | OpenStack | OPNFV Edge | Edge Glossary | GSMA |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Regional Data Centre (DC)<br><br>Fixed | 1000's<br><br>Standardised<br><br>>1 CPU<br><br>>20 cores/CPU | 10's EB<br><br>Standardised<br><br>HDD and NVMe<br><br>Permanence | >100 Gbps<br><br>Standardised | ~100 ms | Highly Secure | Horizontal and unlimited scaling | Rapid spin up and down | Infrastructure architected for resiliency<br><br>Redundancy for FT and HA | Microservices based<br><br>Stateless<br><br>Hosted on Containers | HW Refresh: ? <br><br>Firmware: When required<br><br>Platform SW: CD |  | Central Data Centre |  |  |  
| Metro Data Centres<br>Fixed | 10's to 100's<br><br>Standardised<br><br>>1 CPU<br><br>>20 cores/CPU | 100's PB<br><br>Standardised<br><br>NVMe on PCIe<br><br>Permanence | > 100 Gbps<br><br>Standardised | ~10 ms | Highly Secure | Horizontal but limited scaling | Rapid spin up and down | Infrastructure architected for some level of resiliency<br><br>Redundancy for limited FT and HA | Microservices based<br><br>Stateless<br><br>Hosted on Containers | HW Refresh: ? <br><br>Firmware: When required<br><br>Platform SW: CD |  | Edge Site | Large Edge | Aggregation Edge |  
| Edge<br>Fixed / Mobile | 10's<br><br>Some Variability<br><br>>=1 CPU<br><br>>10 cores/CPU | 100 TB<br><br>Standardised<br><br>NVMe on PCIe<br><br>Permanence / Ephemeral | 50 Gbps<br><br>Standardised | ~5 ms | Low Level of Trust | Horizontal but highly constrained scaling, if any | Rapid spin up (when possible) and down | Applications designed for resiliency against infra failures<br><br>No or highly limited redundancy | Microservices based<br><br>Stateless<br><br>Hosted on Containers | HW Refresh: ? <br><br>Firmware: When required<br><br>Platform SW: CD |  | Far Edge Site | Medium Edge | Access Edge / Aggregation Edge |  
| Mini-/Micro-Edge<br>Mobile / Fixed | 1's<br><br>High Variability<br><br>Harsh Environments<br><br>1 CPU<br><br>>2 cores/CPU | 10's GB<br><br>NVMe<br><br>Ephemeral<br><br>Caching | 10 Gbps<br><br>Connectivity not Guaranteed | <2 ms<br><br>Located in network proximity of EUD/IoT | Untrusted | Limited Vertical Scaling (resizing) | Constrained | Applications designed for resiliency against infra failures<br><br>No or highly limited redundancy | Microservices based or monolithic<br><br>Stateless or Stateful<br><br>Hosted on Containers or VMs<br><br><br>Subject to QoS, adaptive to resource availability, viz. reduce resource consumption as they saturate | HW Refresh: ? <br>Firmware: ? <br><br>Platform SW: ? |  | Fog Computing (Mostly deprecated terminology)<br><br>Extreme Edge<br><br>Far Edge | Small Edge | Access Edge |
