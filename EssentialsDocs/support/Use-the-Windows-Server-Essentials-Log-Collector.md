---
title: Usar el recolector de registro de Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
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
ms.openlocfilehash: d003c6a45159548f7e34d86ca242f74868659d2f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="use-the-windows-server-essentials-log-collector"></a>Usar el recolector de registro de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Para solucionar problemas del equipo, puede solicitar un representante del servicio al cliente de Microsoft y soporte técnico para recopilar registros de servidores, equipos de la red, o ambos mediante el recolector de registro de Windows Server Essentials.  
  
 El recolector de registro copia los registros del programa, registros de eventos revisor e información del entorno relacionados en un solo archivo zip en una ubicación especificada. Puedes ejecutar el recolector de registro directamente desde el servidor o en cualquier equipo en la red, o mediante una conexión remota a los equipos.  
  
> [!NOTE]
>  -   El recolector de registro no analizar los problemas de red o realizar cambios en cualquier equipo en la red o el servidor. Para obtener información acerca de cómo solucionar problemas de red, consulta la documentación de ayuda para el producto de servidor.  
> -   En esta guía, los equipos de la red, que no sean de tu servidor, se denominan equipos de la red.  
> -   [Descargar el paquete de instalación de Windows Server Essentials registro recopilador](https://go.microsoft.com/fwlink/?LinkID=266341).  
  
 Para instalar y ejecutar el recolector de registro, realiza los pasos en los siguientes temas:  
  

1.  [Instalar al selector de registro](Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [Ejecuta el selector de registro](Run-the-Windows-Server-Essentials-Log-Collector.md)  

1.  [Instalar al selector de registro](../support/Install-the-Windows-Server-Essentials-Log-Collector.md)  
  
2.  [Ejecuta el selector de registro](../support/Run-the-Windows-Server-Essentials-Log-Collector.md)  

  
## <a name="environment-information-collected"></a>Recopilar información del entorno  
 Para cada equipo de la red o el servidor que especifique, el recolector de registro recopila la siguiente información de entorno y lo coloca en el archivo de registro de la colección.  
  
-   Versión del sistema operativo  
  
-   Descripción y el fabricante de CPU  
  
-   Asignación y la cantidad de memoria  
  
-   Adaptadores de red que están enlazados a TCP/IP  
  
-   Configuración regional  
  
-   Procesos  
  
-   Configuración de almacenamiento  
  
-   Información de archivo de host  
  
-   Registros de eventos como la aplicación, sistema, Windows Server y Media Center  
  
-   Mensajes del Administrador de Control de servicio  
  
-   Reiniciar eventos y eventos de Windows Update  
  
-   Errores del sistema y los errores de la aplicación  
  
## <a name="services-information-collected"></a>Información recopilada de servicios  
  
### <a name="server-services"></a>Servicios de servidor  
  
-   Servicio copia de seguridad del equipo de cliente de Windows Server  
  
-   Servicio de copia de seguridad del proveedor de equipo cliente de Windows Server  
  
-   Proveedor de dispositivos de Windows Server  
  
-   Administración del nombre de dominio de Windows Server  
  
-   Registro del proveedor de servicio de servidor de Windows  
  
-   Proveedor de configuración del servidor de Windows  
  
-   Servicio de dispositivo UPnP de Windows Server  
  
-   Proveedor de administración de acceso Web remoto de Windows Server  
  
-   Servicio de estado del servidor de Windows  
  
-   Servicio de almacenamiento del servidor de Windows  
  
-   Servicio de Windows Server SQM  
  
### <a name="network-computer-services"></a>Servicios del equipo de red  
  
-   Servicio de copia de seguridad del proveedor de equipo cliente de Windows Server  
  
-   Servicio de estado del servidor de Windows  
  
-   Registro del proveedor de servicio de servidor de Windows  
  
-   Servicio de Windows Server SQM  
  
## <a name="logs-and-registry-information-collected"></a>Registros y la información de registro recopilado  
 Para cada equipo de la red o un servidor especificado, el recolector de registro recopila información de registro y registro desde el servidor y equipo de la red como sigue.  
  
### <a name="server-logs-and-registry-information"></a>Registros de servidor y la información del registro  
  
-   Registros de productos de servidor, desde \Microsoft\Windows Server\Logs < ProgramData\ >  
  
-   Tareas programadas  
  
-   Registros de la API de instalación  
  
-   Registros de Windows Update  
  
-   Archivo de alertas de estado  
  
-   Archivo de información de dispositivos  
  
-   Archivo de registro de copia de seguridad de servidor  
  
-   Archivo de registro Panther  
  
-   Servicios  
  
-   Claves del registro, desde  
  
    -   \\\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DevicesProviderSvc  
  
    -   \\\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DomainManagerProviderSvc  
  
### <a name="network-computer-logs-and-registry-information"></a>Registros de equipo de la red y la información del registro  
  
-   Registros de producto de equipo de red en \Microsoft\Windows Server\Logs < ProgramData\ >  
  
-   Archivo de alertas de estado en \Microsoft\Windows Server\Data < ProgramData\ >  
  
-   Registros de Windows Update  
  
-   Registros de la API de instalación  
  
-   Información de las tareas programadas  
  
-   Claves del registro de \\\HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows Server\  
  
## <a name="logs-for-computers-that-do-not-run-a-version-of-the-windows-operating-system"></a>Registros para equipos que ejecuten una versión del sistema operativo Windows  
 El recolector de registro no recopilar archivos de registro de los equipos que ejecutan una versión del sistema operativo Windows. Para equipos no son de Windows, copiar manualmente los siguientes archivos de registro en la misma ubicación donde se almacenan los archivos de registro selector.  
  
-   System.log  
  
-   Server.log biblioteca/registros de Windows  
  
-   Biblioteca/registros/CrashReporter/LaunchPad-< nnn\ > (copiar todos los archivos de .crash LaunchPad-< nnn\ >)  
  
-   Biblioteca/registros/DiagnosticReports/LaunchPad-< nnn\ > (copiar todos los archivos de .crash LaunchPad-< nnn\ >)  
  
## <a name="see-also"></a>Consulta también  
  

-   [Solucionar errores de selector de registro](Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

-   [Solucionar errores de selector de registro](../support/Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

