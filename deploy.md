---

copyright:
  years: 2026
lastupdated: "2026-03-30"

keywords:

subcollection: pattern-pvs-ibmi-resiliency

---

{{site.data.keyword.attribute-definition-list}}


# Deploying Resilient IBM i workloads on {{site.data.keyword.powerSys_notm}}
{: #deploy}

This deployment guide outlines steps required to deploy IBM i resilient environments in IBM Cloud PowerVS.

This is written for customers who need a resilient solution for their IBM i LPARs in a multizone and multiregional layout. It details several different resiliency scenarios that can be deployed in PowerVS to protect business critical workloads running on IBM i.
This guide only includes Operating system related resiliency methods and does not go into application or database layers.

The guide details how to deploy two different High availbility options:
- High Availability within a single Availability Zone (AZ)
- High Availability between two Availability Zones

And how to deploy two different Disaster Recovery options:
- Disaster Recovery with Global Replication Services (GRS)
- Disaster Recovery with Global Replication Services and Disaster Recovery Automation (DRA)

## Before you begin
{: #pvs-prereqs}

Before we begin, make sure that the following resources are in places as prerequisites.

### Prerequisites
{: #pvs-prereqs}

    Configure VPN (site-to-site or client-to-site) to reach the subnets of the PowerVS workspaces

#### Prerequisites for High Availability with PowerHA:
{: #ha-prereqs}

- Two PowerVS workspaces, both placed in one region but in different AZs
- PowerHA standard edition for local AZ High availability
- PowerHA enhanced edition with Geomirror for High availability between AZs
- One Local transit gateway to connect the two AZs together

#### Prerequisites for Disaster Recovery with GRS:
{: #grs-prereqs}

- Two PowerVS workspaces, one in the "main" Cloud datacenter and one in the "secondary". Make sure they are selected based on the available GRS pairs. GRS pairs available in IBM Cloud: [Global Replication Services](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-getting-started-GRS#locations-GRS)
- One Global Transit Gateway to connect the two regions together.
- IBM Cloud CLI setup [Setup IBM Cloud CLI](https://cloud.ibm.com/docs/cli?topic=cli-getting-started)

#### Prerequisites for Global Replication services with Disaster Recovery Automation:
{: #prereqs-grs-dra}

Follow the steps in: [Disaster Recovery Automation](https://cloud.ibm.com/docs/dr-automation-powervs?topic=dr-automation-powervs-getting-started#prereqs)
We do not need the VPC schematics ID as we are going to use our own VPC with its subnet configured to a public gateway.

- Two PowerVS workspaces, one in the "main" Cloud data center and one in the "secondary". Make sure they are selected based on the available GRS pairs.
- One Global Transit Gateway to connect the two regions together.
- Two VPCs placed in each data center for the public gateways that will be used by the ksys orchestrator LPARs.
- API key for the cloud account user who will deploy the DRA.


### Deploying IBM i High Availability with Standard PowerHA in a single Availability Zone
{: #deployment-steps}

    The following steps will help you deploy an IBM i PowerHA cluster with shared storage. The two nodes are placed into the same AZ but different Power Servers. The guide only describes the configuration in IBM Cloud, it does not detail PowerHA configuration on IBM i.


1. Create a PowerVS workspace within a single Availability Zone

    - Make sure that the workspace is PER enabled.

2. Create Server Placement Groups

    - Go into the Power Virtual Server workspace
    - Select Virtual Server instances
    - Select Server Placement Groups and Create Group
    - Make sure that the Colocation Policy is set to "Different Server"

3. Create Two IBM i LPARs using the Colocation Policy and configure them for PowerHA clustering.

    - Make sure that PowerHA is selected at the IBM i Licenses when creating the IBM i LPARs.
    - Create shared storage volumes, that will be placed in IASP.
    - In the Create Volume menu, make sure that the Shareable flag is ON.
    - Attach the storage volumes to the IBM i LPARs

4. Summary:

    - We have now two IBM i LPARs created on two different Power servers, with Shared storage.

### Deploying IBM i High Availability with Enhanced edition PowerHA with GeoMirror
{: #deploy-ha-geomirror}

The following steps will help you deploy an IBM i PowerHA cluster with GeoMirror, syncing data across two Availability Zones. The guide only describes the configuration in IBM Cloud, it does not detail PowerHA configuration on IBM i.

1. Create two PowerVS workspaces, each placed in one Availability Zone. Make sure that the workspaces are PER enabled.

2. Create a Local Transit Gateway and add the two PowerVS workspaces to it.

3. Create two IBM i LPARs in each PowerVS workspace with PowerHA license selected during LPAR creation. By selecting the PowerHA license the image will come with installed PowerHA SystemMirror for i.

4. Create a cluster with PowerHA software and configure IASP with GeoMirror.

Note that the two nodes will have different IP subnets and different service IP addresses. This is because we need to connect the two with Transit Gateway and we cannot use the same IP subnets on the TGW.

### Deploying IBM i Disaster Recovery configuration with GRS and PowerHA SystemMirror for i.
{: #deploy-dr}

The following steps will help you deploy IBM i systems with GRS enabled storage volumes, syncing data across Regions. The GRS disks will be handled by PowerHA. Please note that Global Replication Services is as an asynchronous replication, therefore some data loss always excepted during failover.

1. Select two Regions that has GRS enabled in between.

2. Create two PowerVS workspaces within those Regions.

3. Deploy two IBM i LPARs (a primary and a secondary) within each Regions' PowerVS workspace. Make sure that the boot disk of the primary IBM i is NOT set to GRS enabled during instance creation. Only the data disks need to be GRS enabled. The secondary IBM i will need the auxiliary (secondary) volumes attached.

4. Create GRS Volume Replication Group (VRG) under the primary PowerVS workspace Storage volumes menu (there can be more than one VRG created if needed). The GRS enabled storage volumes are going to be placed into the VRG after they get attached to the IBM i LPAR.

5. Create GRS enabled storage volumes at the primary region and attach them to the primary IBM i LPAR. Make sure the GRS enabled storage volumes are attached to the IBM i LPAR before attempting to add it to the VRG. After the GRS enabled storage volume is on the IBM i LPAR it will show "Replication status" - Action Required.

6. Add the GRS enabled storage volumes at the primary region to the VRG created at step 4.

7. Wait until the VRG state becomes "Consistent copying"

8. The GRS secondary storage volume, called auxiliary volumes, are now created at the secondary region, however they cannot be seen under the secondary PowerVS workspace until they get "Onboarded". To "Onboard" an auxiliary storage volume the user need IBM Cloud CLI. Use [Onboard Auxiliary volumes](https://cloud.ibm.com/docs/power-iaas?topic=power-iaas-getting-started-GRS#onboarding-an-auxiliary-volume) to make the auxiliary volumes available at the secondary site.

9. Once the Auxiliary volumes can be seen at the secondary PowerVS workspace, they can be added to the secondary IBM i LPAR. The data volumes must be attached to the secondary IBM i LPAR.

10. Once both IBM i nodes see their respective volumes create the PowerHA cluster.

11. Create and varyon IASP on the primary node

12. Create Cluster Resource Group on the primary node and add configuration of the workspaces (CRN) and private endpoints of PowerVS service and IAM.

13. Add copy description to the GRSed volumes and add the volumes to the created copy descriptions (this is the same as we would do for GeoMirror).

14. As a last step configure GRS on primary IBM i node.

### Deploying IBM i Disaster Recovery configuration with Disaster Recovery Automation
{: #deploy-dra}

The following steps will help you deploy IBM i systems with GRS enabled storage volumes, syncing data across Regions. Please note that Global Replication Services is as an asynchronous replication, therefore some data loss is always expected after a failover. We are going to use Disaster Recovery Automation to automate the failover and fallback operations. This deployment guide will only describe the configuration without ksys High Availability.

1. Create two PowerVS workspaces in two different Regions, that are GRS replication pairs.

2. Create a Global Transit Gateway and add the primary PowerVS workspace to it. Do not add the secondary PowerVS workspace as it will be done when we deploy the Disaster Recovery Automation from the IBM Cloud Catalog.

3. Create a VPC in the secondary Region. The VPC will be used to place our Public Gateway Virtual Machine. The Public Gateway is going to allow the ksys orchestrator LPAR to reach IBM Cloud Services. Follow the steps to configure Public Gateway: [Enable Communication via VPC](https://cloud.ibm.com/docs/dr-automation-powervs?topic=dr-automation-powervs-idep-the-orch#procedure-ena-ppro-comm)

4. Deploy the Disaster Recovery Automation from the IBM Cloud Catalog. Search for Power Virtual Server DR Automation "Select a location" should be set to Global, Global is the only location that allows us to select PowerVS DR Automation Pricing Plan. Do not go with the private one as that is for between on-prem and IBM Cloud. Add a name to the service and a resource group.

5. Once the service is created for the DRA we will be able to see it under the IBM Cloud Account's Resource List menu. Click on the service and a setup menu will appear for the initial configuration of the DRA.

6. Guidance to fill out the configuration menu:

    | Question | Value |
    | -------- | ------|
    | DR Orchestrator name | Can be anything
    |DR Orchestrator password | Is the password for root user(will be used to login to the orchestrator interface from the internet) |
    | IBM Cloud API key | The API key for the cloud user. |
    | DR location | The location of the DR PowerVS workspace (where we want to restore our LPAR). The ksys orchestrator will be placed there as well. |
    |Custom VPC | Use manually created proxy VSI|
    |Proxy details | Use the Private IP address of the proxy server and the squid port, eg.: 10.83.64.5:3128 |
    | DR Power Virtual Server workspace |The name of the secondary PVS workspace, that resides in the DR location.|
    |Use a secret | Untick the box and select the ssh key name for ssh key. |
    |Storage tier | Tier for the ksys orchestrator to run on. |
    |Machine type | What type of power machine will host the ksys orchestrator  |
   {: caption="Guidance for configuration menu" caption-side="bottom"}

7. Reach the ksys orchestrator via web browser to configure the DRA:

    https://private_IP_of_ksys_orchestrator:3000

8. Add Sites to the configuration, again, they must be GRS enabled regions. ![DRA_site_configuration](images/DRA_Site_Config.svg "DRA Site Configuration"){: caption="Figure 1: Configure Sites with DRA" caption-side="bottom"}{: external download="DRA_Site_Config.svg"}

9. Manage VM
- Create New Session
- Select the Primary Logical Partition (VM) on the left side
- Select the target PowerVS workspace on the right side
- Name your Work Group at Work Group Name
- Save the settings
- This will create a WorkGroup for the LPAR. With DRA, only Work groups can be moved between sites instead of individual LPARs. ![DRA_site_configuration](images/Manage_VM.svg "Manage VM"){: caption="Figure 2: Manage VM" caption-side="bottom"}{: external download="Manage_VM.svg"}

10. After checking the summary -> Submit & Deploy

11. Failover to the secondary site
- Select the work group that contains the primary LPAR
- Select Move Work Group at the top left ![LPAR_Failover](images/LPAR_Failover.svg "LPAR Failover"){: caption="Figure 3: LPAR_Failover" caption-side="bottom"}{: external download="LPAR_Failover.svg"}

12. Check the events logged in the ksys orchestrator web page, additionally the progression can be verified by checking the secondary site's PowerVS workspace.

### Restore IBM i using falconstor VTL
{: #restore-ibmi}

    This part focuses only on the Operating system and does not detail application or database file restore.

In IBM Cloud there are several supported ways to restore an IBM i Operating system.

1. Deploy falconstor storsafe on both on-prem and in IBM Cloud. From the catalog both versions can be provisioned.

2. Create two IBM i LPARs in IBM Cloud PowerVS (one COR image and one standard) and assign Virtual Tape Libraries to both on-prem IBM i and COR ibm i (the standard, third IBM i does not need VTLs to be attached). The COR ibm i server is going to be used to access the transfered IBM i image through VTL and to allow that transfered IBM i image to be used as network install source to restore the third IBM i LPAR.

3. Save IBM i image onto on-prem falconstor storsafe and replicate the saved and deduplicated image into the falconstor storsafe instance in IBM Cloud.

4. Use the transfered IBM i image through the COR IBM i server to restore the third IBM i in PowerVS. Falconstor can provide scripts to make the whole procedure (save and restore) easier.
