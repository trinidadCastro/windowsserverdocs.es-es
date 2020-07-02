---
title: SMBv1 no se instala de forma predeterminada en Windows 10 versión 1709, Windows Server versión 1709 y versiones posteriores
description: Describe el comportamiento del protocolo SMBv1 en Windows 10 Fall Creators Update y Windows Server, versión 1709 y versiones posteriores.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 07/01/2020
ms.openlocfilehash: 18c315a8b3562c25b5fe1c537a8922fc148e444b
ms.sourcegitcommit: c40c29683d25ed75b439451d7fa8eda9d8d9e441
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2020
ms.locfileid: "85833291"
---
# <a name="smbv1-is-not-installed-by-default-in-windows-10-version-1709-windows-server-version-1709-and-later-versions"></a>SMBv1 no se instala de forma predeterminada en Windows 10 versión 1709, Windows Server versión 1709 y versiones posteriores

## <a name="summary"></a>Resumen

En Windows 10 Fall Creators Update y Windows Server, versión 1709 (RS3) y versiones posteriores, el protocolo de red del bloque de mensajes del servidor versión 1 (SMBv1) ya no se instala de forma predeterminada. Se ha sustituido por los protocolos SMBv2 y posteriores a partir de 2007. Microsoft ha dejado de usar públicamente el protocolo SMBv1 en 2014. 

SMBv1 tiene el siguiente comportamiento en Windows 10 y Windows Server a partir de la versión 1709 (RS3): 
 
- SMBv1 Now tiene características secundarias de cliente y servidor que se pueden desinstalar por separado.    
- Windows 10 Enterprise, Windows 10 Education y Windows 10 Pro para estaciones de trabajo ya no contienen el cliente o servidor de SMBv1 de forma predeterminada después de una instalación limpia.    
- Windows Server 2016 ya no contiene el cliente o servidor de SMBv1 de forma predeterminada después de una instalación limpia.    
- Windows 10 Home y Windows 10 Pro ya no contienen el servidor SMBv1 de forma predeterminada después de una instalación limpia.    
- Windows 10 Home y Windows 10 Pro todavía contienen el cliente de SMBv1 de forma predeterminada después de una instalación limpia. Si el cliente de SMBv1 no se usa durante 15 días en total (excluyendo el equipo que se está desactivando), se desinstala automáticamente.    
- Las actualizaciones en contexto y los vuelos internos de Windows 10 Home y Windows 10 Pro no quitan SMBv1 inicialmente. Si el cliente o el servidor de SMBv1 no se utiliza durante 15 días en total (excluyendo el tiempo durante el que el equipo está apagado), cada uno de ellos se desinstalará automáticamente.     
- Las actualizaciones en contexto y los vuelos internos de las ediciones Windows 10 Enterprise, Windows 10 Education y Windows 10 Pro for Workstations no quitan automáticamente SMBv1. Un administrador debe decidir desinstalar SMBv1 en estos entornos administrados. 
- La eliminación automática de SMBv1 después de 15 días es una operación única. Si un administrador vuelve a instalar SMBv1, no se realizarán más intentos para desinstalarlo.
- Las características de SMB versión 2,02, 2,1, 3,0, 3,02 y 3.1.1 siguen siendo totalmente compatibles y se incluyen de forma predeterminada como parte de los binarios de SMBv2.    
- Dado que el servicio Examinador de equipos se basa en SMBv1, el servicio se desinstala si se desinstala el cliente o el servidor de SMBv1. Esto significa que la red del explorador ya no puede mostrar equipos Windows mediante el método de exploración de datagramas NetBIOS heredado.    
- SMBv1 todavía se pueden volver a instalar en todas las ediciones de Windows 10 y Windows Server 2016.    

SMBv1 tiene los siguientes comportamientos adicionales en Windows 10 a partir de la versión 1809 (RS5). Todos los demás comportamientos de la versión 1709 siguen siendo aplicables:

