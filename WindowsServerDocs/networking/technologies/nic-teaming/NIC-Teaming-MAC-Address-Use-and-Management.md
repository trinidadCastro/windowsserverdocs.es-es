---
title: Administración y uso de direcciones MAC en formación de equipos NIC
description: Al configurar un equipo NIC con el modo independiente de conmutador y hash de dirección o distribución de carga dinámica, el equipo usa que la media access control dirección (MAC) del miembro del equipo NIC principal en el tráfico saliente. El miembro del equipo NIC principal es un adaptador de red seleccionado por el sistema operativo desde el conjunto inicial de los miembros del equipo.
manager: dougkim
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
ms.openlocfilehash: 074a55d2f0a5423af5892ed73dc8a2b8dbda9d5b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824256"
---
# <a name="nic-teaming-mac-address-use-and-management"></a>Administración y uso de direcciones MAC en formación de equipos NIC

>Se aplica a: Windows Server 2016

Al configurar un equipo NIC con el modo independiente de conmutador y hash de dirección o distribución de carga dinámica, el equipo usa que la media access control dirección (MAC) del miembro del equipo NIC principal en el tráfico saliente. El miembro del equipo NIC principal es un adaptador de red seleccionado por el sistema operativo desde el conjunto inicial de los miembros del equipo.  Es el primer miembro del equipo para enlazar con el equipo después de crearlo o después de reinicia el equipo host. Como miembro del equipo principal puede cambiar de manera no determinista en cada inicio, NIC deshabilitar/Habilitar acción u otras actividades de reconfiguración, miembro del equipo principal podrían cambiar y la dirección MAC del equipo pueden variar.  
  
En la mayoría de las situaciones esto no causa problemas, pero hay algunos casos donde pueden surgir problemas.  
  
Si el miembro del equipo principal se quitará el equipo y, a continuación, se coloca en la operación puede haber un conflicto de direcciones MAC. Para resolver este conflicto, deshabilitar y, a continuación, habilitar la interfaz de equipo. El proceso de deshabilitar y habilitar la interfaz de equipo hace que la interfaz seleccionar una nueva dirección MAC de los restantes miembros del equipo, lo que evita el conflicto de dirección MAC.  
  
Puede establecer la dirección MAC del equipo NIC en una dirección MAC específica si se establece en la interfaz principal del equipo, tal como se puede hacer al configurar la dirección MAC de cualquier NIC física.  
  
## <a name="mac-address-use-on-transmitted-packets"></a>Usar dirección MAC de paquetes transmitidos  
Al configurar un equipo NIC en modo independiente de conmutador y hash de dirección o distribución de carga dinámica, los paquetes de un único origen (por ejemplo, una sola máquina virtual) se distribuye simultáneamente en varios miembros del equipo. Para evitar que los modificadores confundirse y prevenir oscilante alarmas de MAC, la dirección MAC de origen se reemplaza con una dirección MAC diferente en los marcos que se transmiten a través de los miembros del equipo que no sea miembro del equipo principal. Por este motivo, cada miembro del equipo usa una dirección MAC diferente y conflictos de direcciones MAC se les hasta que se produce un error.  
  
Cuando se detecta un error en la NIC principal, inicie el software de formación de equipos NIC con la dirección de MAC del miembro del equipo principal en el miembro del equipo que se elige para que actúe como el miembro del equipo principal temporal (es decir, el mismo que aparecerán en el conmutador como el equipo principal me Nú).  Este cambio solo se aplica al tráfico que se va a enviar en el miembro del equipo principal con una dirección MAC del miembro principal del equipo como su dirección MAC de origen. Continúa el tráfico restante se envíe con cualquier dirección MAC habrían usado antes del error de origen.  
  
Siguientes son las listas que describen el comportamiento de sustitución de dirección MAC de la formación de equipos NIC, según cómo esté configurado el equipo:  
  
1.  **En el modo independiente de conmutador con distribución Hash de dirección**  
  
    -   Se envían todos los paquetes ARP y NS en el miembro del equipo principal  
  
    -   Todo el tráfico enviado en la NIC que no sea miembro del equipo principal se envían con la dirección MAC de origen modificada para que coincida con la NIC en el que se envían  
  
    -   Se envía todo el tráfico enviado en el miembro del equipo principal con el origen de dirección MAC (que puede ser una dirección MAC de origen del equipo)  
  
2.  **En el modo independiente de conmutador con la distribución de puerto de Hyper-V**  
  
    -   Cada puerto vmSwitch es afinidad con un miembro del equipo  
  
    -   Todos los paquetes se envían en el miembro del equipo al que se afinidad con el puerto  
  
    -   No se realiza ninguna sustitución de MAC de origen  
  
3.  **En el modo independiente de conmutador con distribución dinámica**  
  
    -   Cada puerto vmSwitch es afinidad con un miembro del equipo  
  
    -   Se envían todos los paquetes de ARP y NS en el miembro del equipo al que se afinidad con el puerto  
  
    -   Los paquetes enviados en el miembro del equipo que sea miembro del equipo con afinidad no tienen ningún origen de reemplazo de dirección MAC realiza  
  
    -   Paquetes enviados a un miembro del equipo que no sea miembro del equipo con afinidad tendrá el reemplazo de dirección MAC de origen hace  
  
4.  **En el modo dependientes del conmutador (todas las distribuciones)**  
  
    -   No se realiza ninguna sustitución de dirección MAC de origen  
  
## <a name="related-topics"></a>Temas relacionados
- [Formación de equipos NIC](NIC-Teaming.md): En este tema, le proporcionamos una visión general de formación de equipos de tarjeta de interfaz de red (NIC) en Windows Server 2016. Formación de equipos NIC permite agrupar entre 1 y 32 adaptadores de red Ethernet físicos en uno o más adaptadores de red virtual basada en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.  

- [Configuración de formación de equipos NIC](nic-teaming-settings.md): En este tema, nos ofrecerle una visión general de las propiedades del equipo NIC como formación de equipos y los modos de equilibrio de carga. También se proporcionan detalles sobre la configuración del adaptador en espera y la propiedad de la interfaz principal del equipo. Si tiene al menos dos adaptadores de red en un equipo NIC, no es necesario designar un adaptador de modo de espera para tolerancia a errores.
  
- [Crear un nuevo equipo NIC en un equipo host o máquina virtual](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md): En este tema, creará un nuevo equipo NIC en un equipo host o en una máquina virtual de Hyper-V (VM) que ejecuta Windows Server 2016.

- [Solución de problemas de formación de equipos NIC](Troubleshooting-NIC-Teaming.md): En este tema, se describen formas de solucionar la formación de equipos NIC, como hardware, los valores de conmutador físico y habilitación o deshabilitación de los adaptadores de red mediante Windows PowerShell. 
  


