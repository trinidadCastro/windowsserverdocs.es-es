---
title: Solución de problemas de formación de equipos NIC
description: En este tema se proporciona información sobre solución de problemas de formación de equipos NIC en Windows Server 2016.
manager: dougkim
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
ms.date: 09/13/2018
ms.openlocfilehash: d39dc6a4dcf5dca8186b0599fb479ed5ae684e0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856256"
---
# <a name="troubleshooting-nic-teaming"></a>Solución de problemas de formación de equipos NIC

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema, se describen formas de solucionar la formación de equipos NIC, como el hardware y los valores de conmutador físico.  Cuando las implementaciones de hardware de protocolos estándar no se ajustan a las especificaciones, podría afectar al rendimiento de la formación de equipos NIC. Además, según la configuración, la formación de equipos NIC puede enviar paquetes desde la misma dirección IP con varias direcciones de MAC, lo que activa las características de seguridad en el conmutador físico.

  
## <a name="hardware-that-doesnt-conform-to-specification"></a>Hardware que no cumple la especificación  
  
Durante el funcionamiento normal, la formación de equipos NIC puede enviar paquetes desde la misma dirección IP, pero con varias direcciones MAC. Según los estándares de protocolo, los receptores de estos paquetes deben resolver la dirección IP del host o máquina virtual a una dirección MAC específica en lugar de responder a la dirección MAC del receptor de paquetes.  Los clientes que implementan correctamente los protocolos de resolución de direcciones (ARP y NDP) envían paquetes con las direcciones MAC de destino correcto, es decir, la dirección MAC de la máquina virtual o un host que posee esa dirección IP. 
  
Algunos tipos de hardware incrustado implementar correctamente los protocolos de resolución de direcciones y también podrían no resolver explícitamente una dirección IP a una dirección MAC con ARP o NDP.  Por ejemplo, podría realizar una controladora de red (SAN) del área de almacenamiento de esta manera. Dispositivos no conformes copien la dirección MAC de origen de un paquete recibido y, a continuación, usarla como la dirección MAC de destino en los paquetes salientes correspondientes, lo que los paquetes enviados a la dirección MAC de destino incorrecto. Por este motivo, se quitan los paquetes mediante el conmutador Virtual de Hyper-V porque no coinciden con cualquier destino conocido.  
  
Si está teniendo problemas conectarse a controladores de SAN u otros incrustados hardware, debe tomar capturas de paquetes para determinar si el hardware implementa correctamente ARP o NDP y póngase en contacto con el fabricante del hardware para obtener soporte técnico.  

  
## <a name="physical-switch-security-features"></a>Características de seguridad del conmutador físico  
Según la configuración, la formación de equipos NIC puede enviar paquetes desde la misma dirección IP con varias direcciones de MAC de origen, lo que activa las características de seguridad en el conmutador físico. Por ejemplo, inspección dinámica ARP o protección del origen IP, especialmente si el conmutador físico no es consciente de que los puertos son parte de un equipo, lo que sucede cuando se configura la formación de equipos NIC en el modo independiente de conmutador. Inspeccione los registros de modificador para determinar si las características de seguridad de conmutador están causando problemas de conectividad. 
  
## <a name="disabling-and-enabling-network-adapters-by-using-windows-powershell"></a>Deshabilitar y habilitar los adaptadores de red mediante Windows PowerShell  

Una razón habitual para un equipo NIC producir un error es que la interfaz de equipo está deshabilitada y en muchos casos, por accidente, cuando se ejecuta una secuencia de comandos.  Esta particular secuencia de comandos no permitir que todos los NetAdapters deshabilitado porque deshabilitar todos los miembros físicos subyacentes de NIC quita la interfaz del equipo NIC. 

En este caso, la interfaz del equipo NIC ya no se muestra en Get-NetAdapter y, por este motivo, **Enable-NetAdapter \***  no permite que el equipo NIC. El **Enable-NetAdapter \***  comandos, habilitar sin embargo, el miembro de las NIC, que, a continuación, (después de una hora corta) vuelve a crear la interfaz de equipo. La interfaz del equipo permanece en estado "deshabilitado" hasta que vuelva a habilitar, lo que permite el tráfico de red empezar a fluir. 

La secuencia de comandos de Windows PowerShell siguiente puede deshabilitar la interfaz de equipo por accidente:  
  
```PowerShell 
Disable-NetAdapter *  
Enable-NetAdapter *  
```  
  

  
## <a name="related-topics"></a>Temas relacionados  
- [Formación de equipos NIC](NIC-Teaming.md): En este tema, le proporcionamos una visión general de formación de equipos de tarjeta de interfaz de red (NIC) en Windows Server 2016. Formación de equipos NIC permite agrupar entre 1 y 32 adaptadores de red Ethernet físicos en uno o más adaptadores de red virtual basada en software. Estos adaptadores de red virtuales proporcionan un rendimiento rápido y tolerancia a errores en caso de que se produzca un error en el adaptador de red.   

- [Administración y uso de direcciones MAC de la formación de equipos NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): Al configurar un equipo NIC con el modo independiente de conmutador y hash de dirección o distribución de carga dinámica, el equipo usa que la media access control dirección (MAC) del miembro del equipo NIC principal en el tráfico saliente. El miembro del equipo NIC principal es un adaptador de red seleccionado por el sistema operativo desde el conjunto inicial de los miembros del equipo.

- [Configuración de formación de equipos NIC](nic-teaming-settings.md): En este tema, nos ofrecerle una visión general de las propiedades del equipo NIC como formación de equipos y los modos de equilibrio de carga. También se proporcionan detalles sobre la configuración del adaptador en espera y la propiedad de la interfaz principal del equipo. Si tiene al menos dos adaptadores de red en un equipo NIC, no es necesario designar un adaptador de modo de espera para tolerancia a errores.
  


