---
title: Novedades de Servicios de Escritorio remoto (RDS)
description: Proporciona la descripción de las nuevas características de RDS en Windows Server 2016.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/11/2016
ms.topic: article
ms.assetid: 04d52dff-e61b-4633-9908-be8600abc2ba
author: ChristianMontoya
manager: scottman
ms.openlocfilehash: 43c280996334ea3b371c54f86b1cd4f86072124b
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86963317"
---
# <a name="whats-new-in-remote-desktop-services"></a>Novedades de Servicios de Escritorio remoto (RDS)

Los Servicios de escritorio remoto (RDS) integrados en Windows Server 2016 constituyen una plataforma de virtualización que permite una amplia gama de escenarios de clientes. Las mejoras en la solución global de RDS incluyen el trabajo realizado por el equipo de Escritorio remoto y por otros asociados tecnológicos de Microsoft. Los escenarios y tecnologías siguientes son nuevos o se han mejorado en Windows Server 2016.

No olvides consultar también nuestra sesión de Ignite 2016: [Aprovechamiento de las mejoras de RDS en Windows Server 2016](https://channel9.msdn.com/Events/Ignite/2016/BRK3098). En este vídeo, el equipo de producto revisa todas las características nuevas y mejoradas de los Servicios de Escritorio remoto, incluida la compatibilidad con vGPU. 

## <a name="app-compatibility---windows-server-2016-and-windows-10"></a>Compatibilidad entre aplicaciones: Windows Server 2016 y Windows 10
Creado basándose en los mismos aspectos básicos de Windows 10, Windows Server 2016 no solo tiene la misma apariencia esperable en un escritorio, sino que también ejecuta muchas de las mismas aplicaciones. El emparejamiento de Windows Server 2016 con las funcionalidades gráficas siguientes te proporciona un entorno en el que todos los usuarios pueden ser productivos. 

## <a name="azure-sql-database---the-new-database-for-your-highly-available-environment"></a>Azure SQL Database: la nueva base de datos para tu entorno de alta disponibilidad
El Agente de conexión a Escritorio remoto es capaz de almacenar toda la información de implementación (por ejemplo, los estados de conexión y las asignaciones de usuarios o hosts) en una base de datos SQL compartida como, por ejemplo, una base de datos de Azure SQL. Olvídate del manual de implementación de los grupos de disponibilidad AlwaysOn de SQL Server, usa la cadena de conexión a la base de datos de Azure SQL y empieza a usar tu entorno de alta disponibilidad.

Información adicional: [Uso de Azure SQL Database para su entorno de alta disponibilidad del Agente de conexión a Escritorio remoto](https://techcommunity.microsoft.com/t5/microsoft-security-and/new-windows-server-2016-capability-use-azure-sql-db-for-your/ba-p/249787)

## <a name="graphics---solving-graphics-needs-across-various-scenarios"></a>Gráficos: Solución de necesidades gráficas en varios escenarios
Gracias a la asignación de dispositivos discreta de Hyper-V, ahora puedes asignar las GPU de una máquina host directamente a una máquina virtual para que las usen las aplicaciones que lo necesiten. También se han realizado mejoras en RemoteFX vGPU, entre las que se incluyen la compatibilidad con OpenGL 4.4, OpenCL 1.1, resolución 4k y máquinas virtuales de Windows Server.

Información adicional: [Asignación de dispositivos discreta](https://techcommunity.microsoft.com/t5/virtualization/bg-p/Virtualization)

## <a name="rd-connection-broker---improved-connection-handling-during-logon-storms"></a>Agente de conexión a Escritorio remoto: Control de conexión mejorado durante tormentas de inicio de sesión
Gracias al control mejorado de las conexiones, el Agente de conexión a Escritorio remoto puede ahora administrar más de 10 000 solicitudes de inicio de sesión simultáneas, lo cual sucede a veces durante las llamadas "tormentas de inicio de sesión". El Agente de conexión a Escritorio remoto mejorado también hace que el mantenimiento de la implementación sea más sencillo ya que permite agregar servidores con mayor rapidez de nuevo en el entorno.

Información adicional: [Rendimiento mejorado del Agente de conexión a Escritorio remoto](https://techcommunity.microsoft.com/t5/microsoft-security-and/improved-remote-desktop-connection-broker-performance-with/ba-p/249559)

## <a name="rdp-10---new-capabilities-built-into-the-protocol"></a>RDP 10: se han integrado nuevas funcionalidades en el protocolo
RDP 10 usa ahora el códec H.264/AVC 444, lo que permite optimizar adecuadamente entre vídeo y texto. En esta versión, también se admite la comunicación remota de lápiz. Con estas funcionalidades, las sesiones remotas empiezan a parecerse aún más a una sesión local.  

Información adicional: [Mejoras de RDP 10 AVC/H.264 en Windows 10 y Windows Server 2016](https://techcommunity.microsoft.com/t5/microsoft-security-and/remote-desktop-protocol-rdp-10-avc-h-264-improvements-in-windows/ba-p/249588)

## <a name="personal-session-desktops---providing-individual-desktops-to-any-end-user"></a>Escritorios de sesión personal: Proporcionar escritorios individuales a cualquier usuario final
Los escritorios de sesión personal son una nueva manera de tener tu propio escritorio personal hospedado en la nube. Los privilegios administrativos y los hosts de sesión dedicados eliminan la dificultad a la hora de hospedar entornos en los que los usuarios desean administrar el escritorio como si fuera el suyo propio.

Información adicional: [Escritorios de sesión personal](rds-personal-session-desktops.md)
