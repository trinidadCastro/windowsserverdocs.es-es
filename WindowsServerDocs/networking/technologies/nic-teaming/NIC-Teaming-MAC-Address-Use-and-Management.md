---
title: Administración y uso de direcciones MAC en formación de equipos NIC
description: Al configurar un equipo NIC con el modo independiente del conmutador y la distribución de la carga dinámica o el hash de la dirección, el equipo usa la dirección Media Access Control (MAC) del miembro del equipo NIC principal en el tráfico saliente. El miembro del equipo NIC principal es un adaptador de red seleccionado por el sistema operativo del conjunto inicial de miembros del equipo.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26d105e0-afc3-44b5-bb5e-0c884a4c5d62
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d8e7130d5774c19cc3d51045786bfef319cf7d16
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316431"
---
# <a name="nic-teaming-mac-address-use-and-management"></a>Administración y uso de direcciones MAC en formación de equipos NIC

>Se aplica a: Windows Server 2016

Al configurar un equipo NIC con el modo independiente del conmutador y la distribución de la carga dinámica o el hash de la dirección, el equipo usa la dirección Media Access Control (MAC) del miembro del equipo NIC principal en el tráfico saliente. El miembro del equipo NIC principal es un adaptador de red seleccionado por el sistema operativo del conjunto inicial de miembros del equipo.  Es el primer miembro del equipo que se enlaza al equipo después de crearlo o después de que se reinicie el equipo host. Dado que el miembro del equipo principal podría cambiar de forma no determinista en cada arranque, acción de deshabilitación o habilitación de NIC u otras actividades de reconfiguración, el miembro del equipo principal podría cambiar y la dirección MAC del equipo podría variar.  
  
En la mayoría de los casos esto no causa problemas, pero hay algunos casos en los que pueden surgir problemas.  
  
Si el miembro principal del equipo se quita del equipo y, a continuación, se coloca en la operación, puede haber un conflicto de direcciones MAC. Para resolver este conflicto, deshabilite y, a continuación, habilite la interfaz de equipo. El proceso de deshabilitación y habilitación de la interfaz de equipo hace que la interfaz Seleccione una nueva dirección MAC del resto de miembros del equipo, con lo que se elimina el conflicto de direcciones MAC.  
  
Puede establecer la dirección MAC del equipo NIC en una dirección MAC específica estableciéndolo en la interfaz de equipo principal, tal como se puede hacer al configurar la dirección MAC de cualquier NIC física.  
  
## <a name="mac-address-use-on-transmitted-packets"></a>Uso de direcciones MAC en los paquetes transmitidos  
Al configurar un equipo NIC en el modo independiente del conmutador y en la distribución de la carga dinámica o el hash de la dirección, los paquetes de un solo origen (por ejemplo, una sola máquina virtual) se distribuyen simultáneamente entre varios miembros del equipo. Para evitar confundir los conmutadores y evitar las alarmas de MAC, la dirección MAC de origen se sustituye por una dirección MAC diferente en los fotogramas transmitidos en los miembros del equipo que no sean el miembro principal del equipo. Por este motivo, cada miembro del equipo utiliza una dirección MAC diferente y los conflictos de direcciones MAC se evitan a menos que se produzca un error.  
  
Cuando se detecta un error en la NIC principal, el software de formación de equipos NIC comienza a usar la dirección MAC del miembro del equipo principal en el miembro del equipo que se elige para que actúe como el miembro del equipo principal temporal (es decir, el que ahora se mostrará al conmutador como equipo principal). miembro).  Este cambio solo se aplica al tráfico que se va a enviar en el miembro del equipo principal con la dirección MAC del miembro del equipo principal como su dirección MAC de origen. Se sigue enviando otro tráfico con cualquier dirección MAC de origen que hubiera usado antes del error.  
  
A continuación se muestran las listas que describen el comportamiento de sustitución de direcciones MAC de formación de equipos NIC, en función de cómo esté configurado el equipo:  
  
1.  **En modo independiente del conmutador con distribución de hash de dirección**  
  
    -   Todos los paquetes ARP y NS se envían en el miembro del equipo principal  
  
    -   Todo el tráfico enviado en NIC distintos del miembro principal del equipo se envía con la dirección MAC de origen modificada para que coincida con la NIC en la que se envían.  
  
    -   Todo el tráfico enviado en el miembro del equipo principal se envía con la dirección MAC de origen original (que puede ser la dirección MAC de origen del equipo)  
  
2.  **En modo independiente del conmutador con distribución de puertos de Hyper-V**  
  
    -   Cada puerto vmSwitch se afinidad con a un miembro del equipo  
  
    -   Cada paquete se envía en el miembro del equipo al que se afinidad con el puerto.  
  
    -   No se ha realizado ningún reemplazo de MAC de origen  
  
3.  **En modo independiente del conmutador con distribución dinámica**  
  
    -   Cada puerto vmSwitch se afinidad con a un miembro del equipo  
  
    -   Todos los paquetes ARP/NS se envían en el miembro del equipo al que se afinidad con el puerto.  
  
    -   Los paquetes enviados en el miembro del equipo que es el miembro del equipo afinidad con no tienen ninguna sustitución de dirección MAC de origen.  
  
    -   Los paquetes enviados por un miembro del equipo que no sea el miembro del equipo afinidad con tendrán el reemplazo de dirección MAC de origen.  
  
4.  **En el modo dependiente del conmutador (todas las distribuciones)**  
  
    -   No se realiza ningún reemplazo de dirección MAC de origen  
  
## <a name="related-topics"></a>Temas relacionados
- [Formación de equipos NIC](NIC-Teaming.md): en este tema se proporciona información general sobre la formación de equipos de tarjetas de interfaz de red (NIC) en Windows Server 2016. La formación de equipos NIC le permite agrupar entre uno y 32 adaptadores de red Ethernet físicos en uno o varios adaptadores de red virtuales basados en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.  

- [Configuración de la formación de equipos NIC](nic-teaming-settings.md): en este tema se proporciona información general de las propiedades del equipo NIC, como la formación de equipos y los modos de equilibrio de carga. También se proporcionan detalles acerca de la configuración del adaptador en espera y la propiedad de la interfaz de equipo principal. Si tiene al menos dos adaptadores de red en un equipo NIC, no es necesario designar un adaptador en espera para la tolerancia a errores.
  
- [Crear un nuevo equipo NIC en un equipo host o una máquina](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)virtual: en este tema, creará un nuevo equipo NIC en un equipo host o en una máquina virtual (VM) de Hyper-V que ejecute Windows Server 2016.

- [Solución de problemas de formación de equipos NIC](Troubleshooting-NIC-Teaming.md): en este tema se describen las formas de solucionar problemas de formación de equipos NIC, como hardware, los valores de los conmutadores físicos y la deshabilitación o habilitación de adaptadores de red con Windows PowerShell. 
  


