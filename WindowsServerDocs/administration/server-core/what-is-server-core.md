---
title: ¿Qué es Server Core?
description: Obtenga información sobre la opción de instalación Server Core de Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 08229e458d0aa0c8e8397f0f053f37a207a1aea5
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "1718539"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>¿Qué es la opción de instalación Server Core de Windows Server?

> Se aplica a: (delimitadas anuales del canal) de Windows Server y Windows Server 2016

La opción Server Core es una opción de instalación mínima que está disponible cuando se va a implementar la edición Standard o Datacenter de Windows Server. Server Core incluye la mayoría, pero no todos los roles de servidor. Server Core tiene un espacio de disco más pequeño y, por lo tanto, una superficie de ataque más pequeña debido a una base de código más pequeña. 

## <a name="server-core-vs-server-with-desktop-experience"></a>Servidor (núcleo) frente a servidor con experiencia de escritorio 
Al instalar Windows Server, instalar sólo los roles de servidor que elija, esto ayuda a reducir el espacio general de Windows Server. Sin embargo, el servidor con la opción de instalación de experiencia de escritorio instala aún muchos servicios y otros componentes que a menudo no son necesarios para un escenario de uso concreto. 

Que es donde entra en juego la Server Core: la instalación Server Core elimina todos los servicios y otras características que no son esenciales para la compatibilidad de determinados suelen usan funciones de servidor. Por ejemplo, un servidor Hyper-V no necesita una interfaz gráfica de usuario (GUI), debido a que puede administrar prácticamente todos los aspectos de Hyper-V, ya sea desde la línea de comandos con Windows PowerShell o mediante el Administrador de Hyper-V de forma remota. 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>La diferencia de Server Core - funcionalidad básica sin el frills
Cuando termine de instalación Server Core en un sistema y el inicio de sesión por primera vez, está en un bit de una sorpresa. La diferencia principal entre el servidor con la opción de instalación de experiencia de escritorio y Server Core es que Server Core no incluye los siguientes paquetes de shell de la interfaz gráfica de usuario:

- Microsoft-Windows-Shell-paquete de servidor
- Microsoft-Windows-Server-Gui-Mgmt-Package
- Microsoft-Windows-Server-Gui-RSAT-Package
- Microsoft-Windows-Cortana-PAL-Desktop-Package

En otras palabras, no hay **ningún escritorio** en Server Core, por diseño. Al tiempo que mantiene las capacidades necesarias para admitir aplicaciones de negocio tradicionales y las cargas de trabajo basado en roles, Server Core no tiene una interfaz de escritorio tradicional. En su lugar, Server Core está diseñado para administrar de forma remota a través de la línea de comandos, PowerShell o una herramienta de interfaz gráfica de usuario (como [RSAT](../../remote/remote-server-administration-tools.md) o [Centro de administración de Windows](../../manage/windows-admin-center/overview.md)).

Además de ninguna interfaz de usuario, Server Core difiere también desde el servidor con la experiencia de escritorio de las siguientes maneras:

- Server Core no tiene las herramientas de accesibilidad
- No hay OOBE (out-de-cuadro-la experiencia) para la configuración de Server Core
- No hay compatibilidad con audio

La siguiente tabla muestra las aplicaciones que están disponibles *localmente* en Server Core vs Server con experiencia de escritorio. **Importante**: en la mayoría de los casos, las aplicaciones que se enumeran como "no disponible" a continuación puede ejecutarse de forma remota desde un equipo cliente de Windows y se usa para administrar la instalación Server Core.

> [!NOTE]
> Esta lista está pensado para referencia rápida - que no se pretende ser una lista completa.


| Aplicación                     | ServerCore     | Servidor con Experiencia de escritorio |
|------------------------------------|-----------------|--------------------------------|
| El símbolo del sistema                     | disponible       | disponible                      |
| Windows PowerShell / Microsoft .NET | disponible       | disponible                      |
| PerfMon.exe                        | no está disponible  | disponible                      |
| WinDbg (GUI)                         | admitido       | admitido                      |
| Resmon.exe                         | no está disponible   | disponible                      |
| Regedit                            | disponible       | disponible                      |
| Fsutil.exe                         | disponible       | disponible                      |
| Disksnapshot.exe                   | no está disponible   | disponible                      |
| Diskpart.exe                       | disponible       | disponible                      |
| Diskmgmt.msc                       | no está disponible   | disponible                      |
| Devmgmt.msc                        | no está disponible   | disponible                      |
| Administrador de servidores                     | no está disponible  | disponible                      |
| MMC.exe                            | no está disponible   | disponible                      |
| Eventvwr                           | no está disponible  | disponible                      |
| Wevtutil (consultas de eventos)           | disponible       | disponible                      |
| Services.msc                       | no está disponible   | disponible                      |
| Panel de control                      | no está disponible   | disponible                      |
| Actualización de Windows (GUI)                 | no está disponible | disponible                      |
| Explorador de Windows                   | no está disponible   | disponible                      |
| Barra de tareas                            | no está disponible   | disponible                      |
| Notificaciones de la barra de tareas              | no está disponible   | disponible                      |
| Taskmgr                            | disponible       | disponible                      |
| Internet Explorer o perimetral          | no está disponible   | disponible                      |
| Sistema de ayuda integrada               | no está disponible   | disponible                      |
| Shell de Windows 10                   | no está disponible   | disponible                      |
| Reproductor de Windows Media               | no está disponible   | disponible                      |
| PowerShell                         | disponible       | disponible                      |
| PowerShell ISE                     | no está disponible   | disponible                      |
| IME de PowerShell                     | disponible       | disponible                      |
| Mstsc.exe                          | no está disponible   | disponible                      |
| Servicios de Escritorio remoto            | disponible       | disponible                      |
| Administrador de Hyper-V                    | no está disponible  | disponible                      |


Para obtener más información acerca de qué *está* incluido en Server Core, vea [Roles, servicios de rol y características que se incluyen en Windows Server: Server Core](server-core-roles-and-services.md). Y para obtener información acerca de qué *no está* incluido en Server Core, vea [Roles, servicios de rol y características no incluidas en Server Core](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>Introducción al uso de Server Core
Use la información siguiente para instalar, configurar y administrar la opción de instalación Server Core de Windows Server.

Instalación de Server Core: 
- [Roles, servicios de rol y características que se incluyen en Server Core](server-core-roles-and-services.md)
- [Roles, servicios de rol y características no incluidas en Server Core](server-core-removed-roles.md)
- [Instalar la opción de instalación Server Core](../../get-started/getting-started-with-server-core.md)
- [Configuración básica del servidor con la herramienta SConfig](../../get-started/sconfig-on-ws2016.md)

Uso de Server Core:
- [Tareas básicas de administración de Server Core con Windows PowerShell o la línea de comandos](server-core-administer.md)
- [Administración de Server Core](server-core-manage.md)
- [Aplicación de revisiones de Server Core](server-core-servicing.md)
- [Configurar los archivos de volcado de memoria](server-core-memory-dump.md)