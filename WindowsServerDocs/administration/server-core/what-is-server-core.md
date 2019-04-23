---
title: ¿Qué es Server Core?
description: Obtenga información sobre la opción de instalación Server Core en Windows Server
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 08229e458d0aa0c8e8397f0f053f37a207a1aea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885606"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>¿Qué es la opción de instalación Server Core en Windows Server?

> Se aplica a: Windows Server (canal semianual) y Windows Server 2016

La opción Server Core es una opción de instalación mínima que está disponible al implementar la edición Standard o Datacenter de Windows Server. Server Core incluye la mayoría pero no todos los roles de servidor. Server Core tiene una menor superficie de disco y, por lo tanto, una menor superficie de ataque debido a una base de código más pequeña. 

## <a name="server-core-vs-server-with-desktop-experience"></a>Servidor (Core) frente a servidor con experiencia de escritorio 
Cuando se instala Windows Server, instale los roles de servidor que elija, esto ayuda a reducir la superficie general para Windows Server. Sin embargo, el servidor con la opción de instalación de experiencia de escritorio todavía instala muchos servicios y otros componentes que a menudo no son necesarios para un escenario de uso concreto. 

Aquí es donde Server Core entra en juego: la instalación Server Core elimina todos los servicios y otras características que no son esenciales para la compatibilidad de ciertos normalmente usan roles de servidor. Por ejemplo, un servidor de Hyper-V no necesita una interfaz gráfica de usuario (GUI), ya que puede administrar prácticamente todos los aspectos de Hyper-V desde la línea de comandos con Windows PowerShell o mediante el Administrador de Hyper-V de forma remota. 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>La diferencia de Server Core - funcionalidades básicas sin el frills
Cuando termine la instalación de Server Core en un sistema y de inicio de sesión por primera vez, tiene un bit de una sorpresa. La diferencia principal entre el servidor con la opción de instalación de experiencia de escritorio y Server Core es que Server Core no incluye los siguientes paquetes de shell de interfaz gráfica de usuario:

- Microsoft-Windows-Server-Shell-Package
- Microsoft-Windows-Server-Gui-Mgmt-Package
- Microsoft-Windows-Server-Gui-RSAT-Package
- Microsoft-Windows-Cortana-PAL-Desktop-Package

En otras palabras, no hay **escritorio no** en Server Core, por diseño. Además de conservar las capacidades necesarias para admitir aplicaciones de negocio tradicionales y las cargas de trabajo basado en roles, Server Core no tiene una interfaz de escritorio tradicionales. En su lugar, Server Core está diseñado para administrar de forma remota a través de la línea de comandos, PowerShell o una herramienta de interfaz gráfica de usuario (como [RSAT](../../remote/remote-server-administration-tools.md) o [Windows Admin Center](../../manage/windows-admin-center/overview.md)).

Además de ninguna interfaz de usuario, Server Core también difiere el servidor con experiencia de escritorio de las maneras siguientes:

- Server Core no tiene las herramientas de accesibilidad
- No hay OOBE (out-de-box-experience) para la configuración de Server Core
- No hay compatibilidad con audio

La siguiente tabla muestra las aplicaciones que están disponibles *localmente* en Server Core o servidor con experiencia de escritorio. **Importantes**: En la mayoría de los casos, las aplicaciones que se enumeran como "no disponible" a continuación se puede ejecutar remotamente desde un equipo cliente de Windows y se usa para administrar la instalación de Server Core.

> [!NOTE]
> Esta lista está pensada para referencia rápida: no está pensada como una lista completa.


| Application                     | Server Core     | Servidor con Experiencia de escritorio |
|------------------------------------|-----------------|--------------------------------|
| Símbolo del sistema                     | disponible       | disponible                      |
| Windows PowerShell/ Microsoft .NET | disponible       | disponible                      |
| Perfmon.exe                        | no está disponible  | disponible                      |
| Windbg (GUI)                         | admitido       | admitido                      |
| Resmon.exe                         | no está disponible   | disponible                      |
| Regedit                            | disponible       | disponible                      |
| Fsutil.exe                         | disponible       | disponible                      |
| Disksnapshot.exe                   | no está disponible   | disponible                      |
| Diskpart.exe                       | disponible       | disponible                      |
| Diskmgmt.msc                       | no está disponible   | disponible                      |
| Devmgmt.msc                        | no está disponible   | disponible                      |
| Administrador de servidores                     | no está disponible  | disponible                      |
| Mmc.exe                            | no está disponible   | disponible                      |
| Eventvwr                           | no está disponible  | disponible                      |
| Wevtutil (consultas de eventos)           | disponible       | disponible                      |
| Services.msc                       | no está disponible   | disponible                      |
| Panel de Control                      | no está disponible   | disponible                      |
| Actualización de Windows (GUI)                 | no está disponible | disponible                      |
| Explorador de Windows                   | no está disponible   | disponible                      |
| Barra de tareas                            | no está disponible   | disponible                      |
| Notificaciones de la barra de tareas              | no está disponible   | disponible                      |
| Taskmgr                            | disponible       | disponible                      |
| Internet Explorer o Edge          | no está disponible   | disponible                      |
| Sistema de ayuda incorporado               | no está disponible   | disponible                      |
| Shell de Windows 10                   | no está disponible   | disponible                      |
| Reproductor de Windows Media               | no está disponible   | disponible                      |
| PowerShell                         | disponible       | disponible                      |
| PowerShell ISE                     | no está disponible   | disponible                      |
| IME de PowerShell                     | disponible       | disponible                      |
| Mstsc.exe                          | no está disponible   | disponible                      |
| Servicios de Escritorio remoto            | disponible       | disponible                      |
| Administrador de Hyper-V                    | no está disponible  | disponible                      |


Para obtener más información acerca de qué *es* incluido en Server Core, vea [Roles, servicios de rol y características incluidas en Windows Server - Server Core](server-core-roles-and-services.md). Y para obtener información acerca de qué *no* incluido en Server Core, vea [Roles, servicios de rol y características no incluidas en Server Core](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>Introducción al uso de Server Core
Use la información siguiente para instalar, configurar y administrar la opción de instalación Server Core de Windows Server.

Instalación de Server Core: 
- [Roles, servicios de rol y características incluidas en Server Core](server-core-roles-and-services.md)
- [Roles, servicios de rol y características no incluidas en Server Core](server-core-removed-roles.md)
- [Instalar la opción de instalación Server Core](../../get-started/getting-started-with-server-core.md)
- [Configuración de Server Core con la herramienta SConfig](../../get-started/sconfig-on-ws2016.md)

Uso de Server Core:
- [Tareas básicas de administración de Server Core con Windows PowerShell o la línea de comandos](server-core-administer.md)
- [Administrar Server Core](server-core-manage.md)
- [Aplicación de revisiones de Server Core](server-core-servicing.md)
- [Configurar archivos de volcado de memoria](server-core-memory-dump.md)