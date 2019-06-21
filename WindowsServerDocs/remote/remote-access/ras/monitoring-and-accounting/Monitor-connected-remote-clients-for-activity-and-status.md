---
title: Supervisar la actividad y el estado de los clientes remotos conectados
description: Este tema forma parte de la Guía de supervisión de acceso remoto y las cuentas en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: beb94475-b21f-46a9-ac51-bf2bb28ca94e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bf7aed239eb56eae599078a6088cbb2971c45821
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282739"
---
# <a name="monitor-connected-remote-clients-for-activity-and-status"></a>Supervisar la actividad y el estado de los clientes remotos conectados

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess y el servicio de acceso remoto (RAS) en un solo rol de acceso remoto.  
  
Puede usar la consola de administración en el servidor de acceso remoto para supervisar el estado y actividad del cliente remoto.  
  
> [!NOTE]  
> Debe ser iniciado sesión como miembro del grupo Admins. del dominio o un miembro del grupo Administradores en cada equipo para completar las tareas descritas en este tema. Si no se puede completar una tarea mientras ha iniciado sesión con una cuenta que sea miembro del grupo Administradores, intente realizar la tarea mientras ha iniciado sesión con una cuenta que sea miembro del grupo Admins. del dominio.  
  
#### <a name="to-monitor-remote-client-activity-and-status"></a>Para supervisar el estado y actividad del cliente remoto  
  
1.  En **Administrador del servidor**, haz clic en **Herramientas** y, a continuación, haz clic en **Administración de acceso remoto**.  
  
2.  Haga clic en **informes** para navegar a **informes de acceso remoto** en el **consola de administración de acceso remoto**.  
  
3.  Haga clic en **estado de cliente remoto** para navegar a la interfaz de usuario de actividad y el estado de cliente remoto en el **consola de administración de acceso remoto**.  
  
4.  Verá la lista de usuarios que están conectados al servidor de acceso remoto y detallada las estadísticas sobre ellos. Haga clic en la primera fila de la lista que corresponde a un cliente. Cuando se selecciona una fila, se muestra la actividad del usuario remoto en el panel de vista previa.  
  
![Windows PowerShell](../../../media/Monitor-connected-remote-clients-for-activity-and-status/PowerShellLogoSmall.gif)***<em>comandos equivalentes de Windows PowerShell</em>***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, aunque aquí pueden aparecer con saltos de línea entre varias líneas aquí debido a restricciones de formato.  
  
```  
PS> Get-RemoteAccessConnectionStatistics  
```  
  
Pueden filtrar las estadísticas de usuarios, según las selecciones de criterios con los campos en la tabla siguiente.  
  
|Nombre del campo|Valor|  
|-------|-----|  
|Nombre de usuario|El nombre de usuario o alias del usuario remoto. Se pueden usar caracteres comodín para seleccionar un grupo de usuarios, por ejemplo, contoso\\* o \*\administrator.|  
|Nombre de host|El nombre de la cuenta de equipo del usuario remoto. También puede especificar una dirección IPv4 o IPv6.|  
|Tipo|DirectAccess o VPN. Si se selecciona DirectAccess, se muestran todos los usuarios remotos que están conectados con DirectAccess. Si se selecciona VPN, se muestran todos los usuarios remotos que están conectados mediante VPN.|  
|Dirección ISP|Dirección IPv4 o IPv6 del usuario remoto.|  
|IPv4 address|La dirección IPv4 interna del túnel que conecta el usuario remoto a la red corporativa.|  
|Dirección IPv6|Dirección IPv6 interna del túnel que conecta al usuario remoto con la red corporativa.|  
|Protocolo/túnel|La tecnología de transición que se usa por el cliente remoto. Esto es Teredo, 6to4 o IP-HTTPS para los usuarios de DirectAccess, y es PPTP, L2TP, SSTP o IKEv2 para usuarios de VPN.|  
|Recurso al que se accede|Todos los usuarios que tienen acceso a un extremo o un recurso corporativo en particular. El valor que corresponde a este campo es la dirección IP o nombre de host del servidor.|  
|Servidor|Servidor de acceso remoto al que se conectan los clientes. Relevante únicamente para implementaciones de clúster y multisitio.|  
  
  
  


