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
  org: Huawei Technologies France
  country: France
- name: Amine Lamouchi
  org: Huawei Technologies France
  country: France
- name: Jun Zhang
  org: Huawei Technologies France
  country: France
- name: Andrzej Duda
  org: Grenoble INP - Ensimag, LIG Lab
  country: France
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

This document proposes an extension to the Remote Attestation Procedures architecture as defined in {{-RATS-ARCH}} by introducing the concept of Attester Groups.
This extension aims to reduce computational and communication overhead by enabling collective attestation of devices with similar characteristics, thereby improving the scalability of attestation processes.

--- middle

# Introduction

{{-RATS-ARCH}} defines Attesters as entities comprising at least one Attesting Environment and one Target Environment.
It also introduces configurations, such as Composite Devices and Layered Attesters.
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

An Attester Group is inherently a dynamic entity.
Attesters can join or leave the group, in contrast to Composite Devices that have a static composition with a pre-defined set of Attesting Environments and fixed parameters.
The dynamic nature of an Attester Group allows for the flexibility to tailor group parameters, such as the number of Lead Attesters in the group (if any), the range of devices included in the group, and which or how much Evidence is expected to be produced by each groups.
This kind of flexibility facilitates the implementation of various group aggregation schemes that can optimize the resources required to conduct remote attestation procedures for large device groups.
The table below summarizes the key differences between the Group Attester concept and the Composite Device concept.



| Feature | Composite Device | Attester Group |
| Evidence Submission | One Evidence per composite device | More granular, e.g., one Evidence per two members |
| Lead Attester | One Lead Attester communicates with Verifier | Multiple or no lead Attesters (i.e., distributed collection) |
| Identity | Identifiably by its lead Attester | Identified by unique group-id, independent of lead Attester |
| Flexibility & Dynamics | Static, with pre-defined members and roles | Dynamic, where members can join or leave |

# IANA Considerations

This document has no IANA actions

--- back

# Implementation Considerations

Details on creating and maintaining Attester Groups, choosing the number of Lead Attesters, and methods for evidence collection and signing are left to the implementer's discretion, allowing for tailored security measures.
