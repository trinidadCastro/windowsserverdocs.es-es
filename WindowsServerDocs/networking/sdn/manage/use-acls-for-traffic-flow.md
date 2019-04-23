---
title: Usar el control de acceso (ACL) enumera para administrar el flujo de tráfico de red de centro de datos
description: En este tema, obtendrá información sobre cómo configurar listas de control de acceso (ACL) para administrar el flujo de tráfico de datos con Firewall de centro de datos y las ACL en subredes virtuales. Habilitar y configurar Firewall de centro de datos mediante la creación de ACL que se aplique a una subred virtual o una interfaz de red.
manager: dougkim
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
ms.date: 08/27/2018
ms.openlocfilehash: 1364261f7ab1e732ac77b7c90d49c245aa8cbc25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861426"
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>Usar el control de acceso (ACL) enumera para administrar el flujo de tráfico de red de centro de datos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, obtendrá información sobre cómo configurar listas de control de acceso (ACL) para administrar el flujo de tráfico de datos con Firewall de centro de datos y las ACL en subredes virtuales. Habilitar y configurar Firewall de centro de datos mediante la creación de ACL que se aplique a una subred virtual o una interfaz de red.   
  
Los siguientes ejemplos de este tema muestran cómo usar Windows PowerShell para crear estas ACL.  
  
## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>Configurar Firewall de centro de datos para permitir todo el tráfico  
  
Una vez que implemente SDN, debe probar la conectividad de red básica en su nuevo entorno.  Para ello, cree una regla de Firewall de centro de datos que permite que todo el tráfico de red, sin restricciones.   
  
Use las entradas en la tabla siguiente para crear un conjunto de reglas que permiten todo el tráfico de red entrante y saliente.
  
|IP de origen|IP de destino|Protocolo|Puerto de origen|Puerto de destino|Dirección|Acción|Prioridad |   
|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|
|*    |   *      |   Todos      |    *     |      *   |     Entrante    |   Permitir      |   100  |      
|*    |     *    |     Todos    |     *    |     *    |   Saliente      |  Permitir       |  110  |
---       
  
### <a name="example-create-an-acl"></a>Por ejemplo: Crear una ACL 
En este ejemplo, crear una ACL con dos reglas:
  
1. **AllowAll_Inbound** -permite todo el tráfico de red que se pasará a la interfaz de red donde está configurada esta ACL.    
2. **AllowAllOutbound** -permite todo el tráfico a pasar fuera de la interfaz de red. Esta ACL, identificada por el identificador de recurso "AllowAll-1" ya está lista para su uso en interfaces de red y subredes virtuales.  
  
El siguiente script de ejemplo usa los comandos de Windows PowerShell exportados desde el **NetworkController** módulo para crear esta ACL.  
  
  
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
>La referencia de comandos de Windows PowerShell para la controladora de red se encuentra en el tema [Cmdlets de controlador de red](https://technet.microsoft.com/library/mt576401.aspx).  
  
## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>Utilice ACL para limitar el tráfico en una subred  
En este ejemplo, cree una ACL que impide que las máquinas virtuales dentro de la subred 192.168.0.0/24 comunicarse entre sí. Este tipo de ACL es útil para limitar la posibilidad de que un atacante para distribuir lateralmente dentro de la subred, mientras sigue permitiendo que las máquinas virtuales para recibir las solicitudes desde fuera de la subred, así como para comunicarse con otros servicios de otras subredes.   
  
  
|IP de origen|IP de destino|Protocolo|Puerto de origen|Puerto de destino|Dirección|Acción|Prioridad |  
|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|:-------:|  
|192.168.0.1    | * | Todos | * | * | Entrante|   Permitir      |   100        |
|* |192.168.0.1 | Todos | * | * | Saliente|  Permitir       |  101         |
|192.168.0.0/24 | * | Todos | * | * | Entrante| Bloquear | 102  |
|* | 192.168.0.0/24 |Todos| * | * | Saliente | Bloquear |103  |
|* | *  |Todos| * | * | Entrante | Permitir |104  |
|* | *  |Todos| * | * | Saliente | Permitir |105  |
--- 

La ACL creada por el script de ejemplo siguiente, identificado por el identificador de recurso **subred 192 168 0 0**, ahora se pueden aplicar a una subred de red virtual que usa la dirección de subred "192.168.0.0/24".  Cualquier interfaz de red que se adjunta automáticamente a esa subred de red virtual obtiene aplica las reglas ACL anteriores.  
  
Este es un script de ejemplo mediante los comandos de Windows Powershell para crear esta ACL mediante la API de REST de controladora de red:  
  
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
  
