---
title: Usar listas de control de acceso (ACL) para administrar el flujo de tráfico de red del centro de recursos
description: En este tema, aprenderá a configurar las listas de control de acceso (ACL) para administrar el flujo de tráfico de datos mediante el firewall y las ACL del centro de datos en subredes virtuales. Para habilitar y configurar el firewall del centro de seguridad, puede crear ACL que se apliquen a una subred virtual o a una interfaz de red.
manager: grcusanz
ms.topic: article
ms.assetid: 6a7ac5af-85e9-4440-a631-6a3a38e9015d
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/27/2018
ms.openlocfilehash: f96f41f18b57e53ebe01c70b6775d3608d06b28c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954001"
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>Usar listas de control de acceso (ACL) para administrar el flujo de tráfico de red del centro de recursos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, aprenderá a configurar las listas de control de acceso (ACL) para administrar el flujo de tráfico de datos mediante el firewall y las ACL del centro de datos en subredes virtuales. Para habilitar y configurar el firewall del centro de seguridad, puede crear ACL que se apliquen a una subred virtual o a una interfaz de red.

En los siguientes ejemplos de este tema se muestra cómo usar Windows PowerShell para crear estas ACL.

## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>Configuración del firewall del centro de seguridad para permitir todo el tráfico

Una vez que implemente SDN, debe probar la conectividad de red básica en el nuevo entorno.  Para ello, cree una regla para el firewall del centro de seguridad que permita todo el tráfico de red, sin restricciones.

Use las entradas de la tabla siguiente para crear un conjunto de reglas que permitan todo el tráfico de red entrante y saliente.


| IP de origen | IP de destino | Protocolo | Puerto de origen | Puerto de destino | Dirección | Acción | Prioridad |
|:---------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|    \*     |       \*       |   Todo    |     \*      |        \*        |  Entrada  | Allow  |   100    |
|    \*     |       \*       |   All    |     \*      |        \*        | Salida  | Allow  |   110    |

---

### <a name="example-create-an-acl"></a>Ejemplo: crear una ACL
En este ejemplo, creará una ACL con dos reglas:

1. **AllowAll_Inbound** : permite que todo el tráfico de red pase a la interfaz de red donde está configurada esta ACL.
2. **AllowAllOutbound** : permite que todo el tráfico pase fuera de la interfaz de red. Esta ACL, identificada por el identificador de recurso "AllowAll-1" ya está lista para usarse en subredes virtuales e interfaces de red.

En el siguiente script de ejemplo se usan los comandos de Windows PowerShell exportados desde el módulo **NetworkController** para crear esta ACL.


```PowerShell
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties
$ruleproperties.Protocol = "All"
$ruleproperties.SourcePortRange = "0-65535"
$ruleproperties.DestinationPortRange = "0-65535"
$ruleproperties.Action = "Allow"
$ruleproperties.SourceAddressPrefix = "*"
$ruleproperties.DestinationAddressPrefix = "*"
$ruleproperties.Priority = "100"
$ruleproperties.Type = "Inbound"
$ruleproperties.Logging = "Enabled"
$aclrule1 = new-object Microsoft.Windows.NetworkController.AclRule
$aclrule1.Properties = $ruleproperties
$aclrule1.ResourceId = "AllowAll_Inbound"
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties
$ruleproperties.Protocol = "All"
$ruleproperties.SourcePortRange = "0-65535"
$ruleproperties.DestinationPortRange = "0-65535"
$ruleproperties.Action = "Allow"
$ruleproperties.SourceAddressPrefix = "*"
$ruleproperties.DestinationAddressPrefix = "*"
$ruleproperties.Priority = "110"
$ruleproperties.Type = "Outbound"
$ruleproperties.Logging = "Enabled"
$aclrule2 = new-object Microsoft.Windows.NetworkController.AclRule
$aclrule2.Properties = $ruleproperties
$aclrule2.ResourceId = "AllowAll_Outbound"
$acllistproperties = new-object Microsoft.Windows.NetworkController.AccessControlListProperties
$acllistproperties.AclRules = @($aclrule1, $aclrule2)
New-NetworkControllerAccessControlList -ResourceId "AllowAll" -Properties $acllistproperties -ConnectionUri <NC REST FQDN>
```

