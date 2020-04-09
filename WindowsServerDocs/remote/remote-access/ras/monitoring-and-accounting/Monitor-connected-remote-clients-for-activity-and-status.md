---
title: Supervisar la actividad y el estado de los clientes remotos conectados
description: Este tema forma parte de la guía de supervisión y contabilidad de acceso remoto en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: beb94475-b21f-46a9-ac51-bf2bb28ca94e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 949a8b4da7c85c82c247c638df09768c5d0790e7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860548"
---
# <a name="monitor-connected-remote-clients-for-activity-and-status"></a>Supervisar la actividad y el estado de los clientes remotos conectados

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess y el servicio de acceso remoto (RAS) en un solo rol de acceso remoto.  
  
Puede usar la consola de administración de en el servidor de acceso remoto para supervisar la actividad y el estado de los clientes remotos.  
  
> [!NOTE]  
> Debe haber iniciado sesión como miembro del grupo Admins. del dominio o como miembro del grupo administradores en cada equipo para completar las tareas descritas en este tema. Si no puede completar una tarea mientras inicia sesión con una cuenta que sea miembro del grupo administradores, intente realizar la tarea mientras inicia sesión con una cuenta que sea miembro del grupo Admins. del dominio.  
  
#### <a name="to-monitor-remote-client-activity-and-status"></a>Para supervisar la actividad y el estado del cliente remoto  
  
1.  En **Administrador del servidor**, haz clic en **Herramientas** y, a continuación, haz clic en **Administración de acceso remoto**.  
  
2.  Haga clic en **informes** para navegar a **informes de acceso remoto** en la **consola de administración de acceso remoto**.  
  
3.  Haga clic en **Estado de cliente remoto** para ir a la interfaz de usuario actividad de cliente remoto y estado en la **consola de administración de acceso remoto**.  
  
4.  Verá la lista de usuarios que están conectados al servidor de acceso remoto y estadísticas detalladas sobre ellos. Haga clic en la primera fila de la lista que corresponde a un cliente. Al seleccionar una fila, se muestra la actividad de usuario remoto en el panel de vista previa.  
  
![](../../../media/Monitor-connected-remote-clients-for-activity-and-status/PowerShellLogoSmall.gif)***<em>comandos equivalentes</em> de Windows PowerShell Windows PowerShell***  
  
Los siguientes cmdlets de Windows PowerShell realizan la misma función que el procedimiento anterior. Escriba cada cmdlet en una sola línea, incluso aunque puedan aparecer con las palabras ajustadas en varias líneas aquí debido a las restricciones de formato.  
  
```  
PS> Get-RemoteAccessConnectionStatistics  
```  
  
Las estadísticas de usuario se pueden filtrar en función de las selecciones de criterios mediante los campos de la tabla siguiente.  
  
|Nombre de campo|Valor|  
|-------|-----|  
|Nombre de usuario|El nombre de usuario o alias del usuario remoto. Se pueden usar caracteres comodín para seleccionar un grupo de usuarios, como contoso\\* o \*\Administrator.|  
|nombreDeHost|El nombre de la cuenta de equipo del usuario remoto. También se puede especificar una dirección IPv4 o IPv6.|  
|Tipo|DirectAccess o VPN. Si se selecciona DirectAccess, se muestran todos los usuarios remotos que se conectan mediante DirectAccess. Si se selecciona VPN, se muestran todos los usuarios remotos que se conectan mediante VPN.|  
|Dirección ISP|Dirección IPv4 o IPv6 del usuario remoto.|  
|IPv4 address|La dirección IPv4 interna del túnel que conecta al usuario remoto con la red corporativa.|  
|Dirección IPv6|Dirección IPv6 interna del túnel que conecta al usuario remoto con la red corporativa.|  
|Protocolo/túnel|La tecnología de transición que usa el cliente remoto. Se trata de Teredo, 6to4 o IP-HTTPS para usuarios de DirectAccess, y es PPTP, L2TP, SSTP o IKEv2 para usuarios de VPN.|  
|Recurso al que se accede|Todos los usuarios que tienen acceso a un extremo o un recurso corporativo en particular. El valor que corresponde a este campo es el nombre de host o la dirección IP del servidor.|  
|Server|Servidor de acceso remoto al que se conectan los clientes. Relevante únicamente para implementaciones de clúster y multisitio.|  
  
  
  


