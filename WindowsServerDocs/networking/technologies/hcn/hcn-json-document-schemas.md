---
title: Esquemas de documentos JSON de host de proceso de red (HCN)
description: Información sobre los esquemas de documentos JSON de HCN.
ms.author: daschott
author: daschott
ms.date: 11/05/2018
ms.openlocfilehash: 16d74002fc73c2d1b5467f7f9a4c5f94cc76a989
ms.sourcegitcommit: 0b3d6661c44aa1a697087e644437279142726d84
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90083756"
---
# <a name="hcn-json-document-schemas"></a>Esquemas de documentos HCN JSON

> Se aplica a: Windows Server (canal semianual), Windows Server 2019

## <a name="hcn-schema"></a>Esquema HCN

```json
// Network
{
    "Id" : <string>,
    "Owner" : <string>,
    "SchemaVersion" : {
        "Major" : <uint32>,
        "Minor" : <uint32>
    },
    "Flags" : <enum bit mask>,
         // AsString; Values:
         // "None" (0),
         // "EnableDnsProxy" (1),
         // "EnableDhcpServer" (2),
         // "IsolateVSwitch" (8)
    "Type"  : <enum>,
         // AsString; Values:
         // "NAT" (0),
         // "ICS" (1),
         // "Transparent" (2)
    "Ipams" : [ {
         "Type" : <enum>,
             // AsString; Values:
             // "Static" (0),
             // "Dhcp" (1)
         "Subnets" : [ {
                "IpAddressPrefix" : <ip prefix in CIDR>,
                "Policies" : [ {
                        "Type" : <enum>,
                            // AsString; Values:
                            // "VLAN" (0)
                        "Data" : <any>
                 } ],
                "Routes" : [ {
                       "NextHop" : <ip address of the next hop gateway>,
                       "DestinationPrefix" : <ip prefix in cidr>,
                       "Metric" : <route metric in uint8>,
                 } ],
          } ],
     } ],
    "Policies" : [{
         "Type" : <enum>,
              // AsString; Values:
              // "NetAdapterName" (1),
              // "InterfaceConstraint" (2)
         "Data" : <any>
    }],
    "Dns" : {
        "Suffix" : <local connection specific suffix>,
        "Search" : [<list of additional suffixes>],
        "ServerList" : [<string>],
        "Options" : [<string>],
    },
    "MacPool" : {
        "Ranges" : [ {
              "StartMacAddress" : <string>,
              "EndMacAddress" : <string>
         } ],
    },
}
```

## <a name="hcn-endpoint-schema"></a>Esquema del punto de conexión HCN

```json
// Endpoint
{
    "Id" : <string>,
    "Owner" : <string>,
    "SchemaVersion" : {
        "Major" : <uint32>,
        "Minor" : <uint32>
    },
    "Flags" : <enum bit mask>,
         // AsString; Values:
         // "None" (0),
         // "DisableInterComputeCommunication" (2)
    "HostComputeNetwork" : <string>,
    "MacAddress" : <string>,
    "Policies" : [ {
         "Type" : <enum>,
              // AsString; Values:
              // "PortMapping" (0),
              // "ACL" (1)
         "Data" : <any>
    } ],
    "Dns" : {
        "Suffix" : <local connection specific suffix>,
        "Search" : [<list of additional suffixes>],
        "ServerList" : [<string>],
        "Options" : [<string>],
    },
    "IPConfigurations" : [ {
        "IPAddress" : <ip address>,
        "PrefixLength" : <prefix length uint16>,
    } ],
    "Routes" : [ {
        "NextHop" : <ip address of the next hop gateway>,
        "DestinationPrefix" : <ip prefix in cidr>,
        "Metric" : <route metric in uint8>,
    } ],
}
```

## <a name="hcn-policy-schema"></a>Esquema de directiva de HCN

```json
// VlanPolicy
{
    "Type" : "VLAN",
    "IsolationId" : <uint32>,
}
// PortMappingPolicy
{
    "Type" : "PortMapping",
    "Protocol" : <enum>,
         // AsString; Values:
         // "Unknown" (0),
         // "ICMPv4" (1),
         // "IGMP" (2),
         // "TCP" (6),
         // "UDP" (17),
         // "ICMPv6" (58)
    "InternalPort" : <uint16>,
    "ExternalPort" : <uint16>,
}
```

## <a name="hcn-load-balancer-schema"></a>HCN esquema de equilibrador de carga

```json
// Host Compute LoadBalancer
{
    "Id" : <string>,
    "Owner" : <string>,
    "SchemaVersion" : {
        "Major" : <uint32>,
        "Minor" : <uint32>
    },
    "Flags" : <enum bit mask>,
         // AsString; Values:
         // "None" (0),
         // "EnableDirectServerReturn" (1)
         // "EnableInternalLoadBalancer" (2)
    "HostComputeEndpoints" : [<Host compute Endpoint id>],
    "VirtualIPs" : [<Virtual IpAddress>],
    "PortMappings" : [ {
        "Type" : "PortMapping",
        "Protocol" : <enum>,
             // AsString; Values:
             // "Unknown" (0),
             // "ICMPv4" (1),
             // "IGMP" (2),
             // "TCP" (6),
             // "UDP" (17),
             // "ICMPv6" (58)
        "InternalPort" : <uint16>,
        "ExternalPort" : <uint16>,
    } ],
    "Policies" : [ {
         "Type" : <enum>,
              // AsString; Values:
              // "SourceVirtualIp" (0),
         "Data" : <any>
    } ],
}
```

## <a name="hcn-namespace-schema"></a>Esquema de espacio de nombres HCN

```json
// Namespace
{
    "Id" : <string>,
    "Owner" : <string>,
    "SchemaVersion" : {
        "Major" : <uint32>,
        "Minor" : <uint32>
    },
    "NamespaceId" : <uint32>,
    "NamespaceGuid" : <guid>,
    "Type"  : <enum>,
              // AsString; Values:
              // "Host" (0),
              // "HostDefault" (1),
              // "Guest" (2),
              // "GuestDefault" (3)
    "Resources" : [ {
          "Type"  : <enum>,
              // AsString; Values:
              // "Container" (0),
              // "Endpoint" (1)
          "Data"  : <any>
    } ],
}
```

## <a name="hcn-notification-schema"></a>Esquema de notificación de HCN

```json
// Notification
{
    "ID" : Guid,
    "Flags" : <uint32>,
};
```

## <a name="result-error-schema"></a>Esquema de error de resultados

```json
// ErrorSchema
{
    "ErrorCode" : <uint32>,
    "Error" : <string>,
    "Success" : <bool>,
}
```