---
title: Usar listas de Control de acceso (ACL) para administrar el flujo de tráfico de red de centros de datos
description: Este tema es parte de la guía definido redes Software sobre cómo administrar cargas de trabajo de inquilino y redes virtuales en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a7ac5af-85e9-4440-a631-6a3a38e9015d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 64b7e1abf1ddb8132a8c6692fe82521c589f32df
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>Usar listas de Control de acceso (ACL) para administrar el flujo de tráfico de red de centros de datos

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para aprender a configurar listas de control de acceso para administrar el flujo de tráfico de datos con Firewall de centros de datos y las ACL de subredes virtuales.  
  
Puedes habilitar y configurar el Firewall de centros de datos mediante la creación de ACL que se aplican a una subred virtual o una interfaz de red.  
  
Los ejemplos siguientes muestran cómo usar Windows PowerShell para crear estas ACL.  
  
## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>Configurar el Firewall de centros de datos para permitir todo el tráfico  
  
Después de implementar SDN, se recomienda que pruebes conectividad de red básica en el entorno de nuevo.  
  
Para ello, puede crear una regla de Firewall de centros de datos que permite todo el tráfico de red, sin restricciones.   
  
Puedes usar las entradas en la tabla siguiente para crear un conjunto de reglas que permite todo el tráfico de red entrantes y salientes.  
  
  
  
IP de origen|Destino IP|Protocolo|Puerto de origen|Puerto de destino|Dirección|Acción|Prioridad   
---------|---------|---------|---------|---------|---------|---------|---------  
*    |   *      |   Todos los      |    *     |      *   |     Entrante    |   Permitir      |   100        
*    |     *    |     Todos los    |     *    |     *    |   Saliente      |  Permitir       |  110         
  
  
Este script de ejemplo crea una ACL que contiene dos reglas:    
- La primera regla "AllowAll_Inbound" permite todo el tráfico de red pasar a la interfaz de red donde se configura esta ACL.    
- La segunda regla, "AllowAllOutbound" permite todo el tráfico a pasar fuera de la interfaz de red.  
Esta ACL, identificada por el identificador de recursos "PermitirTodas-1" ya está lista para usarse en virtuales subredes y las interfaces de red.  
  
El siguiente script de ejemplo usa los comandos de Windows PowerShell exportados desde el **NetworkController** módulo para crear esta ACL.  
  
  
```  
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
>La referencia de comandos de Windows PowerShell para el controlador de red se encuentra en el tema [Cmdlets del controlador de red](https://technet.microsoft.com/library/mt576401.aspx).  
  
## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>Usar ACL para limitar el tráfico en una subred  
  
Puedes usar este ejemplo para crear una ACL que impide que máquinas virtuales (VM) dentro de la subred 192.168.0.0/24 desde comunicarse entre sí.   
  
Este tipo de ACL es útil para limitar la capacidad de un atacante para propagar lateralmente dentro de la subred, dejando que las máquinas virtuales para recibir solicitudes desde fuera de la subred, así como para comunicarse con otros servicios de otras subredes.  
  
  
IP de origen|Destino IP|Protocolo|Puerto de origen|Puerto de destino|Dirección|Acción|Prioridad   
---------|---------|---------|---------|---------|---------|---------|---------  
192.168.0.1    | * | Todos los | * | * | Entrante|   Permitir      |   100        
* |192.168.0.1 | Todos los | * | * | Saliente|  Permitir       |  101         
192.168.0.0/24 | * | Todos los | * | * | Entrante| Bloque | 102  
* | 192.168.0.0/24 |Todos los| * | * | Saliente | Bloque |103  
* | *  |Todos los| * | * | Entrante | Permitir |104  
* | *  |Todos los| * | * | Saliente | Permitir |105  
  
La ACL creados por el script de ejemplo siguiente, identificado por el identificador de recurso **subred 192 168 0 0**, ahora puede aplicarse a una subred de red virtual que usa la dirección de subred "192.168.0.0/24".  Cualquier interfaz de red que está vinculado a esa subred de red virtual automáticamente obtiene aplicadas las reglas ACL anteriores.  
  
La siguiente es un script de ejemplo con comandos de Windows Powershell para crear esta ACL con la API de REST de controlador de red:  
  
```  
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
  
