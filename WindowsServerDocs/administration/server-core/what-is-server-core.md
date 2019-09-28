---
title: ¿Qué es Server Core?
description: Más información acerca de la opción de instalación Server Core en Windows Server
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/20/2018
ms.openlocfilehash: 269be253367ba2bc692a5903e7d519a40f487d8b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383339"
---
# <a name="what-is-the-server-core-installation-option-in-windows-server"></a>¿Qué es la opción de instalación Server Core en Windows Server?

> Se aplica a: Windows Server 2019, Windows Server 2016 y Windows Server (canal semianual)

La opción Server Core es una opción de instalación mínima que está disponible cuando se implementa la edición Standard o Datacenter de Windows Server. Server Core incluye la mayoría de los roles de servidor, pero no todos. Server Core tiene una superficie de disco más pequeña y, por lo tanto, una superficie de ataque más pequeña debido a una base de código más pequeña. 

## <a name="server-core-vs-server-with-desktop-experience"></a>Servidor (Core) frente a servidor con experiencia de escritorio 
Al instalar Windows Server, solo se instalan los roles de servidor que elija; esto ayuda a reducir la superficie general de Windows Server. Sin embargo, la opción de instalación servidor con experiencia de escritorio todavía instala muchos servicios y otros componentes que no suelen ser necesarios para un escenario de uso determinado. 

Aquí es donde entra en juego Server Core: la instalación Server Core elimina los servicios y otras características que no son esenciales para la compatibilidad de determinados roles de servidor que se usan habitualmente. Por ejemplo, un servidor de Hyper-V no necesita una interfaz gráfica de usuario (GUI), ya que puede administrar prácticamente todos los aspectos de Hyper-V desde la línea de comandos mediante Windows PowerShell o de forma remota con el administrador de Hyper-V. 

## <a name="the-server-core-difference---core-capabilities-without-the-frills"></a>Las funcionalidades principales de diferencia Server Core sin frills
Al terminar de instalar Server Core en un sistema e iniciar sesión por primera vez, tiene un poco de sorpresa. La diferencia principal entre la opción de instalación servidor con experiencia de escritorio y Server Core es que Server Core no incluye los siguientes paquetes de Shell de GUI:

- Paquete Microsoft-Windows-Server-Shell-
- Microsoft-Windows-Server-GUI-MGMT-Package
- Microsoft-Windows-Server-GUI-RSAT-Package
- Microsoft-Windows-Cortana-PAL-Desktop-Package

En otras palabras, no hay **ningún escritorio** en Server Core, por diseño. A la vez que se mantienen las capacidades necesarias para admitir las aplicaciones empresariales tradicionales y las cargas de trabajo basadas en roles, Server Core no tiene una interfaz de escritorio tradicional. En su lugar, Server Core está diseñado para administrarse de forma remota a través de la línea de comandos, PowerShell o una herramienta de GUI (como [rsat](../../remote/remote-server-administration-tools.md) o el [centro de administración de Windows](../../manage/windows-admin-center/overview.md)).

Además de ninguna interfaz de usuario, Server Core también difiere del servidor con experiencia de escritorio de las siguientes maneras:

- Server Core no tiene ninguna herramienta de accesibilidad
- No hay OOBE (experiencia rápida) para configurar Server Core
- Sin soporte de audio

En la tabla siguiente se muestran las aplicaciones que están disponibles *localmente* en Server Core y Server con experiencia de escritorio. **Importantes**: En la mayoría de los casos, las aplicaciones que se enumeran como "no disponibles" a continuación se pueden ejecutar de forma remota desde un equipo cliente de Windows y usarse para administrar la instalación Server Core.

> [!NOTE]
> Esta lista está pensada para una referencia rápida: no pretende ser una lista completa.


| Application                     | Server Core     | Servidor con Experiencia de escritorio |
|------------------------------------|-----------------|--------------------------------|
| Símbolo del sistema                     | disponible       | disponible                      |
| Windows PowerShell/Microsoft .NET | disponible       | disponible                      |
| Perfmon. exe                        | no está disponible  | disponible                      |
| WinDbg (GUI)                         | admitido       | admitido                      |
| Resmon. exe                         | no está disponible   | disponible                      |
| Regedit                            | disponible       | disponible                      |
| Fsutil. exe                         | disponible       | disponible                      |
| Disksnapshot. exe                   | no está disponible   | disponible                      |
| Diskpart.exe                       | disponible       | disponible                      |
| Diskmgmt. msc                       | no está disponible   | disponible                      |
| Devmgmt. msc                        | no está disponible   | disponible                      |
| Administrador de servidores                     | no está disponible  | disponible                      |
| MMC. exe                            | no está disponible   | disponible                      |
| Eventvwr                           | no está disponible  | disponible                      |
| Wevtutil (consultas de eventos)           | disponible       | disponible                      |
| Services. msc                       | no está disponible   | disponible                      |
| Panel de Control                      | no está disponible   | disponible                      |
| Windows Update (GUI)                 | no está disponible | disponible                      |
| Explorador de Windows                   | no está disponible   | disponible                      |
| Barra de tareas                            | no está disponible   | disponible                      |
| Notificaciones de la barra de tareas              | no está disponible   | disponible                      |
| Taskmgr                            | disponible       | disponible                      |
| Internet Explorer o perimetral          | no está disponible   | disponible                      |
| Sistema de ayuda incorporado               | no está disponible   | disponible                      |
| Shell de Windows 10                   | no está disponible   | disponible                      |
| Reproductor de Windows Media               | no está disponible   | disponible                      |
| PowerShell                         | disponible       | disponible                      |
| PowerShell ISE                     | no está disponible   | disponible                      |
| IME de PowerShell                     | disponible       | disponible                      |
| Mstsc. exe                          | no está disponible   | disponible                      |
| Servicios de Escritorio remoto            | disponible       | disponible                      |
| Administrador de Hyper-V                    | no está disponible  | disponible                      |


Para obtener más información acerca de lo que *se* incluye en Server Core, vea [roles, servicios de rol y características incluidos en Windows Server-Server Core](server-core-roles-and-services.md). Y para obtener información sobre lo que *no se* incluye en Server Core, vea [roles, servicios de rol y características que no se incluyen en Server Core](server-core-removed-roles.md)

## <a name="get-started-using-server-core"></a>Introducción al uso de Server Core
Use la siguiente información para instalar, configurar y administrar la opción de instalación Server Core de Windows Server.

Instalación Server Core: 
- [Roles, servicios de rol y características incluidos en Server Core](server-core-roles-and-services.md)
- [Roles, servicios de rol y características no en Server Core](server-core-removed-roles.md)
- [Instalación de la opción de instalación Server Core](../../get-started/getting-started-with-server-core.md)
- [Configurar Server Core con la herramienta SConfig](../../get-started/sconfig-on-ws2016.md)

Usar Server Core:
- [Tareas básicas de administración de Server Core con Windows PowerShell o la línea de comandos](server-core-administer.md)
- [Administrar Server Core](server-core-manage.md)
- [Revisión de Server Core](server-core-servicing.md)
- [Configurar los archivos de volcado de memoria](server-core-memory-dump.md)