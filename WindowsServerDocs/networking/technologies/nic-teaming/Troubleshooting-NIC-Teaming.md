---
title: Solución de problemas de formación de equipos NIC
description: En este tema se proporciona información acerca de la solución de problemas de formación de equipos NIC en Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fdee02ec-3a7e-473e-9784-2889dc1b6dbb
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 2f21301e0669fb593acda47787fed5f396618daf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405560"
---
# <a name="troubleshooting-nic-teaming"></a>Solución de problemas de formación de equipos NIC

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, se describen las formas de solucionar problemas de formación de equipos NIC, como los valores de hardware y del conmutador físico.  Cuando las implementaciones de hardware de los protocolos estándar no se ajustan a las especificaciones, el rendimiento de la formación de equipos NIC podría verse afectado. Además, en función de la configuración, la formación de equipos NIC puede enviar paquetes desde la misma dirección IP con varias direcciones MAC que responden a las características de seguridad del conmutador físico.

  
## <a name="hardware-that-doesnt-conform-to-specification"></a>Hardware que no se ajusta a la especificación  
  
Durante el funcionamiento normal, la formación de equipos NIC puede enviar paquetes de la misma dirección IP, pero con varias direcciones MAC. Según los estándares de protocolo, los receptores de estos paquetes deben resolver la dirección IP del host o la máquina virtual en una dirección MAC específica en lugar de responder a la dirección MAC desde el paquete receptor.  Los clientes que implementan correctamente los protocolos de resolución de direcciones (ARP y NDP) envían paquetes con las direcciones MAC de destino correctas, es decir, la dirección MAC de la máquina virtual o el host que posee esa dirección IP. 
  
Algunos hardware incrustado no implementan correctamente los protocolos de resolución de direcciones y, además, no pueden resolver explícitamente una dirección IP en una dirección MAC mediante ARP o NDP.  Por ejemplo, un controlador de red de área de almacenamiento (SAN) puede realizar de esta manera. Los dispositivos no conformes copian la dirección MAC de origen desde un paquete recibido y la usan como dirección MAC de destino en los paquetes salientes correspondientes, lo que da lugar a que los paquetes se envíen a una dirección MAC de destino incorrecta. Por este motivo, el conmutador virtual de Hyper-V quita los paquetes porque no coinciden con ningún destino conocido.  
  
Si tiene problemas para conectarse a controladores SAN o a otro hardware incrustado, debe realizar capturas de paquetes para determinar si el hardware está implementando correctamente ARP o NDP y ponerse en contacto con el proveedor de hardware para obtener soporte técnico.  

  
## <a name="physical-switch-security-features"></a>Características de seguridad del conmutador físico  
En función de la configuración, la formación de equipos NIC puede enviar paquetes de la misma dirección IP con varias direcciones MAC de origen que responden a las características de seguridad en el conmutador físico. Por ejemplo, la inspección dinámica de ARP o la protección de origen IP, especialmente si el conmutador físico no es consciente de que los puertos forman parte de un equipo, lo que sucede cuando se configura la formación de equipos NIC en el modo independiente del conmutador. Inspeccione los registros del conmutador para determinar si las características de seguridad del conmutador causan problemas de conectividad. 
  
## <a name="disabling-and-enabling-network-adapters-by-using-windows-powershell"></a>Deshabilitación y habilitación de adaptadores de red con Windows PowerShell  

Un motivo habitual por el que se produce un error en un equipo NIC es que la interfaz de equipo está deshabilitada y, en muchos casos, accidentalmente al ejecutar una secuencia de comandos.  Esta secuencia de comandos en particular no habilita todos los NetAdapters deshabilitados porque si se deshabilitan todos los miembros físicos subyacentes de NIC, se quita la interfaz de equipo NIC. 

En este caso, la interfaz de equipo NIC ya no se muestra en Get-NetAdapter y, por este motivo, **enable-NetAdapter \\** * no habilita el equipo NIC. El comando **enable-NetAdapter \\** *, sin embargo, habilita las NIC de miembros, que posteriormente (después de un breve período) vuelve a crear la interfaz de equipo. La interfaz de equipo permanece en el estado "deshabilitado" hasta que se vuelve a habilitar, lo que permite que el tráfico de red comience a fluir. 

La siguiente secuencia de comandos de Windows PowerShell puede deshabilitar la interfaz de equipo por accidente:  
  
```PowerShell 
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  

  
## <a name="related-topics"></a>Temas relacionados  
- [Formación de equipos NIC](NIC-Teaming.md): En este tema se proporciona información general sobre la formación de equipos de tarjeta de interfaz de red (NIC) en Windows Server 2016. La formación de equipos NIC le permite agrupar entre uno y 32 adaptadores de red Ethernet físicos en uno o varios adaptadores de red virtuales basados en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.   

- [Administración y uso de direcciones MAC de formación de equipos NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): Al configurar un equipo NIC con el modo independiente del conmutador y la distribución de la carga dinámica o el hash de la dirección, el equipo usa la dirección Media Access Control (MAC) del miembro del equipo NIC principal en el tráfico saliente. El miembro del equipo NIC principal es un adaptador de red seleccionado por el sistema operativo del conjunto inicial de miembros del equipo.

- [Configuración de la formación de equipos NIC](nic-teaming-settings.md): En este tema se proporciona información general sobre las propiedades del equipo NIC, como la formación de equipos y los modos de equilibrio de carga. También se proporcionan detalles acerca de la configuración del adaptador en espera y la propiedad de la interfaz de equipo principal. Si tiene al menos dos adaptadores de red en un equipo NIC, no es necesario designar un adaptador en espera para la tolerancia a errores.
  


