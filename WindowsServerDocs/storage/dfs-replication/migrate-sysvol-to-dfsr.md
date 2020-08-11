---
title: Migración de la replicación de SYSVOL a la replicación DFS
ms.date: 07/02/2012
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 6e022e4fdae631199bacca5f67c7953125ddd141
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950739"
---
# <a name="migrate-sysvol-replication-to-dfs-replication"></a>Migración de la replicación de SYSVOL a la replicación DFS


Actualizado: 25 de agosto de 2010

Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 y Windows Server 2008

Los controladores de dominio usan una carpeta compartida especial llamada "SYSVOL" para replicar los scripts de inicio de sesión y los archivos del objeto de directiva de grupo en otros controladores de dominio. Windows 2000 Server y Windows Server 2003 usan el servicio de replicación de archivos (FRS) para replicar SYSVOL, mientras que Windows Server 2008 usa el servicio de replicación DFS más reciente en los dominios que usan el nivel funcional de dominio de Windows Server 2008, y usa FRS para los dominios que ejecutan niveles funcionales de dominio anteriores.

Para usar la replicación DFS en la replicación de la carpeta SYSVOL, puedes crear un nuevo dominio que use el nivel funcional de dominio de Windows Server 2008, o bien puedes usar el procedimiento que se describe en este documento para actualizar un dominio existente y migrar a la replicación DFS.

En este documento se supone que tienes un conocimiento básico de Active Directory Domain Services (AD DS), FRS y la replicación de sistema de archivos distribuido (replicación DFS). Para obtener más información, consulta la [Información general sobre Active Directory Domain Services](https://go.microsoft.com/fwlink/?linkid=147787), la [Información general sobre FRS](https://go.microsoft.com/fwlink/?linkid=121763) o la [Información general sobre la replicación DFS](https://go.microsoft.com/fwlink/?linkid=121762)


> [!NOTE]
> Para descargar una versión imprimible de esta guía, visita <a href="https://go.microsoft.com/fwlink/?linkid=150375">Guía de migración para replicación SYSVOL: FRS a replicación DFS</a> (https://go.microsoft.com/fwlink/?LinkId=150375)
<br>


## <a name="in-this-guide"></a>En esta guía

[Información conceptual de la migración SYSVOL](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd640170(v=ws.10))

  - [Estados de la migración SYSVOL](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd641052(v=ws.10))

  - [Información general del procedimiento de migración SYSVOL](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd639809(v=ws.10))


[Procedimiento de migración SYSVOL](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd639860(v=ws.10))

  - [Migración al estado Prepared](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd641193(v=ws.10))

  - [Migración al estado Redirected](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd641340(v=ws.10))

  - [Migración al estado Eliminated](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd640254(v=ws.10))


[Solución de problemas de migración SYSVOL](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd640395(v=ws.10))

  - [Solución de los problemas de migración SYSVOL](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd639976(v=ws.10))

  - [Revertir la migración SYSVOL a un estado estable anterior](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd640509(v=ws.10))


[Información de referencia de la migración SYSVOL](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd640293(v=ws.10))

  - [Escenarios de migración SYSVOL admitidos](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd639854(v=ws.10))

  - [Comprobación del estado de la migración SYSVOL](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd639789(v=ws.10))

  - [Dfsrmig](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd641227(v=ws.10))

  - [Acciones de las herramientas de migración SYSVOL](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/dd639712(v=ws.10))


## <a name="additional-references"></a>Referencias adicionales

[Serie sobre los estados de la migración SYSVOL: Parte 1: introducción al proceso de migración SYSVOL](https://go.microsoft.com/fwlink/?linkid=121756)

[Serie sobre los estados de la migración SYSVOL: Parte 2: Dfsrmig.exe: la herramientas de migración SYSVOL](https://go.microsoft.com/fwlink/?linkid=121757)

[Serie sobre los estados de la migración SYSVOL: Parte 3: migración al estado "PREPARED"](https://go.microsoft.com/fwlink/?linkid=121758)

[Serie sobre los estados de la migración SYSVOL: Parte 4: migración al estado "REDIRECTED"](https://go.microsoft.com/fwlink/?linkid=121759)

[Serie sobre los estados de la migración SYSVOL: Parte 5: migración al estado "ELIMINATED"](https://go.microsoft.com/fwlink/?linkid=121760)

[Guía paso a paso para sistemas de archivos distribuidos en Windows Server 2008](https://go.microsoft.com/fwlink/?linkid=85231)

[Referencia técnica de FRS](https://go.microsoft.com/fwlink/?linkid=121764)
