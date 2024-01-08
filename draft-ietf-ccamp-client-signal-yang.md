---
title: "A YANG Data Model for Transport Network Client Signals"
abbrev: "Client Signals YANG Model"
category: std

docname: draft-ietf-ccamp-client-signal-yang-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
area: "Routing"
workgroup: "CCAMP Working Group"
keyword:
 - next generation
 - unicorn
 - sparkling distributed ledger
venue:
  group: "Common Control and Measurement Plane"
  type: "Working Group"
  mail: "ccamp@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/ccamp/"
  github: "ietf-ccamp-wg/draft-ietf-ccamp-client-signal-yang"
  latest: "https://ietf-ccamp-wg.github.io/draft-ietf-ccamp-client-signal-yang/draft-ietf-ccamp-client-signal-yang.html"

author:
  -
    name: Haomian Zheng
    org: Huawei Technologies
    street: H1 XiliuBeipo Village, Songshan Lake
    city: Dongguan
    region: Guangdong
    code:
    country: China
    email: zhenghaomian@huawei.com
  -
    name: Aihua Guo
    org: Futurewei
    email: aihuaguo.ietf@gmail.com
  -
    name: Italo Busi
    org: Huawei Technologies
    email: italo.busi@huawei.com
  -
    name: Anton Snitser
    org: Cisco
    email: asnizar@cisco.com
  -
    name: Francesco Lazzeri
    org: Ericsson
    email: francesco.lazzeri@ericsson.com
  -
    name: Chaode Yu
    org: Huawei Technologies
    email: yuchaode@huawei.com

contributor:
  -
    name: Yunbin Xu
    org: CAICT
    email: xuyunbin@caict.ac.cn
  -
    name: Yang Zhao
    org: China Mobile
    email: zhaoyangyjy@chinamobile.com
  -
    name: Xufeng Liu
    org: Alef Edge
    email: xufeng.liu.ietf@gmail.com
  -
    name: TBD
  -
    name: Giuseppe Fioccola
    org: Huawei Technologies
    email: giuseppe.fioccola@huawei.com
  -
    name: Zhe Liu
    org: Huawei Technologies
    email: liuzhe123@huawei.com
  -
    name: Sergio Belotti
    org: Nokia
    email: sergio.belotti@nokia.com
  -
    name: Yingxi Yao
    org: Shanghai Bell
    email: yingxi.yao@nokia-sbell.com
  -
    name: Henry Yu
    org: Huawei Technologies
    email: henry.yu1@huawei.com

normative:
  ITU-T_G.875:
    title: >
        Optical transport network: Protocol-neutral management
        information model for the network element view
    author:
      org: International Telecommunication Union
    date: June 2020
    seriesinfo: ITU-T G.875

informative:
  IEEE_802.1ad:
    title: >
        IEEE Standard for Local and Metropolitan Area Networks -
        Virtual Bridged Local Area Networks -
        Amendment 4: Provider Bridges
    author:
      org: Institute of Electrical and Electronics Engineers
    date: December 2005
    seriesinfo: IEEE 802.1ad-2005
  IEEE_802.1q:
    title: >
        IEEE Standard for Local and Metropolitan Area Networks -
        Bridges and Bridged Networks
    author:
    date: December 2022
    seriesinfo: IEEE 802.1Q-2022
  MEF10:
    title: Ethernet Services Attributes Phase 1
    author:
      org: MEF Forum
    date: November 2004
    seriesinfo: MEF 10
    target: https://www.mef.net/Assets/Technical_Specifications/PDF/MEF_10.pdf

--- abstract

A transport network is a server-layer network to provide connectivity
services to its client.  The topology and tunnel information in the
transport layer has already been defined by generic Traffic-
engineered models and technology-specific models (e.g., OTN, WSON).
However, how the client signals are accessing to the network has not
been described.  These information is necessary to both client and
provider.

This draft describes how the client signals are carried over
transport network and defines YANG data models which are required
during configuration procedure.  More specifically, several client
signal (of transport network) models including ETH, STM-n, FC and so
on, are defined in this draft.

--- middle

# Introduction

## Overview

