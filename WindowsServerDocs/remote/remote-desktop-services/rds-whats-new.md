---
title: Novedades de servicios de escritorio remoto
description: Proporciona la descripción de las nuevas características RDS en Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/11/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04d52dff-e61b-4633-9908-be8600abc2ba
author: ChristianMontoya
manager: scottman
ms.openlocfilehash: ad13fdce251c1f84bac725e9f1ee266c6aae5e13
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816316"
---
# <a name="whats-new-in-remote-desktop-services"></a>Novedades de servicios de escritorio remoto

Servicios de escritorio remoto (RDS) basado en Windows Server 2016 es una plataforma de virtualización que permite una amplia gama de escenarios de clientes. Mejoras en la solución global de RDS incorpora el trabajo realizado por el equipo de escritorio remoto y otros asociados de tecnología de Microsoft. Los escenarios y tecnologías siguientes son nuevas o mejoradas en Windows Server 2016.

Asegúrese también de consultar nuestra sesión de Ignite 2016: [Aproveche las mejoras de RDS en Windows Server 2016](https://channel9.msdn.com/Events/Ignite/2016/BRK3098). En este vídeo, el equipo de producto revisa todas las características nuevas y mejoradas en los servicios de escritorio remoto, incluida la compatibilidad con la vGPU. 

## <a name="app-compatibility---windows-server-2016-and-windows-10"></a>Compatibilidad de aplicaciones - Windows Server 2016 y Windows 10
Basado en la misma base de Windows 10, Windows Server 2016 no sólo tiene la misma apariencia y comportamiento esperar en un equipo de escritorio, pero también puede ejecutar muchas de las mismas aplicaciones. Emparejamiento de Windows Server 2016 con las capacidades gráficas (a continuación) proporciona un entorno para todos los usuarios ser productivos. 

## <a name="azure-sql-database---the-new-database-for-your-highly-available-environment"></a>Azure SQL Database: la nueva base de datos para su entorno de alta disponibilidad
El agente de conexión a Escritorio remoto es capaz de almacenar toda la información de implementación (por ejemplo, Estados de conexión y las asignaciones de usuario o host) en una base de datos SQL compartido, por ejemplo, una base de datos SQL de Azure. Deshágase del manual de implementación de SQL Server grupo de disponibilidad AlwaysOn, tome la cadena de conexión a la base de datos SQL de Azure y comenzar a usar el entorno de alta disponibilidad.

Información adicional: [Uso de Azure SQL Database para su entorno de alta disponibilidad del agente de conexión a Escritorio remoto](https://blogs.technet.microsoft.com/enterprisemobility/2016/05/03/new-windows-server-2016-capability-use-azure-sql-db-for-your-remote-desktop-connection-broker-high-availability-environment/)

## <a name="graphics---solving-graphics-needs-across-various-scenarios"></a>Gráficos - resolver todas las necesidades de gráficos en distintos escenarios
Gracias a la asignación de dispositivo discretos de Hyper-V, ahora puede asignar las GPU en un equipo host directamente a una máquina virtual que será consumido por las aplicaciones que requieren la GPU. También se han realizado mejoras en vGPU de RemoteFX, incluida la compatibilidad con OpenGL 4.4, OpenCL 1.1, 4 resolución k y máquinas virtuales Windows Server.

Información adicional: [Asignación discreta de dispositivos](https://blogs.technet.microsoft.com/virtualization/2015/11/)

## <a name="rd-connection-broker---improved-connection-handling-during-logon-storms"></a>RD Connection Broker - conexión mejorada lleva a cabo durante tormentas de inicio de sesión
Con el control de conexión mejorada, ahora es capaz de controlar más de 10.000 solicitudes de inicio de sesión simultáneos, a veces consideradas durante "tormentas de inicio de sesión" el agente de conexión a Escritorio remoto. El agente de conexión a Escritorio remoto mejorado también hace que el mantenimiento de la implementación más sencillo al más poder agregar rápidamente servidores en el entorno.

Información adicional: [Mejora el rendimiento del agente de conexión a Escritorio remoto](https://blogs.technet.microsoft.com/enterprisemobility/2015/12/15/improved-remote-desktop-connection-broker-performance-with-windows-server-2016-and-windows-server-2012-r2-hotfix-kb3091411/)

## <a name="rdp-10---new-capabilities-built-into-the-protocol"></a>RDP 10 - nuevas capacidades en el protocolo
RDP 10 ahora usa el códec h.264/AVC 444, optimizar adecuadamente en vídeo y texto. Con esta versión, también se admite la comunicación remota de lápiz. Con estas capacidades, las sesiones remotas empezará a sentirse más como una sesión local.  

Información adicional: [RDP 10 AVC/H.264 mejoras en Windows 10 y Windows Server 2016](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

## <a name="personal-session-desktops---providing-individual-desktops-to-any-end-user"></a>Sesión personal equipos de sobremesa escritorios individuales a cualquier usuario final
Escritorios personales sesión es una nueva forma de tener su propio escritorio personal que se hospedan en la nube. Privilegios de administrador y quita los hosts de sesión dedicada la complejidad del hospedaje en entornos donde los usuarios desean administrar el equipo de escritorio como lo es su propios.

Información adicional: [Escritorios personales sesión](rds-personal-session-desktops.md)
