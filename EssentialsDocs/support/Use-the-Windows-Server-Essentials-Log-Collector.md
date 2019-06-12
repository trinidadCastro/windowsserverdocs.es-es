---
title: Uso del compilador de registros de Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c6985518-b42d-4cfb-9761-eaa75306b6d7
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ba5c0de9d8689c63c95ea3410a74fc9a7289aeab
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435992"
---
# <a name="use-the-windows-server-essentials-log-collector"></a>Uso del compilador de registros de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Al solucionar problemas del equipo, un representante del servicio al cliente de Microsoft y soporte técnico puede pedirle que recopile los registros de servidores, equipos de la red, o ambos mediante el uso de Windows Server Essentials Log Collector.  
  
 El Compilador de registros copia registros de programas, registros de revisión de eventos e información del entorno en cuestión en un solo archivo zip en una ubicación específica. Puede ejecutar el Compilador de registros directamente desde el servidor o cualquier equipo de la red, o mediante una conexión remota a los equipos.  
  
> [!NOTE]
> - El Compilador de registros no analiza los problemas de red ni realiza cambios en cualquier servidor o equipo de la red. Para obtener información sobre cómo solucionar problemas de red, consulte la documentación de ayuda del producto de su servidor.  
>   -   En esta guía, los equipos de la red, aparte de su servidor, se denominan equipos de la red.  
>   -   [Descargue el paquete de instalación de Windows Server Essentials Log Collector](https://go.microsoft.com/fwlink/?LinkID=266341).  
  
 Para instalar y ejecutar el Compilador de registros, realice los pasos que se indican en los temas siguientes:  
  

1.  [Instalar al recopilador de registros](Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [Ejecute el recopilador de registros](Run-the-Windows-Server-Essentials-Log-Collector.md)  

1.  [Instalar al recopilador de registros](../support/Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [Ejecute el recopilador de registros](../support/Run-the-Windows-Server-Essentials-Log-Collector.md)  

  
## <a name="environment-information-collected"></a>Información del entorno recopilada  
 Para cada equipo de la red o servidor que especifique, el Compilador de registros recopila la siguiente información sobre el entorno y la incluye en el archivo de compilación de registros.  
  
-   Versión del sistema operativo  
  
-   Descripción y fabricante de CPU  
  
-   Asignación y cantidad de memoria  
  
-   Adaptadores de red que están enlazados a TCP/IP  
  
-   Locale  
  
-   Procesos  
  
-   Configuración de almacenamiento  
  
-   Información de los archivos del host  
  
-   Registros de eventos, incluidos los eventos de aplicación, sistema, Windows Server y Media Center  
  
-   Mensajes del Administrador de control de servicios  
  
-   Eventos de reinicio y de Windows Update  
  
-   Errores del sistema y errores de aplicación  
  
## <a name="services-information-collected"></a>Información de servicios recopilada  
  
### <a name="server-services"></a>Servicios de servidor  
  
-   Servicio de copia de seguridad de equipos cliente de Windows Server  
  
-   Servicio de proveedores de copia de seguridad de equipos cliente de Windows Server  
  
-   Proveedor de dispositivos de Windows Server  
  
-   Administración de nombres de dominio de Windows Server  
  
-   Registro de proveedores de servicios de Windows Server  
  
-   Proveedor de configuración de Windows Server  
  
-   Servicio de dispositivos UPnP de Windows Server  
  
-   Proveedor de administración de Acceso web remoto de Windows Server  
  
-   Servicio de mantenimiento de Windows Server  
  
-   Servicio de almacenamiento de Windows Server  
  
-   Servicio de SQM de Windows Server  
  
### <a name="network-computer-services"></a>Servicios de equipos de red  
  
-   Servicio de proveedores de copia de seguridad de equipos cliente de Windows Server  
  
-   Servicio de mantenimiento de Windows Server  
  
-   Registro de proveedores de servicios de Windows Server  
  
-   Servicio de SQM de Windows Server  
  
## <a name="logs-and-registry-information-collected"></a>Registros e información del registro recopilada  
 En cada equipo de la red o servidor especificado, el Compilador de registros recopila información del registro y del servidor y del equipo de red de la siguiente manera.  
  
### <a name="server-logs-and-registry-information"></a>Registros e información del registro del servidor  
  
-   Registros de productos de servidor, de < ProgramData\>\Microsoft\Windows Server\Logs  
  
-   Tareas programadas  
  
-   Registros del API de instalación  
  
-   Registros de Windows Update  
  
-   Archivo de alertas de estado  
  
-   Archivo de información de dispositivos  
  
-   Archivo de registro de copia de seguridad del servidor  
  
-   Archivo de registro Panther  
  
-   Servicios  
  
-   Claves del registro, de  
  
    -   \\\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DevicesProviderSvc  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DomainManagerProviderSvc  
  
### <a name="network-computer-logs-and-registry-information"></a>Registros y la información del registro del equipo de red  
  
-   Los registros de producto del equipo de red en < ProgramData\>\Microsoft\Windows Server\Logs  
  
-   Archivo de alertas de estado en < ProgramData\>\Microsoft\Windows Server\Data  
  
-   Registros de Windows Update  
  
-   Registros del API de instalación  
  
-   Información de las tareas programadas  
  
-   Las claves del registro de \\\HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows Server\  
  
## <a name="logs-for-computers-that-do-not-run-a-version-of-the-windows-operating-system"></a>Registros para los equipos que no ejecutan una versión del sistema operativo Windows  
 El Compilador de registros no recopilar archivos de registro de los equipos que no ejecutan una versión del sistema operativo Windows. Para los equipos que no ejecutan Windows, copie manualmente los siguientes archivos de registro en la misma ubicación donde se almacenan los archivos del Compilador de registros.  
  
-   Registro del sistema  
  
-   Library/Logs/Windows Server.log  
  
-   Library/Logs/CrashReporter/LaunchPad-< nnn\> (copiar todo el LaunchPad-< nnn\>archivos .crash)  
  
-   Library/Logs/DiagnosticReports/LaunchPad-< nnn\> (copiar todo el LaunchPad-< nnn\>archivos .crash)  
  
## <a name="see-also"></a>Vea también  
  

-   [Solución de problemas de errores del compilador de registros](Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

-   [Solución de problemas de errores del compilador de registros](../support/Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