A transport network is a server-layer network designed to provide
connectivity services for a client-layer network to carry the client
traffic transparently across the server-layer network resources.
Currently the topology and tunnel models have been defined for
transport networks, including {{!I-D.ietf-ccamp-otn-topo-yang}} and
{{!I-D.ietf-ccamp-otn-tunnel-model}}, providing server-layer topology
abstraction and tunnel configuration between PEs.  However, there is
a missing piece for configuring how the PEs should map the client-
layer traffic, received from the CE, over the server-layer tunnels:
this gap is expected to be solved in this document.

This document defines a data model of all transport network client
signals, using YANG language defined in {{!RFC7950}}.  The model can be
used by applications exposing to a transport network controller via a
RESTconf interface.  Furthermore, it can be used by an application
for the following purposes (but not limited to):

- To request/update an end-to-end service by driving a new tunnel to
be set up to support this service;

- To request/update an end-to-end service by using an existing
tunnel;

- To receive notification with regard to the information change of
the given service;

The YANG modules defined in this document conforms to the Network
Management Datastore Architecture (NMDA) defined in {{?RFC8342}}.

# Conventions and Definitions

## Prefixes in Model Names

In this document, names of data nodes and other data model objects
are prefixed using the standard prefix associated with the
corresponding YANG imported modules, including {{!RFC6991}}, {{!RFC8294}}
and {{!I-D.ietf-ccamp-otn-tunnel-model}}, which are shown as follow.

