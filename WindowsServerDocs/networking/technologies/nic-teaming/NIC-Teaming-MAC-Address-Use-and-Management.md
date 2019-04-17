---
title: Agrupación de administración y el uso de la dirección MAC del NIC
description: En este tema proporciona información sobre cómo usa el equipo NIC de que acceder los medios dirección MAC (control) en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26d105e0-afc3-44b5-bb5e-0c884a4c5d62
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ab624665c964b100e6b1ed633120299b55e37df
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="nic-teaming-mac-address-use-and-management"></a>Agrupación de administración y el uso de la dirección MAC del NIC

>Se aplica a: Windows Server 2016

Cuando se configura un equipo NIC con el conmutador de modo independiente y hash de dirección o distribución de la carga dinámica, los usos de equipo que Media access dirección MAC (controlan) de los miembros del equipo de NIC principal en el tráfico saliente. El miembro del equipo de NIC principal es un adaptador de red seleccionado por el sistema operativo desde el conjunto inicial de los miembros del equipo.  
  
El miembro del equipo principal es el primer miembro del equipo para enlazar con el equipo después de crearlo o se haya reiniciado el equipo host. Como miembro del equipo principal puede cambiar de una manera no determinista en cada arranque, NIC habilitar o deshabilitar acción u otras actividades reconfiguración, miembro del equipo principal podrían cambiar y la dirección MAC del equipo pueden variar.  
  
En la mayoría de los casos, esto no cause problemas, pero hay unos pocos casos donde pueden surgir problemas.  
  
Si el miembro del equipo principal se quitará el equipo y, a continuación, se coloca en operación puede haber un conflicto de dirección MAC. Para resolver el conflicto, deshabilitar y, a continuación, habilitar la interfaz de equipo. El proceso de cómo deshabilitar y, a continuación, habilitar la interfaz de equipo hace que la interfaz seleccionar una nueva dirección MAC de los miembros del equipo restantes, lo que elimina el conflicto de dirección MAC.  
  
Puedes establecer la dirección MAC del equipo de NIC a una dirección MAC determinada estableciendo en la interfaz principal del equipo, al igual que puedes hacer cuando se configura la dirección MAC de cualquier NIC física.  
  
## <a name="mac-address-use-on-transmitted-packets"></a>Usar dirección MAC en paquetes transmitidos  
Cuando se configura un equipo NIC en modo de conmutación independientes y hash de dirección o distribución de la carga dinámica, los paquetes de un único origen (por ejemplo, una única VM) simultáneamente se distribuyen entre varios miembros del equipo. Para impedir que los modificadores haciendo un lío e impedir que MAC batir alarmas, la dirección MAC de origen se reemplaza con otra dirección MAC en los marcos que se transmiten a través de los miembros del equipo que no sea miembro del equipo principal. Por este motivo, los miembros del equipo usa una dirección MAC diferente y se les conflictos de direcciones MAC a menos que y hasta que se produce el error.  
  
Cuando se detecta un error en la NIC principal, el software del equipo NIC comienza a usar la dirección MAC del miembro del equipo principal en el miembro del equipo que se ha elegido para que actúe como los integrantes principales del temporales (es decir, el que se mostrará al modificador como miembro del equipo principal).  Este cambio solo se aplica al tráfico que se va a enviar en el miembro del equipo principal con la dirección MAC de integrantes del equipo principal como su dirección MAC de origen. Otro tráfico continúa enviarán con cualquier origen de la dirección MAC que habrían usado antes del error.  
  
A continuación, incluimos las listas que describen el comportamiento de reemplazo de dirección MAC del NIC de colaboración, en función de cómo se configura el equipo:  
  
1.  **En el modo conmutador independiente con la distribución de Hash de dirección**  
  
    -   Todos los paquetes ARP y NS se envían en el miembro del equipo principal  
  
    -   Todo el tráfico enviado en NIC que no sea miembro del equipo principal se envían con la dirección MAC de origen modificada para que coincida con la NIC en el que se envían  
  
    -   Todo el tráfico enviado en el miembro del equipo principal se envía con el origen de dirección MAC (que puede ser el origen del equipo dirección MAC)  
  
2.  **En el modo conmutador independiente con la distribución de puerto de Hyper-V**  
  
    -   Todos los puertos vmSwitch es afinidad a un miembro del equipo  
  
    -   Todos los paquetes se envían en el miembro del equipo al que se afinidad el puerto  
  
    -   No se realiza ninguna fuente de reemplazo de MAC  
  
3.  **En el modo independiente del conmutador con distribución dinámica**  
  
    -   Todos los puertos vmSwitch es afinidad a un miembro del equipo  
  
    -   Todos los paquetes ARP/NS se envían en el miembro del equipo al que se afinidad el puerto  
  
    -   Paquetes enviados por el miembro del equipo que sea miembro del equipo afinidad no tienen ninguna fuente de reemplazo de dirección MAC listo  
  
    -   Paquetes enviados por un miembro del equipo que no sea miembro del equipo afinidad tendrá reemplazo de dirección MAC de origen listo  
  
4.  **En el modo de cambiar dependientes (todas las distribuciones)**  
  
    -   No se realiza ninguna fuente de reemplazo de dirección MAC  
  
## <a name="see-also"></a>Consulta también  
[Crear un nuevo equipo NIC en un equipo Host o máquina virtual](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)  
[El equipo NIC](NIC-Teaming.md)  
  


