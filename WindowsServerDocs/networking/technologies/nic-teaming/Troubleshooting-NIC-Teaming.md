---
title: Solución de problemas de equipo NIC
description: Este tema proporciona información acerca de cómo solucionar el equipo en Windows Server 2016 NIC.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fdee02ec-3a7e-473e-9784-2889dc1b6dbb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 634ec1a5eee0cd661ca89c2673e5b74abd458cd6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshooting-nic-teaming"></a>Solución de problemas de equipo NIC

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Este tema proporciona información acerca de cómo solucionar equipo NIC y contiene las siguientes secciones, que se describen las causas de problemas con el equipo NIC.  
  
-   [Hardware que no cumplan con especificaciones](#bkmk_hardware)  
  
-   [Características de seguridad de interruptor físico](#bkmk_switch)  
  
-   [Cómo deshabilitar y habilitar con Windows PowerShell](#bkmk_ps)  
  
## <a name="bkmk_hardware"></a>Hardware que no cumplan con especificaciones  
Cuando las implementaciones de hardware de protocolos estándar no cumple con la especificación, el equipo NIC de rendimiento puede verse afectado.  
  
Durante una operación normal, el equipo NIC puede enviar paquetes de la misma dirección IP, pero con acceso a medios de origen diferente varias direcciones de control (MAC). Según los estándares de protocolo, los receptores de estos paquetes deben resolver la dirección IP del host o máquina virtual en una dirección MAC específica, en lugar de responder a la dirección MAC desde el que se recibió el paquete.  Los clientes que implementan correctamente los protocolos de resolución de direcciones, protocolo de resolución de direcciones (ARP) de IPv4 o el protocolo de detección de vecino de IPv6 (NDP), enviará paquetes con el destino correspondiente dirección MAC (la dirección MAC de la máquina virtual o de host que posee esa dirección IP).  
  
Sin embargo, algunos incrustados hardware no implementan correctamente los protocolos de resolución de direcciones y, también es posible que no resolver explícitamente una dirección IP a una dirección MAC con ARP o NDP.  Un controlador de red (SAN) del área de almacenamiento es un ejemplo de un dispositivo que puede realizar de esta manera. Dispositivos no conformes copiar el origen de la dirección MAC que se encuentra en un paquete recibido y usan como el destino de la dirección MAC de los paquetes de salida correspondientes.  
  
Esto da como resultado de los paquetes enviados a la dirección MAC de destino incorrecto. Por este motivo, los paquetes se colocan el conmutador Virtual de Hyper-V, porque no coinciden con cualquier destino conocido.  
  
Si estás teniendo problemas conectarse a los controladores de SAN u otros incrustado hardware, debes realizar capturas de paquetes y determinar si el hardware implementa correctamente ARP o NDP y ponte en contacto con el fabricante del hardware para obtener soporte técnico.  
  
## <a name="bkmk_switch"></a>Características de seguridad de interruptor físico  
Según la configuración, el equipo NIC puede enviar paquetes de la misma dirección IP con varias direcciones MAC de origen diferente.  Esto puede viaje la seguridad de protección de características en el conmutador físico como dinámico inspección ARP u origen IP, especialmente si el conmutador físico no es consciente de que los puertos forman parte de un equipo. Esto puede ocurrir si configuras el equipo NIC en modo independiente del conmutador.  Debe inspeccionar los registros de conmutador para determinar si las características de seguridad conmutador están ocasionando problemas de conectividad con el equipo NIC.  
  
## <a name="bkmk_ps"></a>Cómo deshabilitar y habilitar los adaptadores de red mediante Windows PowerShell  
Una razón común para un equipo de NIC a un error es que la interfaz de equipo esté deshabilitada. En muchos casos, la interfaz está deshabilitada por accidente cuando se ejecuta la siguiente secuencia de Windows PowerShell de comandos:  
  
```  
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  
Esta secuencia de comandos no habilita todos los NetAdapters que este deshabilitado.  
  
Esto es porque hace que la interfaz de equipo NIC que desea eliminar y ya no se mostrarán en Get AdaptadorRed deshabilitar todas las NIC físico miembro subyacente. Por este motivo, el **habilitar AdaptadorRed \ *** comando no permite que el equipo NIC, porque ese adaptador se quita.  
  
La **habilitar AdaptadorRed \ *** comando, habilitar sin embargo, el miembro NIC, a continuación, (poco tiempo después) que hace que la interfaz del equipo volver a crear. En este caso, la interfaz de equipo todavía está en un estado "disabled" porque no se ha vuelto a habilitar. Habilitar la interfaz de equipo después de que se vuelve a crear permitirá el tráfico de red empezar a hacer fluir el nuevo.  
  
## <a name="see-also"></a>Consulta también  
[El equipo NIC](NIC-Teaming.md)  
  


