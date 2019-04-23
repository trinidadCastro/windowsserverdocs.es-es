---
title: Información general de Datacenter Firewall
description: Puede utilizar este tema para obtener información acerca de Firewall de centro de datos, que es una capa de red, 5-tupla (números de puerto de protocolo, origen y destino, direcciones IP de origen y destino), un firewall con estado, multiempresa en Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67576533-206b-428a-956c-ed8c53218d9b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f1de50dc61639f4985c9d28fdde6072af650f42e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890836"
---
# <a name="datacenter-firewall-overview"></a>Información general de Datacenter Firewall

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Datacenter Firewall es un nuevo servicio incluido con Windows Server 2016. Es una capa de red, 5-tupla (protocolo, números de puerto de origen y destino, direcciones IP de origen y destino), firewall con estado, para varios inquilinos. Cuando se implementa y se ofrecen como un servicio por el proveedor de servicios, los administradores de inquilinos pueden instalar y configurar las directivas de firewall para ayudar a proteger sus redes virtuales del tráfico no deseado que se origina en Internet y redes de intranet.  
  
![Datacenter Firewall en la pila de red](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
El administrador del proveedor de servicios o el Administrador de inquilinos puede administrar las directivas de Firewall de centro de datos a través de la controladora de red y las API de northbound.  
  
El Firewall de centro de datos ofrece las siguientes ventajas para los proveedores de servicios en la nube:  
  
-   Una solución altamente escalable, administrable y disturbio firewall basada en software que se puede ofrecer a los inquilinos  
  
-   Libertad para mover máquinas virtuales de inquilinos a hosts diferentes de proceso sin interrumpir las directivas de firewall de inquilino  
  
    -   Implementado como un firewall de agente de host de puerto vSwitch  
  
    -   Máquinas virtuales de inquilinos obtener las directivas asignadas a su servidor de seguridad de agente de host de vSwitch  
  
    -   Las reglas de Firewall se configuran en cada puerto vSwitch, independiente del host real que se ejecuta la máquina virtual  
  
-   Ofrece protección para máquinas virtuales independientes del sistema operativo de invitado de inquilino del inquilino  
  
El Firewall de centro de datos ofrece las siguientes ventajas para inquilinos:  
  
-   Capacidad de definir las reglas de firewall para ayudar a proteger las cargas de trabajo en las redes virtuales de Internet.  
  
-   Capacidad de definir las reglas de firewall para ayudar a proteger el tráfico entre máquinas virtuales en la misma L2 subred virtual, así como entre máquinas virtuales en diferentes subredes virtuales L2  
  
-   Capacidad para definir las reglas de firewall para ayudar a proteger y aislar el tráfico de red entre el inquilino local las redes y sus redes virtuales en el proveedor de servicios  
  


