---
v: 3

title: 'Attester Groups for Remote Attestation'
abbrev: Attester Groups
docname: draft-labiod-rats-attester-groups-latest
area: Security
wg: RATS
kw: Internet-Draft
cat: std
stream: IETF

author:
- name: Houda Labiod
  org: Huawei Technologies France S.A.S.U.
  email: houda.labiod@huawei.com
  street: 18, Quai du Point du Jour
  code: '92100'
  city: Boulogne-Billancourt
  country: France
- name: Amine Lamouchi
  org: Huawei Technologies France S.A.S.U.
  country: France
  email:aminelamouchi@huawei-partners.com
- name: Jun Zhang
  org: Huawei Technologies France S.A.S.U.
  email: junzhang1@huawei.com
  street: 18, Quai du Point du Jour
  code: '92100'
  city: Boulogne-Billancourt
  country: France
- name: Andrzej Duda
  org: Grenoble INP - Ensimag, LIG Lab
  country: France
  email: Andrzej.Duda@imag.fr
- name: Henk Birkholz
  org: Fraunhofer SIT
  abbrev: Fraunhofer SIT
  email: henk.birkholz@sit.fraunhofer.de
  street: Rheinstrasse 75
  code: '64295'
  city: Darmstadt
  country: Germany

normative:
  I-D.ietf-rats-ar4si: RATS-AR4SI

informative:
  RFC9334: RATS-ARCH
  I-D.ietf-rats-daa: RATS-DAA

entity:
  SELF: "RFCthis"

--- abstract

This document proposes an extension to the Remote Attestation 
Procedures architecture by introducing the 
concept of Attester Groups. This extension aims to reduce computational 
and communication overhead by enabling collective Evidence appraisal
of high number of homogeneous devices with similar characteristics, thereby improving the scalability 
of attestation processes.

--- middle

# Introduction

{{-RATS-ARCH}} defines Attesters as entities comprising at least one Attesting Environment and one Target Environment
co-located in one entity. It also presents different ways to compose the Attesting and Target Environemtns, 
such as Composite Devices and Layered Attesters. Layed Attester reflects a cascade of staged Environments. It
is more related to one device with different layers and there is a relationship between them. 
However, mechanisms for efficiently managing multiple, independent Attesters are missing.
Assessing the trustworthiness of large numbers of independent devices individually can result in high conveyance and processing overhead.
This comes into effect particularly when these devices share identical hardware or firmware components, which can lead to redundancy between all individual remote attestation procedures.
One example would be a smart factory scenario where numerous sensors of the same model monitor different parts of the manufacturing process.
These sensors share identical hardware and firmware configurations.
This document proposes a model by which these separate sensors devices can be grouped into a single Attester Group and a shared remote attestation procedure can appraise their authenticity collectively rather than individually.
Direct Anonymous Attestation (DAA) {{-RATS-DAA}} has a similar concept of using one unique ID for one group of Attesters, but its goal is to mitigate the issue of uniquely (re-)identifiable Attesting Environments, while scalability is the major concern in this document.

# Terminology

The following terms are imported from {{-RATS-ARCH}}: Attester, Composite Device, Evidence, Layered Attester, Verifier.

Newly defined terms for this document:

Attester Group:

: A role performed by a group of Attesters whose Evidence must be appraised in order to infer the extent to which the individual Attesters comprising the group are considered trustworthy.

group-id:

: A new Attester Identity type (see {{Section 2.2.1. of -RATS-AR4SI}}).
It is a unique identifier assigned to each Attester Group, allowing the group to dynamically adjust its membership without redefining its fundamental identity.

## Requirements Notation

{::boilerplate bcp14-tagged}

# Attester Group and Comparison to Composite Devices

We should be able to leverage the similarities between attesters to avoid redundant attestations.
An Attester Group is by definition a dynamic entity.
Attesters can join or leave the group, in contrast to Composite Devices that have a static composition with a pre-defined set of Attesting Environments and fixed parameters.
The dynamic nature of an Attester Group allows for the flexibility to tailor group parameters.
This kind of flexibility facilitates the implementation of various group attestation schemes that can optimize the resources required to conduct remote attestation procedures for large device groups.
 A composite device is an entity composed of
multiple sub entities. Each sub entity is an Attester. In a composite device we can have multiple Attesters with a Lead
Attester. The Attesters are appraised via the main Lead Attester's help. 
The lead Attester generates Evidence about the layout of the whole composite device, while sub-Attesters generate Evidence about their respective (sub-)modules. 
Composite device model is not enough flexible to  represent our definition of attester group where we do need a leader attester nor a composition of evidences of the attesters. 

The table below summarizes the key differences between the Group Attester concept and the Composite Device concept.



| Composite Device | Attester Group |
| Lead Attester | No Lead Attester |
| The Composite Device is identifiable by the Lead Attester | The Attester Group is identifiable by a group-id a unique identifier|  
| Composition of Evidence of sub-modules (attesters) | No composition |

# Attester Group Extension

In Section 3 (Architectural Overview) of {{-RATS-ARCH}}: we could add a subsection 3.4 titled "Attester Groups". In addidion, Section 2.2 (Non-repudiable Identitythe) of the draft {{-RATS-AR4SI}}, 
we could add an Identity Type "group-id" (i.e add another row in the Table 1 in {{-RATS-AR4SI}}).

# Use Case Scenarios with a large scale network

In this section, we provide two examples of applications where all devices are homogeneous with similar characteristics. 

Use Case 1: Remote maintenance in the aerospace domain  

Context: EU ASSURED H2020 Project.  
Once an aircraft lands, there is the need for the physical presence of an engineer to go and connect to the "head unit" (in the cockpit) for extracting log data so as to check whether something needs to be checked/maintained.
We need attestation of all core PLCs and embedded systems responsible for the core functionalities of the aircraft. All attestation reports are remotely sent (in a secure manner) to the control station once landed. We can group the attested elements into different attester groups.  

Approach: We can consider an attester group of 1000 aircrafts (same manufacturing brand)

Use Case 2: Automotive domain, a Vehicle with embedded Electronic Control Units (ECUs)  

Context: CONNECT EU H2020 project.  
The automotive industry is moving to a more hierarchical in-vehicle architecture where ECUs are monitored by Zonal Controllers and these in turn communicate with the Vehicle Computer. This is, for instance, how kinematic data are extracted from the sensors all the way up to the vehicle computer to be encoded into a V2X message. This data need to be associated with Evidence on the integrity of the sensor as a data source and this is where group attestation is an interesting capability. The attester group can be formed for hierarchical-based attestation,
like attester group of all in-vehicle ECUs or attested group of vehicles within an intersection.     

Approach: we can consider an attester group of a fleet of 70000 vehicles (same brand). We can also consider an attester group of similar ECUs. 

# Security Considerations

[TBD]

# IANA Considerations

This document has no IANA actions

--- back

# Implementation Considerations

Details on creating and maintaining Attester Groups, choosing the number of Lead Attesters, and methods for evidence collection and signing are left to the implementer's discretion, allowing for tailored security measures.