>[!NOTE]
>La referencia de comandos de Windows PowerShell para controladora de red se encuentra en el tema [cmdlets de la controladora de red](https://technet.microsoft.com/library/mt576401.aspx).

## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>Usar ACL para limitar el tráfico en una subred
En este ejemplo, creará una ACL que impide que las máquinas virtuales de la subred 192.168.0.0/24 se comuniquen entre sí. Este tipo de ACL es útil para limitar la posibilidad de que un atacante se extienda posteriormente dentro de la subred, a la vez que permite que las máquinas virtuales reciban solicitudes desde fuera de la subred, así como para comunicarse con otros servicios en otras subredes.


|   IP de origen    | IP de destino | Protocolo | Puerto de origen | Puerto de destino | Dirección | Acción | Prioridad |
|:--------------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|  192.168.0.1   |       \*       |   Todo    |     \*      |        \*        |  Entrada  | Allow  |   100    |
|       \*       |  192.168.0.1   |   Todo    |     \*      |        \*        | Salida  | Allow  |   101    |
| 192.168.0.0/24 |       \*       |   Todo    |     \*      |        \*        |  Entrada  | Bloquear  |   102    |
|       \*       | 192.168.0.0/24 |   Todo    |     \*      |        \*        | Salida  | Bloquear  |   103    |
|       \*       |       \*       |   Todo    |     \*      |        \*        |  Entrada  | Allow  |   104    |
|       \*       |       \*       |   Todo    |     \*      |        \*        | Salida  | Allow  |   105    |

---

La ACL creada por el siguiente script de ejemplo, identificada por el identificador de recurso **subnet-192-168-0-0**, ahora se puede aplicar a una subred de red virtual que use la dirección de subred "192.168.0.0/24".  Cualquier interfaz de red que esté conectada a la subred de la red virtual obtiene automáticamente las reglas de ACL aplicadas.

El siguiente es un script de ejemplo que usa comandos de Windows PowerShell para crear esta ACL mediante la API de REST de la controladora de red:

```PowerShell
import-module networkcontroller
$ncURI = "https://mync.contoso.local"
$aclrules = @()

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties
$ruleproperties.Protocol = "All"
$ruleproperties.SourcePortRange = "0-65535"
$ruleproperties.DestinationPortRange = "0-65535"
$ruleproperties.Action = "Allow"
$ruleproperties.SourceAddressPrefix = "192.168.0.1"
$ruleproperties.DestinationAddressPrefix = "*"
$ruleproperties.Priority = "100"
$ruleproperties.Type = "Inbound"
$ruleproperties.Logging = "Enabled"

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule
$aclrule.Properties = $ruleproperties
$aclrule.ResourceId = "AllowRouter_Inbound"
$aclrules += $aclrule

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties
$ruleproperties.Protocol = "All"
$ruleproperties.SourcePortRange = "0-65535"
$ruleproperties.DestinationPortRange = "0-65535"
$ruleproperties.Action = "Allow"
$ruleproperties.SourceAddressPrefix = "*"
$ruleproperties.DestinationAddressPrefix = "192.168.0.1"
$ruleproperties.Priority = "101"
$ruleproperties.Type = "Outbound"
$ruleproperties.Logging = "Enabled"

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule
$aclrule.Properties = $ruleproperties
$aclrule.ResourceId = "AllowRouter_Outbound"
$aclrules += $aclrule

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties
$ruleproperties.Protocol = "All"
$ruleproperties.SourcePortRange = "0-65535"
$ruleproperties.DestinationPortRange = "0-65535"
$ruleproperties.Action = "Deny"
$ruleproperties.SourceAddressPrefix = "192.168.0.0/24"
$ruleproperties.DestinationAddressPrefix = "*"
$ruleproperties.Priority = "102"
$ruleproperties.Type = "Inbound"
$ruleproperties.Logging = "Enabled"

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule
$aclrule.Properties = $ruleproperties
$aclrule.ResourceId = "DenySubnet_Inbound"
$aclrules += $aclrule

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties
$ruleproperties.Protocol = "All"
$ruleproperties.SourcePortRange = "0-65535"
$ruleproperties.DestinationPortRange = "0-65535"
$ruleproperties.Action = "Deny"
$ruleproperties.SourceAddressPrefix = "*"
$ruleproperties.DestinationAddressPrefix = "192.168.0.0/24"
$ruleproperties.Priority = "103"
$ruleproperties.Type = "Outbound"
$ruleproperties.Logging = "Enabled"

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule
$aclrule.Properties = $ruleproperties
$aclrule.ResourceId = "DenySubnet_Outbound"

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties
$ruleproperties.Protocol = "All"
$ruleproperties.SourcePortRange = "0-65535"
$ruleproperties.DestinationPortRange = "0-65535"
$ruleproperties.Action = "Allow"
$ruleproperties.SourceAddressPrefix = "*"
$ruleproperties.DestinationAddressPrefix = "*"
$ruleproperties.Priority = "104"
$ruleproperties.Type = "Inbound"
$ruleproperties.Logging = "Enabled"

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule
$aclrule.Properties = $ruleproperties
$aclrule.ResourceId = "AllowAll_Inbound"
$aclrules += $aclrule

$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties
$ruleproperties.Protocol = "All"
$ruleproperties.SourcePortRange = "0-65535"
$ruleproperties.DestinationPortRange = "0-65535"
$ruleproperties.Action = "Allow"
$ruleproperties.SourceAddressPrefix = "*"
$ruleproperties.DestinationAddressPrefix = "*"
$ruleproperties.Priority = "105"
$ruleproperties.Type = "Outbound"
$ruleproperties.Logging = "Enabled"

$aclrule = new-object Microsoft.Windows.NetworkController.AclRule
$aclrule.Properties = $ruleproperties
$aclrule.ResourceId = "AllowAll_Outbound"
$aclrules += $aclrule

$acllistproperties = new-object Microsoft.Windows.NetworkController.AccessControlListProperties
$acllistproperties.AclRules = $aclrules

New-NetworkControllerAccessControlList -ResourceId "Subnet-192-168-0-0" -Properties $acllistproperties -ConnectionUri $ncURI
```

