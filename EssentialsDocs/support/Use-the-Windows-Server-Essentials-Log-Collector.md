---
title: Uso del compilador de registros de Windows Server Essentials
description: Obtenga información acerca de cómo usar el recopilador de registros de Windows Server Essentials para recopilar registros de los servidores, los equipos de la red o ambos.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: c6985518-b42d-4cfb-9761-eaa75306b6d7
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 4a540c52ee4cb84455f125c0d9f5c2f4ad0f3bf1
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/29/2020
ms.locfileid: "97810132"
---
# <a name="use-the-windows-server-essentials-log-collector"></a>Uso del compilador de registros de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Al solucionar problemas del equipo, un representante del servicio de atención al cliente y soporte técnico de Microsoft puede pedirle que recopile los registros de los servidores, los equipos de la red o ambos mediante el compilador de registros de Windows Server Essentials.

 El Compilador de registros copia registros de programas, registros de revisión de eventos e información del entorno en cuestión en un solo archivo zip en una ubicación específica. Puede ejecutar el Compilador de registros directamente desde el servidor o cualquier equipo de la red, o mediante una conexión remota a los equipos.

> [!NOTE]
>El Compilador de registros no analiza los problemas de red ni realiza cambios en cualquier servidor o equipo de la red. Para obtener información sobre cómo solucionar problemas de red, consulte la documentación de ayuda del producto de su servidor.
>En esta guía, los equipos de la red que no sean el servidor, se denominan equipos de red.
>
>[Descargue el paquete de instalación del recopilador de registros de Windows Server Essentials](https://www.microsoft.com/download/details.aspx?id=34821).

 Para instalar y ejecutar el Compilador de registros, realice los pasos que se indican en los temas siguientes:

1. [Instalar el compilador de registros](../support/Install-the-Windows-Server-Essentials-Log-Collector.md)

2. [Ejecutar el compilador de registros](../support/Run-the-Windows-Server-Essentials-Log-Collector.md)


## <a name="environment-information-collected"></a>Información del entorno recopilada
 Para cada equipo de la red o servidor que especifique, el Compilador de registros recopila la siguiente información sobre el entorno y la incluye en el archivo de compilación de registros.

-   Versión del sistema operativo

-   Descripción y fabricante de CPU

-   Asignación y cantidad de memoria

-   Adaptadores de red que están enlazados a TCP/IP

-   Configuración regional

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

-   Registros de producto de servidor, desde <ProgramData \> \Microsoft\Windows Server\Logs

-   Tareas programadas

-   Registros del API de instalación

-   Registros de Windows Update

-   Archivo de alertas de estado

-   Archivo de información de dispositivos

-   Archivo de registro de copia de seguridad del servidor

-   Archivo de registro Panther

-   Servicios

-   Claves del registro, de

    -   \\\ HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\

    -   \\\ HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DevicesProviderSvc

    -   \\\ HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DomainManagerProviderSvc

### <a name="network-computer-logs-and-registry-information"></a>Registros y la información del registro del equipo de red

-   Registros de producto del equipo de red en <ProgramData \> \Microsoft\Windows Server\Logs

-   Archivo de alertas de estado en <ProgramData \> \Microsoft\Windows Server\Data

-   Registros de Windows Update

-   Registros del API de instalación

-   Información de las tareas programadas

-   Claves del registro de \\ \ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows Server \

## <a name="logs-for-computers-that-do-not-run-a-version-of-the-windows-operating-system"></a>Registros para los equipos que no ejecutan una versión del sistema operativo Windows
 El Compilador de registros no recopilar archivos de registro de los equipos que no ejecutan una versión del sistema operativo Windows. Para los equipos que no ejecutan Windows, copie manualmente los siguientes archivos de registro en la misma ubicación donde se almacenan los archivos del Compilador de registros.

-   Registro del sistema

-   Library/Logs/Windows Server.log

-   Library/Logs/CrashReporter/LaunchPad-<nnn \> (Copie todos los archivos de Launchpad-<nnn \> . Crash)

-   Library/Logs/DiagnosticReports/LaunchPad-<nnn \> (Copie todos los archivos de Launchpad-<nnn \> . Crash)

## <a name="see-also"></a>Consulte también

-   [Solucionar problemas de errores del compilador de registros](../support/Troubleshoot-Windows-Server-Essentials-Log-Collector-Errors.md)