| Prefix      | YANG module               | Reference
| yang        | ietf-yang-types           | {{!RFC6991}}
| te-types    | ietf-te-types             | \[RFC YYYY]
| rt-types    | ietf-routing-types        | {{!RFC8294}}
| l1-types    | ietf-layer1-types         | \[RFC ZZZZ]
| etht-types  | ietf-eth-tran-types       | RFC XXXX
| clntsvc     | ietf-trans-client-service | RFC XXXX
| ethtsvc     | ietf-eth-tran-service     | RFC XXXX
|clntsvc-types|ietf-trans-client-svc-types| RFC XXXX
{: #tab-prefixes title="Prefixes and corresponding YANG modules"}

RFC Editor Note: Please replace XXXX with the number assigned to the
RFC once this draft becomes an RFC.  Please replace YYYY with the RFC
numbers assigned to {{!I-D.ietf-teas-rfc8776-update}}.  Please replace
ZZZZ with the RFC numbers assigned to {{!I-D.ietf-ccamp-layer1-types}}.

## Terminology and Notations

A simplified graphical representation of the data model is used in
this document.  The meaning of the symbols in the YANG data tree
presented later in this document is defined in {{?RFC8340}}.  They are
provided below for reference.

- Brackets "\[" and "]" enclose list keys.

- Abbreviations before data node names: "rw" means configuration
(read-write) and "ro" state data (read-only).

- Symbols after data node names: "?" means an optional node, "!"
means a presence container, and "*" denotes a list and leaf-list.

- Parentheses enclose choice and case nodes, and case nodes are also
marked with a colon (":").

- Ellipsis ("...") stands for contents of subtrees that are not
shown.

# Transport Network Client Signal Overview

## Overview of Service Request and Network Configuration Scenarios

A global view of a multi-domain service can be described as the
{{fig-overview}}. The customer is usually responsible to configure the CE
nodes and to request to the provider the service intent, from the CE
nodes perspective, while the provider is responsible to configure the
whole network (including the PE nodes) to support the customer
service intent.  Generally speaking, the network configurations
required to support a customer service can be split into two
different groups: CE-PE and PE-PE.  The CE-PE configuration deals
with the client layer one-hop access link, while PE-PE configuration
deals with the server layer tunnel.  In {{fig-overview}} we mark the
intermediate nodes as 'P', which has same switching capability of PE
but just not the 'end-point'.  In this example, the link P-P and PE-P
are a server-layer intra-domain or inter-domain link.

~~~~ ascii-art
         +----+                                            +----+
         | CE |                                            | CE |
         +--+-+                                            +--+-+
            |                                                 |
            | -------------                      -------------|
          //|/             \\\\             /////             |\\\\
        //  |                  \\         //                  |    \\
       | +--+-+    +---+  +---+  |       | +---+   +---+   +--+-+    |
      |  | PE +----+ P +--+ P +---+-----+--+ P +---+ P +---+ PE |     |
       | +----+    +---+  +---+  |       | +---+   +---+   +----+    |
        \\                     //         \\                       //
          \\\\             ////             \\\\\             /////
              -------------                      -------------
                 Domain 1                         Domain 2
~~~~
{: #fig-overview title="Global view of Client Service with the Network Provider"}

According to the responsibilities of each controller in {{?RFC8453}},
the controllers have different views of the service request and
network configuration.  The duty of CNC is to give the MDSC a
description of the customer service intent: candidate YANG models
include L1CSM {{!I-D.ietf-ccamp-l1csm-yang}}, L2SM {{?RFC8466}} and L3SM
{{?RFC8299}}, which are classified as customer service models, according
to {{?RFC8309}}.  These models provide necessary attributes to describe
the customer service intent from the customer/CE perspective, and do
not provide any specific network configuration.  These models also
implies that the customer service description can be considered in a
separate manner rather than integratig with network configurations,
which also enable the controllers to abstract/virtualize the network
resource to make them visible to the customer and also easier to
manage.  In other words, the network knowledge is not necessary at
CNC and CMI, which is seen in an abstracted form as shown in
{{fig-cnc-view}}.

~~~~ ascii-art
                              /---------\
                           ///           \\\
     +----+               |                 |                 +----+
     | CE |--------------+      NETWORK      +----------------| CE |
     +----+               |                 |                 +----+
                           \\\           ///
                              \---------/
~~~~
{: #fig-cnc-view title="CNC Viewpoint on the Client Service"}

The functionalities of MDSC have been described in {{?RFC8453}}, which
include the customer mapping/translation and multi-domain
coordination.  By receiving the request from CNC, MDSC need to
understand what network configuration can support the customer
service intent and turn to the corresponding PNCs for configuration.
The service request is therefore decomposed by MDSC into a few
network configurations and forwarded to one or multiple PNCs
respectively in single-domain and multi-domain scenario.  In general,
the MDSC has the view of both PE and CE nodes and of some abstract
information regarding the P nodes, as shown in {{fig-mdsc-view}}.  It is worth
noting that this MDSC view is different with {{fig-overview}} at the intra-
domain link.  Usually these details are hidden, for scalability
purposes, and therefore the MDSC has only an abstract view of each
domain internal topology.

~~~~ ascii-art
                       ------                -----
                   ////      \\\\        ///-     -\\\
                 //              \\    //             \\
     +----+     | +----+     +---+ |  |+---+     +----+ |     +----+
     | CE |-----+-| PE |-----| P |-+--+| P |-----| PE |-+-----| CE |
     +----+     | +----+     +---+ |  |+---+     +----+ |     +----+
                 \\              //    \\             //
                   \\\\      ////        \\\-     -///
                       ------                -----
                      Domain 1             Domain 2
~~~~
{: #fig-mdsc-view title="MDSC view of both Client Service and Network Abstraction"}

PNC is the controller that configure the physical devices, based on
the network configuration received from the MDSC.  Each PNC has the
detailed view of its own domain, the example of view from PNC in
domain 1 is shown in {{fig-pnc-view}}.  The PNC has all the detailed topology
information on PE and P nodes and on the intra-domain links.  The PNC
configures the tunnel/tunnel segment within its domain based on the
network configuration provided by the MDSC.  The PNC also configures
the network part of the CE-PE access links as well as the mapping of
the client-layer traffic and the server-layer tunnels, based on the
network configuration provided by the MDSC.  The interaction between
PNC and MDSC for the client-layer network configuration is
accomplished by the models defined in this draft.

~~~~ ascii-art
            |                        |                        |
            | -------------          |           -------------|
          //|/             \\\\      |      /////             |\\\\
        //  |                  \\    |    //                  |    \\
       | +--+-+    +---+  +---+  |   |   | +---+   +---+   +--+-+    |
      |  | PE +----+ P +--+ P +---+--+--+--+ P +---+ P +---+ PE |     |
       | +----+    +---+  +---+  |   |   | +---+   +---+   +----+    |
        \\                     //    |    \\                       //
          \\\\             ////      |      \\\\\             /////
              -------------          |           -------------
          PNC View in Domain 1       |       PNC View in Domain 2
~~~~
{: #fig-pnc-view title="PNC view on Network Configuration"}

## Applicability of Proposed Model

Existing TE and technology-specific models, such as topology models
and tunnel models, support the network configuration among PEs and
Ps.  The customer service models, such as L1CSM, L2SM and L3SM, focus
on describing the attributes among CEs.  However, there is a missing
piece on how to configure the CE-PE session.  The models defined in
this document provide the configuration on CE-PE when the provider
server-layer network is TE-based technology.

In the example of OTN as the server-layer transport network, a full
list of G-PID was summarized in {{!RFC7139}}, which can be divided into
a few categories.  The G-PID signals can be categorized into
transparent and non-transparent.  Examples of transparent signals may
include Ethernetphysical interfaces, FC, STM-n and so on.  In this
approach the OTN devices is not aware of the client signal type, and
this information is only necessary among the controllers.  Once the
OTN tunnel is set up, there is no switching requested on the client
layer, and therefore only signal mapping is needed, without a client
tunnel set up.  The models that supporting the configuration of
transparent signals are defined in {{cbr-svc-tree}}.  The other category
would be non-transparent, such as Carrier Ethernet and MPLS-TP, with
a switching request on the client layer.  Once the OTN tunnel is set
up, a corresponding tunnel in the client layer has to be set up to
carry services.  The models that supporting the configuration of
transparent signals are defined in {{eth-svc-tree}}.

It is also worth noting that some client signal can be carried over
multiple types of networks.  For example, the Ethernet services can
be carried over either OTN or Ethernet TE tunnels (over optical or
microwave networks).  The model specified in this document allows the
support from networks with different technologies.  The list of
identities for client signals are defined in
{{!I-D.ietf-ccamp-layer1-types}}.

### Identifier and Readable Information

Name: The identifier of the client signal service can be configured
or retrieved by the name information.  The naming of this attribute
is based on the requirements of unifying with TE tunnel models.  It
can be readable or unreadable.  If it is readable, to make it unique,
it is needed to specify a unified structure for all the PNC systems.
It is hard to do that for the reason of a lot of customized
requirements from different Operators.  In the ICT technology, UUID
format could be a good option.  Title: The title of client signal
service can be configured with its readable name.  Description: More
detail information of client signal service, such as some
administrative information or location information.  User-label: An
alias of client signal service which is used to tag based on the
maintenance requirement.

~~~~ ascii-art
   module: ietf-trans-client-service
      +--rw client-svc
         +--rw client-svc-instances* [client-svc-name]
            +--rw client-svc-name           string
            +--rw client-svc-title?         string
            +--rw user-label?               string
            +--rw client-svc-descr?         string
            ...................................

   module: ietf-eth-tran-service
      +--rw etht-svc-instances* [etht-svc-name]
            +--rw etht-svc-name             string
            +--rw etht-svc-title?           string
            +--rw user-label?               string
            +--rw etht-svc-descr?           string
            ...................................
~~~~

### Unified Resource Identifiers

In {{!I-D.ietf-teas-rfc8776-update}}, it is recognized that if the
used topology resource adopts TE modeling, there could be two
identifier systems, one is the network modeling system and the other
one from TE topology modeling.  The access ports of client signal
service or end points of Ethernet service also adopt the TE topology
resources.  So two identifiers are both fine to specified in the
configuration requests.

~~~~ ascii-art
   module: ietf-trans-client-service
      +--rw client-svc
         +--rw client-svc-instances* [client-svc-name]
            +--rw src-access-ports
            |  +--rw access-node-id?    te-types:te-node-id
            |  +--rw access-node-uri?   nw:node-id
            |  +--rw access-ltp-id?     te-types:te-tp-id
            |  +--rw access-ltp-uri?    nt:tp-id
            |  +--rw client-signal?     identityref
            +--rw dst-access-ports
               +--rw access-node-id?    te-types:te-node-id
               +--rw access-node-uri?   nw:node-id
               +--rw access-ltp-id?     te-types:te-tp-id
               +--rw access-ltp-uri?    nt:tp-id
               +--rw client-signal?     identityref
               ...................................

   module: ietf-eth-tran-service
      +--rw etht-svc
         +--rw etht-svc-instances* [etht-svc-name]
            +--rw etht-svc-end-points* [etht-svc-end-point-name]
               +--rw etht-svc-access-points* [access-point-id]
                  +--rw access-point-id    string
                  +--rw access-node-id?    te-types:te-node-id
                  +--rw access-node-uri?   nw:node-id
                  +--rw access-ltp-id?     te-types:te-tp-id
                  +--rw access-ltp-uri?    nt:tp-id
                  +--rw access-role?       identityref
                  ...................................
~~~~

### Threshold Configuration

Some PM data are quite critical for client signal service users'
experience, e.g. latency value.  For the Operators, they can
configure a service-oriented threshold for the PM data, to make the
maintenance engineer recognize the network issues in time.

~~~~ ascii-art
   module: ietf-trans-client-service
      +--rw client-svc
         +--rw client-svc-instances* [client-svc-name]
            +--rw alarm-shreshold
               +--rw latency-threshold?   uint32
               ...................................

   module: ietf-eth-tran-service
      +--rw etht-svc
         +--rw etht-svc-instances* [etht-svc-name]
            +--rw alarm-shreshold
               +--rw latency-threshold?   uint32
               ...................................
~~~~

## State data of Services

### Generic Requirements

States have been defined to retrieve the status of the delivered
services.  A few generic states defined in {{!RFC8776}} are reused in
this document.  These states include the operational state and the
provisioning state.

~~~~ ascii-art
   module: ietf-trans-client-service
      +--rw client-svc
         +--rw client-svc-instances* [client-svc-name]
            +--ro operational-state?        identityref
            +--ro provisioning-state?       identityref
            ...................................

   module: ietf-eth-tran-service
      +--rw etht-svc
         +--rw etht-svc-instances* [etht-svc-name]
            +--ro state
               +--ro operational-state?    identityref
               +--ro provisioning-state?   identityref
               ...................................
~~~~

### Timestamp and Administrative information

A few other parameters are defined for the management of the
services.  Given the complicated labor division, the creation, update
and maintainance may have different responsible systems.  So the log
of the service would be helpful and should be easy to retrived in a
standard way as well.  These information include, but not restricted
to the following:

- When the service is created and has been last updated, these are
specified in the creation-time and last-updated-time.

- Who created the service and who made the last update to the
service, these are specified in the created-by and last-updated-
by.

- The owner of the service is specifed as owned-by, which would be
useful when the service is delegated by or to a specific system.
The identifier of the system is used to represent such
information.

~~~~ ascii-art
   module: ietf-trans-client-service
      +--rw client-svc
         +--rw client-svc-instances* [client-svc-name]
            +--ro creation-time?            yang:date-and-time
            +--ro last-updated-time?        yang:date-and-time
            +--ro created-by?               string
            +--ro last-updated-by?          string
            +--ro owned-by?                 string
            ...................................

   module: ietf-eth-tran-service
      +--rw etht-svc
         +--rw etht-svc-instances* [etht-svc-name]
            +--ro state
               +--ro creation-time?        yang:date-and-time
               +--ro last-updated-time?    yang:date-and-time
               +--ro created-by?           string
               +--ro last-updated-by?      string
               +--ro owned-by?             string
               ...................................
~~~~

### performance monitoring status

For the Operators, there are also some requirements to provide some
PM data to the service users.  Especially some PM data are quite
critical for users' experience.

For example, one of the great advantage of client signal service is
it can provide deterministic low latency.  Some industry users, e.g.
stock trading company, have great demand of stable short latency
service to ensure their high-frequency fast trading operations can be
finished quickly and definitely.  In addition to provision a low-
latency client signal service, Operators can also provide some other
differentiated services for these kinds of service users.  For
example latency visibility can be provided to the users, to let them
monitor the latency values.

The latency measurement mechanism can be referenced from sub-clause
7.10.1 of {{ITU-T_G.875}}.  And PNC can provide a measurement latency
after service is provisioned if the devices support this measurement
mechanism.  Otherwise providing an estimated value could be a
workaround solution.

~~~~ ascii-art
   module: ietf-trans-client-service
      +--rw client-svc
         +--rw client-svc-instances* [client-svc-name]
            +--ro pm-state
               +--ro latency?   uint32
               ...................................

   module: ietf-eth-tran-service
      +--rw etht-svc
         +--rw etht-svc-instances* [etht-svc-name]
            +--ro state
               +--ro pm-state
                  +--ro latency?   uint32
                  ...................................
~~~~

### Error Message Report

Once the processing of client signal service configuration, including
creation, updating, deletion, meets with some internal problems, such
as no enough available bandwidth, or wrong client signal mapping
.etc., the problems should be reported to the client system to make
the issues read by the maintenance engineers.  Since these error
messages are more technologies-specific, especially sometimes
accurate location information is needed in these messages, it is hard
to standardize these error messages by RESTCONF protocol errors.  For
the client systems, they would like to get these messages both from
notifications and retrieval.  So this document defines an error
container in the YANG data model to let the PNC to provide these
messages.

~~~~ ascii-art
   module: ietf-trans-client-service
      +--rw client-svc
         +--rw client-svc-instances* [client-svc-name]
            +--ro error-info
               +--ro error-code?          uint16
               +--ro error-description?   string
               +--ro error-timestamp?     yang:date-and-time
               ...................................

   module: ietf-eth-tran-service
      +--rw etht-svc
         +--rw etht-svc-instances* [etht-svc-name]
            +--rw etht-svc-end-points* [etht-svc-end-point-name]
            +--ro state
               +--ro error-info
                  +--ro error-code?          uint16
                  +--ro error-description?   string
                  +--ro error-timestamp?     yang:date-and-time
                  ...................................
~~~~

# YANG Model for Transport Network Client Signal

{: #eth-svc-tree}

## YANG Tree for Ethernet Service

~~~~ ascii-art
{::include ./ietf-eth-tran-service.tree}
~~~~
{: #fig-eth-svc-tree artwork-name="ietf-eth-tran-service.tree"}

{: #cbr-svc-tree}

## YANG Tree for other Transport Network Client Signal Model

~~~~ ascii-art
{::include ./ietf-trans-client-service.tree}
~~~~
{: #fig-cbr-svc-tree artwork-name="ietf-trans-client-service.tree"}

# YANG Code for Transport Network Client Signal

## The ETH Service YANG Code

This module imports typedefs and modules from {{!RFC6991}}, {{!RFC8294}},
{{!RFC8776}}.

~~~~ yang
{::include ./ietf-eth-tran-service.yang}
~~~~
{: #fig-eth-svc-yang sourcecode-markers="true" sourcecode-name="ietf-eth-tran-service@2023-12-15.yang"}

## YANG Code for ETH type

This module references a few documents including {{?RFC2697}},
{{?RFC2698}}, {{?RFC4115}}, {{IEEE_802.1ad}}, {{IEEE_802.1q}} and {{MEF10}}.

~~~~ yang
{::include ./ietf-eth-tran-types.yang}
~~~~
{: #fig-eth-types-yang sourcecode-markers="true" sourcecode-name="ietf-eth-tran-types@2023-12-15.yang"}

## Other Client Signal YANG Code

This module imports typedefs and modules from {{!RFC6991}},
{{!I-D.ietf-ccamp-otn-tunnel-model}}, {{!RFC8776}}.

~~~~ yang
{::include ./ietf-trans-client-service.yang}
~~~~
{: #fig-cbr-svc-yang sourcecode-markers="true" sourcecode-name="ietf-trans-client-service@2023-12-15.yang"}

## Other Client Signal Types YANG Code

This module defines the types for other client signal types.

~~~~ yang
{::include ./ietf-trans-client-svc-types.yang}
~~~~
{: #fig-cbr-types-yang sourcecode-markers="true" sourcecode-name="ietf-trans-client-svc-types@2023-12-15.yang"}

# Implementation Status

\[Note to the RFC Editor - remove this section before publication, as
well as remove the reference to RFC {{?RFC7942}}.]

This section records the status of known implementations of the
protocol defined by this specification at the time of posting of this
Internet-Draft, and is based on a proposal described in {{?RFC7942}}.
The description of implementations in this section is intended to
assist the IETF in its decision processes in progressing drafts to
RFCs.  Please note that the listing of any individual implementation
here does not imply endorsement by the IETF.  Furthermore, no effort
has been spent to verify the information presented here that was
supplied by IETF contributors.  This is not intended as, and must not
be construed to be, a catalog of available implementations or their
features.  Readers are advised to note that other implementations may
exist.

According to {{?RFC7942}}, "this will allow reviewers and working groups
to assign due consideration to documents that have the benefit of
running code, which may serve as evidence of valuable experimentation
and feedback that have made the implemented protocols more mature.
It is up to the individual working groups to use this information as
they see fit".

## Usage of the ETH Service YANG Model on ONAP

The implementation of the CCVPN (Cross Domain and Cross Layer VPN)
use-case on ONAP follows the ACTN {{?RFC8453}} architecture.  In the
design of CCVPN, ONAP presumes the responsibility of the MDSC, and
third party network domain controllers the PNCs.  Consequently, the
ETH Service YANG model is used as the MPI between ONAP and the domain
controllers.

- Organization: China Mobile, Huawei Technologies, etc.

- Implementation: ONAP CCVPN uses the ETH Service YANG model as the
ACTN MPI

- Description: ONAP CCVPN realizes the E-LINE and E-TREE service on
a multi-domain network.  Both of the services are modeled on ONAP
by the ETH Service YANG model, and the model instances (e.g., JSON
objects) are sent between ONAP and the domain controllers.  Refer
to the following CCVPN wiki for more information:
https://wiki.onap.org/display/DW/
CCVPN%28Cross+Domain+and+Cross+Layer+VPN%29+USE+CASE

- Maturity Level: Prototype

- Coverage: Partial

- Contact: henry.yu1@huawei.com

# IANA Considerations

It is proposed that IANA should assign new URIs from the "IETF XML
Registry" {{?RFC3688}} as follows:

~~~~ ascii-art
         URI: urn:ietf:params:xml:ns:yang:ietf-eth-tran-service
         Registrant Contact: The IESG
         XML: N/A; the requested URI is an XML namespace.

         URI: urn:ietf:params:xml:ns:yang:ietf-trans-client-service
         Registrant Contact: The IESG
         XML: N/A; the requested URI is an XML namespace.

         URI: urn:ietf:params:xml:ns:yang:ietf-eth-tran-types
         Registrant Contact: The IESG
         XML: N/A; the requested URI is an XML namespace.

         URI: urn:ietf:params:xml:ns:yang:ietf-trans-client-svc-types
         Registrant Contact: The IESG
         XML: N/A; the requested URI is an XML namespace.
~~~~

This document registers following YANG modules in the YANG Module
Names registry {{!RFC6020}}.

~~~~ ascii-art
   name:         ietf-eth-tran-service
   namespace:    urn:ietf:params:xml:ns:yang:ietf-eth-tran-service
   prefix:       ethtsvc
   reference:    RFC XXXX: A YANG Data Model for Transport
                           Network Client Signals

   name:         ietf-eth-tran-types
   namespace:    urn:ietf:params:xml:ns:yang:ietf-eth-tran-types
   prefix:       etht-types
   reference:    RFC XXXX: A YANG Data Model for Transport
                           Network Client Signals

   name:         ietf-trans-client-service
   namespace:    urn:ietf:params:xml:ns:yang:ietf-trans-client-service
   prefix:       clntsvc
   reference:    RFC XXXX: A YANG Data Model for Transport
                           Network Client Signals

   name:         ietf-trans-client-svc-types
   namespace:    urn:ietf:params:xml:ns:yang:ietf-trans-client-svc-types
   prefix:       clntsvc-types
   reference:    RFC XXXX: A YANG Data Model for Transport
                           Network Client Signals
~~~~

# Manageability Considerations

TBD

# Security Considerations

The data following the model defined in this document is exchanged
via, for example, the interface between an orchestrator and a network
domain controller.

The YANG module defined in this document can be accessed via the
RESTCONF protocol defined in {{!RFC8040}}, or maybe via the NETCONF
protocol {{!RFC6241}}.

There are a number of data nodes defined in the YANG module which are
writable/creatable/deletable (i.e., config true, which is the
default).  These data nodes may be considered sensitive or vulnerable
in some network environments.  Write operations (e.g., POST) to these
data nodes without proper protection can have a negative effect on
network operations.

--- back

# Acknowledgments
{:numbered="false"}

We would like to thank Igor Bryskin and Daniel King for their
comments and discussions.
