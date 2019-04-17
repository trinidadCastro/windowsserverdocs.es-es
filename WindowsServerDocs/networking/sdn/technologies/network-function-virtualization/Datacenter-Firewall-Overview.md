---
title: Introducción a Datacenter Firewall
description: Puedes usar este tema para obtener información sobre Firewall del centro de datos, que es un nivel de red, la tupla de 5 (los números de puerto de protocolo, origen y destino, direcciones IP de origen y destino), el firewall con estado y para varios inquilinos en Windows Server 2016.
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
ms.openlocfilehash: 0c9b9fb5b0fb9aa09ed783b2b66a8ad370a627c3
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="datacenter-firewall-overview"></a>Introducción a Datacenter Firewall

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Firewall de centros de datos es un nuevo servicio que se incluye con Windows Server 2016. Es un nivel de red, la tupla de 5 (protocolo, números de puerto de origen y destino, direcciones IP de origen y destino), el firewall con estado y para varios inquilinos. Cuando se implementa y ofrece como un servicio por el proveedor de servicios, los administradores de inquilino pueden instalar y configurar las directivas de firewall para ayudar a proteger sus redes virtuales de tráfico no deseado que se origina desde Internet y redes de intranet.  
  
![Firewall de centros de datos en la pila de red](../../../media/Datacenter-Firewall-Overview/MultitenantFirewallOverview2.png)  
  
El Administrador de proveedor de servicio o el Administrador de inquilinos puede administrar las directivas de Firewall del centro de datos a través de la API northbound y el controlador de red.  
  
El Firewall del centro de datos ofrece las siguientes ventajas para proveedores de servicios de nube:  
  
-   Una solución muy escalables, manejables y disturbio firewall basado en software que se puede ofrecer a los inquilinos  
  
-   Libertad de movimiento de máquinas virtuales de inquilino a los hosts de cálculo diferentes sin interrumpir las directivas de firewall del inquilino  
  
    -   Implementar como un firewall de agente de host de puerto vSwitch  
  
    -   Máquinas virtuales de inquilino obtener las directivas asignadas a su firewall de agente de host vSwitch  
  
    -   Las reglas de Firewall están configuradas en cada puerto vSwitch, independiente del host real ejecutando la máquina virtual  
  
-   Ofrece protección de máquinas virtuales independientes del sistema operativo de invitado de inquilino de inquilinos  
  
El Firewall del centro de datos ofrece las siguientes ventajas de inquilinos:  
  
-   Capacidad para definir las reglas del firewall para ayudar a proteger Internet hacia cargas de trabajo en las redes virtuales  
  
-   Capacidad para definir las reglas del firewall para ayudar a proteger el tráfico entre máquinas virtuales en la misma L2 virtual subred así entre máquinas virtuales en diferentes subredes virtuales de L2  
  
-   Capacidad para definir las reglas del firewall para ayudar a proteger y aislar el tráfico de red entre inquilino local redes y sus redes virtuales en el proveedor de servicio  
  