- Windows 10 Pro ya no contiene el cliente de SMBv1 de forma predeterminada después de una instalación limpia.
- En Windows 10 Enterprise, Windows 10 Education y Windows 10 Pro para estaciones de trabajo, un administrador puede activar la eliminación automática de SMBv1 activando la característica "eliminación automática de SMB 1.0/CIFS".

  > [!NOTE]
  > Windows 10, versión 1803 (RS4) Pro controla SMBv1 de la misma manera que Windows 10, versión 1703 (RS2) y Windows 10, versión 1607 (RS1). Este problema se corrigió en Windows 10, versión 1809 (RS5). Todavía puede desinstalar SMBv1 manualmente. Sin embargo, Windows no desinstalará SMBv1 automáticamente después de 15 días en los siguientes escenarios: 

-  Realiza una instalación limpia de Windows 10, versión 1803.     
-  Actualice Windows 10, versión 1607 o Windows 10, versión 1703 a Windows 10, versión 1803 directamente sin actualizar primero a Windows 10, versión 1709.     
 
Si intenta conectarse a dispositivos que solo admiten SMBv1, o si estos dispositivos intentan conectarse a usted, puede recibir uno de los siguientes mensajes de error:     

```
You can't connect to the file share because it's not secure. This share requires the obsolete SMB1 protocol, which is unsafe and could expose your system to attack.
Your system requires SMB2 or higher. For more info on resolving this issue, see: https://go.microsoft.com/fwlink/?linkid=852747  
```

```
The specified network name is no longer available.
```

```
Unspecified error 0x80004005
```

```
System Error 64
```

```
The specified server cannot perform the requested operation.
```

```
Error 58
```    

Los siguientes eventos aparecen cuando un servidor remoto requiere una conexión SMBv1 desde este cliente, pero SMBv1 se desinstala o deshabilita en el cliente.

```
Log Name:      Microsoft-Windows-SmbClient/Security
 Source:        Microsoft-Windows-SMBClient
 Date:          Date/Time
 Event ID:      32002
 Task Category: None
 Level:         Info
 Keywords:      (128)
 User:          NETWORK SERVICE
 Computer:      junkle.contoso.com
 Description:
 The local computer received an SMB1 negotiate response. 

Dialect: 
SecurityMode 
Server name: 

Guidance: 
SMB1 is deprecated and should not be installed nor enabled. For more information, see https://go.microsoft.com/fwlink/?linkid=852747.
```

```
Log Name:      Microsoft-Windows-SmbClient/Security
 Source:        Microsoft-Windows-SMBClient
 Date:          Date/Time
 Event ID:      32000
 Task Category: None
 Level:         Info
 Keywords:      (128)
 User:          NETWORK SERVICE
 Computer:      junkle.contoso.com
 Description: 
SMB1 negotiate response received from remote device when SMB1 cannot be negotiated by the local computer. 
Dialect: 
Server name: 

Guidance: 
The client has SMB1 disabled or uninstalled. For more information: https://go.microsoft.com/fwlink/?linkid=852747.     
```

Es probable que estos dispositivos no ejecuten Windows.Es más probable que ejecuten versiones anteriores de Linux, Samba u otros tipos de software de terceros para proporcionar servicios SMB. A menudo, estas versiones de Linux y samba ya no se admiten. 

> [!NOTE]
> Windows 10, versión 1709 también se conoce como "Fall Creators Update".   

## <a name="more-information"></a>Más información

Para solucionar este problema, póngase en contacto con el fabricante del producto que solo admite SMBv1 y solicite una actualización de software o firmware que admita SMBv 2.02 o una versión posterior. Para obtener una lista actualizada de los proveedores conocidos y sus requisitos de SMBv1, consulte el siguiente artículo del blog del equipo de ingeniería de almacenamiento de Windows y Windows Server: 

[Centro de activación de productos de SMBv1](https://techcommunity.microsoft.com/t5/Storage-at-Microsoft/SMB1-Product-Clearinghouse/ba-p/426008) 
#### <a name="leasing-mode"></a>Modo de concesión

Si se requiere SMBv1 para proporcionar compatibilidad de aplicaciones para el comportamiento del software heredado, como un requisito para deshabilitar bloqueos oportunistas, Windows proporciona una nueva marca de recurso compartido de SMB que se conoce como modo de concesión. Esta marca especifica si un recurso compartido deshabilita la semántica moderna de SMB, como concesiones y bloqueos oportunistas.

Puede especificar un recurso compartido sin usar bloqueos oportunistas o leasing para permitir que una aplicación heredada funcione con SMBv2 o una versión posterior. Para ello, use los cmdlets de PowerShell **New-SmbShare** o **set-SmbShare** junto con el parámetro **-LeasingMode None**   .

> [!NOTE]
> Esta opción solo se debe usar en los recursos compartidos que requiera una aplicación de terceros para la compatibilidad heredada si el proveedor indica que es necesario. No especifique el modo de concesión en recursos compartidos de datos de usuario o recursos compartidos de CA utilizados por servidores de archivos de escalabilidad horizontal. Esto se debe a que la eliminación de bloqueos oportunistas y concesiones provoca daños en los datos y la inestabilidad en la mayoría de las aplicaciones. El modo de concesión solo funciona en modo de uso compartido. Lo puede usar cualquier sistema operativo cliente.

#### <a name="explorer-network-browsing"></a>Exploración de red del explorador

El servicio Examinador de equipos se basa en el protocolo SMBv1 para rellenar el nodo de red del explorador de Windows (también conocido como "entorno de red"). Este protocolo heredado es largo en desuso, no se enruta y tiene una seguridad limitada. Dado que el servicio no puede funcionar sin SMBv1, se quita al mismo tiempo.

Sin embargo, si todavía tiene que usar el explorador Entrada de red los entornos Home y Small Business Workgroup para buscar equipos basados en Windows, puede seguir estos pasos en los equipos basados en Windows que ya no usan SMBv1: 
 
1. Inicie los servicios "host del proveedor de detección de funciones" y "publicación de recursos de detección de funciones" y, a continuación, establézcalo en **automático (Inicio retrasado)**.

2. Cuando abra el explorador de red, habilite la detección de redes cuando se le solicite.    
 
Todos los dispositivos de Windows de esa subred que tienen esta configuración aparecerán ahora en la red para la exploración. Usa el protocolo WS-DISCOVERY. Póngase en contacto con otros proveedores y fabricantes si sus dispositivos todavía no aparecen en esta lista de exploración después de que aparezcan los dispositivos Windows. Es posible que este protocolo esté deshabilitado o que solo admita SMBv1.

> [!NOTE]
> Se recomienda asignar unidades e impresoras en lugar de habilitar esta característica, que todavía requiere la búsqueda y exploración de sus dispositivos. Los recursos asignados son más fáciles de encontrar, requieren menos aprendizaje y son más seguros de usar. Esto es especialmente cierto si estos recursos se proporcionan automáticamente a través de directiva de grupo.Un administrador puede configurar impresoras para su ubicación mediante métodos distintos del servicio explorador de equipos heredado mediante el uso de direcciones IP, Active Directory Domain Services (AD DS), Bonjour, MDN, uPnP, etc.

Si no puede usar ninguna de estas soluciones alternativas o si el fabricante de la aplicación no puede proporcionar versiones compatibles de SMB, puede volver a habilitar SMBv1 manualmente siguiendo los pasos descritos en [Cómo detectar, habilitar y deshabilitar SMBv1, SMBv2 y SMBv3 en Windows](detect-enable-and-disable-smbv1-v2-v3.md).

> [!IMPORTANT]
> Le recomendamos encarecidamente que no vuelva a instalar SMBv1. Esto se debe a que este protocolo más antiguo tiene problemas de seguridad conocidos relacionados con ransomware y otro malware.  

#### <a name="windows-server-best-practices-analyzer-messaging"></a>Mensajería del analizador de procedimientos recomendados de Windows Server

Los sistemas de operaciones de servidor Windows Server 2012 y versiones posteriores contienen un analizador de procedimientos recomendados (BPA) para los servidores de archivos. Si ha seguido las instrucciones en línea correctas para desinstalar SMB1, la ejecución de este BPA devolverá un mensaje de advertencia contradictorio:

    Title: The SMB 1.0 file sharing protocol should be enabled
    Severity: Warning
    Date: 3/25/2020 12:38:47 PM
    Category: Configuration
    Problem: The Server Message Block 1.0 (SMB 1.0) file sharing protocol is disabled on this file server.
    Impact: SMB not in a default configuration, which could lead to less than optimal behavior.
    Resolution: Use Registry Editor to enable the SMB 1.0 protocol.

Debe omitir la guía de esta regla de BPA específica, que está en desuso. Repetimos: no habilite SMB 1,0.

## <a name="references"></a>Referencias

[Dejar de usar SMB1](https://aka.ms/stopusingsmb1)
